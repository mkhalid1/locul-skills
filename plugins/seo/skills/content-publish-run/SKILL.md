---
name: content-publish-run
description: Runs a repeatable content publishing job end to end across multiple locales: selecting work from a backlog tracker, reconciling it against the live publishing system before writing anything, publishing and verifying by read-back, adding reciprocal internal links, translating into every target locale under the parent slug, writing results back to the tracker, and emitting one strict run summary. Enforces backfill-first ordering across runs, a denylist of invalid reasons for skipping quota, and the slug and URL-shape rules that silently break translated pages. This skill should be used when running or building a scheduled multi-locale publishing job, when a previous run ended partway through, or when duplicate articles keep appearing because a local backlog was trusted over the live site.
---

# Content publish run

## The claim this skill is built on

The obvious shape of a publishing job is: read the backlog, write the next few articles, publish them, mark them done. It works for about a month.

Then two things go wrong, and neither of them is a writing problem.

The first is drift. The backlog is a plan. The live site is a fact. They are maintained by different processes, they diverge quietly, and the moment a run trusts the plan over the fact it publishes something that already exists. A duplicate is much worse than a miss. A missed article costs you one article. A duplicate costs you two competing pages on the same keyword, an internal link graph split across both, and a later argument about which one to delete and where to point the redirect.

The second is quota erosion. A run that is partly executed by a model will, under pressure, find a reasonable-sounding reason to do three of four locales, or one of two articles, and then write a summary that reads as a success. No individual run looks wrong. Four runs later the backlog is a month behind and nobody can point at the moment it happened.

So the two load-bearing ideas here are an explicit authority split, and a denylist. Everything else is ordering, and the ordering has reasons.

## Phase 0: pre-flight in one batch, and fail loudly

Before reading a single file, enumerate every capability and every file the run will need, phase by phase, to the end. Load them in **one** batch call rather than one at a time, because each round trip is latency you pay again on every run, and because a capability discovered missing at phase six has already wasted the write.

Report which loaded and which did not, by name.

If a **required** capability is missing, do not start a degraded run. Report `could not start`, name the missing thing, and stop. The failure mode this prevents is a run that publishes the article and then discovers it cannot translate, leaving a permanent asymmetry in the corpus that nobody goes back to fix.

Liveness-check anything that **generates** assets before you depend on it. Text generation is cheap to redo. An image or diagram pipeline that fails at the asset step, after the article body exists and before the publish, is the single most expensive place in the run to discover a problem. One trivial generation request at pre-flight costs seconds and de-risks the whole run.

## Phase 1: read context in a fixed order

Four reads, in this order, and the order is not cosmetic:

1. **The backlog tracker.** What is outstanding, at what priority, in what state.
2. **The writing guidelines.** How anything you produce must be shaped.
3. **The cluster topology.** Which hubs exist, which articles belong to which cluster, what links to what.
4. **The source of product or subject facts.** The only place claims may come from.

You cannot select work before you know what is outstanding. You cannot judge whether a candidate duplicates an existing cluster member before you have the topology. You cannot write a factual claim before you have the fact source, and a run that reads it last has already written the article.

An unreadable **required** file aborts the run, naming the file. Do not proceed with whatever loaded. A run missing the fact source will invent facts, confidently, and publish them.

## Phase 2: select work, backfills first

**No date gating.** Do not select rows by a scheduled date. A run that misses a day leaves a permanent hole in a date-gated backlog, and clocks, time zones and daylight saving will disagree with your tracker at least twice a year.

Selection is:

- Eligible rows are those whose status is neither `published` nor `skipped`.
- Sort by priority ascending, then by row identifier ascending. Both keys, always, so two runs starting from the same state select the same rows. A single-key sort with ties is non-deterministic and makes any failure unreproducible.
- Cap at N per run.

**Backfills owed from the previous run execute first, and count against N.** This is the rule that stops debt compounding. A half-finished row is worth more than a fresh one because most of its cost is already sunk: the research is done, the body exists, the assets are uploaded, and what remains is the cheap tail. Starting new work while a previous row sits without translations is how a corpus ends up with a locale coverage hole that nobody discovers for six months.

