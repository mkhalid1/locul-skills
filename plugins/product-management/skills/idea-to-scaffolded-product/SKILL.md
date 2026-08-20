---
name: idea-to-scaffolded-product
description: Takes a raw product idea through a fixed twelve-phase pipeline to a scaffolded, launch-ready product: validation, MVP scope, keyword portfolio, positioning, competitor intelligence, provisioning, publishing infrastructure, a generated publishing skill, a content backlog, an unattended job prompt, a design brief and a launch checklist. Pauses at exactly two approval gates, asks permission exactly once at the live deploy, blocks rather than guessing whenever a real identifier is missing, and keeps a status file rewritten in full on every state change. This skill should be used when an idea has been decided on and the work in front of you is the scaffolding rather than the product.
---

# Idea to scaffolded product

## The claim this skill is built on

An idea rarely dies of being wrong. It dies of the twenty setup tasks between it and existing.

The obvious approach fails at both ends. Asked to help plan a product, a model produces a plan, and the plan is the output: a document about work rather than the work. Asked to run the setup, it produces the opposite failure, a conversation. It completes a step, describes it, asks whether to continue, and waits. Twelve phases become twelve interruptions, each landing in a different gap in somebody's day, and the run dies at phase four on a Tuesday afternoon.

This skill exists to make the middle possible: a run that proceeds on its own, stops only where a wrong answer is expensive to unwind, refuses to invent the values it does not have, and stays legible from outside the conversation the whole time.

Two design decisions carry all of that, and they are the content of this file. **Gates only where reversal is costly.** **A status file that behaves as if it were live.**

## The autonomy model

**Two approval gates, and only two.**

- **Gate 1, phase 2, the MVP feature list.** Everything after it is derived from it. The keyword portfolio, the positioning, the competitor set, the backlog, the design brief and the launch checklist all inherit its scope. A wrong feature list is not one wrong artefact, it is nine.
- **Gate 2, phase 9, the first ten rows of the content backlog.** The first ten reveal the type mix, the demand assumption and the voice. Presenting all thirty is not a review, it is a rubber stamp, because nobody reads thirty rows of anything at the end of a working day. Ten is the largest sample a person actually reads.

**Everything else proceeds.** A completed phase announces what it produced and where, in one or two lines, and continues. Announcing is not asking.

**One permission step, at the very end: the live deploy in phase 12.** Keep this separate in your head from the gates, because it is a different question. An approval gate asks "is this right", and a wrong answer costs rework. A permission step asks "may I do this to the world", and a wrong answer costs a public artefact you cannot fully retract. Everything reversible proceeds. The one irreversible act asks.

**Why not more.** Each gate costs a context switch and a wait. A pipeline with eight gates advances one phase per gap between meetings and takes a week, which is long enough for the owner to lose the thread. The rule for adding one: **a step is a gate only if the answer changes at least three later phases and reversing it would cost more than an hour.** By that rule this pipeline has two.

**Why not fewer.** Zero gates produces a fully formed product built on a scope nobody agreed to, discovered at the design brief when the screens describe features the owner never wanted. Gate 1 cannot be removed.

## The status file, rewritten in full on every state change

A checklist that lives in the conversation is invisible to everyone not in the conversation, and it disappears when the session ends. So the pipeline keeps one file, at a declared path such as `docs/build/status.md`, using forward slashes, which every operating system's tooling accepts.

**The technique: on every state change, rewrite the whole file. Never patch a line.** Three reasons, all mechanical.

1. **Appending produces a log.** A log is read from the top, so the current state ends up at the bottom, where nobody looks. A full rewrite puts the current state at the top by construction.
2. **A partial update lets two lines disagree.** Entering phase 6 changes the phase's own state, the single next-action line at the top, and possibly the blocked banner. Update one and leave the others and you have a file whose lines contradict each other, which is worse than having no file, because a status file is trusted by whoever reads it.
3. **A full rewrite is idempotent.** It can be produced from the pipeline's own state without reading what was there, so a run that crashed and resumed makes the file correct again on its first write rather than inheriting whatever half-state was left behind.

**Five states, and no others: pending, doing, done, blocked, skipped.** The rules that make them mean something:

