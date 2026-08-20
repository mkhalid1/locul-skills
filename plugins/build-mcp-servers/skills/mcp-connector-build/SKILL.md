---
name: mcp-connector-build
description: Builds a Model Context Protocol server end to end on protocol revision 2026-07-28, from pinning the revision and installing the correct package line through the per-request handler factory, the host and origin guards, the authentication seam, one smoke test, and adding the result to Claude as a custom connector. Carries the verified package versions, the stateless request model that replaced the initialise handshake, and the plan and UI paths for connecting it. This skill should be used when starting a new MCP server, when following a tutorial that constructs a transport object and calls connect, or when an existing server needs to be brought forward from a 2025-era revision.
---

# MCP connector build

**Verified against live documentation on 20 August 2026, for protocol revision 2026-07-28.** Sources checked: the specification pages for that revision, `schema.ts` in the specification repository, the TypeScript SDK repository on `main`, the npm registry, and the published Claude support and platform documentation. Anything that could not be verified is labelled unverified in the section near the end, and is not asserted anywhere else in this file.

## The claim, and why the obvious approach fails

Building an MCP server that returns a tool list takes an afternoon. Building one that a current client can actually talk to is a different problem, and the difference is entirely down to timing.

Protocol revision 2026-07-28 made four removals at once. It removed protocol-level sessions and the `Mcp-Session-Id` header. It removed the standalone HTTP GET stream. It removed SSE resumability, meaning `Last-Event-ID` and event ids. And it removed the `initialize` handshake altogether, replacing it with a per-request `_meta` envelope carrying `io.modelcontextprotocol/protocolVersion` and `io.modelcontextprotocol/clientCapabilities` on every single request. MCP is now a stateless protocol: a server must not infer anything from a previous request on the same connection.

Almost every tutorial, blog post and code sample written before mid-2026 describes the removed model, and describes it accurately for its time. That is the trap. The output is not obviously wrong. It compiles, it runs, it answers a `curl`, and it fails only when a current client makes its first real request.

So the first act of building is not writing code. It is pinning the revision, and then choosing the package line that implements it.

## Stage 1: pin the revision before anything else

`LATEST_PROTOCOL_VERSION` in the specification schema is `2026-07-28`. The prior revisions, most recent first, are `2025-11-25`, `2025-06-18`, `2025-03-26` and `2024-11-05`.

The SDK names two eras rather than five versions, and the distinction is the one that matters operationally. **Modern** means `2026-07-28` and later: per-request metadata, no handshake. **Legacy** means `2024-10-07` through `2025-11-25`: an `initialize` handshake that establishes a session. **Dual-era** means an implementation that serves both.

Write down which of the three you are building before you install anything. The default answer for a server that other people will connect is dual-era, because you do not control their clients. The stage 6 handoff covers how that is configured and what it costs.

## Stage 2: install the v2 line, not the v1 line

There are two live package lines and their names do not make the split obvious.

**The v2 line, which implements 2026-07-28.** Verified on the npm registry on 20 August 2026:

- `@modelcontextprotocol/server` 2.0.0, engines `node >=20`, depends on `@modelcontextprotocol/core` 2.0.0 and `zod ^4.2.0`
- `@modelcontextprotocol/core` 2.0.0, engines `node >=20`
- `@modelcontextprotocol/client` 2.0.0, for the test client you will need at stage 7

Then exactly one framework adapter, all published at 2.0.0 with `node >=20`, each declaring `@modelcontextprotocol/server ^2.0.0` as a peer:

- `@modelcontextprotocol/node`, for plain `node:http`
- `@modelcontextprotocol/express`
- `@modelcontextprotocol/fastify`
- `@modelcontextprotocol/hono`

**The v1 line.** `@modelcontextprotocol/sdk` 1.30.0, engines `node >=18`. It is the frozen v1 generation. Install it only to maintain something that already exists on it. A frozen copy of the old SSE transport also ships separately as `@modelcontextprotocol/server-legacy` 2.0.0, marked deprecated by its own package description.

The single clearest signal that you are following a stale guide: it tells you to install `@modelcontextprotocol/sdk` for a new build.

## Stage 3: write the server as a per-request factory

In v1 you built one server instance, built a transport, and connected them. In v2 you hand `createMcpHandler` a **factory**, and it returns `{ fetch, close, notify, bus }`.

