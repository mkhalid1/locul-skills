---
name: mcp-tool-schema
description: Designs the tool contract an MCP server advertises, on protocol revision 2026-07-28. Covers the full tool field set and display-name precedence, the conflicting specification and directory-policy limits on tool names, the annotation defaults that treat an unannotated tool as destructive and open-world, input and output schema rules including the 2020-12 dialect and $ref restrictions, the two error channels, opaque cursor pagination, the required ttlMs and cacheScope fields, and the x-mcp-header constraints whose only symptom is the tool vanishing from tools/list. This skill should be used when writing or reviewing MCP tool definitions, when a read-only tool triggers confirmation prompts, when a registered tool does not appear in the tool list, or when preparing a server for directory submission.
---

# MCP tool schema design

## The revision this describes

This file describes **MCP protocol revision 2026-07-28**, verified against the published specification on **20 August 2026**, alongside the **Anthropic Software Directory Policy effective 15 April 2026**. Where the two disagree, that is stated rather than resolved, because both are in force and they apply at different moments.

Anything marked **unverified** could not be confirmed against live documentation and is flagged rather than guessed.

## The claim: the defaults are against you

Most advice about MCP tools is about wording. Write clear descriptions, use verbs, keep tools focused. That advice is fine and it is not where servers actually go wrong.

Servers go wrong at the parts with a specification behind them, and specifically at the parts that fail without a symptom. A tool with no annotations is not neutral: it is announced as destructive and open-world, because those two hints default to true. A tool name of ninety characters is valid protocol and invalid policy, and nothing at runtime will ever tell you. A tool whose header annotation sits on a `number` rather than an `integer` is silently removed from the tool list by any client using Streamable HTTP, and your server logs a successful `tools/list`.

So work in this order: name, annotations, schema, description, result shape, list mechanics. The order matters because names and annotations are the two that are expensive to change later, names because they are referenced by everything downstream and annotations because a wrong one trains users to click through prompts.

## Step 1. Name it inside both constraint sets

**The specification.** Tool names **SHOULD** be between 1 and 128 characters inclusive, **SHOULD** be treated as case-sensitive, and the only allowed characters **SHOULD** be uppercase and lowercase ASCII letters, digits, underscore, hyphen and dot. Spaces, commas and other special characters **SHOULD NOT** appear. Names **SHOULD** be unique within a server. The specification's own valid examples are `getUser`, `DATA_EXPORT_v2` and `admin.tools.list`, which is worth noticing: lowercase `verb_noun` is a readability convention that many servers follow, not a rule the protocol imposes.

**The directory policy.** "MCP tool names must not exceed 64 characters."

Both are true. Target 64 if you might ever list publicly, because the cost of complying early is nothing and the cost of renaming later is every saved prompt, every piece of documentation and every transcript that mentions the old name. **Unverified:** whether the 64-character cap is measured before or after any namespacing a host applies. Leave headroom rather than sitting at 63.

**Uniqueness is scoped to one server, and that is narrower than it sounds.** A client or proxy aggregating several servers **MAY** hit collisions, two servers each exposing `search` being the specification's example, and **SHOULD** disambiguate by prefixing with a server identifier. You do not control that prefix. Worse, the server `name` from `serverInfo` is **not guaranteed unique** across servers and **SHOULD NOT** be relied on for disambiguation. Practical consequence: if your tool is called `search`, it is competing for selection against every other `search` the user has connected, and you cannot fix that from your side. Name for the domain, not for the verb.

## Step 2. Set every annotation explicitly

This is the single most useful fact in this file. The defaults, taken verbatim from the schema:

| Hint | Default | Notes |
| --- | --- | --- |
| `readOnlyHint` | `false` | True means the tool does not modify its environment |
| `destructiveHint` | **`true`** | Meaningful only when `readOnlyHint` is false |
| `idempotentHint` | `false` | Meaningful only when `readOnlyHint` is false |
| `openWorldHint` | **`true`** | True means the tool may interact with an open world of external entities |
| `title` | none | Human-readable title for the tool |

Read the two bold rows again. **A tool with no annotations is described to every client as a destructive, non-idempotent, open-world operation.** A pure search tool that omits them is indistinguishable, at the protocol level, from one that deletes records. Clients are told to keep a human in the loop and to present confirmation prompts for operations, so a read tool that draws a prompt on every call is not a client bug: it is your missing annotation block.