- **Exactly one phase in `doing` at any moment.** Two means the pipeline has lost track of itself, and that is a stop condition, not a note.
- **`blocked` fills a banner at the top of the file with the exact ask**, phrased so somebody who has not read the conversation can act on it. Not "waiting for access". Instead: "Blocked: needs the workspace identifier from the content system, shown on the settings screen once the site has been created. Paste it back here."
- **`skipped` carries a one-line reason.** A skipped item with no reason is indistinguishable from a forgotten one, and at the end of a run nobody can tell which it was.
- **Every rewrite updates the timestamp and the single next action.** One next action, not a list. A list is a plan; the point of this line is that a person glancing at the file knows what is happening right now.

## The twelve phases

They group into four blocks, which is the only way to hold them in your head: **decide** (1 to 2), **position** (3 to 5), **build the machine** (6 to 8), **feed and launch** (9 to 12).

### Phase 1. Validation

Delegate the assessment, then present **only the synthesis**: three to five findings and the single thing most likely to kill the idea. Ask exactly one question, which is whether this matches the owner's own thinking, and lock the resulting brief.

Handing over the full assessment converts the question into homework, and homework gets a yes without being read.

### Phase 2. MVP scope. Approval gate.

Numbered features. Each one a bold title and at most two to three lines of prose. **No nested bullets.** Five to eight features total.

The three-line limit is not a formatting preference, it is a **comprehension test**. Nested bullets are where an unresolved feature hides, because the sub-bullets are almost always the questions you have not answered, written as though they were answers. If a feature needs six sub-bullets, you do not have a feature, you have a category, and a category will be estimated as one thing and built as five.

The check: read the feature aloud to somebody who does not know the product and ask them to describe the screen. If they cannot, split it or cut it.

The locked list is then quoted verbatim into every later phase, rather than paraphrased, so scope cannot drift by restatement.

### Phase 3. Keyword portfolio. No gate.

Produce the file, announce its path and row count, continue. Group by demand band rather than reporting precise volumes you cannot stand behind: a false decimal gets treated as evidence, a band gets treated as the estimate it is.

### Phase 4. Answer-engine positioning. No gate.

Write the sentences you want quoted back when somebody asks an assistant for a tool like this: one definition sentence, one comparison sentence, and three sentences naming who it is for. **Each must stand alone without the paragraph around it**, because a retrieval system lifts a sentence, not a page. A sentence beginning "it also" is unusable and will be quoted anyway.

### Phase 5. Competitor intelligence. No gate.

Order matters here and it is the reverse of what most people do. **Data first:** who ranks, how their pages are structured, what they rank for, how often they are cited by assistants and in what tone, and the complaints their users leave in public forums. All of that is cheap, repeatable and needs nobody.

**Only then the visually locked residue**, delegated as one block: the hero headline, the call to action wording, the pricing tiers, and the most common complaint in reviews. That material lives inside a rendered page and needs eyes on it. Doing it first spends a person's attention on things a query would have answered.

### Phase 6. Provisioning. Hard dependency. Never invent an identifier.

This is the loudest rule in the pipeline, and it is the one worth remembering if you remember nothing else.

**A wrong identifier does not raise an error.** It succeeds against the wrong target, or it succeeds against nothing and reports success. Every downstream phase inherits it: the publishing configuration, the scheduled job, the sitemap, the analytics property, the share images. A wrong site or workspace identifier discovered in phase 12 costs the whole of block C.

So an identifier enters the pipeline in exactly one of two ways: it is **returned by the call that created the resource**, or it is **read back from the live account**. Never derived from the product name. Never a plausible-looking string of the right shape. Never the one from the previous project, which is the most dangerous variant of all, because it is real and it points somewhere that already exists.

**The echo test**, one extra call, worth it every time: after obtaining an identifier, read the resource back by that identifier and confirm the name matches the product you are building. This catches transposition and it catches stale reuse.

If no identifier is available, the phase goes to `blocked`, the banner is written with the exact ask, and the pipeline waits. **Waiting is correct behaviour, not failure.**

### Phase 7. Publishing infrastructure. Read the reference before writing.

Read the reference implementation files. Do not write configuration from memory.

Configuration formats drift and keys get renamed, and a key renamed two releases ago is exactly the detail that gets reproduced confidently and wrongly from memory. The file on disk is current by definition. Memory is an average of every version that ever existed.