```ts
import { createMcpHandler, McpServer } from '@modelcontextprotocol/server';
import * as z from 'zod/v4';

const handler = createMcpHandler(({ authInfo, era, requestInfo }) => {
    const server = new McpServer({ name: 'docs-search', version: '1.0.0' });
    server.registerTool(
        'search_docs',
        { description: 'Search the documentation index', inputSchema: z.object({ query: z.string() }) },
        async ({ query }) => ({ content: [{ type: 'text', text: await runSearch(query) }] })
    );
    return server;
});
```

**The factory runs once per HTTP request.** That single sentence is the whole design, and it has two consequences worth stating plainly.

The first: register every tool, resource and prompt **inside** the factory. A tool registered on a shared instance outside it survives across requests, and on a server that varies its behaviour by caller, that is one user's data appearing in another user's results. It will not reproduce under manual testing, because manual testing is serial. It appears under concurrency.

The second: keep the factory cheap and free of side effects. Connection pools, caches and clients go at module scope and are closed over. Building a database pool per request is a resource leak with a slow fuse.

`handler.fetch` is a web-standard `(Request) => Promise<Response>`. Nothing is listening yet.

## Stage 4: mount it, and put the guards in front

On a web-standard runtime, `export default handler` is the entire mount. Under a Node framework, wrap once with `toNodeHandler(handler)` from `@modelcontextprotocol/node`.

The handler validates no `Host` header, no `Origin` header, and no token. It trusts its caller completely, by design, and the checks belong in front of it. The framework app factories, `createMcpExpressApp`, `createMcpHonoApp` and `createMcpFastifyApp`, arm both header checks by default on a localhost bind. On bare `node:http` you compose them yourself:

```ts
const nodeHandler = toNodeHandler(handler);
const validateHost = localhostHostValidation();
const validateOrigin = localhostOriginValidation();
createServer((req, res) => {
    if (!validateHost(req, res) || !validateOrigin(req, res)) return;
    void nodeHandler(req, res);
}).listen(3000, '127.0.0.1');
```

The `Host` check is what stops DNS rebinding, where a malicious page resolves its own domain to `127.0.0.1` so the browser treats your local server as same-origin. Nothing fails without it. That is exactly why it gets skipped.

## Stage 5: tool design, handed to `mcp-tool-schema`

Names, descriptions, schemas, output schemas, cursors and cache fields belong to that file. One fact belongs here, because it is invisible and it bites at this stage rather than that one.

**The annotation defaults are hostile.** In the specification schema, `readOnlyHint` defaults to `false`, `destructiveHint` defaults to **`true`**, `idempotentHint` defaults to `false`, and `openWorldHint` defaults to **`true`**. `destructiveHint` and `idempotentHint` are meaningful only when `readOnlyHint` is false. A tool that ships with no annotations is therefore treated as a destructive, non-idempotent, open-world tool. A pure search tool that omits them earns a confirmation prompt on every call, and the symptom presents as the client being annoying rather than as your tool definition being incomplete.

Annotations are also advisory and untrusted: clients must treat them as untrusted unless the server is trusted, and they never change how the SDK runs the tool.

## Stage 6: authorisation, handed to `mcp-oauth-setup`

That file owns the metadata documents, the challenge headers, the audience validation rules and the client registration priority. All of it describes revision 2026-07-28, which added RFC 9207 `iss` validation and deprecated Dynamic Client Registration in favour of Client ID Metadata Documents.

The seam is what belongs here, because it is the join between two files and neither owns it alone:

**`authInfo` is pass-through.** The handler never reads a token from a header and never verifies one. You verify the bearer token in front of the handler and hand the result in as the second argument: `handler.fetch(request, { authInfo })`. The factory reads it back as `authInfo`, and tool handlers read it as `ctx.http.authInfo`. On stdio that value is undefined, so guard it rather than dereferencing it.

## Stage 7: hosting, scaling and era routing, handed to `remote-mcp-deploy`

The endpoint's wire behaviour, its status codes, its streaming rules, and the one piece of state that still crosses nodes on a stateless deployment all belong there. Read it before you deploy, not after, because most of what it covers is invisible on localhost and appears the moment a proxy sits in front.

## Stage 8: one smoke command, then hand to `mcp-server-test`

Before building a suite, prove the endpoint answers. This command is identical on macOS and on Windows:

