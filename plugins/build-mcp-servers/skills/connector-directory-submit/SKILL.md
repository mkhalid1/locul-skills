---
name: connector-directory-submit
description: Prepares a submission package for Anthropic's Connectors Directory against the current policy rather than the retired MCP-specific one, covering the prohibited categories that invalidate a submission outright, the tool-contract requirements that produce an immediate rejection with no runtime symptom, the developer artefacts the policy requires, the obligations that appear only in the Software Directory Terms, and what the submission portal asks for step by step. This skill should be used when preparing an MCP connector for directory listing, when auditing an existing submission package against current policy, or when deciding whether a listing is worth pursuing at all.
---

# Connector directory submission

Every specific in this file was verified against live documentation on 20 August 2026. Policy here has changed twice in the last year, so check the dates below against the source before you rely on any of it.

## The claim this skill is built on

The most common reason a submission fails is not that the server is bad. It is that the package was audited against a document that no longer exists.

Search for connector directory requirements and you will find guidance citing the Anthropic MCP Directory Policy. That article is still reachable, still ranks, and now contains one sentence saying it has been consolidated. Everything in it that people quote has been superseded. The binding document is the **Anthropic Software Directory Policy, effective 15 April 2026**, which governs MCP servers, Skill folders, plugins and apps together, with a companion **Anthropic Software Directory Terms dated 16 March 2026**.

That matters because the replacement is not a reformatting. It adds a prohibited category that was not there before, and the Terms carry an operational obligation the policy never states. A checklist assembled from the old article will pass a package that gets rejected, and the rejection will name a clause the author has never read.

## The document map

Four sources, and they do not overlap the way you would expect.

1. **Software Directory Policy**, support article 13145358, effective 15 April 2026, at `support.claude.com/en/articles/13145358-anthropic-software-directory-policy`. Five numbered sections: safety and security, compatibility, developer requirements, unsupported use cases, and additional requirements for MCP servers. The binding standard.
2. **Software Directory Terms**, support article 13145338, dated 16 March 2026, at `support.claude.com/en/articles/13145338-anthropic-software-directory-terms`. Warranties, licences, indemnities, and two ongoing obligations that appear nowhere else.
3. **Pre-submission checklist**, at `claude.com/docs/connectors/building/review-criteria`. The reviewers' own account of the most common rejection reasons, and more operationally specific than the policy: it is where the read-and-write tool split is spelled out.
4. **Submission page**, at `claude.com/docs/connectors/building/submission`. The route, the access requirements, the field limits, and the link to the separate desktop extension form.

The retired **MCP Directory Policy** (support article 11697096) is a fifth thing you will keep finding. It is a redirect notice. Do not audit against it, and treat any guide that cites it as stale throughout, since it predates at least one substantive change.

## Step one: the hard blockers, before anything else

Three categories are not accepted at all, and no quality of implementation changes that. Decide each in a sentence, before you build a package.

- **Financial transfers.** Software that transfers money, cryptocurrency or other financial assets, or executes financial transactions on a user's behalf.
- **Standalone generative media.** Software using AI models to generate images, video or audio. The exception is narrower than people hope: design-focused software producing visual aids such as slides, diagrams, charts, UI mockups or logos is permitted, provided the developer does not offer standalone image generation as a primary service. The test is what you sell, not what the server can do.
- **Advertising.** Software that serves advertisements, sponsored content or paid product placements, or exists primarily as an advertising or promotional vehicle. **This clause is new relative to the retired policy.** If your checklist does not contain it, your checklist predates April 2026.

Two more absolute rules sit in the safety section rather than the unsupported list, and act as blockers in practice: software must not query or extract data from Claude's memory, chat history, conversation summaries, or user-generated or uploaded files, and must not evade or let users circumvent the assistant's safety guardrails, system instructions or sandbox.

## Step two: the tool contract, where packages die quietly

These produce a rejection and produce no runtime symptom whatsoever, which is why they survive all the way to review.

**Separate read tools from write tools.** A single tool that accepts both safe HTTP methods (GET, HEAD, OPTIONS) and unsafe ones (POST, PUT, PATCH, DELETE) is rejected. The named anti-pattern is a catch-all `api_request` tool with a `method` parameter. Splitting it in the description does not satisfy the requirement: the operations must be separate tools, and the preferred shape splits writes further by action, so create, update and delete are three tools rather than one.