Set all five on every tool. It costs one object per tool and it is the highest-value five lines in the file.

**Two caveats that keep this honest.** First, annotations are **hints**. The schema says so directly: they are not guaranteed to be a faithful description of behaviour, including descriptive properties like `title`. Second, clients **MUST** treat them as untrusted unless the server is trusted. They never change how the SDK runs your tool. They change what the client tells the user, which is exactly why an accurate one matters and a flattering one is worthless.

**If you may list publicly**, the directory policy separately requires applicable annotations including `readOnlyHint`, `destructiveHint` and `title`. Note that `title` is on that list, and `title` is the one people leave out.

## Step 3. Titles, and the display-name precedence

Three fields can supply a display name and the order is fixed. For a tool, a client displays `title` if present; otherwise `annotations.title` if present; otherwise `name`. The `name` field is intended for programmatic use and is a fallback, not a label.

The practical rule: set `title` on every tool, in the words a non-specialist would use. Set `annotations.title` as well if you want a directory submission that satisfies the policy's annotation requirement without relying on the top-level field. Never let `name` be the thing a user reads.

## Step 4. inputSchema

`inputSchema` is required and it is real JSON Schema. It **MUST** be a valid JSON Schema object, never `null`, and its root **MUST** be `type: "object"`. It defaults to **JSON Schema 2020-12** when no `$schema` field is present, and implementations **MUST** support 2020-12 at minimum, **MAY** declare another dialect via `$schema`, and **MUST** handle a dialect they do not support by returning an error rather than guessing.

For a tool that takes no parameters there are two valid forms and they are not equivalent. `{"type": "object", "additionalProperties": false}` is the recommended one and accepts only an empty object. `{"type": "object"}` accepts any object at all, including one full of properties you never declared.

**What changed in this revision.** SEP-2106 loosened the schemas to allow any JSON Schema 2020-12 keyword, so `oneOf`, `anyOf`, `allOf`, `not`, `if`/`then`/`else`, `$ref`, `$defs` and `$anchor` are all available alongside `type`. Two guard rails came with it. Implementations **MUST NOT** automatically dereference a `$ref` that resolves to a network URI; any opt-in fetching mode **MUST** be disabled by default and **SHOULD** enforce a host allowlist, or at minimum reject loopback, link-local and private addresses, with timeouts, size limits and logging. A schema that fails validation because of an unresolved external `$ref` **SHOULD** be rejected rather than quietly treated as permissive. And because composition keywords are expensive to validate, implementations **SHOULD** bound them with a maximum depth, a cap on subschemas, or a per-validation time budget, since a malicious schema is otherwise a denial-of-service vector against the validator.

Design consequence: keep `$ref` local, to `$defs` in the same document. A remote `$ref` is not a portability feature here, it is a schema that will not resolve.

## Step 5. Descriptions, which are the only documentation the model gets

There is no other channel. The model sees the name, the title, the description, the schema and the annotations, and nothing else.

**There is no published limit on description length, on the number of tools a server may expose, or on the size of a response.** None. Anyone quoting you a number has invented it. Budget against the model's context window, and note that the directory policy's only relevant constraint is qualitative: token usage should be economical and tool responses proportionate to task complexity.

A description that earns its space answers four questions:

1. **What it does**, in one sentence, in domain words rather than implementation words.
2. **When to invoke it**, and specifically what distinguishes it from the neighbouring tool a model might pick instead.
3. **What comes back**, in shape terms: one record, a page of records, a link.
4. **The constraint that stops misuse.** The one sentence that prevents the wrong call. "Searches only issues in the current workspace" prevents more bad calls than three sentences of description.

Per-argument descriptions state **format**, not only meaning. "The issue identifier" is close to useless. "The issue identifier, in the form ABC-123, from the URL or the issue header" is a description a model can satisfy on the first attempt. In a Zod-based SDK, `.describe()` becomes the property description, so this is one call per field.

## Step 6. outputSchema and structuredContent

Declare `outputSchema` when the result is machine-readable. If you declare it, servers **MUST** return structured results conforming to it, and clients **SHOULD** validate against it. `structuredContent` may be any JSON value, not only an object, so an array result is legal.

