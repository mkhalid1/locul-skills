---
name: prompt-injection-defence-build
description: Builds the defence design for an LLM agent that reads text the operator did not write, producing an untrusted-input inventory, a capability table classifying every tool by reversibility, blast radius and outward channel, a containment pattern chosen from the published catalogue, a confirmation boundary, and the boundary tests. It starts from the position that the injection succeeds, because instruction-level and filtering defences do not hold against an adaptive attacker. This skill should be used when designing or reviewing an agent that ingests web pages, email, tickets, documents, repository contents or third-party tool output while holding credentials or any ability to send data outward.
---

# Prompt injection defence build

## The position this starts from, and its date

Verified on 20 August 2026 against the OWASP GenAI LLM Top 10 2026, published 4 August 2026, whose canonical source is a public repository under the OWASP GenAI Security Project. The 2026 edition reordered the list against a corpus of 7,714 real incidents, of which 6,639 carried enough detail to classify. Prompt Injection remains **LLM01:2026**. Excessive Agency moved up from LLM06:2025 to **LLM03:2026**. System Prompt Leakage was replaced by **LLM08:2026 Hidden Context Exposure**, and Improper Output Handling moved from LLM05:2025 down to **LLM10:2026**. If a page you are reading cites the 2025 numbering, it predates August 2026.

The position, stated in that chapter: prompt injection is intrinsic to current generative AI, LLMs make no architectural distinction between instructions and data because both are tokens on one stream, and no reliable prevention mechanism exists today. OWASP records that position as consistent with NIST AI 100-2 E2025 and with the UK National Cyber Security Centre's published note that prompt injection is not SQL injection. There is no parameterised query for a prompt.

Everything below follows from that. You are not designing a filter. You are designing what happens after the model has been persuaded.

## Why the obvious approach fails

The instinct is one of two moves: tell the model in its system prompt to ignore instructions found in retrieved content, or run the content through a classifier first.

Both raise the cost of an attack. Neither is a control you can put load on. The published evidence is specific. In October 2025 Nasr, Carlini, Sitawarin, Schulhoff, Hayes, Ilie, Pluto, Song, Chaudhari, Shumailov, Thakurta, Xiao, Terzis and Tramèr published *The Attacker Moves Second*, evaluating twelve recent defences against jailbreaks and prompt injections. Most had reported near-zero attack success rates. Under adaptive attacks built with gradient descent, reinforcement learning, random search and human-guided exploration, attack success exceeded 90% for most of them. StruQ, a structured-query defence presented at USENIX Security 2025, was among those bypassed under adaptive attack.

Three structural reasons this keeps happening:

1. **There is no separation to enforce.** Instruction and data are the same tokens. A marker scheme that labels external content works until an attacker who knows the scheme mimics the markers.
2. **The attacker has unlimited attempts and full knowledge.** Your system prompt can be inferred. Your filter's behaviour can be probed. A defence evaluated only against a fixed attack set is measuring the wrong thing, which is why OWASP's 2026 mitigation list explicitly says to reject static-only attack-success claims.
3. **The channel is wider than text.** Payloads arrive in images below the visual threshold, in audio, in base64 or ROT13, in low-resource languages, split across multiple form fields and recombined at evaluation, or in Unicode that renders as nothing at all.

Keep the filter. Budget nothing on it. The design consequence is the whole of this file: **build as if the injection succeeds**, and make the successful injection worthless.

## Direct versus indirect, attributed

The term **prompt injection** was named by Simon Willison on 12 September 2022, crediting Riley Goodside's demonstration the day before. That original case is **direct** injection: the person typing into the box is the attacker.

**Indirect prompt injection** was named and demonstrated by Kai Greshake, Sahar Abdelnabi, Shailesh Mishra, Christoph Endres, Thorsten Holz and Mario Fritz in *Not what you've signed up for*, arXiv:2302.12173, published 23 February 2023. The model ingests content from somewhere else, and that content carries the instruction.

Indirect is the one that matters for an agent, for a reason worth stating plainly: **the attacker never talks to your system.** They publish a page, send an email, file an issue, name a file, add a code comment, write a package README, or send a calendar invite. Your agent reads it, holding your credentials, and does the work the attacker could not do directly.

