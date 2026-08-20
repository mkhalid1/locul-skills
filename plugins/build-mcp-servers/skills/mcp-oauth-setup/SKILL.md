---
name: mcp-oauth-setup
description: Sets up authorisation for a remote MCP server acting as an OAuth 2.1 resource server on protocol revision 2026-07-28. Covers the RFC 9728 protected resource metadata document and its two well-known paths, the canonical resource URI and why it is compared byte for byte, the WWW-Authenticate challenge, audience validation and the prohibition on token passthrough, Client ID Metadata Documents replacing deprecated Dynamic Client Registration, RFC 9207 issuer validation, per-client consent for proxy servers, and the SDK wiring including the expiresAt trap. This skill should be used when adding authentication to an MCP server, when an OAuth flow completes but no MCP calls follow, or when migrating an authorisation flow written against an earlier MCP revision.
---

# MCP server OAuth setup

## The revision this describes

This file describes **MCP protocol revision 2026-07-28**, verified against the published specification on **20 August 2026**. Read that sentence before you read anything else, because on this subject a page without a revision line is worthless.

MCP's authorisation approach has been rewritten four times. 2025-03-26 introduced OAuth. 2025-06-18 made RFC 9728 protected resource metadata and RFC 8707 resource indicators mandatory. 2025-11-25 refined it. 2026-07-28 added RFC 9207 issuer validation and deprecated Dynamic Client Registration in favour of Client ID Metadata Documents. Protocol versions are dated `YYYY-MM-DD` and the date marks the last backwards-incompatible change, so a tutorial from 2025 is not merely old, it is describing a different contract.

Anything below marked **unverified** could not be confirmed against live documentation and is flagged rather than guessed.

## Your role, stated precisely

A protected MCP server is an **OAuth 2.1 resource server**. It accepts and validates access tokens. It does not issue them, it does not run an authorisation endpoint, and it does not store passwords. The authorisation server is a separate role, and it may be a product you buy.

Authorisation is **OPTIONAL** for MCP as a whole. HTTP transports **SHOULD** conform to this specification. Stdio implementations **SHOULD NOT** follow it and should read credentials from the environment instead. If you are shipping a stdio server, stop here.

The normative base is OAuth 2.1 (draft-ietf-oauth-v2-1-13), RFC 6750 bearer token usage, RFC 8414 authorisation server metadata, RFC 8707 resource indicators, RFC 9728 protected resource metadata, RFC 9207 issuer identification, the Client ID Metadata Document draft (draft-ietf-oauth-client-id-metadata-document-00), OpenID Connect Discovery 1.0 and OpenID Connect Dynamic Client Registration 1.0. RFC 7591 Dynamic Client Registration is still listed, and is now deprecated as a mechanism.

## Step 0. Turn on HTTP access logging first

Do this before you write a line of authorisation code, because the most expensive failure in this subject produces no error anywhere.

Many application servers log 4xx and 5xx by default and do not log 2xx. When a client completes the entire OAuth flow successfully and then refuses to send a single MCP request, every request involved was a 2xx, so the whole episode is invisible and you spend a day debugging a server that never received the request you are looking for. Log method, path and status for every request, including both well-known paths, and keep the log where you can read it during a connection attempt.

## Step 1. Fix the canonical resource URI, in writing

Choose one string now and write it down, because it appears in at least four places and they are compared exactly.

The `resource` value is the canonical URI of your MCP server, as defined by RFC 8707 section 2. Clients **MUST** include it in **both** the authorisation request and the token request, and **MUST** send it whether or not the authorisation server appears to support it.

Valid canonical URIs, from the specification: `https://mcp.example.com/mcp`, `https://mcp.example.com`, `https://mcp.example.com:8443`, and `https://mcp.example.com/server/mcp` where a path is needed to identify one server among several. Invalid: `mcp.example.com`, which has no scheme, and `https://mcp.example.com#fragment`, which has a fragment.

