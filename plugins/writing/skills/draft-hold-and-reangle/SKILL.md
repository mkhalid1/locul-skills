---
name: draft-hold-and-reangle
description: Takes a draft that collides with something already published or already queued and produces the quarantined, re-angled, complementary replacement instead of a deletion or a duplicate. Covers the three-layer hold that stops any publishing path shipping it by accident, a closed set of five verdicts, the operational definition of differentiation, the rule for choosing which of two live twins is canonical, and the non-destructive fallback where a system has no canonical or redirect field. This skill should be used when a collision has already been identified and someone has to decide what happens to the draft.
---

# Draft hold and re-angle

## The claim this skill is built on

A colliding draft has two obvious fates and both are bad. Delete it and you lose the research, while the tracker row that commissioned it still says pending, so in three months somebody writes it again. Publish it and you own two pages chasing one intent, which usually leaves both worse off than the original was alone.

The output that is actually worth having is a third thing: a complementary replacement that covers something the live page does not, and points at the live page rather than competing with it. Producing that takes a procedure, because the natural way to "re-angle" a draft is to change its title and leave the body alone, and that produces a duplicate with better camouflage.

The second claim is smaller and it is the one that saves you: **a hold recorded in one place is not a hold.** Publishing paths are plural. A scheduled run reads frontmatter. A person opens the file and pastes it into the editor. A batch manifest hands a list of file paths to a publisher. Each of those can ship a draft without ever seeing the field the other one respects.

---

## Phase 1: quarantine in three layers, before any thinking

Do this first, before deciding anything, because the window in which a draft can publish itself is the window between finding the collision and finishing the analysis.

1. **`status: hold` in the frontmatter.** This is what a scheduled publisher reads.
2. **A `cannibalisation_hold:` note in the frontmatter** naming the live URL it collides with and the date. Without the URL, the next person to open the file cannot verify the hold and will assume it is stale.
3. **A `<!-- HOLD: ... -->` comment as the first line of the body.** Not the third line. The first.

The third layer is the one people leave out and it does two jobs. If any path renders the body without reading the frontmatter, the marker is the first thing in the output and a human sees it. And if the frontmatter is stripped, which happens on import into a content system that does not recognise the fields, layer three is the only thing that survives.

**The publisher has two hard requirements.** It must skip `status: hold`, and it must skip the batch manifest file itself. The second one sounds silly until the manifest is published as an article, which happens exactly once per operation and is always memorable. With both in place, held drafts can sit in the same directory as clean ones, which matters because moving a file breaks every tracker row that points at its path.

Verify the skip rather than assuming it. A dry run over the directory that reports what it would publish, with the held file absent from the list, is the only evidence that layer one is real.

---

## Phase 2: establish what the live page actually owns

Fetch the live page. Do not work from the tracker, the plan row or your memory of what was commissioned, because those describe what was intended and the live page is what exists. Where the two disagree, the live page wins.

Write down two lines before choosing a verdict:

- **What the live page owns**: its intent in one sentence, its heading set, and the query it actually ranks for if you know it.
- **What the draft was aiming at**: its intent in one sentence, and its heading set.

That heading set comparison is what makes the later differentiation test possible. Capture it now, while both are open, because reconstructing it after the rewrite is impossible.

---

## Phase 3: choose exactly one verdict from a closed set of five

The verdict field takes one of five tokens and nothing else.

| Verdict | Selected when | The artefact it produces |
|---|---|---|
| CONSOLIDATE | Both cover the same intent and at least one is live | One canonical article, the other retired non-destructively |
| DIFFERENTIATE | The draft contains a genuine sub-intent the live page does not cover | A narrowed draft plus an up-link |
| REDIRECT | The draft's target has no independent demand and the live page answers it | No article. The tracker row closes with the live URL recorded |
| DE-OPTIMISE | Two live pages both worth keeping, but one competes for a term it should not own | The weaker page retargeted to a different term, its content kept |
| LEAVE | The overlap is real but harmless: different funnel stage, audience or language | A recorded decision with a date to revisit |

The set is closed for a reason that only appears later. Verdicts get aggregated: at the end of a quarter somebody wants to know how many collisions were found and what happened to them. Free text cannot be counted, and a notes field containing forty unique sentences is a record that nobody will ever read twice. It also forces a decision now. "Probably merge these at some point" is not a state a publisher can act on, so the draft stays in limbo and eventually somebody publishes it to clear the queue.

---

## Phase 4: what DIFFERENTIATE actually means

This is the heart of the procedure and the step that is routinely faked. Differentiation is two operations, and doing only the first is the most common way this goes wrong.

