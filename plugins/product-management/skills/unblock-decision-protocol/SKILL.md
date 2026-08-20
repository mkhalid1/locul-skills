---
name: unblock-decision-protocol
description: Keeps long-running or unattended work moving by committing a recorded decision instead of stopping to ask. Gates a halt behind a four-way conjunction (irreversible AND high-stakes AND out of scope AND un-stubbable), stubs and defers every resource only a human can obtain such as credentials, keys, accounts and domains, and writes each decision into an append-only ledger as an executable instruction that later steps inherit and may not contradict. This skill should be used when an agent or a long build is blocked on a question the operator is not available to answer, when a run keeps stalling on missing access, or when the decisions made during a run need to be auditable afterwards.
---

# Unblock decision protocol

## The claim this skill is built on

A long autonomous run rarely fails because it made a bad decision. It fails because it stopped.

The arithmetic is one-sided. A wrong reversible decision costs one edit later. A halt costs every remaining hour of the run, plus the operator's re-entry cost, which is the expensive part, because they have to rebuild the context the run had and no longer holds.

The obvious approach, ask the human when uncertain, fails for a precise reason: uncertainty is the normal state of a long run. If uncertainty is the gate, the gate is permanently open. The result is a system that pauses on a missing key at 00:41 and hands over an empty branch and a polite question at breakfast.

The second-order failure is subtler and worse. A system that decides without recording contradicts itself. It picks snake case in one file and camel case in the next, calls the same object two names, chooses one date format at the start of a document and another at the end. From outside this looks like carelessness. It is actually the absence of a ledger.

This skill produces one artefact: an append-only decision ledger, and inside it a record per decision, each one written so the next step can act without deciding again.

## The ledger, defined before anything appends to it

Keep it as a plain file in the run's working directory, for example `decisions/ledger.md`, using forward slashes, which every mainstream toolchain accepts on both Windows and Unix-like systems. One entry per decision, newest at the bottom.

**Append only.** Never edit an existing entry. To change a decision, append a new entry that names the entry it supersedes and says why. This is not bureaucracy: it is the mechanism that stops a later step quietly reversing a choice that an earlier artefact already depends on. A ledger that can be rewritten cannot be trusted for exactly the failure it exists to catch.

Read the ledger before every decision. This is step two below, and it is the reason the ledger is defined first.

## The record: six fields, and the first one is not an opinion

Every commitment carries all six.

**1. DECISION, written as an executable instruction.** This is the load-bearing field. Test it this way: **could a different agent, with no access to this conversation, carry it out without asking a question?** If not, it is an opinion, and the next step will have to re-decide, sometimes differently.

The shape is an imperative verb, a named object, concrete values, and an explicit exclusion wherever a plausible alternative exists.

- Not a decision: "the pricing page should probably show three tiers".
- A decision: "Publish three tiers at 9, 29 and 79 per month, mark the middle one recommended, and do not add an annual toggle in this run."
- Not a decision: "we should handle the missing key somehow".
- A decision: "Read the key from `BILLING_API_KEY`. If it is unset, log a warning and disable the checkout button rather than throwing."

**2. CONFIDENCE.** High, medium or low. Low is allowed and is not a reason to stop. Low confidence plus reversible is a decision; it is only the combination with irreversibility that matters.

**3. WHY**, two to three sentences tied to the objective as stated, not to general good practice. "Because it is cleaner" is not a reason. "Because the objective says ship by Friday and this path has no migration" is.

**4. ASSUMPTIONS**, everything inferred rather than read, listed so the operator can sanity check it later. An empty assumptions field on an inferred decision is the single biggest obstacle to auditing a run afterwards.

**5. REVERSIBILITY.** Reversible, reversible with cost, or irreversible, plus the undo path in one clause. **If you cannot name the undo path, record it as irreversible.**

**6. CONSULTED.** The roles asked, or none.

## Step 1. Restate the objective, the block, and what is already decided

Take the objective verbatim. State the blocking question in one sentence. List the decisions already made that touch it.

If any of those is missing, infer it from the objective and put the inference in the assumptions field. **Never refuse for lack of input, and never ask a question back in place of deciding.** A question returned to an absent operator is a halt wearing a helpful tone.

## Step 2. Read standing preferences before inventing one

Preferences have a precedence order, and a lower source may never overrule a higher one:

1. An explicit instruction in the objective.
2. The run's own ledger. **Never contradict a prior decision in the same run.**
3. Conventions checked into the work itself: a conventions file, the existing code style, the existing schema, the naming already used.
4. House or project-wide conventions.
5. The default ladder in step 3.

The rule for reading conventions out of existing work: **two occurrences is a convention, one is a precedent worth following, zero sends you to the ladder.** This matters most for the small choices that make an artefact look incoherent when they are made twice: date formats, casing, directory layout, quote style, the words used for the same concept.

## Step 3. The default ladder

When information is genuinely missing, apply these in order and stop at the first rung that discriminates.

