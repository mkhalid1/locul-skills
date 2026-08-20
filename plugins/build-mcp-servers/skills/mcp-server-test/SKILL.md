---
name: mcp-server-test
description: Builds a three-layer verification harness for an MCP server, combining an in-process layer that serves the deployed request handler with no socket, a command-line layer that drives the deployed URL through the MCP Inspector CLI and branches on its exit codes, and a raw HTTP layer that asserts the transport and authorisation behaviour the SDK cannot assert for you. Contains the exit-code taxonomy, the flags that make a run non-interactive, the status and error codes the 2026-07-28 revision requires, and the reason the obvious negative test passes on a broken tool. This skill should be used when writing or reviewing tests for an MCP server, when adding an MCP check to continuous integration, or when a server passes its suite but a client cannot connect to it.
---

# Testing an MCP server

Every specific in this file was verified against live documentation on 20 August 2026, and the protocol revision described is 2026-07-28.

## The claim this skill is built on

A green MCP suite sitting over a connector that will not connect is the ordinary outcome, not an unlucky one. Three separate mechanisms produce it, and none of them is a lapse of care. They are properties of the tools.

**A tool handler failure is not an exception.** In the current TypeScript SDK a failing tool resolves as an ordinary result carrying `isError: true`. There is nothing to catch. The negative test everyone writes first, some form of `expect(call).rejects.toThrow()`, therefore never fires. It passes when the tool works and it passes when the tool is broken, which makes it a test that cannot fail. Arguments the input schema rejects produce the same shape and never reach your handler at all, so they assert the same way.

**The in-process harness never touches your deployment.** The documented pattern passes `handler.fetch` as the client transport's `fetch` option, so the transport never dials the URL. That is exactly what you want for speed, and it means your reverse proxy, your content delivery network, your `Origin` validation, your answer to a GET and your authorisation challenge are all outside the test.

**The command-line client is interactive by default.** Point the Inspector CLI at a server that requires authorisation and it will start an OAuth flow on a loopback callback listener and open a browser. It only fails fast when neither stdin nor stderr is a terminal. A runner that allocates a pseudo-terminal, which many do, gets the waiting behaviour instead.

So: three layers, and the discipline is that you do not stop after the first one. The order matters because each layer can only see faults the previous one is structurally blind to, and because layers one and two are cheap enough to run on every commit while layer three is worth running on every deploy.

## Layer one: in-process, no socket

Serve the exact request handler you deploy, in the test process, with no port bound.

Pass `handler.fetch` as the `fetch` option of a Streamable HTTP client transport pointed at any URL you like. The URL is never dialled; every request is served in process by the same handler factory that runs in production. One `Client` plus one handler is a complete integration test of the server, minus its hosting.

**Assert on `structuredContent` for the happy path.** If a tool declares an output schema, the structured result is the machine-readable contract and the text block is the compatibility copy. Asserting on the text block is asserting on prose you will later want to change.

**Assert failures by shape, never by exception.** The correct negative test reads the result:

```
const failed = await client.callTool({ name: 'apply_discount', arguments: { price: -5 } });
assert.equal(failed.isError, true);
assert.match(failed.content[0].text, /price must be/);
```

Assert on the message too, not only the flag. The message is what the model reads and what it self-corrects from, and a tool that returns `isError: true` with an empty or generic body is a directory rejection reason as well as a bad test.

**Tear down in order: client first, then handler.** Closing the handler aborts any exchange still in flight, so a hung call cannot leak into the next test. Reverse the order and you get a class of cross-test interference that looks like flakiness and is not.

**Pin the negotiated era.** The client reports which protocol era the connection landed on. Assert it. The 2026-07-28 revision has no `initialize` handshake; the 2024-10-07 through 2025-11-25 family does. A dependency bump that flips negotiation back to the older family will not fail a single behavioural test, because both eras serve the same tools. Only an explicit assertion catches it.

**In-memory linked transport pairs are not a substitute.** The SDK's linked-pair helper connects instances of the older era only. If your suite is built entirely on it, you have no coverage of the current revision at all, and nothing will tell you.