The backfill states, in the order they are owed:

1. Published in the source language, zero reciprocal inbound links.
2. Published in the source language, one or more target locales missing.
3. Written but publish never verified by read-back.
4. Everything done but the tracker row never written back or never committed.

## Phase 3: reconcile before writing

This is the load-bearing rule of the whole procedure.

**The authority split, stated explicitly:**

- **The publishing system is the authority on what exists.** It is the only artefact a reader can reach. If it says an article is live, the article is live, whatever any local file believes.
- **The tracker is the authority on what is planned.** Priority, intent, target keyword, cluster membership, ownership. It is a to-do list.
- **Neither is an authority on the other's question.** A tracker row marked `published` is not evidence that a page exists. A live page absent from the tracker is not evidence that it was unplanned.

Before writing any selected row, query the publishing system three ways: by focus keyword, by title, and by the candidate slug. Three queries because a match on any one is a match, and search implementations differ in what they index. A title search that misses because the live title was edited after publication will not miss on the slug.

Then apply the outcomes:

- **Exists, tracker says pending.** The tracker is stale. Mark the row `published`, backfill the live URL, and **do not rewrite the article**. Move to the next candidate. This row does not consume quota, because no writing happened.
- **Exists, tracker says published, URL field empty.** Backfill the URL. Nothing else.
- **Does not exist, tracker says published.** The opposite drift. Do not blindly republish. Go to the decision rule below.
- **Does not exist, tracker says pending.** Proceed to the per-row gates.

The asymmetry is deliberate. When in doubt, the run publishes nothing and reports, because publishing nothing is recoverable on the next run and publishing a duplicate is not.

## Phase 4: per-row gates

Three gates, each with a defined outcome rather than a judgement call.

**The parent hub resolves.** The article must have somewhere to belong. Before declaring a hub missing, **re-resolve the slug**. A single 404 on a URL you constructed by concatenating strings is not evidence that the hub does not exist. Hubs get renamed, get a section prefix, change case. Query the hub index by name, and only then conclude.

**No duplicate focus keyword.** Against the live corpus, not the tracker. This is the same reconcile query with a different question, and it catches the case where a different row already covers this keyword under a different title.

**The topic is still current.** A backlog row written eight months ago may refer to a version, a price, an interface or a policy that no longer exists. If the fact source contradicts the row's premise, the row is not written, it is recorded as `blocked: stale-premise` and returned to whoever owns the backlog.

## Phase 5: write to the locked structure, and verify assets before publishing

Write to the structure the guidelines specify. Then, **before the publish call**:

- The exact required count of real asset references is present. A reference to an asset identifier that was never actually uploaded is not a reference, it is a broken image on a live page.
- Every asset has alternative text.
- The lead asset is set, not merely present in the body.
- Every internal link is in **the markup the destination actually renders**. Many front ends escape raw HTML in a body field, which means an anchor tag written as HTML into a markdown field ships as literal text on the live page. Check the field's rendering mode once, per system, and record it.

Asset verification happens before publishing rather than after because a published page with a missing image is live and wrong, and repairing it needs a second edit that some systems do not support cleanly on an already-published record.

## Phase 6: publish, then read back

A publish call that returns success is a claim. It is not proof.

Read the record back by its identifier, and fetch the public URL. Both. The identifier read-back catches a record created in draft state and never promoted. The URL fetch catches a record that exists but is not routed, which is exactly what the URL shape error in phase 8 produces.

Only after both reads succeed does the row count as live, and only then may its URL be written anywhere.

## Phase 7: reciprocal inbound links, immediately, before translations

Run the reciprocal linking pass **immediately after the source-language publish is verified**, not after translations.