Two details decide whether this works. Implementations **SHOULD** use the form **without** a trailing slash unless the slash is semantically meaningful, and both forms are technically valid URIs, which is exactly why a framework that redirects `/mcp` to `/mcp/` will quietly give you two identities. And while the canonical form uses a lowercase scheme and host, implementations **SHOULD** accept uppercase for robustness. Pick the lowercase, no-slash form, and use it verbatim in the metadata document, in the challenge, in the audience check and in your documentation.

## Step 2. Publish protected resource metadata

MCP servers **MUST** implement RFC 9728. The document you return **MUST** include `authorization_servers` with at least one entry. That is the field the whole discovery chain hangs from.

You **MUST** implement at least one of two ways for a client to find it, and clients must support both:

1. **The `WWW-Authenticate` header** on a 401, carrying `resource_metadata`. This is the path clients prefer when it is present.
2. **A well-known URI.** There are two forms, and the sub-path form is the one people get wrong. For an endpoint at `https://example.com/public/mcp`, the document may be served at `https://example.com/.well-known/oauth-protected-resource/public/mcp`, with the endpoint path inserted after the well-known suffix. It may also be served at the root form, `https://example.com/.well-known/oauth-protected-resource`. Clients that fall back to probing try the sub-path form first, then the root.

Serve both if you can. The sub-path form looks like a typo and is not.

`scopes_supported` is intended to be the **minimal** set needed for basic functionality, not a catalogue of everything you can do. Publishing every scope you own is how clients end up requesting maximum privilege on the first prompt, and how users learn to decline the dialogue.

## Step 3. The 401 challenge

Return a challenge that tells the client where to go and what to ask for:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer resource_metadata="https://mcp.example.com/.well-known/oauth-protected-resource",
                         scope="files:read"
```

Including `scope` is a **SHOULD** and it is the single cheapest thing on this page. It is what lets a client request least privilege on the first attempt rather than everything you advertise. Clients **MUST** treat the challenge scopes as authoritative for the current operation, and they may be a subset, a superset, or neither, relative to `scopes_supported`.

Do not put `offline_access` in your challenge or in `scopes_supported`. Refresh tokens are a client concern, not a resource requirement, and the specification says resource servers **SHOULD NOT** advertise it.

## Step 4. Validate the audience, which is the MUST that matters

MCP servers **MUST** validate that access tokens were issued specifically for them, per RFC 8707 section 2. They **MUST** only accept tokens valid for their own resources, and **MUST NOT** accept or transit any other tokens. Clients, correspondingly, **MUST NOT** send your server tokens issued by anyone other than your authorisation server.

**Token passthrough is explicitly forbidden.** If your server calls an upstream API, it acts as an OAuth client to that API and uses a **separate** token issued by that API's authorisation server. It **MUST NOT** forward the token the MCP client gave it. The specification names four reasons, which are the arguments you will have to make to whoever suggests the shortcut: security controls such as rate limiting and request validation are keyed to the audience and get bypassed; the audit trail breaks in both directions, because you cannot tell which client called you and the downstream logs show the wrong identity; the downstream service's trust assumptions about the caller become false; and a server that starts as a pure proxy needs its own controls eventually, which is far harder to retrofit.

Invalid or expired tokens **MUST** get a 401.

## Step 5. The status codes, and the one-challenge rule

| Status | Use |
| --- | --- |
| 401 | Authorisation required, or the token is invalid |
| 403 | Invalid scopes or insufficient permissions |
| 400 | Malformed authorisation request |

For a runtime insufficient-scope failure the server **SHOULD** answer 403 with a challenge carrying `error="insufficient_scope"`, `scope="..."` naming the scopes needed, `resource_metadata` for consistency with the 401, and optionally `error_description`.

**Emit every scope the operation needs in one challenge.** Returning one missing scope, then another on the retry, forces a separate authorisation round trip per scope and each one is a browser window in front of a human. Servers must also account for scope hierarchies, so a token carrying a broader scope that implies a narrower one is sufficient.

## Step 6. Client registration, and the deprecation

Clients **MUST** obtain a client ID through one of three mechanisms. The priority order is fixed:

1. **Pre-registration**, if the client already has credentials for this authorisation server.
2. **Client ID Metadata Documents**, if the authorisation server advertises `client_id_metadata_document_supported` in its metadata.
3. **Dynamic Client Registration**, if the server advertises a `registration_endpoint`. This is **deprecated** as of 2026-07-28 and retained only for authorisation servers that do not support CIMD.
4. **Prompt the user** for client details if nothing else is available.

**Client ID Metadata Documents, in the detail you need.** The `client_id` is itself an HTTPS URL, and it **MUST** use the `https` scheme and **MUST** contain a path component, for example `https://example.com/client.json`. The document at that URL **MUST** include at least `client_id`, `client_name` and `redirect_uris`. The `client_id` value inside the document **MUST** match the document URL **exactly**. On the authorisation server side, it **MUST** validate that match, **MUST** validate presented redirect URIs against the document, and **MUST** validate that the document is valid JSON containing the required fields; it **SHOULD** fetch documents when it sees a URL-formatted `client_id` and **SHOULD** cache them respecting HTTP cache headers.