1. **Prefer the reversible option**, even when the irreversible one looks better.
2. **Prefer lower cost**: money, time to a working version, and new dependencies. A new dependency is a cost paid forever by everyone who touches the project afterwards.
3. **Prefer convention over novelty**: the boring choice a stranger will recognise.
4. **Prefer shipping over polishing**: the version that works end to end over the version that is better in one place and unfinished.
5. **Prefer the choice you would not regret explaining** with the reasoning attached.

The order is the content. Reversibility outranks cost because a cheap irreversible mistake can still cost the whole run. Cost outranks convention because an expensive conventional choice can sink a run that a cheap unconventional one would have finished. Convention outranks shipping because the shortcut nobody recognises is paid for later by somebody else. The last rung is a tie-break rather than a criterion, because it is the only subjective one and putting it higher would let taste overrule arithmetic.

## Step 4. The role panel, and when not to convene it

Convene a review panel **only** for calls that materially move revenue, pricing, positioning or buyer perception. Everything else goes straight to the ladder.

The panel is made of **roles defined by expertise**, never of people. Pick two to four from: a pricing and packaging lens, a positioning and messaging lens, a conversion and funnel lens, a technical feasibility lens, a legal and compliance lens, an operations and support lens. Dispatch them in parallel with the same brief. Each returns a recommendation and **the one fact that would change its mind**, which is the part worth having, because it converts a disagreement into something checkable.

Then resolve the conflict yourself, using the ladder, and commit. **The panel advises. The protocol decides.** A panel that ends in a summary of views has produced nothing, because the run still cannot proceed.

Budget rule: a panel is worth roughly what being wrong would cost. Convening three roles for a button label is not thoroughness, it is spending the budget the pricing decision needed an hour later.

## Step 5. Stub and defer every resource only a human can obtain

**Missing credentials, keys, OAuth clients, paid accounts, domains, DNS records, payment details, signing certificates and real customer contacts are never a halt.**

The procedure:

1. Write the code as though the resource exists.
2. Reference it through a named environment variable.
3. Put an obviously fake placeholder in the example environment file, with a comment naming where the real value comes from.
4. Record a **deferred blocker**: the exact variable name, what it is for, where the operator obtains it, what stops working until it exists, and which file holds the placeholder.
5. Continue.
6. Surface every deferred item together at the end as one handoff list, ordered by how much each one blocks.

**The placeholder convention, which has to behave identically on Windows and on Unix-like systems.** Use uppercase names with underscores and ASCII only, because Windows treats environment variable names case-insensitively and POSIX shells treat them case-sensitively, and uppercase snake case is the only convention that means the same thing on both. Keep the real values in a `.env` style file the application reads, rather than in shell instructions, so nothing depends on which shell is running. Never write a value that looks real: `REPLACE_ME_BILLING_API_KEY` costs nothing to spot, while a plausible-looking fake key costs an hour of somebody's debugging. Read the variable through the language's environment interface rather than by shell interpolation, and build every file path with the language's path helper rather than by joining strings with a slash. Where a command genuinely has to appear in documentation, give both forms and label them: `export NAME=value` for POSIX shells, and `$Env:NAME = "value"` for the current PowerShell session or `setx NAME "value"` to persist it. Assuming one platform's syntax is the most common way a handoff list becomes unusable for half its readers.

## Step 6. The halt gate is a four-way conjunction

Halt only when **all four** are true:

- **Irreversible.** There is no undo path you can execute. Not merely tedious to undo.
- **High-stakes.** Being wrong costs real money, real data, a real relationship, or public reputation. Not "would be embarrassing in review".
- **Out of scope.** The stated objective did not ask for this action. If the objective says publish the article, publishing the article is in scope.
- **Un-stubbable.** The run cannot proceed with a placeholder, a dry run, a staging target, a draft, or a deferred item.

**Why the conjunction is the whole point.** Each condition alone is ordinary. Irreversibility alone describes every push, every deletion, every sent message. High stakes alone describes most product decisions worth making. Out of scope alone happens the moment an adjacent improvement suggests itself. Un-stubbable alone is rare and, in the case people invoke it most, missing credentials, it is simply false, since a credential is the most stubbable thing in software. A system gated on any single condition therefore halts several times a night. Requiring all four converts nearly every apparent blocker into a decision plus a deferred item, and leaves a small residue of genuine halts.

Evaluate it as a boolean and write the four answers into the record whenever the answer is close. **If three are true and one is false, name the false one and continue.**

| Blocking moment | Irrev. | High stakes | Out of scope | Un-stubbable | Verdict |
|---|---|---|---|---|---|
| Missing payment provider key | no | no | no | no | Decide, stub, defer |
| Choosing between two libraries | no | no | no | no | Decide by the ladder |
| Buying the intended domain name | yes | yes | yes | **no** | Deploy to the preview host, write the custom-domain config, defer |
| Sending an announcement to a real list | yes | yes | yes | **no** | Draft it, do not send, defer |
| Deleting the live deployment in the only slot | yes | yes | yes | yes | **Halt** |
| Rotating a shared credential others depend on | yes | yes | yes | yes | **Halt** |

**The branch for when you cannot tell.** If you cannot determine reversibility, treat the action as irreversible, then ask whether it can be stubbed. Almost always it can, and the stub is the answer. If you cannot tell whether it is in scope, the objective's literal wording decides, and silence means out of scope, which again points at stubbing rather than acting. So the cannot-tell branch resolves to stub and defer. It never resolves to a halt, and it never resolves to performing the risky action to find out.

