---
name: remote-mcp-deploy
description: Takes an MCP server from a working local process to a correctly behaving public endpoint on protocol revision 2026-07-28. Covers the single POST path, what to answer for the verbs the revision removed, the required mirrored request headers, the exact HTTP status and JSON-RPC error code pairings, SSE buffering and keep-alives, cancellation by stream close, horizontal scaling and the one piece of state that still crosses nodes, and the legacy era posture. Includes a raw HTTP conformance suite that runs on macOS and Windows. This skill should be used before an MCP endpoint is exposed publicly, when a deployed endpoint behaves differently from localhost, or when a streamed call hangs behind a proxy.
---

# Remote MCP deploy

**Verified against live documentation on 20 August 2026, for protocol revision 2026-07-28.** Sources checked: the Streamable HTTP transport and base protocol pages for that revision, `schema.ts` in the specification repository, and the TypeScript SDK repository on `main`. Anything that could not be verified is labelled unverified near the end of this file and is not asserted anywhere else in it.

## The claim, and why a working local server proves nothing

An MCP endpoint that works on `127.0.0.1` has demonstrated exactly one thing: your handler runs. It has not demonstrated any of the transport contract, because roughly half of that contract is about what happens when something sits between the client and your process.

Revision 2026-07-28 makes this sharper than it used to be, because it removed a great deal. Protocol sessions are gone, along with the `Mcp-Session-Id` header. The standalone HTTP GET stream is gone. SSE resumability is gone, so `Last-Event-ID` and event ids no longer mean anything. And the `initialize` handshake is gone: every request now carries its own protocol version and client capabilities in `_meta`, and servers **must** implement `server/discover`.

What replaced all of it is a very small surface with very specific answers. One path. One verb. Three required headers. Six status codes. One response header that only matters behind a proxy. Get them right and the endpoint is boring. Get one wrong and the symptom will point somewhere else entirely.

## 1. One path, and it accepts POST

The server must provide a single HTTP endpoint path, the MCP endpoint, that supports POST. For example `https://example.com/mcp`. The revision defines POST and nothing else.

Every JSON-RPC message from the client is its own POST. The body is a single JSON-RPC request or notification, never a response.

## 2. Answer the removed verbs deliberately

A server that only speaks this revision, receiving traffic from an older client, should answer as follows:

- **GET or DELETE on the MCP endpoint: 405 Method Not Allowed.** Not 404, and specifically not a framework's HTML 404 page.
- **An `Mcp-Session-Id` header on a request: ignore it.** Do not mint one, do not echo one.
- **A `Last-Event-ID` header: ignore it.** Streams are not resumable.

The 405 matters more than it looks. A dual-era client deciding what a server is inspects the response body of a 4xx: a recognised modern JSON-RPC error means the server is modern and the client should correct its request, and anything else means fall back. A framework's HTML 404 is indistinguishable from a dead route, so the client falls back on the wrong branch or fails on the wrong one.

## 3. Validate `Origin`, and bind loopback locally

Servers **must** validate the `Origin` header on all incoming connections to prevent DNS rebinding. If the header is present and invalid, respond **403 Forbidden**. The body may be a JSON-RPC error response with no `id`.

When running locally, bind only to `127.0.0.1` rather than `0.0.0.0`.

The specific trap: binding `0.0.0.0` behind a proxy usually drops the SDK's default localhost host validation, and every request then 403s until you list the public hostname you actually serve in the allowed hosts. It looks like a certificate or routing fault and is neither.

## 4. The three required request headers

The transport mirrors selected body fields into HTTP headers so that intermediaries can route and inspect requests without parsing the body. The body remains the source of truth.

| Header | Mirrors | Required for |
| --- | --- | --- |
| `MCP-Protocol-Version` | `_meta` protocol version | every POST |
| `Mcp-Method` | `method` | all requests |
| `Mcp-Name` | `params.name` or `params.uri` | `tools/call`, `resources/read`, `prompts/get` |

Header **names** are case-insensitive and must be compared case-insensitively. Header **values**, including method names, are **case-sensitive**.

Servers that process the body **must** reject any request whose header values do not match the corresponding body values, because a load balancer routing on the header while your server executes on the body is a real security gap.