The operational consequence is worth stating plainly: **CIMD client IDs are portable across authorisation servers**, because they are self-hosted URLs the server resolves on demand. Pre-registered and dynamically registered credentials are not. Clients **MUST** key persisted credentials by the authorisation server's `issuer`, **MUST NOT** reuse them with a different authorisation server, and **MUST** re-register when the authorisation server changes.

**If you are still using DCR, set `application_type`.** Clients **MUST** specify it. Omitting it defaults to `"web"` under OpenID Connect, which conflicts with localhost redirect URIs and gets your registration rejected for a reason the error message will not make obvious. Desktop applications, mobile applications, CLI tools and locally hosted applications reached over `localhost` **SHOULD** send `"native"`. Non-OIDC servers ignore the parameter safely.

## Step 7. Authorisation server discovery, and the issuer identity check

Clients **MUST** try the discovery endpoints in a fixed order. For an issuer **with** a path component, such as `https://auth.example.com/tenant1`:

1. `https://auth.example.com/.well-known/oauth-authorization-server/tenant1`
2. `https://auth.example.com/.well-known/openid-configuration/tenant1`
3. `https://auth.example.com/tenant1/.well-known/openid-configuration`

For an issuer **without** a path:

1. `https://auth.example.com/.well-known/oauth-authorization-server`
2. `https://auth.example.com/.well-known/openid-configuration`

Then the check that makes the rest safe: the `issuer` value inside the returned document **MUST** be identical to the issuer identifier used to build the URL. If it differs, the client **MUST NOT** use the metadata. The specification's own example is a document fetched from `https://attacker.example/.well-known/oauth-authorization-server` that claims `"issuer": "https://honest.example"`, and it must be rejected.

## Step 8. RFC 9207 issuer validation, new in this revision

Before redirecting the user agent, the client **MUST** record the `issuer` from the validated authorisation server metadata, in the same per-request record that holds the PKCE code verifier and the `state`. Authorisation servers **SHOULD** include `iss` in authorisation responses, **including error responses**, and any server that does **MUST** advertise `authorization_response_iss_parameter_supported: true`.

On the response, before the code goes anywhere near a token endpoint:

| `authorization_response_iss_parameter_supported` | `iss` present | Client action |
| --- | --- | --- |
| true | yes | Compare with the recorded issuer |
| true | no | Reject |
| false or absent | yes | Compare with the recorded issuer |
| false or absent | no | Proceed |

**The comparison rule is the part to get right.** It is simple string comparison per RFC 3986 section 6.2.1. After decoding the value from the form-encoded response, clients **MUST NOT** apply scheme or host case folding, default-port elision, trailing-slash normalisation, or percent-encoding normalisation before comparing. A URL library that helpfully normalises `https://auth.example.com:443` to `https://auth.example.com` has just defeated the check. On mismatch the client **MUST NOT** act on or display `error`, `error_description` or `error_uri`, because those fields are attacker-controlled at that point.