For backwards compatibility, a tool returning structured content **SHOULD** also return the serialised JSON in a text content block. Do both. The cost is one `JSON.stringify` and the benefit is that a client that ignores structured content still gets something.

## Step 7. Content blocks

One result may mix types: `text`; `image`, carrying base64 `data` and a `mimeType`; `audio`, same shape; `resource_link`, carrying a `uri` with no bytes; and an embedded `resource` with its contents inline. All of them support the resource annotation block for audience, priority and last-modified time.

The one to reach for more often is `resource_link`. It hands the client a URI it can fetch on demand instead of pushing bytes through the model's context. Note that resource links returned by a tool are **not guaranteed** to appear in a `resources/list` response, so do not treat the link as a promise that the resource is discoverable elsewhere.

## Step 8. Two error channels, and they are not interchangeable

**Protocol errors** are JSON-RPC errors and cover problems with the request itself: an unknown tool, a malformed request that fails the `CallToolRequest` schema, and server errors. The specification's example for an unknown tool uses code **-32602**. Clients **MAY** pass these to the model, and they are noted as less likely to produce a useful recovery.

**Tool execution errors** are ordinary successful results carrying `isError: true` and content the model can act on: API failures, input validation failures such as a date in the wrong format or a value out of range, and business logic failures. Clients **SHOULD** feed these back to the model so it can self-correct.

The rule follows from that split: **if a model could plausibly fix the problem by calling again with different arguments, it is an execution error, not a protocol error.** "Invalid departure date: must be in the future. Current date is 08/08/2025" is a good execution error because it names the rule and the current value. "Bad request" is not.

One number worth knowing when reading older code: resource-not-found moved from `-32002` to `-32602` in this revision. Implementations of 2026-07-28 **MUST NOT** emit `-32002`, and clients **SHOULD** still accept it from servers running earlier revisions.

## Step 9. Paginate every list, with opaque cursors

Pagination is supported on `tools/list`, `prompts/list`, `resources/list` and `resources/templates/list`, and it is cursor-based rather than page-numbered.

The cursor is an opaque string. Clients **MUST** treat it as opaque: no parsing, no modification, and no inference from its value beyond whether a non-null value was provided. An empty string is a valid cursor and **MUST NOT** be read as the end of results. Page size is the server's choice and clients **MUST NOT** assume a fixed one. Servers **SHOULD** provide stable cursors and handle invalid ones gracefully; an invalid cursor **SHOULD** produce `-32602`.

The design consequence is for your own tools, not just for list methods: any tool that can return an unbounded set needs the same shape. A tool that returns four thousand rows because nobody put a limit on it does not fail. It consumes the context and the task dies two turns later for reasons that look like model behaviour.

## Step 10. ttlMs and cacheScope are required, and one of them can leak

Servers **MUST** include caching hints on results with `resultType: "complete"` returned by `server/discover`, `tools/list`, `prompts/list`, `resources/list`, `resources/templates/list` and `resources/read`.

`ttlMs` is an integer count of milliseconds and servers **MUST** provide a value greater than or equal to 0. Zero means treat as immediately stale. If absent, clients **SHOULD** assume 0, which means they re-fetch your tool list on every turn, which looks like erratic model behaviour and is actually a missing field.

`cacheScope` is `"public"` or `"private"`. Private results may be reused within the same authorisation context, and caches **MUST NOT** be shared across authorisation contexts, so a different access token requires a different cache. Public means the response contains no user-specific data.

**The trap is in the security note and it is worth reading twice.** A `"public"` result may be shared between callers **even when it came from an authenticated endpoint**. If your `tools/list` is filtered by the caller's scopes and you label it public, a cache may serve one user's filtered list to another. Servers **MUST** apply per-primitive access controls and **MUST NOT** rely on `cacheScope` alone to prevent unauthorised access. One more constraint if you paginate: the same `cacheScope` **MUST** apply to every page of a given list request.

## Step 11. Ordering, and how the tool set may vary

Servers **SHOULD** return tools in a deterministic order, meaning the same order across requests while the underlying set is unchanged. This is not cosmetic: it lets clients cache the list and it improves prompt cache hit rates when the tools are placed in model context.

The tool set **MAY** change over time but **MUST NOT** vary per-connection or as a side effect of other requests on the connection. It **MAY** vary by the authorisation presented on the request, since credentials are per-request input rather than connection state. That is the sanctioned way to show a writer more tools than a reader. Varying by anything else, such as what the caller did five minutes ago, is not.