Two encoding details that produce confusing failures. A value that cannot be represented as plain ASCII is carried as `=?base64?<encoded>?=`, and the markers are lowercase and exact; servers must decode before comparing to the body. And `x-mcp-header` in a tool's input schema mirrors an argument into `Mcp-Param-<Name>`; intermediaries that do not recognise such a header must forward it and otherwise ignore it.

## 5. The status and error code table

This is the part that is not guessable, and the part a single command each can prove.

| Condition | HTTP | JSON-RPC code |
| --- | --- | --- |
| Origin present and invalid | 403 | none required |
| GET or DELETE on the endpoint | 405 | none |
| Required header missing, mismatched, or containing invalid characters | 400 | `-32020` HeaderMismatch |
| Request missing a required `_meta` field | 400 | `-32602` Invalid params |
| Client capability the server needs was not declared | 400 | `-32021` MissingRequiredClientCapability, with `data.requiredCapabilities` |
| Protocol version not implemented | 400 | `-32022` UnsupportedProtocolVersion, with `data.supported` and `data.requested` |
| RPC method not implemented | **404** | `-32601` Method not found |
| Notification accepted | **202, empty body** | none |
| Notification rejected | 4xx, for example 400 | error response with no `id` |

Two range rules govern anything you add. `-32000` to `-32019` is legacy and implementation-defined, and new implementations should not use it. `-32020` to `-32099` is **reserved for the MCP specification**, and implementations must not emit a code from that range that the specification has not defined. Resource-not-found moved from `-32002` to `-32602` in this revision; clients should still accept `-32002` from older servers.

The 404 for an unknown method is the one that surprises people. It is deliberate: the JSON-RPC error body is what distinguishes it from a 404 returned by a legacy server that does not host the modern endpoint at all.

## 6. Choose a response shape per request

The client **must** send an `Accept` header listing both `application/json` and `text/event-stream`. For a request, the server returns either a single JSON object or an SSE response stream scoped to that request, and the client must support both.

In the v2 SDK, `responseMode` pins the choice. `'json'` never streams and **drops mid-call notifications**, delivering only the terminal result. `'sse'` always streams. Left unset, the handler answers with JSON and upgrades to a stream only when a tool handler emits a notification before its result. A `subscriptions/listen` stream stays SSE whichever you pick.

Pinning `'json'` to simplify a deployment silently discards progress and log notifications. Nothing errors. The tool just looks slower and quieter than it did locally.

## 7. Set `X-Accel-Buffering: no` on every SSE response

When initiating an SSE stream, servers **should** include `X-Accel-Buffering: no`. This tells nginx-class reverse proxies to disable response buffering. Without it, a proxy may accumulate events and release them together.

This is the failure that costs the most time to diagnose, because the evidence points away from the cause. Locally it is perfect. Behind the proxy the call appears to hang, and your own access log records a completed 200 with a normal duration, because from your process's point of view the response finished. The delay lives entirely in the intermediary.

For long-lived streams, especially `subscriptions/listen`, emit an SSE **comment line** periodically as a keep-alive: a line beginning with a colon, for example `:` followed by the line terminator. Any line beginning with a colon is a comment carrying no event data, and clients must ignore it rather than treat it as malformed. Without one, an idle stream is closed by an intermediary or a client idle timeout during a quiet period, and the symptom is a subscription that dies after a fixed interval that matches nothing in your code.

## 8. Cancellation is the client closing the stream

There is no `notifications/cancelled` on Streamable HTTP. That message exists only on stdio.

Closing the SSE response stream **must** be treated by the server as cancellation of that request. Because each request has its own response stream, the disconnect is unambiguous. The server should stop work as soon as practical and **must not** send any further messages for that request.

A server that ignores this keeps running an expensive query, finishes it, and writes into a socket nobody is holding. Under load it is a compounding waste: every abandoned call still consumes a full unit of work.

## 9. Change notifications go through `subscriptions/listen`

The GET stream and `resources/subscribe` were both replaced by `subscriptions/listen`: one long-lived POST response stream carrying only the notification types the client opted in to. The server **must not** send types the client did not request.