**PKCE does not cover this.** The specification says so directly: in a mix-up attack the client transmits its `code_verifier` to the attacker's token endpoint, so the proof of possession travels with the code. Resource indicators do not help either when the attacker's authorisation server is intercepting requests upstream. Issuer validation is the mitigation, and it depends on honest authorisation servers emitting `iss`.

## Step 9. PKCE, stated as what it is

PKCE is required here by inheritance rather than by a separate MCP rule. Clients **MUST** implement PKCE per OAuth 2.1 section 7.5.2, and **MUST** use the `S256` code challenge method when technically capable, **as required by OAuth 2.1 section 4.1.1**. Present it that way when you write it down: it is OAuth 2.1's constraint, cited by MCP, not an MCP invention.

What MCP does add is a discovery requirement. Since neither OAuth 2.1 nor PKCE defines a way to probe for support, clients **MUST** rely on authorisation server metadata: if `code_challenge_methods_supported` is absent, clients **MUST refuse to proceed**. That holds for OpenID Connect Discovery too, even though the OpenID provider metadata does not formally define the field, and authorisation servers offering OIDC Discovery **MUST** include it for MCP compatibility.

Communication security is inherited in the same way: all authorisation server endpoints **MUST** be HTTPS, and all redirect URIs **MUST** be either `localhost` or HTTPS.

## Step 10. Refresh tokens

Clients that want refresh tokens **SHOULD** include `refresh_token` in their `grant_types` client metadata, **MAY** add `offline_access` to the scope parameter when the authorisation server lists it in `scopes_supported`, and **MUST NOT** assume refresh tokens will be issued at all. The authorisation server keeps that discretion. For public clients, authorisation servers **MUST** rotate refresh tokens.

## Step 11. Wiring, with the trap that costs a day

Verified on 20 August 2026: the current server package is `@modelcontextprotocol/server` version 2.0.0, requiring Node 20 or later, depending on `zod` ^4.2.0 and `@modelcontextprotocol/core` 2.0.0. The Express adapter is a separate package. None of this differs between Mac and Windows and nothing here depends on the shell.

Web-standard hosts import `requireBearerAuth` and `oauthMetadataResponse` from `@modelcontextprotocol/server`. Express hosts import `requireBearerAuth`, `getOAuthProtectedResourceMetadataUrl` and `mcpAuthMetadataRouter` from the Express middleware package. The gate takes a `verifier` holding your `verifyAccessToken` function, `requiredScopes`, and a `resourceMetadataUrl` built with `getOAuthProtectedResourceMetadataUrl(mcpServerUrl)`. Your verifier returns an `AuthInfo` with four fields: `token`, `clientId`, `scopes` and `expiresAt`.

**Always populate `expiresAt`.** The gate answers `401 invalid_token` for a token whose `expiresAt` is unset, exactly as it does for an expired one. Fill it from the JWT `exp` claim or from introspection. The symptom is a user who reconnects, works for a moment, is signed out again, repeatedly, against a verifier that appears to be succeeding.

To reject a token, throw `OAuthError` with `OAuthErrorCode.InvalidToken`, which produces the 401 challenge. Any other exception comes back as a 500, which tells the client nothing.

On a web-standard host the shape is: run the gate, return the response if it returned one, otherwise pass the verified auth through as `handler.fetch(request, { authInfo: auth })`. Inside a tool, read `ctx.http?.authInfo`. The optional chaining is not decoration: on stdio there is no HTTP request and it is undefined.

**Per-tool scopes.** `requiredScopes` gates the whole endpoint. When one tool needs a scope the rest do not, check `ctx.http?.authInfo?.scopes` inside that handler and return a tool execution error with `isError: true`, so the model reads the refusal. Answering HTTP 403 `insufficient_scope` instead triggers the client's automatic step-up flow, which is right when you want a new token and wrong when you simply want to say no.

## Step 12. If your server proxies a third-party API

An MCP server that sits in front of a third-party API using one static client ID has a confused deputy problem, and the mitigation is a set of MUSTs.