```
npx @modelcontextprotocol/inspector --cli --transport http --server-url https://mcp.example.com/mcp --method tools/list
```

The Inspector at 2.3.0 requires Node 22.19.0 or later, which is stricter than the SDK's Node 20 floor, so a machine that builds the server may not run the Inspector. Exit code 0 means success. On any non-zero exit it writes a single JSON line to stderr that you can parse rather than scrape.

Everything past that first green run belongs to `mcp-server-test`: in-process tests against `handler.fetch`, the exit-code branches, and the raw HTTP assertions the SDK cannot make on your behalf.

## Stage 9: add it to Claude as a custom connector

Verified on 20 August 2026. Custom connectors using remote MCP are available on Free, Pro, Max, Team and Enterprise plans. **Free is capped at one custom connector.**

**As an individual.** Customize, then Connectors, then the plus button, then "Add custom connector". Paste the remote MCP server URL. Optionally open Advanced settings to supply an OAuth Client ID and Client Secret. Then "Add".

**On Team or Enterprise.** An organisation Owner must add it first, at Organization settings, then Connectors, then Add, then hover Custom and choose Web, then paste the URL and optionally configure OAuth under Advanced settings. Members connect afterwards. A member cannot add it from their own Customize panel, and the failure looks like a missing feature rather than a permission.

Shipping as a custom connector requires no submission and no review. Public listing does, and it is handed to `connector-directory-submit`.

## Sidebar: two different products called a connector

Do not conflate these. They are separate surfaces with separate constraints.

**claude.ai custom connectors** are what stages 8 and 9 describe: a remote MCP server a user or an organisation adds to Claude.

**The Claude API MCP connector** lets the Messages API call a remote MCP server with no MCP client of your own. It is beta, gated behind the header `mcp-client-2025-11-20`. The earlier `mcp-client-2025-04-04` is deprecated. Of the MCP feature set **only tool calls are supported**: no resources, no prompts. The server must be publicly reachable over HTTP, both Streamable HTTP and SSE are accepted, and local stdio servers cannot be connected directly. It is **not eligible for zero data retention**.

A team that builds for one and tests against the other will spend a day on a resource handler that the other surface was never going to call.

## Decision rule: modern-only, dual-era, or you cannot tell

1. **You control every client** and can confirm all of them speak `2026-07-28`. Build modern-only. The endpoint answers a legacy `initialize` with 400 and `-32022`, naming the versions it supports, which is an actionable error rather than a silent failure.
2. **You have an existing sessionful deployment with live 2025 clients.** Keep the legacy leg. Route in front of a strict handler with the SDK's own classification predicate so the branch cannot disagree with the handler about which era a request is.
3. **You are publishing an endpoint for clients you do not control.** Serve both eras. Take the default posture, where legacy requests are served statelessly from the same factory.
4. **You cannot tell**, because the host has not documented which revision its client speaks. This is the common case and it includes Claude, which is labelled unverified below. Take branch 3 and stay there. Serving both costs one option value and no code, and the cost of guessing wrong in the other direction is an endpoint that a whole class of client cannot connect to at all, with no error message that names the reason. Do not resolve the ambiguity by testing once and hard-coding the result: era support is a property the host can change without telling you, and a passing connection today is not evidence about next quarter.

## Worked example, compressed

A documentation search service wants an MCP server exposing two tools: `search_docs` and `fetch_page`.

**Stage 1.** Revision pinned to `2026-07-28`. Clients unknown, so dual-era by default. Recorded in the repository README with the date.

**Stage 2.** `@modelcontextprotocol/server` 2.0.0, `@modelcontextprotocol/core` 2.0.0, `zod ^4.2.0`, and `@modelcontextprotocol/express` 2.0.0. Node pinned at 22 rather than 20, so the same machine can run the Inspector at stage 8. The first draft had reached for `@modelcontextprotocol/sdk`, caught here rather than three days later.

**Stage 3.** A search-index client is built once at module scope. Both tools are registered inside the factory. A first attempt registered them outside, on a shared `McpServer`, which passed every local test because local tests are serial.

**Stage 4.** Mounted under Express through `toNodeHandler`, with the app factory arming the `Host` and `Origin` checks.

**Stage 5.** `search_docs` gets `readOnlyHint: true` and `openWorldHint: false`, because the index is closed. Both were previously omitted, and the symptom was a confirmation prompt on every search. The rest of the schema work is handed off.