The filter fields are exactly these: `toolsListChanged`, `promptsListChanged`, `resourcesListChanged` (all booleans) and `resourceSubscriptions` (an array of resource URIs). All are optional; omitting one means not subscribing to it.

The server **must** send `notifications/subscriptions/acknowledged` as the first message, carrying the subscription id in `_meta` under `io.modelcontextprotocol/subscriptionId`, and every later notification on the stream carries the same field. The value is the JSON-RPC id of the `subscriptions/listen` request. The acknowledgement's own `notifications` field reflects the subset the server agreed to honour, so a client can detect that a type it asked for is unsupported.

When the server ends a subscription itself, for instance during shutdown, it should respond to the original request with an empty result before closing, which is how a client distinguishes a graceful close from a dropped connection.

## 10. Implement `server/discover`

Servers **must** implement it. Clients may call it before anything else for up-front version selection, and on stdio it doubles as the backward-compatibility probe.

The result carries `supportedVersions` (the versions the server supports), `capabilities`, an optional `instructions` string of natural-language guidance, `io.modelcontextprotocol/serverInfo` in `_meta`, and, because it is a cacheable result, the **required** `ttlMs` and `cacheScope`.

`ttlMs` is a freshness hint in milliseconds with a minimum of 0, where 0 means immediately stale. `cacheScope` is `"public"` or `"private"`, and `"private"` means caches **must not** be shared across authorisation contexts. The same two fields are required on results from `tools/list`, `prompts/list`, `resources/list`, `resources/read` and `resources/templates/list`. Any list whose contents vary by the caller's token is `"private"`, and getting that wrong on a shared gateway serves one tenant's tool list to another.

## 11. Scaling, and the one thing that still crosses nodes

The stateless model is the scaling story. Each node builds a fresh server instance per request and holds nothing between requests, so put the nodes behind any load balancer: no session affinity, nothing shared, nothing to configure.

One thing still crosses nodes, and it is easy to miss because it works perfectly on one node. **`subscriptions/listen` streams deliver events published on the handler's `ServerEventBus`, and the default bus is in-process.** A change published on node A never reaches a subscriber whose stream node B is holding.

The fix is to implement the two-method `ServerEventBus` interface, `publish` and `subscribe`, over your own pub/sub backend, and pass the same instance to every node's handler.

The symptom without it is genuinely nasty. Nothing errors. Roughly half your users see the updated tool list and roughly half do not, and which half depends on which node happened to hold their stream. It presents as flakiness in the client.

## 12. Declare a legacy posture on purpose

The SDK handler has two postures. The default serves 2025-era clients statelessly from the same factory, per request, with no sessions; under it a legacy GET or DELETE answers 405. The strict posture answers a 2025 `initialize` with 400 and `-32022` naming the one revision the endpoint serves.

To keep an existing sessionful deployment alive for the clients it already has, route in front of a strict handler using the SDK's own classification predicate, so the branch cannot disagree with the handler about which era a request belongs to. One practical trap: behind an Express body parser the Node stream is already drained, so build the request object the predicate takes from the parsed body rather than from the raw stream.

The 2024-11-05 HTTP+SSE transport is classified as **Deprecated** under the feature lifecycle policy, which carries a minimum twelve-month window. The v2 server never serves it. If a deployment truly cannot move yet, the frozen v1 transport ships separately, mounted on a GET stream route plus a POST message route, and it needs its JSON body limit raised above the framework's 100kb default, because that transport accepts messages up to 4mb.

TLS from a recognised certificate authority is required by Anthropic's directory policy for remote servers that connect to a remote service and require authentication. Assume it is required regardless.

## The conformance suite, on macOS and on Windows

Two portability rules first, and both cause misleading failures.

**PowerShell aliases `curl` to `Invoke-WebRequest`,** which takes entirely different arguments. On Windows use `curl.exe` explicitly. On macOS and Linux, `curl` is correct.

**Send bodies with `--data-binary @file`, never inline quoted JSON.** This applies on both platforms. Nested quoting differs between shells, and a mangled body arrives as a JSON parse error, which reads as a server fault and is a shell fault. Put the request in a file.