The reason is interruption. Translation is the longest phase of the run and the one most likely to be cut short. If the run dies during translation, a row that already carries inbound links is a complete, useful, discoverable page in one language. A row that has translations but no inbound links is an orphan in every language at once, which is strictly worse. Ordering the cheap, high-value step before the long, fragile one means a partially completed row still gets its link equity.

The rules for the pass:

- **Skip hub pages** as link sources. They already link to everything in the cluster, and adding more dilutes what they pass.
- **Skip any article already carrying eight or more outbound internal links.** Past that point you are diluting an existing page to help a new one, which is a trade, not a gain.
- **Never link to a planned but unpublished article.** The backlog contains slugs that do not exist yet. A link to one renders as a perfectly normal link and 404s. This is the dead sibling link, and it is the most common way a publishing loop ships a broken link.
- Link from the body, in the rendered markup, using descriptive anchor text drawn from the destination's topic rather than its title verbatim.

## Phase 8: translate, and get the slug and the URL shape right

Every target locale is mandatory. A locale is not optional because it is smaller, newer, or lower priority.

**Reuse the same asset URLs.** Do not regenerate images per locale. Translate only the alternative text and the sentence that introduces each asset.

**Brand, product and feature names stay in the source language.** A translated feature name is unsearchable, unsupportable, and does not match the interface the reader is looking at.

**The translated slug equals the source slug.** Pass it explicitly on every call. Left to itself, a publishing system will transliterate the translated title into a new slug, or append a locale suffix, or add a numeric suffix on collision. All three resolve, so nothing 404s and nobody notices. What breaks is quieter: the translation is no longer recognisably the same document, parent and child relationships do not form, and locale annotations pair the wrong URLs or pair nothing. Assert slug equality after every translation call and reject a mismatch rather than accepting it.

**The locale segment is a leading prefix.** The shape is:

```
/<locale>/blog/<slug>
```

Not:

```
/blog/<locale>/<slug>
```

The second shape 404s. A router matching `/blog/:slug` first will read the locale code as the slug, find no article with that slug, and return not found for every translated page in every language, while the publish call reports success because the record was created. Verify each translated URL with a real request. Never construct the string and assume.

## Phase 9: write back with a surgical single-row edit

Edit exactly the rows that changed. **Never regenerate the tracker file.**

A full-file rewrite corrupts multi-value columns. A comma-separated locale list, a URL containing a query string, a quoted title containing a comma: each of these round-trips through a regenerating writer differently than it went in. Worse, a model asked to rewrite a file will silently normalise rows it was not asked to touch, and the diff makes that look like a formatting pass rather than data loss.

The check is arithmetic and it is absolute: **the diff should touch as many rows as the run changed, and no more.** Two rows changed means two changed lines. A diff touching four hundred rows after a two-row run is a corruption event regardless of how clean it looks.

Commit the tracker write and the corpus append in the same commit, using repository-relative paths with forward slashes so the same run works on Windows and macOS. One commit, because the record of what was planned and the record of what exists must never disagree in the history.

## Phase 10: one strict summary, in fixed formats

One summary per run. Fixed-format line per artefact class: article, translation, reciprocal link, asset, tracker write.

Each class has one defined success form and three or four defined partial forms, and **every partial form carries the verbatim error text**. Verbatim, not paraphrased, because a paraphrased error is unsearchable and unprovable.

```
ARTICLE   <row-id>          OK        <url>
ARTICLE   <row-id>          RETRY     <operation> attempts=3 <verbatim error>
ARTICLE   <row-id>          CEILING   emitted=<n> remaining=<n> resume-at=<phase>
ARTICLE   <row-id>          BLOCKED   <reason-code> <evidence-url> <evidence-url>
TRANSLATE <row-id> <locale> OK        <url> slug-equal=yes
TRANSLATE <row-id> <locale> DISCONNECT last-step=<phase> owed=<artefact list>
```

A reader who cannot tell from the summary alone exactly what is owed to the next run has been given a bad summary.

## The denylist: invalid reasons for skipping quota

This is the part that does not survive being paraphrased, so it is quoted.

