---
name: security-policy-header-build
description: Builds a deployable Content-Security-Policy and the surrounding security header set. Covers the choice between nonce and hash, the 'strict-dynamic' construction and what it silently nullifies, base-uri and object-src, violation reporting through report-to and report-uri, and a report-only rollout with an enforcement cutover. Also removes the headers that are obsolete as of 2026. This skill should be used when adding or fixing a content security policy, when a first draft policy turns out to be a list of allowed hosts, or when a site's whole security header set is being reviewed or rewritten.
---

# Security policy header build

## The claim this is built on

Almost every first content security policy is a list of the hosts a site loads from. That approach was measured and it fails.

Weichselbaum, Spagnuolo, Lekies and Janc, "CSP Is Dead, Long Live CSP! On the Insecurity of Whitelists and the Future of Content Security Policy", presented at the 23rd ACM Conference on Computer and Communications Security in Vienna in 2016, analysed a search corpus of roughly 100 billion pages from over 1 billion hostnames, covering CSP deployments on 1,680,867 hosts with 26,011 unique policies. Their findings, in their words:

- bypasses in **94.72% of all distinct policies**
- **75.81% of distinct policies use script whitelists that allow attackers to bypass CSP**
- **94.68% of policies that attempt to limit script execution are ineffective**
- **99.34% of hosts with CSP use policies that offer no benefit against XSS**

The mechanism is not exotic. One allowlisted host somewhere serves something that can be made to execute attacker-controlled code: a JSONP endpoint whose callback parameter is reflected into the start of the response, or a copy of a templating library that evaluates expressions in the DOM. The authors found 194,908 domains exposing JSONP endpoints and 101,330 hosting the AngularJS library. Of the fifteen domains most commonly allowlisted for scripts, fourteen contained unsafe endpoints. At the median policy length of 12 allowlisted hosts they bypassed 94.8% of policies, and the top ten bypass domains alone were enough to bypass 68% of all unique policies.

So the allowlist is not a weaker version of the right answer. The right answer is that the page names the scripts it trusts, per response, and the browser trusts nothing else.

**What you produce.** Four things, none of them a list of findings: the policy string per environment, the rollout plan with dates, the triage rules for the report endpoint, and the header set with the obsolete headers explicitly removed.

## Step 1. Decide nonce or hash, before writing anything

This choice determines the whole policy, so make it first and write down the branch you took.

- **The pages are rendered per request by your own server.** Use a **nonce**. Generate it in the request path, put it in the header and on every trusted `script` tag in that same response.
- **The responses are static or cached at the edge with no per-request work possible**, such as a fully pre-rendered site on a CDN. Use **hashes**. Accept the consequence: every change to an inline script changes its hash, so the policy becomes a build artefact and a stale deploy means a blank page.
- **You cannot tell, or the site is a mix of rendered routes and cached routes.** Do not try to write one policy for both. Start report-only with a nonce policy on the rendered routes and put the cached routes explicitly out of scope in writing, with a named owner and a date. A policy scoped to what you can actually control ships. A universal one does not, and a policy that never ships protects nobody.

An edge worker that rewrites HTML can inject a nonce into cached responses, which moves a cached route into the first branch. Confirm it rewrites the body and not only the headers, because a header-only rewrite gives you a nonce nothing on the page carries, and every script dies.

## Step 2. Write the policy

The target shape, from section 8.5 of the CSP Level 3 draft (W3C Working Draft, 13 August 2026), which the specification itself calls **Strict CSP**:

```
Content-Security-Policy: script-src 'strict-dynamic' 'nonce-{RANDOM}'; base-uri 'self'; object-src 'none';
```

Hash-based, same shape:

```
Content-Security-Policy: script-src 'strict-dynamic' 'sha256-{HASHED_INLINE_SCRIPT}'; base-uri 'self'; object-src 'none';
```

**Nonce mechanics.** Section 7.1 of the draft: if a server delivers a nonce source expression, the server **MUST** generate a unique value each time it transmits a policy, the value **SHOULD** be at least 128 bits long before encoding, and it **SHOULD** come from a cryptographically secure random number generator. The draft is blunt about why: nonces override the other restrictions in the directive they appear in, so a guessable nonce makes bypassing the policy trivial. Two consequences. A nonce must never be cached with the HTML, because a cached page serves one fixed nonce to everyone forever. And it must never sit anywhere an attacker can read it back, which is the dangling markup attack of section 7.2: an injection point earlier in the document that swallows the following markup can repurpose a legitimate nonce.