`req.json`:

```json
{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{"_meta":{"io.modelcontextprotocol/protocolVersion":"2026-07-28","io.modelcontextprotocol/clientCapabilities":{}}}}
```

The seven assertions, each printing only the status code. macOS:

```
URL=https://mcp.example.com/mcp
curl -s -o /dev/null -w '%{http_code}\n' -X GET "$URL"
curl -s -o /dev/null -w '%{http_code}\n' -X DELETE "$URL"
curl -s -o /dev/null -w '%{http_code}\n' -H 'Origin: https://evil.example' -X POST "$URL" --data-binary @req.json
curl -s -w '\n%{http_code}\n' -X POST "$URL" \
  -H 'Content-Type: application/json' -H 'Accept: application/json, text/event-stream' \
  -H 'MCP-Protocol-Version: 2026-07-28' --data-binary @req.json
curl -s -w '\n%{http_code}\n' -X POST "$URL" \
  -H 'Content-Type: application/json' -H 'Accept: application/json, text/event-stream' \
  -H 'MCP-Protocol-Version: 2026-07-28' -H 'Mcp-Method: tools/list' --data-binary @req.json
```

Windows PowerShell, same requests:

```
$URL = "https://mcp.example.com/mcp"
curl.exe -s -o NUL -w "%{http_code}`n" -X GET $URL
curl.exe -s -o NUL -w "%{http_code}`n" -X DELETE $URL
curl.exe -s -w "`n%{http_code}`n" -X POST $URL `
  -H "Content-Type: application/json" -H "Accept: application/json, text/event-stream" `
  -H "MCP-Protocol-Version: 2026-07-28" -H "Mcp-Method: tools/list" --data-binary "@req.json"