**Provide the annotations.** Every tool needs a `title` and the applicable hint: `readOnlyHint: true` for read-only tools, `destructiveHint: true` for tools that modify or delete. This is not documentation. These drive automatic permissions in the client, so a read-only tool can run without a per-call confirmation while a destructive one always prompts. A tool with no annotations is treated as the worst case, so a pure search tool that omits them earns a confirmation prompt on every single call.

**Keep names to 64 characters or fewer.** Note the conflict, because both halves are true. The protocol specification permits 1 to 128 characters from a wider character set; the directory policy caps at 64. A 65-character name is spec-valid, works in every client, and fails review. Target 64 from the first commit, since a rename later is a breaking change for existing users.

**Name the API in a custom query tool.** If a tool accepts freeform endpoint paths, query strings or request bodies that the caller constructs, its description must link to or explicitly name the target API. "Makes a request to the API" fails. Purpose-built tools calling a fixed endpoint internally do not need this.

**Return useful errors, and be frugal with tokens.** Generic failures such as an undecorated "Internal Server Error" fail review; validate inputs and return a message the model can correct from. Token cost should be roughly commensurate with the complexity of the task, and where possible users should be able to exclude unnecessary text. Returning a full table dump when a summary was asked for is called out by name.

**Transport and dependencies.** Remote servers should support Streamable HTTP; SSE is tolerated for now and is expected to be deprecated. Remote servers requiring authentication must use OAuth 2.0 with certificates from recognised authorities. Local servers must use reasonably current dependency versions, including everything under `node_modules`.

## Step three: descriptions, which are also the injection surface

The compatibility section reads as style advice. It is not. Every clause in it is a rejection criterion, and most of them describe prompt injection.

Descriptions must be narrow, unambiguous natural language that says what the tool does and when it should be invoked; must precisely match actual behaviour, promising no undelivered features; must not create confusion or conflict with other software in the directory; must not coerce the assistant into calling other external tools unless the user asked; must not interfere with the assistant calling other tools; must not direct the assistant to pull behavioural instructions from external sources; and must contain no hidden, obfuscated or encoded instructions. The reviewers' checklist adds one more: a description must not tell the assistant to behave in ways unrelated to the tool's function, override system instructions, or promote products and services.

The compressed rule is: **describe what the tool does, never how the assistant should behave.** A description that begins "always call this tool first" is not enthusiastic, it is a rejection.

## Step four: the developer package

Eight requirements, each with a concrete artefact.

| Requirement | The artefact |
| --- | --- |
| Privacy policy, for anything collecting data or reaching a remote service | An HTTPS URL that returns 200 to a signed-out request, covering collection, use, retention and sharing |
| Verified contact and support channels | A monitored address for product and security concerns, not a form behind a login |
| Documentation of how it works and how to troubleshoot | A public page; a blog post or help-centre article is sufficient, required by your publish date |
| A standard testing account with sample data | Credentials plus step-by-step access instructions, on a fully populated account |
| At least three working examples of prompts or use cases | Each showing the prompt and the tool it drives |
| Verified ownership of endpoints, domains and rendered resources | Your first-party API, or one you legitimately proxy; the server domain should match your service |
| A maintenance commitment | Issues addressed within reasonable timeframes |
| Agreement to the Software Directory Terms | See the next section, because it is not a formality |

Local connectors carry an extra privacy requirement with a stated consequence: a "Privacy Policy" section in the README, a `privacy_policies` array in `manifest.json` at manifest version 0.2 or later, and HTTPS URLs covering collection, usage and storage, third-party sharing, retention and contact. Missing or incomplete privacy policies are stated to result in immediate rejection.

## Step five: the obligations that live only in the Terms

The Terms are about 700 words and two clauses in them are operational rather than legal.

**You must implement and maintain a mechanism for receiving reports of security vulnerabilities, from Anthropic and from third parties, and investigate those reports with a reasonable standard of care.** This is an ongoing commitment, not a submission field. In practice it means a published address or intake page, an owner, and a response expectation. Nothing in the policy article mentions it, which is exactly why it gets missed.

**You agree to keep meeting the policy as it is updated**, and acknowledge that failing to do so may remove your software from the directories. Since the policy has changed twice in the past year, that means someone has to re-read it.