Proxy servers **MUST** obtain user consent for each dynamically registered client before forwarding to the third-party authorisation server: keep a registry of approved `client_id` values per user and check it **before** starting the third-party flow. The consent page **MUST** identify the requesting client by name, display the third-party scopes requested, show the registered `redirect_uri` where tokens will be sent, implement CSRF protection, and prevent framing with `frame-ancestors` or `X-Frame-Options: DENY`. Consent cookies **MUST** use the `__Host-` prefix, set `Secure`, `HttpOnly` and `SameSite=Lax`, be cryptographically signed or backed by a server-side session, and be bound to the specific `client_id` rather than recording that the user consented to something once. Redirect URIs **MUST** be validated by exact string match, with no patterns and no wildcards, and re-registration is required if the URI changes.

The `state` parameter carries the enforcement, and its ordering is the part that gets missed. Generate it with a secure random source. Store it server-side **only after** consent has been explicitly approved. Set the tracking cookie **immediately before** redirecting to the third party, never earlier. Validate an exact match at the callback, reject anything missing or mismatched, make it single use, and expire it in around ten minutes. A cookie set before approval renders the consent screen decorative, because a crafted authorisation request walks straight past it.

## Step 13. SSRF on every OAuth URL you fetch

Discovery means fetching URLs someone else supplied: the `resource_metadata` URL from a challenge, the `authorization_servers` entries, and the endpoints inside authorisation server metadata. An authorisation server supporting CIMD fetches a URL supplied by an unknown client, which is the same exposure pointed the other way.

Require HTTPS except for loopback in development. Block private and reserved ranges: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`, `127.0.0.0/8`, `::1`, link-local `169.254.0.0/16` where cloud metadata services live, and IPv6 `fc00::/7` and `fe80::/10`. Apply the same validation to redirect targets rather than following them blindly. Do not hand-roll the IP parsing: the specification warns that attackers use octal, hex and IPv4-mapped IPv6 encodings custom parsers miss.

## Step 14. State handles are not credentials

MCP is stateless in this revision, so anything spanning calls is an explicit handle passed as an ordinary tool argument. A handle is a name, not a capability. Servers that implement authorisation **MUST** verify every inbound request and **MUST NOT** treat possession of a handle as authentication. Generate handles from a secure random source, bind them server-side to the authenticated user by keying stored state as `<user_id>:<handle>` where the user id comes from the **verified token** and not from the request body, and reject a handle presented by any other principal.

## Decision rule: which registration mechanism

- **You control the client and the authorisation server, and can register once.** Pre-registration. Simplest, and the credentials are tied to that one issuer.
- **The client is a third party you have never met, and the authorisation server advertises `client_id_metadata_document_supported`.** Client ID Metadata Documents. This is the case MCP was redesigned around.
- **The authorisation server advertises `registration_endpoint` but not CIMD.** Dynamic Client Registration, deprecated, and set `application_type` explicitly.
- **Neither is advertised.** Prompt the user for client details. This is a real branch, not a failure.
- **You cannot tell, because the metadata document will not load, or its `issuer` does not match the identifier you used to construct the URL.** Stop. Do not guess and do not fall back. A mismatched issuer **MUST** be rejected, and a client that proceeds anyway has just accepted an attacker's endpoints. Fix discovery before choosing a registration mechanism.

## Worked example, compressed

A documentation search server is deployed at `https://docs-mcp.example.com/mcp` behind a shared edge proxy. OAuth was implemented from a 2025 tutorial and then patched. A user connects: the consent screen appears, they approve, the browser returns to the client, and the connector shows as connected. No tool ever runs. Nobody can reproduce an error because there is no error.

**Step 0 finds it.** Access logging was off for 2xx. Once enabled, one connection attempt produces: `GET /.well-known/oauth-protected-resource/mcp` 200, `POST /mcp` 401, the token exchange at the authorisation server, and then nothing at all. Not a 401, not a 403. Nothing.

**The defect.** The metadata document declares `"resource": "https://docs-mcp.example.com/mcp/"`, with a trailing slash, because the framework normalises paths and someone copied the redirected URL. The challenge advertises the no-slash form. The two identities are the same server and different strings, and the comparison is exact.