OWASP's 2026 chapter splits the delivery surface by trust profile, and the middle row is the one people get wrong:

- **Untrusted**: public web pages, mail from unknown senders, search results. Everyone defends these.
- **Semi-trusted**: issue titles in a public tracker, package READMEs and changelogs, third-party API responses. Content the operator chose to retrieve but did not author.
- **Trusted**: your own repositories, databases, internal documents and mail. The trap is that an attacker may have placed content here through an unrelated low-privilege channel, such as a public bug-report form that writes into an internal ticket.

## Step 1. Build the untrusted-input inventory

The rule, and it is deliberately harsh: **anything not typed by the authenticated operator in this session is untrusted.** That includes documents a colleague wrote, rows in your own production database, and the output of another model. Trust here is about who could have written the bytes, not about which network they arrived on.

Enumerate every channel. A working list to check yourself against:

- Web fetch and search results
- Documents: PDF, spreadsheet, slide, and anything converted to text
- Email bodies, subjects, sender display names and attachments
- Tickets, issues, pull request titles and bodies, code review comments
- Repository contents: source, comments, READMEs, changelogs, commit messages, branch names
- Tool responses from any third-party API
- MCP server tool names and tool descriptions, which are read by the model before any call happens
- File names, directory names and file metadata
- Images, audio and video, including anything a vision encoder reads
- Output from another model or another agent
- Persistent memory and retrieval corpora, which are the ones that persist

For each channel record four things: who can write to it, whether it reaches the context automatically or only on request, whether the content persists across sessions, and which encodings you strip on the way in. On that last point, strip at ingest and again at render: tag block characters U+E0000 to U+E007F, variation selectors U+FE00 to U+FE0F, and zero-width characters U+200B, U+200C, U+200D and U+2060. These are invisible in normal rendering and have been used both to smuggle instructions in and to carry bytes out.

Treat persistence as a severity multiplier rather than a property. PoisonedRAG, published at USENIX Security 2025 by Zou, Geng, Wang and Jia, reported that as few as five poisoned documents reached roughly 90% attack success against a knowledge base of millions of texts. One write, every future session.

## Step 2. The capability table

This is the artifact. Every tool the agent can call gets a row and five columns.

| Column | What you record | The distinction that matters |
| --- | --- | --- |
| Reversibility | Who undoes it, in how many steps, within what window | A draft is reversible. A sent email is not, whatever your outbox says |
| Blast radius | This session's own data, this user's data, this tenant's data, or every tenant | A tool using a shared service identity is always tenant-wide, regardless of what the prompt asks for |
| Outward | Does any byte reach a destination outside your trust boundary | A URL fetch is an outward channel: the path and query string are the payload |
| Credential | The operator's own delegated credential, or a service identity | A per-user tool holding a generic high-privilege identity is the classic excessive-permission finding |
| Spend | Does it cost money, consume quota, or rate-limit a shared resource | Irreversible in a different currency |

Two rules for filling it in honestly. First, classify the tool as built, not as documented: a mail tool that reads and also exposes a send function is a send tool. Second, classify open-ended tools at their maximum: a shell tool or a generic HTTP tool takes the worst row in the table, because its scope is whatever the model writes into the argument.

**The dangerous shape is the combination, not any single row.** Simon Willison named it the **lethal trifecta** on 16 June 2025: access to private data, exposure to untrusted content, and the ability to communicate externally. Any one of the three alone is ordinary. All three in one agent is the condition for high-impact exploitation, and removing any one leg removes the condition.

Meta published the same diagnosis as an operating rule on 31 October 2025, the **Agents Rule of Two**: an agent may process untrustworthy inputs [A], may access sensitive systems or private data [B], and may change state or communicate externally [C], but should satisfy no more than two of the three within a session. An agent that needs all three without a fresh context window should not operate autonomously and requires human-in-the-loop approval or equivalent validation at minimum. OWASP's 2026 LLM01 chapter adopts this as a floor: any [A,B,C] agent needs per-action human approval, and [A,B] or [A,C] configurations need an explicit residual-risk assessment. The published criticism is worth carrying too: Noma Security's note that the rule is silent on autonomy depth, so an agent satisfying only two properties across a hundred unsupervised steps is still unaddressed by it.