## Step 12. x-mcp-header, where the failure is silence

`x-mcp-header` mirrors a tool argument into an HTTP header so that load balancers, proxies and firewalls can route on it without parsing the body. It is placed inside the JSON Schema of the property to be mirrored, and its value is the name portion of the resulting `Mcp-Param-{name}` header. Annotating a `region` property with `"x-mcp-header": "Region"` and calling the tool with `"region": "us-west1"` makes the client send `Mcp-Param-Region: us-west1`.

Every one of these is a **MUST**:

- **Not empty.**
- **Matches HTTP field-name token syntax**, `1*tchar` per RFC 9110 section 5.1.
- **No control characters**, including carriage return and line feed.
- **Case-insensitively unique** among all `x-mcp-header` values in the same `inputSchema`.
- **Primitive types only**: integer, string, boolean. **`number` is not permitted.** Integer values **MUST** sit within the IEEE 754 double-precision safe range, from minus 2^53 plus 1 to 2^53 minus 1.
- **Statically reachable from the schema root**, through `properties` keys only. Not through `items`, not through `oneOf`, `anyOf`, `allOf` or `not`, not through `if`/`then`/`else`, and not through `$ref`.

And the consequence: a client using Streamable HTTP **MUST** reject a tool definition that violates any of these, and rejection means it **MUST exclude that tool from the result of `tools/list`**. It **SHOULD** log a warning naming the tool and the reason, on the client side, where you are not looking. Clients on other transports, stdio for example, **MAY** ignore `x-mcp-header` entirely, which is why the same server can work locally and lose a tool remotely.

Finally, do not annotate sensitive parameters. Servers **SHOULD NOT** mark passwords, API keys, tokens or personal data with `x-mcp-header`, because the value becomes a header visible to every intermediary on the path.

## Step 13. Stateful tools

MCP has no protocol-level session in this revision, so anything spanning calls uses an explicit handle: a creation tool returns one, later tools take it as an ordinary argument, and the model carries it forward. This part of the specification is explicitly non-normative design guidance, and three points of it belong in your schema rather than in your head.

Make handles **opaque**, because a handle that encodes structure invites guessing. State the **retention policy in the creation tool's description**, for example that a basket expires after 24 hours of inactivity, so the model can see it at the moment it decides to create state. And when a call arrives with an expired or unknown handle, return a **tool execution error that says so**, so the model can recover by creating a new one instead of retrying the same dead identifier.

On an authenticated server a handle is a name, not a capability: validate the caller's authorisation against it on every call. On an unauthenticated server the handle is unavoidably a bearer token, so generate it with real entropy and give it a bounded lifetime.

## Step 14. The security requirements, which are MUSTs

Servers **MUST** validate all tool inputs, implement proper access controls, rate limit tool invocations, and sanitise tool outputs. That last one is easy to skip: content you pass back reaches a model, and unsanitised third-party text in a tool result is an injection surface.

## Decision rule: what to set the annotations to

- **The tool only reads.** `readOnlyHint: true`. The destructive and idempotent hints become meaningless once that is true, and setting them anyway costs nothing and reads clearly.
- **The tool writes, and calling it twice with the same arguments changes nothing further.** `readOnlyHint: false`, `idempotentHint: true`, and `destructiveHint: false` if the effect is purely additive.
- **The tool deletes, overwrites or sends something irreversible.** `destructiveHint: true`. This is the default anyway, so state it deliberately rather than inheriting it.
- **The tool reaches an unbounded set of external entities**, a web search being the specification's example. `openWorldHint: true`. A tool confined to one known corpus is `false`.
- **You cannot tell, because the tool wraps an endpoint whose behaviour depends on configuration you do not control.** Leave the conservative defaults in place, which means destructive and open-world, say why in the description, and do not claim `readOnlyHint: true` to remove a confirmation prompt. The prompt is the point. The annotation is untrusted advice from you to a client that has no way to check it, and the only thing a false one buys is a user who has learned to click through.

## Worked example, compressed

A project management server exposes four tools and a support ticket says the assistant "keeps asking permission to search".