```

Expected: 405, 405, 403, then 400 with `-32020` for the request with no `Mcp-Method`, then 200 for the complete one. Add `-D -` and grep the headers of any streaming response for `x-accel-buffering`. Then swap the version in both the header and the body for a nonsense one and expect 400 with `-32022` carrying `data.supported`, and change the method in both header and body to a method you do not implement and expect **404** with `-32601`.

## Decision rule: is this a buffering fault, a header fault, or something you cannot tell yet?

1. **A call hangs, and your access log shows a completed 200 with a normal duration.** Buffering. Check for `X-Accel-Buffering: no` on the response and for streaming support on the intermediary. Your process finished; the bytes did not arrive.
2. **A call hangs and your log shows no completion.** Your handler. Not a transport problem. Stop reading this file.
3. **`tools/list` returns 200 and `tools/call` returns 400 with `-32020`.** A required header, almost always `Mcp-Name`, is missing at your process. Log the raw inbound headers at the edge of your handler and compare against what the client sent. If they differ, an intermediary stripped or rewrote it, and the fix is in the proxy configuration rather than in your code.
4. **Notifications reach some clients and not others, with no errors anywhere.** More than one node with an in-process event bus. Confirm by pinning traffic to a single node and seeing the inconsistency vanish.
5. **You cannot tell.** The most common shape of this is an intermittent failure with a 2xx in every log you own. Before theorising, do two things in this order. **Turn on HTTP access logging including 2xx**, because many application servers do not log successful requests by default, which makes a fully successful exchange that the client nonetheless rejected completely invisible. Then run the conformance suite above from outside your network, not from a machine inside it, because half these faults are introduced by the layer a local test skips. If the suite is clean from outside and the fault persists, it is not the transport, and the next file to open is the authorisation one, where a mismatched canonical resource URI produces exactly this signature: a flow with nothing but 2xx in it that the client abandons without making a single MCP call.

## Worked example, compressed

A project management service deploys its MCP endpoint behind a managed edge proxy, on two nodes.

**Local.** Everything passes. Tools list, tools call, a streamed long search emits progress.

**First deploy, first run of the suite.** GET returns the framework's HTML 404 rather than 405. Fixed with an explicit route. DELETE the same. Origin check returns 200 on a hostile origin, because the bind moved to `0.0.0.0` and dropped the default host validation; the public hostname is added to the allowed hosts and the check returns 403.

**The streaming symptom.** The long search hangs for about forty seconds and then delivers everything at once. The service's own log shows a 200 completed in four seconds. `X-Accel-Buffering: no` was never set, because the SDK sets it and a custom response wrapper was rebuilding headers and dropping it. Restored.

**The header symptom.** `tools/list` is fine. Every `tools/call` returns 400 with `-32020`. Inbound header logging at the handler edge shows `Mcp-Name` absent, while the client sent it. An allowlist in the edge configuration was forwarding only known headers. Fixed in the proxy, not in the server.

**The notification symptom.** After a tool set change, some sessions re-fetch and some do not. Two nodes, default in-process bus. A shared bus over the existing pub/sub backend is implemented and handed to both handlers.

**The era decision.** An older internal client still connects with `initialize`. The default posture is kept rather than switching to strict, so both eras are served from the same factory, and the decision is recorded with its date.

**Verdict: five distinct faults, none of which was visible from a green local run, and four of which produced a symptom pointing at the wrong layer.** The suite found four of them in under ten minutes. The fifth, the split-brain notifications, produced no error anywhere and was only found because someone knew the default bus is in-process.

## What is unverified in this file

- **Which protocol revision Claude's own connector client speaks.** Not documented anywhere checked on 20 August 2026. This is why the recommendation is the dual-era default rather than a strict endpoint.
- **Any numeric limit on response size, tool count or description length.** None is published. Do not invent one, and do not size your pagination against a limit you assumed. Budget against the model's context window.
- **How long an intermediary will hold an idle SSE stream open.** Entirely platform-specific and not part of the protocol. The keep-alive comment line is the mitigation; the actual timeout is your host's to document.
- **Whether any given managed platform buffers SSE by default.** Varies, changes, and must be tested rather than assumed. The suite above is the test.

## Failure modes

**The buffered stream.** Works on localhost, appears to hang behind the CDN, and the server's own log shows a completed 200 of normal duration. The delay is entirely in an intermediary, so every piece of evidence you own points at the wrong layer.

**The split-brain notification.** Two nodes, an in-process bus, and half the subscribers never see a change. Nothing errors, nothing is logged, and the report arrives as intermittent client flakiness.

**The 404 that reads as server-down.** An HTML 404 where a JSON-RPC `-32601` belongs. A dual-era client cannot distinguish a modern server from a broken route and falls back or fails on the wrong branch.

**The header the load balancer rewrote.** `Mcp-Name` stripped in transit. Every `tools/call` 400s while `tools/list` is fine, which reads as a per-tool bug and is a proxy configuration.

**The session that will not die.** Sticky routing rules kept from a sessionful deployment that no longer needs them. Nothing breaks, nothing errors, and you keep paying for affinity that buys nothing and blocks even distribution.

**The 403 nobody expects.** Bound to `0.0.0.0` behind a proxy without listing the public hostname in the allowed hosts. Every request 403s from the moment of deploy, and it presents as a routing or certificate fault.

**The silent response-mode downgrade.** `responseMode: 'json'` pinned to simplify a proxy configuration, which drops every mid-call notification. Progress and log messages vanish, no error is raised, and the tool simply looks slower and quieter than it did locally.

**The subscription that dies on a timer.** No keep-alive comment line on a long-lived listen stream, so an intermediary closes it during a quiet period. The interval matches nothing in your code, which is what makes it hard to attribute.

## What this skill does not do

- It does not write or review the server. The handler, the tools and the token verification are upstream and belong to sibling files.
- It cannot inspect your proxy, your CDN or your load balancer. Every header rule here is a claim about what must arrive at your process, and only the curl suite can tell you whether it does.
- It says nothing about capacity, cost, cold starts, concurrency limits or how long a streamed call may run. Those need real traffic and a metrics stack.
- It does not cover stdio deployment, where none of the HTTP rules apply and the constraints are different, starting with never writing to standard output.
- It does not tell you which host to pick. Managed edge platforms, container hosts and plain virtual machines all satisfy this contract, and the trade-offs between them are outside it.
- It will be wrong on a date it cannot predict. It names its revision and its verification date so the staleness is visible instead of silent, and the removals described here happened to rules that were correct for the sixteen months before them.