An agent under pressure does not decide to cheat. It reasons its way to a defensible-sounding partial result. The denylist works because it converts that judgement call into a lookup, and a lookup cannot be talked round.

**These are never valid reasons to publish fewer artefacts than the quota:**

- "context budget pressure"
- "the tool response was too large"
- "the run is taking too long"
- "I will do it next run"

Anything that rhymes with those is also on the list: the remaining locale is lower value, the topic did not feel strong enough, the earlier articles were good enough, the tracker can be caught up later. None of them are failures. They are preferences, expressed as though they were constraints.

**Exactly three reasons are valid, and each has its own reporting format:**

1. **A documented three-attempt failure of a specific operation.** Not one attempt. Three, against the same operation, with the verbatim error each time. The line reports the operation, the attempt count, and the error text. If the three errors differ, all three are reported, because a changing error is diagnostic and a repeated one is not.

2. **A mid-run disconnect.** The run lost its connection to a system it had already been using. This is reported differently because nothing failed on its merits: the line names the last completed phase and the exact list of artefacts owed, and that list becomes the first work of the next run under phase 2's backfill rule. It is debt with a name, not a skip.

3. **A genuine per-turn output ceiling.** The run hit a hard limit on how much it could emit in one turn. Distinct from a disconnect because everything is healthy and nothing errored: the line reports what was emitted, what remains, and the exact resumption point.

**The rule that binds them:** if the reason is not one of those three, the work is not skipped, it is done. If it genuinely cannot be done now, it becomes explicit backfill debt with a named resumption point. It never becomes silence, and it never becomes a sentence in a summary that reads like an explanation.

## Decision rule: should this row be written?

1. Does the publishing system return a match on the focus keyword, the title, or the candidate slug? **Yes:** reconcile, do not write, do not consume quota.
2. No match, tracker says `published`, URL present. **Fetch the URL.** A 200 means the live search index is lagging: leave the row alone and move on. A 404 means the tracker is wrong: treat the row as pending and write it.
3. No match, tracker says `published`, URL absent. Ambiguous. Continue to 5.
4. No match, tracker says pending, no near-match returned. Write it.
5. **You cannot tell.** The search returned a near-match on the same topic from a different angle, or the system's search is paginated, rate limited or plainly unreliable, or the row claims a publication with no URL to check. Do not write, and do not mark it published. Fetch the near-match's live body and compare its H1 and its focus keyword against the row's. If that resolves it, act on the resolution. If it does not, record the row as `BLOCKED ambiguous-existence` with both URLs in the summary, leave the tracker row untouched, and move to the next candidate. It does not consume quota and it does not become silent debt, because it is named in the summary. The asymmetry is the point: when you cannot tell whether a page exists, the cheap error is publishing nothing and escalating, and the expensive error is publishing a duplicate.

## Worked example, compressed

A documentation site for a fictional billing service. Source locale plus two targets. Quota N is 2. The previous run ended partway through.

**Pre-flight.** Nine capabilities requested in one batch. All nine load. The diagram generator gets a liveness ping and returns a valid asset in four seconds, so the run may safely reach the asset phase.

**Selection.** The previous run left row 41 published in the source language with no reciprocal links and no translations. It is backfill state 1, so it goes first and counts as one of two. Eligible rows sort to row 47 and row 58, both priority 2, and row 47 wins on identifier. Row 47 is the second selection.

**Reconcile, row 47.** Keyword search returns a live article with a near-identical title. Slug search confirms it. The tracker says pending. **The tracker is stale.** Mark it published, backfill the URL, write nothing. It does not consume quota, so selection advances to row 58.

**Reconcile, row 58.** No match on keyword, title or slug. Proceed.

**Gates, row 58.** The constructed hub URL 404s. Rather than declare the hub missing, re-resolve by name against the hub index: the hub exists under a different section prefix. Gate passes. No duplicate focus keyword. Fact source current.

**Write and assets.** Two assets required, two present, both real uploads. One is missing alternative text. Fixed before publishing, not after.