**Stdio has no in-process shortcut.** Spawn the real process with the stdio client transport. Give the command path relative to the repository root so it resolves identically on macOS and on Windows, and never hard-code a platform separator. One rule dominates here: a stdio server must never write anything to stdout that is not a protocol message. A stray `console.log` corrupts the stream, and the symptom is a parse error that names none of the code responsible.

## Layer two: the Inspector CLI against the deployed URL

The MCP Inspector is one package, `@modelcontextprotocol/inspector`, with three clients: a web UI (the default), `--cli`, and `--tui`. Version 2.3.0 was published on 19 August 2026 and requires Node 22.19.0 or newer. Two things about the command line are load-bearing and neither is guessable.

**The mode flag must come first**, immediately after the binary name. Everything after it is forwarded to that client unchanged.

**Under `--cli`, the server target must precede every flag.** The CLI reads the leading run of non-dash tokens as the target. Put a flag first and the target is discarded without an error, and the run falls back to your catalogue file. It appears to work, against a different server. This is the highest-cost trap in the whole tool, because the failure mode is a passing test on the wrong subject.

```
mcp-inspector --cli node build/index.js --method tools/list      # correct
mcp-inspector --cli --method tools/list node build/index.js      # target silently dropped
```

**Transport is inferred only from the URL path suffix.** A path ending `/mcp` infers Streamable HTTP, a path ending `/sse` infers SSE, and anything else is an error telling you to pass `--transport` explicitly. The suffix match is exact, so a trailing slash is ambiguous too. Pass `--transport http` and stop relying on inference.

**Argument coercion has two modes and you must choose deliberately.** `--tool-arg key=value` JSON-parses and coerces, so `count=1` arrives as a number and a zero-padded identifier such as `012` arrives as the integer 12. `--tool-args-json '{"code":"012"}'` is passed verbatim. They are mutually exclusive. Any test involving identifiers, postcodes, account numbers or version strings should use the JSON form, or the harness silently tests a different input than production sends.

**Branch on exit codes, never on scraped prose.**

| Code | Meaning |
| --- | --- |
| 0 | Success |
| 1 | Usage or unexpected error |
| 2 | No MCP App on the tool, from an `--app-info` probe |
| 3 | Server requires authentication |
| 4 | Server unreachable: DNS failure, connection refused, timeout |
| 5 | Tool error: the call returned `isError: true`, or the tool was not found |

On any non-zero exit the CLI writes one JSON line to stderr carrying `code`, `message`, and where known `status` and `url`. Because it is exactly one line, parse it with `2>&1 | tail -1 | jq .error`. Note that code 5 covers both a failing tool and a missing tool; if you need to tell those apart, assert on the envelope's `code` string rather than the exit status.

The consequence people meet first: a `tools/call` that returns `isError: true` still prints its payload but exits 5, so an `&&` chain stops. A pipeline that "started failing after the upgrade" is usually a tool that was failing all along.

**Make the run non-interactive with `--stored-auth-only`.** It never starts an interactive flow, never opens a browser, uses a stored token if one exists, and otherwise fails immediately with an authentication-required envelope. Without it, the behaviour depends on whether a terminal is attached: with no terminal on stdin or stderr the CLI fails fast, but with one it can wait on a loopback callback for up to fifteen minutes. Do not rely on your runner being terminal-free. Set the flag.

**Windows is a first-class target and needs two adjustments.** In PowerShell, `curl` is an alias for `Invoke-WebRequest` and does not take the same flags, so every raw HTTP command in layer three must be written `curl.exe`. And an `&&` chain plus `jq` is Bash syntax: run the recipe under Git Bash or WSL, or translate it, checking `$LASTEXITCODE` after each invocation instead of chaining.

## Layer three: transport conformance over the real wire

These are the assertions the SDK cannot make for you, because they are statements about your deployment rather than about your handler. Send bodies with `--data-binary @request.json`, never as inline quoted JSON: nested shell quoting mangles the body and you get a misleading parse error instead of the behaviour you were testing.