**Hash mechanics.** Allowed algorithms are `sha256`, `sha384` and `sha512`. The hash covers the UTF-8 bytes of the element's content regardless of the document's own encoding. CSP Level 3 added a useful case: a hash source can match an **external** script when the `script` element carries `integrity` metadata listed in the policy, so hashes are no longer only for inline blocks.

**What `'strict-dynamic'` actually does.** From section 8.2, when present in `script-src` or `default-src` it has two effects. Host and scheme source expressions, together with `'unsafe-inline'` and `'self'`, are **ignored when loading script**. Nonce and hash sources are honoured. And script requests triggered by non-parser-inserted `script` elements are allowed, so a trusted script can load its own dependencies without every URL appearing in the policy. Scripts created with `document.createElement` load. Scripts written with `document.write` do not, because those are parser-inserted.

Two things follow that teams get wrong constantly. First, **an allowlist kept alongside `'strict-dynamic'` is dead weight**: modern browsers ignore it, so it protects nothing and misleads whoever reads the policy next. Second, `'strict-dynamic'` is a real trade. The draft warns that scripts created at runtime will execute, so if an attacker controls the URL passed to a dynamic loader, the policy allows it. Audit any call that builds a script URL from data.

**Why `'unsafe-inline'` can appear without weakening the policy.** A source list allows all inline behaviour only if it contains `'unsafe-inline'` and nothing overrides it. Any nonce or hash source in the same list overrides it, and for scripts `'strict-dynamic'` overrides it too. Note the asymmetry: `'unsafe-inline' 'strict-dynamic'` still allows inline **style**, because `'strict-dynamic'` applies to scripts only.

That is what makes the backwards-compatibility ladder work. Section 8.2 gives it directly: the policy `'unsafe-inline' https: 'nonce-abcdefg' 'strict-dynamic'` behaves as `'unsafe-inline' https:` in a CSP1 browser, as `https: 'nonce-abcdefg'` in a CSP2 browser, and as `'nonce-abcdefg' 'strict-dynamic'` in a CSP3 browser. The published production form is:

```
Content-Security-Policy: object-src 'none'; script-src 'nonce-{random}' 'unsafe-inline' 'unsafe-eval' 'strict-dynamic' https: http:; base-uri 'none'; report-uri https://your-report-collector.example.com/
```

In the words of the strict-csp guide: "In the presence of a CSP nonce the unsafe-inline directive will be ignored by modern browsers. Older browsers, which don't support nonces, will see unsafe-inline and allow inline scripts to execute." Comment it in your config, because it looks like a mistake to any reviewer who has not read section 8.2.

## Step 3. The directives that matter, in order of what they buy

| Directive | What it closes |
| --- | --- |
| `script-src` with a nonce or hash plus `'strict-dynamic'` | Execution of injected script. This is the whole point and everything else is secondary. |
| `object-src 'none'` | Plugin content. An attacker who can upload a file to any allowed host and have it interpreted as a plugin object gets script execution. Almost nobody needs plugins, so this costs nothing. |
| `base-uri 'self'` or `'none'` | An injected `base` element. Without it, an attacker who can inject one tag rewrites the resolution of every **relative** script URL on the page and your nonce policy loads their scripts. This is the single cheapest directive on the list and it is missing from most policies. |
| `frame-ancestors` | Clickjacking and UI redressing. It does **not** fall back to `default-src`, so `default-src 'none'` alone still lets anyone frame you. |
| `form-action` | A form whose target has been repointed at an attacker's collector. |
| `require-trusted-types-for 'script'` | DOM cross-site scripting at the sink, by rejecting plain strings passed to sinks like `innerHTML`. Baseline newly available since February 2026 per MDN. It is defined in the Trusted Types specification, a W3C Working Draft of 23 June 2026, and referenced by CSP3. |
| `default-src` | Exfiltration through request types no specific directive covers, `prefetch` among them. Section 8.6 notes a policy with no `default-src` cannot mitigate exfiltration, and that a single wildcard such as `img-src *` reopens it anyway. |