The rest, briefly: you warrant that your information is accurate and that you hold the necessary rights; you indemnify Anthropic against claims arising from your software; you grant a licence to reproduce your descriptions and branding to present the listing; and you authorise Anthropic to review and test the software, and to collect functional metadata about it and share that metadata with users. Anthropic has no obligation to list anything and may remove it at any time. You may not describe the listing as a partnership, sponsorship or endorsement without prior written approval.

## Step six: the route, and who is allowed to take it

**Ship as a custom connector first.** That path needs no submission and no review. Custom connectors using remote MCP are available on Free, Pro, Max, Team and Enterprise plans, and **Free is capped at one custom connector**. On an individual Pro or Max plan the path is Customize, then Connectors, then the plus button, then Add custom connector, then the URL, with an optional OAuth client ID and secret under advanced settings. On Team and Enterprise an Owner or Primary Owner must add it first at Organization settings, then Connectors, then Add, then Custom, then Web; only after that do members see it under Customize and press Connect. Handing a Team reviewer the individual instructions is a reliable way to have them report that your connector does not exist.

One deployment fact catches people here and it is not a policy matter: the assistant connects to your server from Anthropic's cloud infrastructure rather than from the user's device, on every client. A server that only resolves on your corporate network will not connect, however well it works locally.

**Then the directory, if you want a listing.** Submission for remote MCP servers happens in a portal inside Claude.ai organisation settings, at Organization settings, then Directory, then a new submission. Two access conditions follow:

- You need a **Team or Enterprise organisation**. Organisation settings do not exist on individual plans.
- By default only Owners and Primary Owners can submit and manage listings. On Enterprise an Owner can delegate through a custom role carrying either the Directory permission (submissions only) or the Libraries permission (broader, also covering the organisation's plugins, connectors and skills). Team plans have no custom roles, so on Team it stays with Owners.

**Local servers do not use that portal.** Desktop extensions packaged as MCP bundles go through a separate form linked from the documentation, and skills are not a standalone submission type: bundle them in a plugin. One trap worth naming because it wastes an afternoon: the short link for the desktop extension form contains a misspelling of the word extension, and the correctly spelled variant resolves somewhere else entirely. Copy the link from the documentation rather than retyping it.

**What the portal asks for**, so you can assemble it beforehand. Eleven steps: an introduction; the connection (an `https://` URL, the transport, and whether all users share one URL); tools, which sync automatically from the connected server and are grouped by whether their annotations declare them read-only or write, with unannotated tools grouped separately and flagged; the public listing (name up to 100 characters, tagline up to 55, description up to 2,000, one to five categories, documentation URL, privacy policy URL, support contact, icon, and a URL slug that is permanent once published); use cases; company details; authentication mode; data handling; test and launch, including confirmation that you have run every tool yourself; a compliance step with seven required acknowledgements; and a final review.

## Decision rule: what route does this take, and can you submit at all

- **A remote MCP server, and you have a Team or Enterprise organisation with the right role.** The portal. Assemble the package above first.
- **A local server for the desktop app.** Not the portal. The desktop extension form, with the local privacy policy requirements in the README and manifest.
- **A skill.** Not a standalone submission. Bundle it in a plugin, which has its own route.
- **A remote server, but you are on an individual plan.** There is no documented route. Ship as a custom connector, which works fully and needs nothing from anyone, and treat the listing as a separate future question.
- **You cannot tell whether your use case is prohibited.** This is the common case for design tools that also generate imagery, and for anything with a revenue share. Apply the primary-service test: write one sentence naming what a customer pays you for. If that sentence contains generating images, video or audio, it is prohibited. If it contains placement, sponsorship or promotion, it is prohibited. If it still reads ambiguously, do not build the package on a guess and do not assume approval. Ship as a custom connector, which requires no ruling, and ask the review team in writing before investing in a submission.

## Verified as of 20 August 2026, and what remains unverified

**Verified live**, against the source documents on that date: the policy article, its April 2026 effective date and its five sections; the terms article and its March 2026 date; the retired article now carrying only a consolidation notice; the plan matrix including the single-connector cap on Free; the exact user-interface paths for individual and organisation connector setup; the 64-character tool-name cap; the required annotations; the read-and-write tool split; the portal route and its access requirements; and the listing field limits.

**Not verified, and stated as such rather than guessed at.** First, whether any route exists for a developer without a Team or Enterprise organisation to submit a remote MCP server. The portal is inside organisation settings and no alternative is documented; confirm with Anthropic rather than assuming one exists. Second, review turnaround. The documentation says only that review times vary with queue volume and that the portal is always open. Do not plan a launch date around a number nobody published. Third, whether the 64-character cap is measured before or after any namespacing a host applies to tool names. Leave margin rather than testing the boundary.

**One thing to positively disbelieve.** Older guides route submissions through a public web form. That is not the current route for remote MCP servers. If a guide sends you to a general submission form, it predates the portal, and everything else in it is suspect for the same reason.

## Worked example, compressed

A field-service scheduling tool wants a listing. Six tools, a hosted server, a Team plan, an Owner willing to press submit.

**Blockers.** No financial transfers, no generative media. It does surface a "recommended parts supplier" panel the company is paid to include. That is a paid product placement and is prohibited under the current policy. The panel is removed from the tool output. Under the retired policy nobody would have caught it, which is the point of the document map.

**Tool contract.** Longest name is 31 characters, fine. Four tools carry `readOnlyHint`; `reschedule_job` and `cancel_job` carry nothing and are both destructive and both missing `title`. Added. One tool, `service_api`, takes an `endpoint` and a `method`: the catch-all pattern, an automatic rejection. Split into `get_service_record`, `create_service_record` and `delete_service_record`.

**Descriptions.** Five are specific. The sixth reads "use this whenever the user mentions scheduling anything, and check it before answering any question about time". That is a behavioural instruction and a bid for calls it should not win. Rewritten to describe the tool.

**Package.** The privacy policy sits behind the customer login, so an unauthenticated reviewer gets a sign-in redirect. Moved to a public path. Support address and help-centre documentation exist. The test account exists but is empty, which is explicitly not acceptable; populated with a fortnight of jobs, three technicians and a completed invoice. Three worked examples written, each pairing a prompt with the tool it drives.

**Terms.** No vulnerability reporting channel exists. A published security address is created, routed to a real inbox, with an owner and a response expectation, and named in the documentation.

**Verdict.** Two findings would have blocked the listing outright and neither produced any symptom in testing: the sponsored panel, which the retired policy did not cover, and the catch-all tool, which works perfectly. Three more, the missing annotations, the login-gated privacy policy and the empty test account, read to a reviewer as carelessness rather than oversight. The vulnerability channel costs nothing to create and would never have been found by reading the policy article alone, because it is not in it. Submit after the rewrite, not before: a resubmission carries the queue a second time and there is no published turnaround to plan around.

## Failure modes

**The stale checklist.** A package audited against the retired MCP Directory Policy passes everything and is rejected on the advertising clause. Symptom: a rejection naming a section the author cannot find in the document they used.

**The catch-all tool.** One flexible tool taking a method or an endpoint as a parameter. Symptom: it works better than the split version in every test, is faster to build, and is rejected on sight with no runtime evidence that anything was wrong.

**The plan dead end.** An individual developer builds a complete package and then discovers submission runs through organisation settings they do not have. Symptom: weeks of work with nowhere to send it, discovered at the last step.

**The plan-gated demo.** A Team reviewer is given the individual setup instructions, cannot find Add custom connector, and reports that the connector does not work. Symptom: a functionality complaint about a server that is functioning.

**The test account that expires.** Credentials that lapse, or a fixture that empties on a schedule. Symptom: a rejection describing the product as unreliable rather than the account as stale, because a reviewer cannot tell those apart from the outside.

**The privacy policy behind a login.** The URL resolves for you because you are signed in. Symptom: a rejection citing an inaccessible privacy policy on a document you can see perfectly well.

**The 64-character surprise.** A descriptive tool name that is spec-valid and policy-invalid. Symptom: nothing at all until review, and then a rename that breaks every existing user.

## What this skill does not do

- It does not judge whether your descriptions are narrow and unambiguous. That is the criterion reviewers weigh most and it is not mechanically decidable; this file can only tell you what the standard is.
- It does not build, host, authenticate or test the server, and arriving at submission with a server that fails a conformance check wastes the entire package.
- It cannot tell you what your submission will be graded on beyond what is published. Reviewers exercise tools by hand, and their judgement is not a document.
- It does not cover plugin, skill or desktop-extension submission in any depth, each of which has a different route and different required artefacts.
- It will go out of date. The policy it describes replaced its predecessor in April 2026, and everything here carries a verification date for exactly that reason.