## Step 3. Choose a containment pattern, and pay for it

Beurer-Kellner, Buesser, Creţu, Debenedetti, Dobos, Fabian, Fischer, Froelicher, Grosse, Naeff, Ozoani, Paverd, Tramèr and Volhejn published *Design Patterns for Securing LLM Agents against Prompt Injections*, arXiv:2506.08837, on 10 June 2025. Its core principle is the sentence to design against: once an agent has ingested untrusted input, it must be constrained so that it is impossible for that input to trigger any consequential action.

The six patterns, with the cost each one charges. Pattern names are given as published.

- **Action-Selector.** The agent picks from a fixed set of predefined actions and never feeds untrusted data back into planning. Cost: the work moves into designing the action set, and you lose most of the fuzzy capability you wanted the model for.
- **Plan-Then-Execute.** The plan is committed before untrusted data is read, so the data cannot change which tools get called. Cost: it does not protect the arguments, and it breaks when tool choice genuinely depends on what was read.
- **LLM Map-Reduce.** Isolated instances process individual items, with constrained outputs so an injection cannot cross into the reduce step. Cost: only works for tasks that decompose.
- **Dual LLM.** Described first by Simon Willison on 25 April 2023. A privileged model holds the tools and never sees untrusted text; a quarantined model reads untrusted text and has no tools; a plain software controller passes symbolic variable references between them so the privileged side handles names, never content. Cost: architectural complexity, and the quarantined model is still injectable, it just cannot do anything.
- **Code-Then-Execute.** The agent writes a program that calls tools and spawns unprivileged models, rather than reasoning over untrusted data directly. Cost: as with plan-then-execute, injected data can still shape the content of what gets written or sent.
- **Context-Minimization.** Unneeded context, notably the original user prompt, is dropped after action selection. Cost: the response cannot adapt to detail that did not survive the summary.

The strongest published instance is **CaMeL**, from Debenedetti, Shumailov, Fan, Hayes, Carlini, Fabian, Kern, Shi, Terzis and Tramèr, arXiv:2503.18813, March 2025. It extracts control flow and data flow from the trusted query so untrusted data can never affect program flow, then attaches capabilities to values and enforces a policy at every tool call. Its published AgentDojo figure is the number to quote when someone asks what security costs: 77% of tasks solved with provable security, against 84% undefended.

Alongside the pattern, apply the boring controls: a credential scoped per operation rather than per agent, the user's own delegated authorisation preserved across every agent hop rather than collapsing into a service identity, a deterministic policy engine between the tool and the downstream system that re-validates intent and arguments at execution time, and an egress allowlist naming the destinations any outward action may reach.

## Step 4. Design the confirmation so it is a control

A confirmation is a control only if a tired human can decide correctly in four seconds. That requires three things on screen:

1. **The concrete action.** Not "the agent wants to use a tool". The verb and the tool.
2. **The concrete target, resolved.** Not a variable, not a summary, not an identifier the human cannot evaluate. The actual recipient address, the actual file path, the actual amount.
3. **The data that will leave.** The exact bytes, rendered as they will be sent.

OWASP's 2026 guidance adds the reason the third item is not optional: surface the exact rendered action rather than a summary, because invisible-character smuggling can make the displayed action differ from the executed one. A confirmation dialogue that displays a paraphrase produced by the same model that was just injected is not a control at all.

**Gate on the action class, never on suspicion.** Suspicion-gating fails for the same reason filtering fails: the model deciding whether this looks like an attack is the model already under the attacker's influence, and a successful injection will simply also suppress the alarm. Classify by the row in the capability table and gate every row that is irreversible, spends money, or transmits outward, whether or not anything looks wrong.

**Then budget the confirmations, because volume destroys them.** OWASP states plainly that approval fatigue degrades reviewer judgement at volume. The design rule that follows: confirmed actions must be a small minority of tool calls in the routine path. If a design produces a confirmation on most steps, do not train the operator to click through, change the design. Convert actions out of the confirmed class by making them reversible instead, using the graduated enforcement that OWASP's LLM03:2026 chapter describes as audit, warn, block, escalate. Their published example is exactly the shape to copy: a refund issued as store credit is recoverable and auto-approves, while an external payout is irreversible and routes to a human.