Argue for `base-uri` and `object-src` first. Each is one token, neither costs anything operationally, and both were among the causes of bypass in the CCS study.

Two delivery details that waste afternoons. `frame-ancestors`, `report-uri` and `sandbox` are **ignored inside a `meta` element**, and `Content-Security-Policy-Report-Only` is not supported in `meta` at all, so a meta-tag policy cannot be rolled out report-only. CSP3 also changed URL matching so `'self'` covers the `https:` and `wss:` variants of the page's origin.

## Step 4. Reporting, which is what makes the rollout possible

`report-uri` is deprecated in favour of `report-to`, which rides on the Reporting API. Three facts decide your configuration:

- `report-uri` only takes effect if `report-to` is **absent**. A browser that supports both uses `report-to` and ignores `report-uri`, which is exactly the backwards compatibility you want.
- `report-to` names an endpoint group, not a URL. The mapping lives in a separate `Reporting-Endpoints` response header. Forgetting that header is the reason a correct-looking policy sends nothing.
- `report-to` reached Baseline newly available in **March 2026** per MDN, so the long-standing claim that it is Chrome-only is out of date. MDN still recommends sending both directives during the transition, and `report-to` cannot be set via a `meta` element.

Send both:

```
Reporting-Endpoints: csp-endpoint="https://collector.example.com/csp"
Content-Security-Policy-Report-Only: script-src 'strict-dynamic' 'nonce-{RANDOM}'; base-uri 'self'; object-src 'none'; report-uri https://collector.example.com/csp; report-to csp-endpoint
```

**Your collector must parse two different JSON shapes.** The deprecated `report-uri` body uses hyphenated keys (`document-uri`, `blocked-uri`, `violated-directive`, `script-sample`, `source-file`, `line-number`). The Reporting API body uses camelCase (`documentURL`, `blockedURL`, `effectiveDirective`, `sample`). Same event, different field names, and a collector written against one silently drops the other. Despite its name, `script-sample` is populated for non-script violations too.

Add `'report-sample'` to the directive if you want the offending source in the report. The sample is the **first 40 characters** of the source, and violations from an external file carry no sample at all.

## Step 5. The rollout, which is the part that decides whether it ships

**Phase 0, before any header.** Stand up the collector and confirm it survives volume: each `report-uri` violation is one request, which the specification itself calls unscalable. Keep a switch that drops the reporting directives without touching the policy.

**Phase 1, report-only, for at least one full traffic cycle.** Ship `Content-Security-Policy-Report-Only` carrying the exact policy you intend to enforce. A full cycle means every weekday, both weekends, and any job that runs weekly or monthly. If a billing page renders once a month from a different template, the cycle is a month. This step is the one that gets shortened, and it decides whether the cutover is an outage.

**Triage, which is the actual work.** Reports are noisy, and no proportion is quoted here because it depends entirely on your audience. Sort structurally instead:

- **Almost always noise:** a `blocked-uri` or `blockedURL` with an extension scheme (`chrome-extension:`, `moz-extension:`, `safari-extension:`), or values like `about:blank`. These are the user's own browser, not your site.
- **Usually noise:** scripts injected by antivirus products, corporate proxies and some mobile carriers, appearing as unfamiliar hosts concentrated in a few user agents or one network.
- **Usually real:** a host you recognise, appearing across many user agents and pages: a tag manager, an analytics loader, a chat widget, a payment iframe.
- **Always investigate:** `blocked-uri` of `inline` or `eval` on a page you control, and any violation of `base-uri` or `object-src`, which should be at zero.

Route reports from your own domain and your own asset hosts to a separate queue from everything else. That one split does most of the sorting.

**Phase 2, the cutover.** Change the header name to `Content-Security-Policy`, on a low-traffic day, with a rollback ready. Watch error rates rather than report volume, because a blocked script usually shows up as a dead button rather than an exception.

**Phase 3, the standing arrangement.** Keep a `Content-Security-Policy-Report-Only` header alongside the enforced one, carrying the **next**, tighter policy. Both headers are legal at once and the browser handles each separately. That is the difference between a policy that improves and one frozen on the day it shipped.

## Worked example

A marketing site for a mid-size logistics company. Server-rendered templates, a tag manager, one analytics script, an embedded video, and a monthly pricing page that renders from a different template.