Then run a written self-check before committing anything: every referenced path exists, every key appears in the reference, the job runs once locally, secrets are referenced rather than embedded, and nothing generated has been committed that should have been ignored.

### Phase 8. Generate the product's own publishing skill

The pipeline produces a child skill that runs the product's ongoing content job. Generating it is a split, not a copy, and the split is the part that matters.

**Preserve verbatim:** pre-flight checks, failure handling, retry and idempotency rules, the strict summary formats, and every clause of the form "do not proceed if X".

**Write fresh:** article structure, voice rules, banned words, closing copy, the audience description, the positioning gate.

**Why the preserved sections are preserved.** They are scar tissue. Each clause exists because a run failed in that exact way once, usually unattended, usually at an hour nobody wants to repeat, and the clause is not derivable from first principles. Paraphrasing loses the specificity that made it work. "Check the item does not already exist before creating it" survives a rewrite. "Check by the exact slug, not the title, because the title gets translated" does not, and the day it is lost you get duplicates in every language but the first.

**The test for which pile a line belongs in:** if you can name the specific failure the line prevents, preserve the wording. If you cannot, it is generic advice and belongs in the fresh half, where it should be rewritten for this product rather than inherited.

There is a second reason to write the fresh half fresh: copying voice rules between products is how a portfolio ends up sounding like one company with several logos.

### Phase 9. Content backlog. Approval gate on the first ten rows.

Thirty articles, in a fixed type mix: **4 pillar, 8 how-to, 6 comparison, 6 use-case, 4 question-cluster, 2 round-up.** Sorted pillars first, then by demand band.

The mix is fixed because a backlog left alone drifts towards whatever is easiest to write, which is how-to. Thirty how-tos give you nothing that can rank for the category term and nothing that catches comparison intent, the intent closest to a decision, and the drift is invisible until six months of publishing have happened.

### Phase 10. The scheduled job prompt. Fully self-contained.

The prompt runs unattended. There is nobody to ask.

**The rule: no pronoun, demonstrative or definite reference may point at something that exists only in this conversation.** Ban, specifically: "it", "that file", "the site", "the same folder", "as discussed", "as above", "the usual format". Replace each with the literal path, the literal identifier, the literal format block.

**The test procedure:** read the prompt as though in a fresh session with no history and check that every noun resolves. Anything that does not is rewritten, not clarified.

The failure this prevents looks like this from the outside: the first scheduled run produces nothing, or writes to the wrong place, and the log contains no error, because the prompt was valid, fluent English that simply did not refer to anything.

### Phase 11. Design brief

Product overview. Target user. Four to six core screens, each with three to five required elements. Three adjectives for how it should feel and **three for what it must not feel like**. One directional reference sentence. The constraints: platforms, existing component library, accessibility floor.

The negative adjectives carry most of the information. Positive ones get agreed with and forgotten. "Not playful, not corporate, not dense" removes more of the possibility space than any three positive words, because each is a boundary somebody would otherwise have crossed in good faith.

### Phase 12. Launch checklist

In dependency order: repository, containerisation, continuous integration, domain and certificates, **the sending domain kept separate from the human inbox domain**, analytics, search registration, sitemap, robots, canonical tags, alternate-language tags, structured data, share images, the 404 page, then a post-deploy smoke test.

The sending domain rule is skipped most and costs most later. Bulk sending and human correspondence are two different reputation systems, and mixing them means one bad send damages the deliverability of the address people reply to, discovered weeks later as silence.

**Exactly one step in this phase asks permission: the live deploy.** Everything else proceeds.

### Completion block

End the run with three lists: every file created and its path, every delegated task still outstanding, and the single most important next action. One next action, not a summary of everything.

## Decision rule: proceed, delegate, or block

- **If the step can be completed with the tools and access in hand:** proceed. Announce the artefact and its path in one line. Do not ask.
- **If the step needs a browser, a human decision about money or terms, or a credential you do not hold:** emit a delegation block, set the phase to `blocked`, write the banner, and wait. The delegation block names the exact action, where to perform it, and what to bring back, in a form somebody can follow without reading the conversation.
- **If you cannot tell whether you have the access:** do not find out by doing. Read first. List the resource, fetch the account, check whether the key is present. A read that fails is cheap and tells you exactly what is missing. A write that half succeeds against the wrong target is the single most expensive outcome available in this pipeline, because it produces a real identifier for the wrong thing, and every later phase inherits it silently.