## Step 5. The exfiltration channels that are not tools

These are missed because they never appear in a tool list.

- **Markdown image rendering.** The model emits an image whose URL encodes the private context, and the client fetches it. The user sees an image. This is the canonical indirect-injection exfiltration and it needs no tool call at all.
- **Links and link previews.** Same channel, one click or one automatic preview away.
- **Invisible Unicode.** Johann Rehberger demonstrated ASCII smuggling against Microsoft 365 Copilot in August 2024, exfiltrating a Slack multi-factor code inside text that rendered as nothing.
- **Terminal escape sequences.** Rehberger's *Terminal DiLLMa*, December 2024, showed model output hijacking a terminal through ANSI sequences.
- **Auto-triggered tool calls.** Output that a client parses and acts on without a turn boundary is an outward channel with no confirmation point.

Mitigations, in the order they are cheap: allowlist image and link hosts to destinations you control; strip the invisible ranges at render as well as at ingest; render untrusted-derived output as plain text rather than active markdown; disable automatic fetching of URLs that appear in output; and never let model output trigger a tool call without passing the policy engine. Note that classifier-based and redaction-based mitigations here have been bypassed in production: Aim Security's zero-click demonstration against Microsoft 365 Copilot, recorded in OWASP's 2026 chapter and tracked as CVE-2025-32711, defeated both the deployed prompt-injection classifier and the link-redaction filter.

## The decision rule

For any action the agent proposes:

- **Reversible, scoped to this session's own data, and sends nothing outward.** Run it. No confirmation.
- **Irreversible, spends money, or transmits data to a destination not fixed in advance.** Require confirmation showing the resolved target and the exact payload.
- **The session has consumed untrusted content and the action is not reversible.** Require confirmation regardless of the action class, and regardless of how ordinary the action looks.
- **The action writes to persistent memory or a shared corpus.** Treat it as privileged. Log the causing prompt, classify the write for instruction-bearing or role-modifying content, and require approval before anything instruction-shaped persists across sessions.
- **You cannot tell whether the content the session consumed was untrusted.** Treat the session as tainted. Taint is cheap to track forward and impossible to reconstruct afterwards, so the default when the provenance is unknown is tainted, not clean.

Track the taint flag per session, set it on the first untrusted read, and never clear it within a session. Clearing it requires a fresh context window, which is the same escape hatch the Rule of Two names.

## Worked example

A subscription billing service builds a support agent. It reads incoming tickets, searches a customer database, and sends email replies. Three tools, each proposed in a different sprint.

**Inventory.** Ticket bodies and subjects are semi-trusted, written by anyone with the support form URL, and reach context automatically on assignment. Attachments are untrusted. The customer database is trusted by origin but contains free-text fields customers wrote, so those rows are untrusted content sitting in a trusted store. Email replies render as HTML in the recipient's client.

**Capability table.** `search_customers`: reversible, blast radius tenant-wide because it connects with a shared reporting credential, not outward, service identity. `read_ticket`: reversible, session-scoped, not outward. `send_email`: irreversible the moment it leaves, blast radius unbounded because the recipient is an argument, outward, and it holds a verified sending domain.

**The combination.** Untrusted content in, private data access, outward channel. All three, in one agent, in one session. Under the Rule of Two this configuration cannot operate autonomously.

**Removing a leg.** Read-only is not an option because replying is the product. Blocking untrusted content is not an option because reading tickets is the product. So the outward leg gets constrained rather than removed.

**The design.** Plan-Then-Execute, with the plan fixed before any ticket body is read: retrieve, search, draft, queue. `send_email` is replaced by `queue_draft`, which is reversible and writes to a review queue. The shared reporting credential is replaced with a per-request credential scoped to the single customer record referenced by the ticket, so the blast radius drops from tenant-wide to one record. A deterministic policy engine sits in front of the queue and rejects any draft whose recipient is not the ticket's originating address, which is fixed before the untrusted content is read and therefore cannot be moved by it.

**The confirmation.** One per reply, showing the resolved recipient address, the subject, the full body as it will send, and a list of every customer field referenced. Not a summary, and not produced by the drafting model.