## Step 7. When the work stalls, decide among alternatives rather than escalating

Define stalled: three iterations with no measurable progress, where progress means a new artefact, a test moving from failing to passing, or an error changing.

Never repeat an identical command more than twice. **The third attempt must differ in kind**, not in detail.

On a stall, write down what was attempted with the exact errors, list the blockers, generate three alternatives that differ in approach rather than in parameters, and **decide among them with the ladder**. Record it and continue. If the chosen alternative also fails, the objective is the wrong shape: record a scope reduction, deliver the largest subset that works, and put the remainder on the handoff list. Reducing scope in writing is a decision. Waiting is not.

## Worked example, compressed

**Objective, given at 22:00 to an unattended run:** "Build and deploy the marketing site for a new note-taking product, with a working email capture form."

1. **No email service credentials.** Not irreversible, not un-stubbable. The form posts to a handler reading `NEWSLETTER_API_KEY`; the placeholder goes into the example environment file; deferred blocker recorded. Decision written as: "Read the newsletter key from `NEWSLETTER_API_KEY`; when unset, accept the submission, log it locally and show the success state."
2. **Which email service.** Reversible, so the ladder applies. Rung 3 does not discriminate, since the repository uses none. Rung 2 does: choose the one with a plain HTTP interface so no dependency is added. Recorded with the exclusion "do not add a client library in this run".
3. **Pricing numbers on the page.** Materially moves buyer perception, so a three-role panel is convened: pricing and packaging, positioning, conversion. Two of the three want a free tier and one wants a trial. The protocol resolves by rung 1, reversibility, since page copy is trivially editable: publish three tiers with the middle one marked recommended, record the disagreement in assumptions for the operator to sanity check.
4. **The intended domain is not registered.** Irreversible yes, high-stakes yes, out of scope yes, un-stubbable **no**. Three of four. Deploy to the platform's preview host, write the custom-domain configuration with the intended name, record the deferred blocker.
5. **An announcement to the existing subscriber list would help the launch.** Irreversible yes, high-stakes yes, out of scope yes, un-stubbable **no**, because a draft is a perfectly good stub. Three of four. Draft it, do not send, defer.
6. **The deploy target already hosts a live site and the plan has one slot.** Proceeding means deleting it. Irreversible yes, high-stakes yes, out of scope yes, un-stubbable yes, because there is no second environment configured. **All four. Halt**, with everything else finished, one specific question, and the two options that follow from either answer.

**Verdict:** six blocking moments, five carried by decisions and three deferred blockers, one genuine halt reached at 01:12 with the site built, deployed to a preview host and waiting on a single yes or no. A single-condition gate would have halted at moment one, four hours earlier, with nothing built at all.

## Failure modes

**The false halt on a missing key.** The log ends politely at 00:41 and the morning shows an empty branch. This is the most common failure by a wide margin, and the ironic part is that credentials are the most stubbable resource that exists.

**The question a stated default already answered.** The operator replies by pasting a line from the instructions file the run was given. Every occurrence is a step 2 that was skipped.

**Self-contradiction across the run.** The artefact uses two names for the same thing, and the switch happens exactly where a later step decided again. From outside it reads as sloppiness. It is an unread ledger.

**The opinion filed as a decision.** The ledger says "we should probably", and the next step opens by re-deciding the same question, occasionally the other way. The visible symptom is the same question appearing twice in one log with two answers.

**The panel convened for a button label.** Three role outputs on a trivial call, and the budget is gone before the pricing question arrives. Thoroughness spent in the wrong place looks identical to thoroughness from the inside.

**Silent assumptions.** The deliverable is wrong in a way nobody can trace, because the assumptions field is empty everywhere and there is nothing to check against.

**The retry loop.** The same command with the same error fourteen times, timestamps two seconds apart. Attempt three onwards should have differed in kind.

**The reconstructed ledger.** The ledger has exactly as many entries as there were open questions, and every timestamp sits inside the last minute of the run, because it was written at the end rather than appended along the way. It reads as an audit trail and is a summary.

**The plausible fake value.** A committed placeholder that looks like a real key, and an operator who spends an hour debugging an authentication failure against a value that was invented.

## What this skill does not do

- It does not make decisions correct. It makes them recorded, bounded and reversible where possible, so a wrong one is cheap and traceable.
- It does not enforce anything. A permission allow list or a sandbox in the harness is what actually prevents a destructive action, and it should be configured whether or not this is in use.
- It does not judge the objective. Given a badly framed brief it will move that brief forward efficiently, which is worse than stalling.
- It does not replace the panel's job on genuinely hard questions. Where the disagreement between lenses is itself the deliverable, run a structured debate instead and bring the answer back here as an input.
- It does not work without a persistent file. Without a ledger the run inherits nothing between steps, and contradiction protection disappears.
- It is not calibrated for regulated or safety-critical domains, where reversible is not a synonym for safe and where the correct configuration is a narrower scope rather than a faster one.