**Operation one: narrow.** The replacement targets a sub-intent that the live page does not cover. The test is that you can name the question the live page does not answer and point at the absence in its heading set. If the live page covers the sub-intent in a paragraph, that is not enough on its own, but it is a warning: you are writing an expansion of one of its sections, and it needs to be a section that genuinely deserves its own page.

**Operation two: link up.** The replacement links to the page it used to compete with, in the first third of the body, using the parent term as the anchor text. This is what converts a competitor into a feeder. A narrowed page with no up-link is still an orphan competing for attention with its own parent.

**The test that catches a fake.** Map the replacement's headings onto the live page's headings. If more than roughly 60 per cent of them map, it is a retitle, not a re-angle. That threshold is a working default, not a published figure: the real signal is qualitative and the number is there so the test can be run by someone other than the author.

**The second test.** Write one sentence saying what the live page owns and a different sentence saying what the draft now owns. If you cannot write two sentences that a stranger would agree are about different things, the re-angle has not happened.

### Three re-angle shapes that worked

Generalised from a real multi-site operation, with the specifics changed:

- **A head-to-head comparison collided with an existing comparison of the same two things.** It became a piece about taking both, in one process, in sequence. A comparison serves a reader choosing between two options. A reader who wants both has a logistics question, which the comparison never covers and never will, and which has its own real demand.
- **A second comparison of the same pair** became an identity-clarification piece: which of these two do you already have. The comparison serves a chooser. This serves somebody already holding one of them who cannot tell which it is, and that reader arrives from a completely different query.
- **A duplicate requirements page** became one narrower sub-topic: the single requirement that changes most often and has the most conditions, with the parent requirements page linked up in the first paragraph.

The pattern behind all three: the original draft and the live page shared a topic, and the re-angle found a different **reader state**. Choosing, already holding, or trying to do both are three different states about one subject, and only one of them was taken.

**Record the rationale in the `notes:` field with two clauses**: what the live page owns, and what this draft now owns. A rationale with one clause is a decision nobody can check. Then restore `status: draft`, remove the hold comment, and update the manifest's hold register in the same commit, so the three layers come down together rather than leaving one behind to confuse the next reader.

---

## Phase 5: CONSOLIDATE, when both twins are already live

**Pick the canonical by which twin holds the assets, not by which has the prettier slug.** Enumerate them before deciding: depth and word count, translations sitting under it, inbound internal links, external links, navigation placement, impressions and clicks, and age. Accept an ugly canonical rather than break a URL. A slug is cosmetic and a URL with history is not.

Then merge into the canonical: move the unique sections across first, verify they are live, and only then retire the other one.

**If the system has no per-document canonical field and no redirect map**, set `robots_noindex = true` on the redundant twin and keep the more complete article indexed. This is non-destructive and reversible, which is exactly what you want from a decision made with incomplete information.

Two facts about that fallback that are easy to get wrong:

- **The page must remain crawlable for the directive to work.** Blocking it in robots.txt at the same time means the crawler never fetches the page, never sees the noindex, and the URL can stay indexed indefinitely. The two controls look complementary and they are mutually exclusive.
- **On an incrementally revalidated front end, the change only takes effect at the next revalidation.** The page keeps serving its previous HTML for the length of the revalidation window, so a same-day check shows the old markup, the change is reported as failed, and somebody applies it a second time. Record the revalidation window next to the verdict.

**Never delete, archive or re-slug a live URL that returns 200.** Archiving without a redirect produces a 404, and every external link to that URL dies silently, which is the one part of this nobody can undo later.

---

## Phase 6: the batch manifest

One manifest per batch, sitting next to the drafts, in this order:

1. Generation date and count
2. Which gate the batch passed, and when
3. Publish state
4. The **HOLD register**: one row per held draft, with the colliding URL, the verdict and the current state
5. The batch table: `| # | target term | type | angle | words | file |`

The hold register comes before the table because the person opening this file at the start of the day is looking for what is blocked, not for what is fine. Burying the register under a forty-row table means it gets read on the second visit.

The manifest is a working file, not content, and the publisher must skip it explicitly.

---

## Phase 7: housekeeping, and the republish risk

Delete or archive the stale draft file once its row is closed. Both halves matter, and the accident needs both to happen.

In one audit of a real operation, seventeen tracker rows still read `pending` while a draft that overlapped an already-live article by around 92 per cent sat beside it, under the same slug. Neither fact alone is dangerous. A row that says pending invites somebody to run the batch again, and a file that still exists gives that run something to publish. Together they are a live republish waiting for a quiet week.

**Treat a row and its file as one unit.** Closing a row deletes or archives its file. Changing a file's status updates its row. Reconcile in both directions, because each catches a different failure: orphan files with no row, and rows pointing at files that no longer exist.