**Second defect, found while looking.** The token verifier decodes the JWT, checks the signature and the expiry, and never looks at the audience. It would have accepted a token issued for a different service entirely. Separately, `expiresAt` is left unset in the returned `AuthInfo`, which under the SDK gate produces `401 invalid_token` on every request even for a perfectly valid token, so the trailing slash was hiding a second failure behind it.

**Third defect.** The search tool forwards the incoming access token to the upstream knowledge base API. That is token passthrough and it is forbidden. The upstream call must use a separate token issued by the upstream authorisation server, with the MCP server acting as an OAuth client there.

**Verdict: the server is not broken, its identity is, and it is failing open on the audience.** Fix in this order. Republish the metadata with the no-slash canonical URI and use that same string in the challenge and the audience check. Populate `expiresAt` from the JWT `exp`. Add the audience check and reject anything not issued for this server. Then remove the passthrough and register the server as a client of the upstream API. Do not touch the transport, the tool schemas or the client configuration: none of them was ever involved.

## Failure modes

**The audience-blind server.** The verifier checks the signature and the expiry and accepts any well-formed token from a trusted issuer. Nothing fails, ever, until someone replays a token minted for a different service and it works. There is no symptom before that, which is why the specification makes it a MUST.

**The mismatched `resource`.** OAuth completes end to end, every response is a 2xx, and the client then makes zero MCP calls. Because many servers do not log 2xx, the entire successful flow is invisible and the server looks idle. Turn on access logging first, then compare the `resource` string in the metadata document, in the challenge and in the token's audience, character by character, including the trailing slash and the case of the host.

**The metadata that 404s at the edge.** A shared proxy forwards only `/mcp` to your process, so both well-known paths are answered by the proxy's own HTML 404. Discovery fails, the connector reports a generic error, and no request ever reaches your code, so no log line of yours exists. This is a proxy fault that presents as an application fault.

**The expired-token loop.** The verifier omits `expiresAt`, the SDK gate treats an unset expiry as invalid, and users reconnect, work briefly, and get signed out again. Every log line says `invalid_token` about a token that is perfectly valid.

**The normalised issuer comparison.** Somebody compares `iss` using a URL object rather than the raw string. Case folding on the host, or dropping the default port, or adding a trailing slash makes two different issuers compare equal, and the mix-up attack the check exists to stop passes silently. The code looks more correct than the correct version.

**The DCR dead end.** A registration written against an OpenID Connect authorisation server omits `application_type`, defaults to `"web"`, and gets rejected for localhost redirect URIs with an error that does not mention `application_type`. Meanwhile the mechanism itself is deprecated, so the time spent debugging it is spent on the path you were supposed to be leaving.

**The consent-cookie bypass.** The consent cookie is set when the authorisation request arrives rather than after the user approves. Every subsequent crafted request finds a cookie, skips the consent screen, and the screen you built is decorative. Nothing errors, and the flow looks correct in every manual test, because a human always approves the first time.

**The token you passed through.** The upstream API accepts the MCP client's token because it happens to trust the same issuer. It works. The audit trail on both sides is now wrong, your rate limits key on the wrong identity, and adding any control later means re-authenticating every user.

## What this skill does not do

- It does not implement an authorisation server. It covers the resource server half only, and if you need to issue tokens that is a separate and much larger project.
- It does not tell you which protocol revision any particular client speaks. That is **unverified** here on purpose, and it belongs to the transport and hosting decision.
- It cannot confirm that your authorisation server honours the `resource` parameter or what it puts in the audience claim. Providers differ. Request a token and read the claim.
- It does not cover the redirect URI a specific host registers for its own connector client. One such value circulates in older material; it is absent from current documentation and is treated here as **unverified** and probably obsolete, since clients now supply their own redirect URI through CIMD or registration.
- It cannot see your proxy, CDN or ingress configuration, which is where the well-known 404 and the stripped header both come from.
- It is dated. Revision 2026-07-28, verified 20 August 2026. Check the revision line on the specification before trusting any of it.