| Request | Required response |
| --- | --- |
| POST a valid request | 200 with `Content-Type: application/json` or `text/event-stream` |
| POST an accepted notification | 202 Accepted with no body |
| GET the MCP endpoint | 405 Method Not Allowed |
| DELETE the MCP endpoint | 405 Method Not Allowed |
| POST with an invalid `Origin` header present | 403 Forbidden |
| POST omitting `Mcp-Method` | 400 with JSON-RPC error `-32020` |
| POST where `Mcp-Name` disagrees with `params.name` | 400 with `-32020` |
| POST an unknown method | 404 with `-32601` |
| POST `MCP-Protocol-Version: 1999-01-01` | 400 with `-32022`, body listing supported versions |

The reserved range matters when you are writing the assertions: `-32020` to `-32099` belongs to the specification, `-32000` to `-32019` is legacy and implementation-defined, and resource-not-found moved to `-32602` in this revision from the older `-32002`. Header names are compared case-insensitively; header values, including method names, are case-sensitive.

The 404 case deserves its own assertion because of what it is for. A client that speaks both eras uses the body to tell a modern server's `-32601` apart from a plain 404 from a framework that never routed the request. Answer with an HTML 404 and a dual-era client cannot distinguish your server from one that is simply down.

**The authorisation boundary, if you have one.** With no token, expect 401 carrying a `WWW-Authenticate: Bearer` challenge with a `resource_metadata` pointer. With a valid token missing a required scope, expect 403 with `error="insufficient_scope"`. With a token minted for a different audience, expect 401. Then fetch the metadata documents from a public network and require 200 with valid JSON: protected resource metadata at the sub-path form matching your endpoint's path, and at least one of the two authorisation server metadata documents. Only one of those two is needed; a 404 on the other is expected and normal.

**Two deployment-shape checks that only fire from outside.** First, follow no redirects and confirm the endpoint does not answer 3xx to a different host: on a cross-host redirect the `Authorization` header is dropped by standard client behaviour, the target sees an unauthenticated request, and the failure surfaces much later as an authorisation error. This is the precise reason a server can work in the Inspector and in a local client and fail in a hosted assistant, because local clients fail fast on the redirect while a hosted client follows it. Second, resolve the hostname from a machine outside your network and confirm every returned address is globally routable and that an IPv4 `A` record exists. Split-horizon DNS is invisible from inside.

## Decision rule: which layer owns this failure

- **Layer one fails.** The fault is in the handler, the schema, or the registration. Nothing about hosting is implicated. Fix it before running anything else, because layers two and three will report the same symptom with less information.
- **Layer one passes, layer two fails.** The fault is between your handler and the network: the framework adapter, the build output, the route, the bind address, the process. Read the stderr envelope's `code` first; exit 4 is a reachability problem and exit 3 is an authorisation problem, and they lead to completely different investigations.
- **Layers one and two pass, layer three fails.** The fault is in the edge: proxy, gateway, load balancer, WAF, or a framework default answering the removed verbs. Your application logs will look clean, because the request often never reaches it.
- **All three pass and a real client still cannot connect.** Turn on access logging for 2xx responses before anything else. Many application servers log only errors, so a fully successful flow that the client nonetheless rejects is completely invisible. Then re-run layer three from a network outside your own.
- **You cannot tell which layer owns it,** because the symptom is intermittent or the logs are silent. Do not guess. Take the cheapest disambiguating measurement, which is the same request issued twice: once through the in-process handler and once over the wire with `--data-binary`. If the two disagree, the fault is below the handler and above the socket, and that narrows it to the adapter and the edge. If they agree and the real client still fails, the fault is in what sits between the client and your edge, and no local test will ever reproduce it.

## Worked example, compressed

A warehouse inventory service. Four tools: `list_locations`, `find_stock`, `adjust_stock`, `transfer_stock`. Deployed behind a managed gateway. The suite is 34 tests and green, and the connector fails to appear in a hosted assistant.