**When a hold is resolved by re-angling rather than by deletion, confirm both halves.** The old target term no longer exists as a draft, and a genuinely differentiated replacement does. A draft that simply disappeared is not a resolution, it is an unrecorded deletion, and the row will bring it back.

---

## Decision rule: which twin is canonical?

- **If one twin holds the translations, the internal links and the impressions**, it is canonical, even if its slug is worse and its body is thinner. Move the text into it.
- **If the assets are split**, one has the rankings and history, the other has the better content, the one with the rankings is canonical. Text is cheap to move. History is not movable at all.
- **If you cannot tell**, because there is no analytics access, or both pages are under a few months old and have no history to compare, do not consolidate. Set the verdict to LEAVE with a review date about ninety days out, and de-optimise the weaker title in the meantime so they stop competing directly. Consolidation is the only irreversible move in this procedure, and doing it on a guess is how an operation loses a page that was quietly earning.

---

## Worked example, compressed

An invented documentation site for a time-tracking tool. The collision check reports that a queued draft targeting "timesheet approval workflow" collides with a live page at `/guides/approve-timesheets`.

**Phase 1.** `status: hold`, `cannibalisation_hold: /guides/approve-timesheets, 2026-08-20`, and `<!-- HOLD: collides with /guides/approve-timesheets, verdict pending -->` as the first body line. Dry run confirms the publisher lists nine files and not this one.

**Phase 2.** The live page is fetched. It owns the single-approver flow: nine headings, all covering one manager approving one person's week. It has no heading for approval chains, none for delegation while somebody is away, and none for bulk rejection. The draft's seven headings are: what approval is, why it matters, single approver, multi-level approval, delegation, bulk actions, and a summary.

**Phase 3.** Verdict: DIFFERENTIATE. The draft carries three sub-topics with no coverage on the live page.

**Phase 4.** The replacement targets multi-level approval chains. It drops "what approval is" and "why it matters" outright, since both are covered upstream, keeps multi-level, delegation and bulk rejection, and adds a section on what happens when an approver in the chain leaves. It links up to `/guides/approve-timesheets` in the second paragraph, anchored on "approving timesheets". Heading overlap falls from eight of nine to two of seven, or about 22 per cent, and both remaining overlaps are one-paragraph references that point up rather than re-explaining.

Notes field: "Live page owns single-approver timesheet approval. This draft now owns multi-level approval chains, delegation and bulk rejection." Status restored to draft, hold comment removed, manifest register updated in the same commit. The freed target term goes back to the backlog as covered.

**Verdict: DIFFERENTIATE, heading overlap 22 per cent, up-link present in paragraph two, one term returned to the backlog.**

---

## Failure modes

**A held draft published because the hold lived in one place.** The piece is live, the tracker says hold, and nobody can reconstruct which path published it. This is the failure the third layer exists for, and it is always discovered by a reader rather than by a check.

**Free-text verdicts.** At the quarter's end nobody can say how many collisions were resolved, because the field contains forty distinct sentences, several of which describe intentions rather than outcomes.

**Differentiation that is a retitle.** Two pages with different titles and the same eight headings. Both now rank worse than the original did on its own, and the person who approved it read the new title and not the body.

**Consolidating onto the prettier slug.** The surviving page has no translations, so six locales either 404 or silently fall back to English, and the fix is republishing every translation under a URL that no longer matches the one they were indexed at.

**Archiving a live URL that was returning 200.** A 404 appears in the next crawl, and an external link that had been sending steady traffic now sends none. This is the only step in the procedure with no clean undo.

**Stale drafts republished months later.** A duplicate article appears with a slug already in use, and the content system silently appends a suffix, so you now have two URLs, one of which nobody meant to create.

**Noindex applied to a page that is also disallowed in robots.txt.** The page never leaves the index because the crawler never sees the directive. From the outside it looks like the de-index simply did not work, and the usual response is to apply it again.

**Verifying a de-index on the same day, on an incrementally revalidated front end.** The old HTML is still being served, the change reads as failed, and it gets applied twice, which is harmless but burns the trust in every later verification.

---

## What this skill does not do

- It does not find collisions. Something else names the pair, and a query report is the only thing that proves two URLs actually compete rather than merely resembling each other.
- It cannot see rankings, links, navigation or revenue, which are exactly the facts that decide the direction of a merge.
- It does not know whether the sub-intent it narrows to has any demand. That check happens in the backlog, before the replacement is written.
- It executes nothing on the live site. Redirects, canonical tags, noindex flags and deletions are applied by a person afterwards, and on some platforms half of them are not available at all.
- It does not rewrite the body for you. It defines what the replacement has to be and gives you the test that catches a fake, and the writing is still a writing job.