**As shipped.** `search_issues_across_all_connected_project_workspaces_by_full_text_query` with no annotations, no title, a description reading "Searches issues", an input schema of `{"type": "object"}` and a result of every matching issue. `create_issue`, no annotations. `delete_issue`, no annotations. `export_workspace`, taking a `region` property typed `number` and annotated `"x-mcp-header": "Region"`. The list result carries neither `ttlMs` nor `cacheScope`.

**What is actually wrong.**

The search tool draws a confirmation prompt on every call because it has no annotations, so it is advertised as destructive and open-world. Nothing is misconfigured on the client. Its name is 68 characters, which is inside the specification's 128 and outside the directory's 64, so it passes every test today and fails review later. Its schema accepts any object at all, so a malformed call reaches the handler instead of being rejected by the client. Its description does not say the search is scoped to the current workspace, which is both the constraint that prevents misuse and the sentence that distinguishes it from a search tool on another connector. And it returns every match, unbounded, with no cursor.

`export_workspace` is not in `tools/list` at all, on remote clients only. Two independent violations: `number` is not a permitted type for `x-mcp-header`, and the property sits under `items` rather than being reachable from the root through `properties`. The client dropped the tool and logged a warning on its own side. The server logged a successful `tools/list`. Locally over stdio the tool works, because stdio clients may ignore the annotation.

The missing `ttlMs` means clients assume 0 and re-fetch the tool list every turn, which had been filed as a performance complaint against the model.

**Verdict: three of the four defects have no error message anywhere, and the one visible symptom is the least serious.** Fix in this order. Rename to `search_issues`, under 64 characters, accepting that this breaks saved prompts and doing it before more users arrive. Add all five annotations to all four tools, `readOnlyHint: true` on search, `destructiveHint: true` stated explicitly on delete. Move the `region` property to the schema root, retype it `string`, and keep the header annotation. Tighten the search schema to declare its properties with `additionalProperties: false`, add a cursor and a page size, and rewrite the description to name the workspace scope. Add `ttlMs` and `cacheScope`, and make it `"private"`, because this list is filtered by the caller's permissions.

## Failure modes

**The unannotated read tool.** Every call produces a confirmation prompt. The team looks for a client setting, finds none, and concludes the client is over-cautious. The cause is a missing five-line object and the default value of `destructiveHint`.

**The 128-character name that fails review.** Spec-valid, policy-invalid, and there is no runtime symptom at all. You discover it at submission, which is the most expensive moment to rename a tool, because by then external users have saved prompts referencing it.

**The tool that silently disappears.** An illegal `x-mcp-header`, usually a `number` type or a property nested under `items`, and the client removes the tool from `tools/list` while logging the reason on its own side. The server reports a successful list request containing everything. It works over stdio and not over HTTP, which sends people looking at the transport.

**The unbounded list.** Four thousand rows come back, the context fills, and the task fails two turns later in a way that reads as the model losing the thread. Nothing errors and the tool call was a success.

**The description that steals calls.** A vague description wins tool selection against a better-suited tool on a different connector, because the model had nothing to discriminate on. The symptom is a model that "picks the wrong tool", and the fix is one sentence naming the scope, not a longer system prompt.

**The missing `ttlMs`.** Clients assume 0, treat every result as immediately stale, and re-fetch the tool list constantly. It gets filed as latency or as model behaviour, and it is a required field nobody added.

**The public cache scope on a private list.** The tool list is filtered by the caller's scopes but labelled `"public"`, so a cache is permitted to serve one user's filtered list to another. Nothing fails. There is no error and no log line, and the specification's own security note is the only place this is written down.

## What this skill does not do

- It does not give you a maximum description length, a maximum tool count or a maximum response size, because none is published. Budget against the model's context.
- It cannot resolve the naming conflict for you. The specification says 1 to 128, the directory policy says 64, both are in force, and whether the cap is applied before or after host namespacing is **unverified**.
- It does not tell you whether your tool set is well factored for an assistant to use. That is a design judgement with no mechanical ground truth, and it is the half of this subject that is not gradeable.
- It does not touch the handler. A perfect contract in front of a slow, wrong or unsafe implementation still fails on the first call.
- It cannot see how any particular client renders your titles, honours your annotations, or namespaces your tools next to another server's. Connect the Inspector and look.
- It is dated. Revision 2026-07-28, verified 20 August 2026, against a directory policy effective 15 April 2026. Check both before relying on any number here.