**Publish and read back.** Record read back by identifier: present, state published. Public URL fetched: 200.

**Reciprocal links.** Three candidate siblings. One is a hub, skipped. One already carries nine outbound internal links, skipped. One qualifies and receives the link. A fourth candidate from the backlog is not yet published, so it is not linked, avoiding a dead sibling.

**Translation, row 58.** First locale: the platform returns a slug with a locale suffix appended. Rejected, slug passed explicitly, reasserted, equal. URL verified at `/<locale>/blog/<slug>`, 200. The reversed shape is spot-checked once and 404s, confirming the router behaviour. Second locale: same, clean.

**Backfill, row 41.** Reciprocal links first, then the first target locale, published and verified. On the second target locale the run hits a per-turn output ceiling. Nothing failed. It is reported under valid reason 3 with what was emitted, what remains, and the resumption point.

**Write-back.** Three surgical row edits: row 47 (reconciled), row 58 (published, both locales), row 41 (partial). The diff touches three lines. Committed together with the corpus append.

**Verdict: quota met at 2 of 2, one duplicate averted without writing a word, and exactly one named debt carried forward.** Row 41 is complete except for its second target locale, which is the next run's first action under backfill state 2. Nothing in this run was skipped for a reason on the denylist, and the summary names the one thing that was not done, the reason class, and where to resume.

## Failure modes

**Tracker drift into duplicates.** From the outside: two live articles on the same keyword with slightly different titles, both indexed, both ranking below where either would have ranked alone, and a tracker showing a single row. Caused by trusting the local list over the publishing system.

**The excuse skip.** A run summary reporting one of two, with a fluent sentence where an error message should be and no verbatim error text anywhere in the file. Individually invisible. Across four runs, a backlog a month behind that nobody can date.

**Silent backfill debt.** A cluster of source-language articles with no translations, discovered when somebody finally runs a locale coverage count. Caused by a run that started fresh work instead of finishing the previous one.

**Dead sibling link.** A published article linking to a slug lifted from the backlog. It renders as a normal link, it 404s, and it will be found by a crawler months later rather than by anyone reading the run summary.

**Escaped raw anchors.** Literal angle-bracket markup visible in the body of the live page, because an HTML anchor was written into a field the front end escapes. Every link in that article leaks as text and passes nothing.

**The locale-after-blog URL shape.** Every translation 404s, in every language, while every publish call reported success. The records exist. Only the route is wrong, which is why nothing in the run logs looks broken.

**The auto-suffixed translated slug.** URLs that resolve perfectly and are therefore never reported. The translations simply stop being recognisable as the same document, so locale annotations pair nothing and the parent relationship never forms.

**Publish without read-back.** A summary citing a URL that returns 404, or a record left in draft state that the publish call happily reported as created.

**Full-file tracker rewrite corruption.** A diff touching hundreds of rows after a two-row run. Multi-value columns collapsed or requoted, status values reformatted, and the damage disguised as a tidy-up.

**The half-batch pre-flight.** A run that reaches the asset phase before discovering the generator is unavailable, having already spent the entire write. Prevented by one liveness request costing seconds.

## What this skill does not do

- It does not write the article. Topic research, angle selection and drafting are a separate job, and this procedure treats the body as an input it validates rather than authors.
- It cannot verify translation quality. It verifies that a translated artefact exists, at the correct URL, under the correct slug, with its assets attached. That is a different claim from the translation being good.
- It cannot enforce anything against a publishing system that exposes no search, no read-back by identifier, and no explicit slug parameter. On such a system the reconcile rule and the slug rule are unenforceable, and a uniqueness constraint in the database is the honest substitute.
- It does not decide whether a backlog row deserves to exist. It will execute a bad backlog precisely and on time.
- It is not a scheduler and it has no clock. Something else has to run it, and if that something skips a week this procedure will not notice, because it deliberately does not gate on dates.
- It does not check anything after the run: rankings, indexation, traffic, or whether the links it created were ever crawled. Those need separate tools and a different time horizon.