**Stage 6.** A bearer verifier runs in front of the handler and passes `authInfo` through `fetch`. `fetch_page` reads `ctx.http.authInfo` and guards for undefined, so the same build still works over stdio.

**Stage 7.** Handed off before the first deploy.

**Stage 8.** The Inspector CLI returns exit 0 and both tool names on the deployed URL.

**Stage 9.** Added as a custom connector by an organisation Owner, since the account is on Team. The first attempt was made by a member from the Customize panel and the option was not there.

**Verdict: reachable on the current revision, with two build errors caught at the stage that owns them rather than after deployment.** The shared-instance registration would have leaked between concurrent users, and the missing annotations would have been read as a client defect. Neither was visible from a green local test run.

## What is unverified in this file

These are stated as unknown rather than guessed. Confirm them yourself before depending on any of them.

- **Which protocol revision Claude's own connector client speaks.** Not documented anywhere checked on 20 August 2026. The safe instruction is the SDK's default dual-era posture, which serves both from one factory.
- **The claude.ai OAuth redirect URI.** Older material names a fixed callback URL. It does not appear in the live documentation, and the connector UI now takes a Client ID and Client Secret you supply, which suggests the client provides its own. Do not hard-code one.
- **Whether a developer without a Team or Enterprise organisation has any listing route at all.** Submission runs through a portal inside organisation settings, verified live on 20 August 2026, and it is gated to Team and Enterprise. The old public form is retired. `connector-directory-submit` carries the portal path, the eleven-step flow, the field limits and the escalation address, and labels the no-organisation case as unverified.
- **Any numeric ceiling on tool description length, tool count, or response size.** None is published. Do not invent one. Budget against the model's context window instead.
- **Whether Anthropic's 64-character tool-name cap is measured before or after any namespacing a host applies.** Unknown. The specification separately says names should be 1 to 128 characters, which conflicts with the policy cap; target 64 if you may ever list.

## Failure modes

**The wrong-SDK build.** A 2025 tutorial, `StreamableHTTPServerTransport`, an explicit `connect` call. From the outside: it builds, it starts, `curl` gets a response, and a current client's first POST fails. Nothing in the server logs identifies the era as the cause.

**The shared-instance leak.** Tools registered outside the factory. From the outside: correct in every manual test, and under concurrency one caller's results carry another caller's data. The bug is invisible until the traffic is real, which is also when it is most expensive.

**The naked handler.** Mounted with no `Host` or `Origin` guard. From the outside: nothing at all, indefinitely, until a page in someone's browser resolves its domain to loopback and reaches the server as same-origin.

**The unannotated read tool.** From the outside: a confirmation prompt on every call to a tool that only reads. Reported as the client being difficult. Caused by four defaults that lean the wrong way.

**The half-migrated endpoint.** GET on the MCP path returns the framework's HTML 404 rather than 405. From the outside: a dual-era client cannot tell a modern server from a broken one, so it falls back or fails on the wrong branch.

**The "it connected once" trap.** `tools/list` succeeds, every `tools/call` returns an error before your handler runs, and your logs show nothing because your code was never reached. Almost always a required mirrored header missing or rewritten in transit.

**The plan-gated demo.** A Team user is told to add the connector from the Customize panel and the option is not there, because an Owner must add it at organisation level first. Reads as a broken URL. Is a permission.

**The wrong-product build.** A resource handler and a prompt library built against the API MCP connector, which supports tool calls only. Everything works locally and half of it is never called.

## What this skill does not do

- It does not carry the transport contract, the authorisation flow, the tool schema rules, the test harness or the directory policy. It names the stage and hands off, and the five files it hands to are where the detail lives.
- It does not cover stdio servers beyond noting where the HTTP path diverges. A locally launched subprocess server has a different transport, no authorisation specification to follow, and different debugging.
- It cannot verify anything about your deployment. Every assertion here is about the protocol and the packages. Whether your proxy passes the required headers through is a different question with a different file.
- It is TypeScript on Node only. The protocol is the same in every language; none of the identifiers are.
- It has no view on whether your server should exist, whether the tools are well factored, or whether an assistant will pick them correctly. Those are design questions, and the two of them that can be helped at all are helped by `mcp-tool-schema`.
- It will be wrong on a date it cannot predict. It names its revision and its verification date so the staleness is visible instead of silent.