**The naive first attempt**, written by reading the network tab:

```
script-src 'self' https://tagmanager.example-vendor.com https://analytics.example-vendor.com 'unsafe-inline';
```

It fails twice over. `'unsafe-inline'` with no nonce or hash present allows all inline script, so an injected `<script>alert(1)</script>` runs and the rest of the directive is theatre. Remove it and the policy is still the shape the CCS study measured: two vendor hosts, either of which may serve a JSONP endpoint or an old templating library. No `base-uri`, so one injected `base` tag repoints every relative script. No `object-src`.

**The replacement**, deployed report-only:

```
Content-Security-Policy-Report-Only: object-src 'none'; script-src 'nonce-{RANDOM}' 'unsafe-inline' 'strict-dynamic' https:; base-uri 'self'; frame-ancestors 'self'; form-action 'self'; report-uri https://collector.example.com/csp; report-to csp-endpoint
```

The nonce is generated per request from a cryptographically secure source and stamped on the two first-party script tags. The vendor hosts are gone on purpose: the tag manager is loaded by a nonced first-party script, so `'strict-dynamic'` carries it and everything it loads.

**Three weeks of reports produce three things worth reading.**

1. `blocked-uri: inline`, `document-uri` on the monthly pricing page, roughly 40 hits clustered on one date. Real. The template carries an inline sizing script that nobody had touched in years and it never got a nonce. It appeared only after the monthly render, which a three-day report-only window would have missed entirely.
2. `blocked-uri: https://cdn.other-vendor.example/player.js`, steady, across many user agents. Real. The video embed loads its player through `document.write`, which is parser-inserted, so `'strict-dynamic'` does not cover it. Fixed by switching the embed to the vendor's dynamic loader.
3. `blocked-uri: chrome-extension://...`, high volume, wide spread of pages. Noise. A popular coupon extension injecting into product pages. Excluded at the collector, not in the policy.

**Verdict: enforce, after the two real fixes and one more monthly cycle.** Nonce the pricing page's inline script, replace the video embed, wait for the next month-end render, then cut over. Keep a report-only header carrying the next policy, which drops `https:` and `'unsafe-inline'` and adds `require-trusted-types-for 'script'`.

## The rest of the header set, current as of August 2026

Worth setting:

- **`Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`**. For the preload list, hstspreload.org requires a valid certificate, a redirect from HTTP to HTTPS on the same host if you listen on port 80, all subdomains served over HTTPS including `www` where a DNS record exists, and the header on the base domain with `max-age` of at least **31536000** (one year) plus `includeSubDomains` and `preload`. Any further HTTPS redirect must still carry the header. Preloading is close to irreversible: removal requests are generally honoured but take months to reach users through browser updates, so do not preload until every subdomain is genuinely HTTPS-only.
- **`X-Content-Type-Options: nosniff`**. Two effects per MDN: for `script` and `style` destinations the browser blocks a response whose MIME type does not match, and elsewhere it stops sniffing, so a `text/plain` upload containing HTML is not treated as a document.
- **`Referrer-Policy`**. The browser default has been `strict-origin-when-cross-origin` since the November 2020 spec revision, previously `no-referrer-when-downgrade`. Set it only to go tighter, for example `same-origin` where URLs carry identifiers.
- **`Permissions-Policy`**. Structured syntax, empty allowlist means nobody: `Permissions-Policy: geolocation=(), camera=(), microphone=()`. It replaced Feature-Policy.
- **`Cross-Origin-Opener-Policy`, `Cross-Origin-Embedder-Policy`, `Cross-Origin-Resource-Policy`**, with the cost stated. Full cross-origin isolation needs COOP `same-origin` plus COEP `require-corp` or `credentialless`, after which every cross-origin resource fetched in no-cors mode must carry CORP or be requested with CORS, so third-party embeds break until each vendor cooperates. Pay that when you need `SharedArrayBuffer` or unthrottled `performance.now()`. Otherwise set COOP alone.
- **`X-Frame-Options`**, only as a fallback for very old clients. `DENY` and `SAMEORIGIN` work; `ALLOW-FROM` is obsolete and modern browsers ignore the whole header when they see it. An enforcing policy containing `frame-ancestors` makes the browser ignore `X-Frame-Options` anyway, so `frame-ancestors` is the real control.