## Worked example, compressed

**Idea:** a shift-swapping tool for small retail teams.

**Kickoff, two questions:** who may swap with whom, and whether a manager approves the swap. Then the pipeline starts.

**Phase 1.** Assessment delegated. Synthesis presented as four findings and one killer risk: a two-sided cold start inside every shop, since a swap needs a taker. The owner answers yes and the brief is locked.

**Phase 2, gate.** Six features drafted. One of them, "notifications", fails the three-line test: it cannot be described without four sub-bullets covering channel, timing, opt-out and digest. It is split. "Shift offer alert" survives as three lines and stays in the MVP. The rest moves out of scope. The owner approves the six.

**Phases 3 to 5.** Keyword file, 140 rows across three demand bands. Five positioning sentences, each readable alone. The competitor pass finds four ranking products from data; the visually locked residue is delegated as one block and comes back inside a quarter of an hour.

**Phase 6.** Blocked for forty minutes. The banner names exactly where the workspace identifier is found. The pipeline does not guess. On receipt, the echo test reads the workspace back and the name matches.

**Phase 7.** Reference implementation read first. One configuration key had been renamed since the version in memory, and the file on disk carried the current name.

**Phase 8.** Six sections preserved verbatim, five written fresh. One clause was nearly moved to the fresh pile, then kept once its failure could be named: it prevents duplicate publishing when a translation shares a slug.

**Phase 9, gate.** First ten rows approved with one change, a comparison row retargeted at a competitor found in phase 5 rather than the one assumed in phase 3.

**Phase 10.** The prompt fails its own self-containment check twice. "Publish to the same site" and "use the usual summary format" both resolve only inside the conversation, and are replaced by the literal identifier and the literal format block.

**Phases 11 and 12.** Design brief written with three negative adjectives. Checklist worked in order. One permission request, at the live deploy, granted.

**Verdict:** twelve phases, two approval gates, one permission request, one forty-minute block carrying a named ask, and nineteen files at declared paths. Two errors were caught by rules rather than by review: a feature that read well in prose and failed the three-line test, and a scheduled prompt that was fluent English and not self-contained. Nothing was invented. The one identifier the pipeline needed came from the account, and until it did, the run stayed blocked.

## Failure modes

**The invented identifier.** Everything runs green and nothing appears where it should. The worse version is that something appears somewhere else, in a workspace belonging to a different project, and is found weeks later.

**Writing from memory instead of reading the reference file.** The configuration looks right and one key is from an older version. The symptom arrives at the first unattended run, as a message about an unknown field, with nobody watching.

**The context-dependent scheduled prompt.** It works perfectly when tested inside the conversation that produced it, and returns nothing on its first real run. The tell is a log with no error in it.

**The stale status page.** The conversation is at phase 9 and the file says phase 4, so anyone checking on progress reports the project as stalled. The visible tell is a file timestamp older than the artefacts the file should already be listing.

**Gate skipping.** Phases 3 to 12 get built on a scope the owner never saw. It surfaces at the design brief, where the screens describe features nobody agreed to, and by then eight artefacts carry the error.

**Improvising a human step.** A plausible account name, a placeholder key, a configuration pointing at a resource nobody created. This is the failure that looks most like success at the moment it happens, which is why it needs a rule rather than vigilance.

**Bundling shared infrastructure per product.** The fourth product gets its own copy of the analytics pipeline, its own database, its own bill, and a change to shared logic has to be made four times. It looks like independence and is four maintenance surfaces.

**Gate inflation.** The run spans four days, two of which are waiting. The pipeline is technically correct and practically abandoned, and the owner finishes the setup by hand.

## What this skill does not do

- It does not validate demand. Phase 1 synthesises an assessment and talks to no customers, so a completed pipeline can produce a well organised product nobody wants.
- It does not touch a browser, a consent screen, a payment form or a set of terms. Those become delegation blocks and the run waits for a person.
- It does not write the product. It scaffolds the surround: the repository, the content system, the publishing job, the briefs and the checklist.
- It does not design. The design brief is a brief, and a designer or a design tool turns it into screens.
- It does not operate anything afterwards. Once phase 12 finishes, the scheduled job runs on its own and nothing here is watching it.