**Layer one.** Written from scratch against the deployed handler factory. 32 of 34 existing tests survive unchanged. Two do not: both were `rejects.toThrow()` assertions on invalid quantities. Rewritten to read `isError`, they reveal that `adjust_stock` returns `isError: true` with the body `Error`. The tool has been failing for every negative quantity since it shipped and the suite recorded that as a pass. Era assertion added, reporting the current era, correct.

**Layer two.** First run: exit 4, envelope `code: "fetch failed"`. The command had been written with `--method` before the URL, so the target was dropped and the run fell back to an empty catalogue. Reordered, and `tools/list` returns four tools, exit 0. `find_stock` called with `--tool-arg sku=00742` returns the wrong record: the argument was coerced to the integer 742. Switching to `--tool-args-json` returns the right one. The production client sends a string, so the harness had been testing an input production never produces.

**Layer three.** GET the endpoint: 200, with the gateway's HTML index page. DELETE: 200, same page. Both should be 405. A request omitting `Mcp-Method`: 200, because the gateway strips unrecognised headers before forwarding, so the server never sees the mismatch it is required to reject. `curl -sI` on the endpoint: 308 to the `www.` host, which drops the `Authorization` header on the way through. Protected resource metadata: 200 at the origin root, 404 at the sub-path the challenge advertises.

**Verdict.** Nothing was wrong with the server. Three of the four faults were in the gateway, and the fourth, the redirect, was a canonicalisation rule nobody associated with the connector. The order of work follows the layers: register the `www.` host as the endpoint rather than the apex, since that is what silently breaks authorisation; stop the gateway stripping the protocol headers, since header validation is a security control and not a formality; route the removed verbs to a 405 from the application rather than to the gateway's index page; serve the metadata document at the path the challenge names. Then fix `adjust_stock` to return a message a model can act on. Do not add tests until layer three is green, because tests written over a lying edge encode the lie.

## Failure modes

**The test that cannot fail.** A negative case written as `rejects.toThrow()`. Symptom: the negative tests have never once gone red, including across a refactor that changed the error path.

**The green suite over a broken deployment.** Everything runs in process, so the proxy's behaviour is never exercised. Symptom: full coverage, a passing pipeline, and a connector that will not connect, with nothing in the application logs.

**The job that hangs.** No `--stored-auth-only`, a runner with a pseudo-terminal, and a browser opened on a headless machine. Symptom: a step that used to take forty seconds sits until the job's own timeout kills it, and the log ends with an authorisation URL.

**The silently dropped target.** A flag placed before the server target under `--cli`. Symptom: assertions pass against a server nobody meant to test, and the tool list looks subtly wrong to anyone who reads it carefully.

**The coerced argument.** An identifier with a leading zero, or a version string, passed with `--tool-arg`. Symptom: a lookup that works by hand and fails in the harness, or the reverse, with no error anywhere.

**The leaked handler.** Teardown closes the client but not the handler, or closes them in the wrong order. Symptom: a test that passes alone and fails in the suite, usually the one immediately after a long-running call.

**Era drift.** A dependency bump flips negotiation to the older family. Symptom: nothing. Every behavioural test still passes because both eras expose the same tools, and the first sign is a client that expects the current revision failing months later.

**The buffered stream.** An SSE response held by an intermediary until it completes. Symptom: works against localhost, hangs behind the edge, and the server's own logs show a completed 200.

## What this skill does not do

- It does not tell you whether your tool set is well designed. Conformance and usefulness are unrelated, and a perfectly conformant server can expose tools an assistant will never choose correctly.
- It does not cover load, latency or cost. A directory reviewer will notice a tool that returns forty thousand tokens; no assertion here will.
- It cannot see faults that only appear between a hosted client and your edge, such as an egress range blocked by a firewall rule you do not own. It can only tell you to look there.
- It does not replace running the server against a real client once, by hand, before you believe any of it.
- It says nothing about the content of your OAuth implementation beyond the boundary responses. Verifying that you validate the token audience correctly is a different job with different assertions.