Remove:

- **`X-XSS-Protection`**. MDN marks it deprecated and non-standard, and warns that in some cases it "can create XSS vulnerabilities in otherwise safe websites", because the filter can strip one script while leaving a later one running and so change the page's behaviour. Delete the header, or send `X-XSS-Protection: 0` if a scanner insists on seeing it.
- **`Expect-CT`**. Mostly obsolete since June 2021 per MDN, and deprecated in Chromium from version 107, which enforces certificate transparency by default.
- **`Public-Key-Pins`**. MDN no longer publishes a reference page for it: the URL returns 404 as of 20 August 2026. Any guide still recommending HTTP public key pinning is unmaintained.
- **`block-all-mixed-content`**. Marked obsolete in the specification and deprecated on MDN. Use `upgrade-insecure-requests` if you still have mixed content, and fix the URLs.
- **`X-Powered-By`** and framework version banners. Not a control, just free reconnaissance.

## Failure modes

**Allowlist Faith.** The policy is a list of hosts, it looks thorough, and it is bypassable through any one of them. It passes review precisely because it is long. The tell is a policy with no nonce, no hash and more than about five entries in `script-src`.

**`'unsafe-inline'` Reinstated.** Somebody hits a broken page, adds `'unsafe-inline'` to make it work, and removes the nonce in the same commit because "it was not doing anything". The header is still there, the browser still reports it, and script protection is gone. Nothing errors, ever.

**Allowlist Kept Beside `'strict-dynamic'`.** The vendor hosts stay in `script-src` next to `'strict-dynamic'` because deleting them feels like loosening the policy. Modern browsers ignore them. The result is a policy that is longer, harder to review, and no stronger, and the next engineer spends a day adding a host that was never consulted.

**Nonce Reuse.** The nonce is generated once at process start, or generated per request but then cached with the HTML at the edge. Everything works in testing. The policy is now equivalent to `'unsafe-inline'` for anybody who fetches one page and reads the value.

**Report Flood Ignored.** The endpoint takes thousands of extension-scheme entries a day, nobody separates them, and the queue is declared unreadable. The cutover is postponed indefinitely and the site carries a report-only header for two years, which protects nobody.

**Missing `base-uri`.** The policy is otherwise correct and modern. One injected `base` element repoints every relative script URL, and the nonce is irrelevant because the attacker's file is then loaded by a legitimate nonced tag. It grades well in any review that reads only `script-src`.

**Enforcement Without A Cycle.** Report-only runs three quiet midweek days, then gets enforced. The month-end job, the seasonal banner and the admin export use templates nobody saw, and they break one at a time over the following weeks, each looking like an unrelated incident.

**Header On One Route.** The policy lives in application middleware covering the main app only, so marketing pages, error pages, the file-download route and anything served straight from the CDN carry no policy. Coverage looks complete from the homepage.

**Obsolete Header Cargo Cult.** The header set is copied from an old blog post, so it carries `X-XSS-Protection: 1; mode=block`, `Expect-CT` and `X-Frame-Options: ALLOW-FROM`, and it earns a good grade from a checklist that has not been updated either. One of those three can actively make things worse and the other two do nothing.

## What this skill does not do

- It does not find or fix the injection defect underneath. A content security policy is defence in depth: a site with a perfect policy and an unescaped template still has cross-site scripting, and the policy only makes it harder to exploit.
- It cannot test your site. A policy that is correct in prose can still break a page, and the report-only phase is the only way to find out. Run the finished string through CSP Evaluator, which grades it faster and more accurately than reading it will.
- It says nothing about server-side issues, and nothing about an API that returns JSON, where no policy is applied because no document is parsed.
- It does not cover subresource integrity in depth, beyond the CSP3 rule that a hash source can match an external script carrying matching integrity metadata.
- It has nothing to say about native or mobile applications, or about anything outside a browser's document context.
- A permissive policy is worse than none, because it earns a passing grade on a questionnaire and a false sense of coverage. If you cannot get past `'unsafe-inline'` in `script-src`, ship the parts that do work, `object-src 'none'`, `base-uri 'none'`, `frame-ancestors` and `form-action`, and record the script gap as an open item.