**Rendering.** Outbound HTML is stripped of images and of the invisible Unicode ranges. Links are allowlisted to the service's own domains.

**Verdict: it can ship, with the send tool removed.** The agent as originally specified cannot, because the recipient was a model-chosen argument on an irreversible outward action taken in a tainted session. The version that ships has a queue instead of a send, a per-record credential instead of a reporting credential, a recipient fixed by the policy engine rather than by the model, and one confirmation per reply rather than one per step.

## Failure modes

**Instruction Armour.** A system prompt says to ignore instructions found in retrieved content, and that sentence is entered on the risk register as a mitigation. It reduces casual attempts and holds against nothing adaptive. The signature is a design document where the injection control and the tone-of-voice control are in the same paragraph.

**Filter Faith.** A classifier sits in front of the context and its accuracy figure is quoted as coverage. The figure came from a static attack set. Attack success against filtered systems is measured properly only with the defence specification disclosed to the testers, and low-resource languages, alternative encodings and payload splitting across fields all degrade the classifier at once.

**Confirmation Fatigue.** The design is technically correct and produces a dialogue on most steps. Within a week the operator approves without reading, and the control has inverted: it now provides an audit trail showing a human approved the exfiltration.

**Trifecta By Accretion.** No single change was wrong. Retrieval was added in March, the customer lookup in May, the notification webhook in July, each reviewed and each reasonable. Nobody re-ran the three-way check after the third change, because there was no artifact holding the previous two.

**Tool Scope Creep.** A tool was added for one function and its library exposes six. A read tool connects with a credential that also has update and delete rights. A trialled tool was replaced but left registered. The model can call all of it.

**Rendering Channel Overlooked.** The tool list is defensible and the client renders markdown. The agent emits an image whose URL carries the context, and no tool was ever called, so no policy engine ever ran and no log records an outward action.

**Trust By Source.** An internal wiki, a production database or a first-party ticket store is treated as trusted because of where it lives. Customers, contractors and public forms all write into it. Provenance is about who typed the bytes, not which network the store sits on.

**Taint Amnesia.** Memory writes are treated as data rather than as privileged operations, so an injection persists a preference that survives into every later session, and the later sessions look clean because their own inputs were.

**Evaluation Absence.** The defence is never tested against anyone who has read it. Static results look excellent, the design ships, and the first adaptive attacker finds a defence that has only ever faced attacks it was designed for.

## Proving the boundary holds

Two kinds of test, and they are not interchangeable.

**Deterministic boundary tests** are the ones you write, and they are the ones that can pass. For each row in the capability table, assert that the action cannot execute without its gate: that a draft with a substituted recipient is rejected by the policy engine, that the scoped credential cannot read a second customer record, that stripped Unicode ranges are absent from rendered output, that a tainted session cannot reach an irreversible tool. These test your policy engine, not your model, so they are stable and they belong in continuous integration.

**Adversarial evaluation** is the one that only ever gives you a lower bound on your exposure. Baseline with AgentDojo, from Debenedetti, Zhang, Balunović, Beurer-Kellner, Fischer and Tramèr, arXiv:2406.13352, which ships 97 realistic user tasks and 629 security test cases, and with JailbreakBench from Chao and colleagues. Then red-team with your full defence specification disclosed to the testers, and refuse to accept any static-only attack success number, including your own.

## What this skill does not do

- It does not make prompt injection impossible. Nothing published as of August 2026 does, and the OWASP 2026 chapter says so directly.
- It does not cover jailbreaks aimed at content policy, model alignment, training-time attacks, or poisoning of the model weights themselves. Those are different entries on the list and different work.
- It cannot evaluate your specific system. It builds a design from the tool list you give it, and an incomplete list yields a design that is confidently wrong in the same place.
- It is not a red team. A tester with your defence specification and an afternoon will find more than this file contains, and the published evidence says that gap is large.
- It does not cover general application security or authorisation depth. Object-level authorisation, secret storage and network policy are inputs here, not outputs.
- Its recommendations cost capability. An agent built to this design does less, calls fewer tools, and sometimes needs two model calls where it had one. That is the trade being made, not a side effect of it.
