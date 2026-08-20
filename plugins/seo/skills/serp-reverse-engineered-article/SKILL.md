---
name: serp-reverse-engineered-article
description: Writes an article intended to rank by first capturing the live results page for the exact keyword, reverse-engineering the top three organic results into a beat sheet of table stakes, gaps and unanswered questions, drafting against that beat sheet, and then gating publication on a separate head-to-head verdict against those same three URLs. Covers the intent gate that runs before any capture, the browser versus API capture rule for parallel batches, and the run record that makes the claim auditable afterwards. This skill should be used whenever an article is being written with the intention that it outranks pages that already exist.
---

# SERP reverse-engineered article

## The claim this skill is built on

An article written from a topic is written against a memory. An article written from a captured
results page is written against three specific pages, at three specific URLs, with three specific
sets of gaps.

The normal process is: pick a keyword with volume, write a comprehensive article, optimise the
on-page elements, publish. Every step in that process is defensible, and none of them is a step at
which the draft can be discovered to be worse than the pages it will compete with, because those
pages are never opened. So the failure is silent. The article ships, it is genuinely good, and it
settles below three pages that happened to cover more.

This skill replaces the guess with two artifacts and one bar.

The artifacts are the **capture**, which is the top three to five organic results plus the
people-also-ask list and the related searches, recorded with the method used and the date, and the
**beat sheet**, which is the union of what the incumbents cover plus every gap plus every question.

The bar is that **matching the incumbents is a failure**. A new page with no accumulated links
that is as good as an established page has given nobody a reason to reorder anything. The
advantage has to be nameable in one sentence before the draft is written, and provable against the
captured pages before it publishes.

The order of the stages is fixed for one reason: each stage is cheap to run and expensive to undo
once the next one has run. Changing the target after R0 costs nothing. Changing it after R3 costs
an article.

## R0. The intent gate, which runs before anything is captured

Four checks. All four pass or the keyword is refused, and the refusal is a legitimate output.

1. **Real demand.** A verified volume figure from a data source, not an impression. The default
   floor is roughly 70 searches a month. Below that, the page needs a written reason for existing
   that is not traffic, and that reason goes in the run record.
2. **Intent you can serve.** The person typing this can plausibly become a reader or a customer
   you want. Volume attached to the wrong intent is a number, not an outcome.
3. **A content type you can produce.** If the results page is dominated by interactive tools,
   template galleries, product pages or video, an article is the wrong artifact and writing a
   better article will not change that. Match the dominant content type or target a different
   query.
4. **No cannibalisation.** Nothing you have already published targets this intent. If something
   does, improving that page is nearly always the correct move, and publishing a second page is
   the most common self-inflicted ranking injury there is.

Record the four answers. A keyword refused here should not come back next quarter with the same
answers.

## R1. Capture the results page

Capture, do not skim. Everything downstream is judged against this file.

For the exact keyword, in the target country and language, record:

- The **top three to five organic results**: position, URL, page title, and snippet.
- The **people-also-ask questions**, verbatim, in the order shown.
- The **related searches**, verbatim.
- Which results-page features are present, in particular a generated answer block, a featured
  snippet and which page fills it.

Ignore paid results. Ignore your own domain when counting positions, and note that you did.

**Record the capture method and the timestamp in the file itself.** A capture with no method is
a rumour, and a capture older than about 30 days should be treated as a different results page.

### The capture-method decision rule

This is decided by contention, not by capability.

- **You are writing one article and a browser session is available.** Drive the browser. It sees
  the page a person sees, including the features an API may flatten or omit.
- **You are running a fanned-out batch of articles, or no browser is available.** Use a results
  API. One shared browser across parallel writers is a queue with a fancy name: every writer
  blocks on it, wall-clock time goes superlinear in the number of writers, and the run looks hung
  rather than failing. Assume contention above roughly four concurrent writers.
- **You cannot tell whether the browser will be contended**, because you do not know how many
  writers the orchestrator will start, or whether another process holds the session. Use the API,
  record that you did, and note the reason. A slightly less faithful capture that completes beats
  a faithful one that deadlocks, and the run record makes the trade visible later.

## R2. Reverse-engineer the top three

For each of the top three, capture:

- Approximate **word count**.
- The full **H2 and H3 tree**, verbatim.
- Every **subtopic** covered, as a list, at a finer grain than the headings.
- The **data, examples, tables and visuals** used.
- The **angle**: who it is written for and what it argues.
- The **credibility signals**: named author, stated experience, original data, dates, citations.

Then the half that people skip, because it is the half that produces positions rather than
observations. Capture the **weaknesses**:

missing subtopics, facts that are out of date, sections that are thin, no original data, no worked
example carried end to end, no comparison table where the query is comparative, no answer to the
obvious follow-up, weak or absent first-hand experience, and poor scannability.

## The beat sheet

Emit one file with exactly three parts. This is the artifact the draft is written from, and if it
does not exist the article is being written blind.

1. **Table stakes.** The union of everything all three cover. Missing an item here is
   disqualifying. Covering only these produces the fourth-best version of a page that already
   exists, so this section is the floor and never the plan.
2. **Gaps.** Everything at least two of the three fail to do. These are the positions. If this
   list is empty, either the teardown was lazy or the query is genuinely well served, and the
   second case is a reason to go back to R0 and pick a different target.
3. **Questions.** Every people-also-ask question, verbatim. Each one must end up as a heading or
   as an explicitly answered passage in the draft.

## R3. Draft

The draft has to satisfy four conditions, and they are checkable rather than aspirational.

- Every table stake is covered.
- Every question from the beat sheet is answered, as a heading or as a passage that states the
  answer in its first sentence.
- Every gap is filled.
- The page carries at least one thing none of the three have: original framing, data you own, a
  worked example carried end to end, a comparison table, or a genuine FAQ built from the captured
  questions rather than invented.

On length: the section count should sit at or above the median of the top three, and being far
above is not a virtue. More than roughly one and a half times the median usually means padding,
which the gate should catch as length without coverage. Length is a consequence of covering more.
It is never the target.

## R4. The head-to-head gate

**A different evaluation pass does this, not the writer.** The writer has just spent its effort
making the case for the draft and is the worst available judge of it. In practice this means a
fresh context that receives the draft, the three captured URLs and the beat sheet, and nothing
about how hard the drafting was.

Score against each captured competitor on six axes: intent match, coverage versus that competitor,
depth and originality, structure and scannability, keyword and entity coverage, and experience
signals.

The verdict is exactly one token.

- **PASS.** Clearly better than all three on coverage, depth, freshness and usefulness. Publish.
- **REVISE.** Specific, listed, numbered fixes. Not "add more depth" but "the second competitor
  covers reconciliation across two accounts and the draft does not". Apply them, then re-run R4.
- **REJECT.** The target or the format is wrong. Do not publish. Send the keyword back to R0.

Two rules that keep the gate honest:

- **Only PASS publishes.** A REVISE that ships because of a deadline is a REJECT with extra steps.
- **Two revision rounds, then stop.** If two rounds of specific fixes have not produced a clearly
  better page, the problem is the target, not the draft, and the third verdict is REJECT.

### The decision rule, including the branch people avoid

- The draft covers everything all three cover **and** adds at least one nameable advantage you can
  state in a sentence. **PASS.**
- The draft is missing named items you can list. **REVISE**, with the list.
- The results page wants a different artifact, or the keyword fails R0 in hindsight. **REJECT.**
- **You cannot tell whether it is better.** You have read all four pages and cannot state the
  advantage in one sentence. That is a **REVISE**, not a PASS. An advantage that cannot be named
  is not an advantage, and "it feels more thorough" is the exact feeling that precedes a page
  ranking at position fourteen.

## R5. The run record

Write one record per article, and write it whether the verdict was PASS or not:

keyword, volume figure and its source, capture method, capture timestamp, the three competitor
URLs, the path to the beat sheet, the verdict token, the six axis scores, and the number of
revision rounds.

This exists so that in four months, when the page is at position nine and somebody asks what it
was supposed to beat, the answer is a file rather than a memory. An unrecorded head-to-head claim
is indistinguishable from no head-to-head at all.

## Two run modes

**Single article.** One browser session does R1 and, where the incumbents need rendering, R2. The
stages run in order in one pass.

**Bulk fan-out.** One **serial** browser sweep runs first, across every keyword in the batch, and
writes a shared results map: keyword, the three to five URLs, the questions, the related searches,
the capture timestamp. Only then do parallel writers start, and each does its R2 by fetching the
URLs from the map directly rather than asking for the browser. Evaluators run in parallel too, and
each one is a fresh context.

The map is the whole trick. It converts a resource that cannot be shared into a file that can.

## Worked example, compressed

A team building a time tracking tool for agencies. Target keyword: "how to bill clients for
overtime", volume 880 a month, informational with a commercial secondary intent.

**R0.** Volume is above the floor. The intent is served by an article. The results page is
dominated by long-form guides, which is a type the team can produce. Nothing on their site targets
this intent, though a published page on "hourly versus fixed fee" is adjacent and will need a
cross-link. Gate passes, four answers recorded.

**R1.** Browser capture, single article mode, timestamped. Top three organic: a payroll vendor's
blog post, an accounting software help centre article, and a freelancing community guide.
Placeholder URLs stand in here for the real ones, which in a real run are recorded literally:
`https://vendor-a.example/blog/overtime-billing`, `https://vendor-b.example/help/overtime`,
`https://community-c.example/guides/overtime-invoicing`. Six people-also-ask questions captured
verbatim. A generated answer block sits above the organic results, noted as a ceiling on clicks.

**R2.** Word counts 1,450, 900 and 2,100. Union of subtopics: what counts as overtime, contract
clauses, rate multipliers, how to record the hours, how to present it on the invoice, and client
pushback. Gaps: none of the three shows a worked invoice with the overtime line broken out, two of
the three are written from a payroll perspective rather than an agency one, none answers what to do
when the overtime was caused by the client changing scope, and none carries a comparison of the
three common multiplier conventions.

**Beat sheet.** Six table stakes, four gaps, six questions.

**R3.** Draft at 2,400 words, eleven sections against a competitor median of nine. Every question
becomes an H3. The scope-change case gets its own section. A three-column table compares the
multiplier conventions. A worked invoice is shown end to end with the overtime line itemised.

**R4, first pass. Verdict: REVISE.** Three named fixes: two of the six questions are answered
implicitly rather than stated, the contract clause section is thinner than the second competitor's,
and the piece never says which multiplier convention the authors use themselves, which is the one
experience signal all three incumbents also lack and therefore the cheapest gap on the sheet.

**R4, second pass. Verdict: PASS.** Better than all three on question coverage, on the scope-change
subtopic none of them touch, and on the worked example. Not better on brand authority, which is
recorded as a known risk in the run record rather than pretended away.

**R5.** Record written: keyword, 880 from the volume source, browser capture with its timestamp,
the three URLs, the beat sheet path, verdict PASS, six axis scores, one revision round.

## Failure modes

**Writing blind.** No capture file exists anywhere in the run. From the outside this looks like a
perfectly reasonable article and an author who is confident it is better, with no way to check.

**As-good-as drift.** The gate returns PASS on a draft that matches the incumbents. Symptom: the
evaluator's reasoning contains the phrase "comparable to" or "covers the same ground" and still
passes.

**Browser contention deadlock.** Fan-out mode with no serial sweep. Symptom: six writers start,
one finishes, the rest sit at zero progress, and the orchestrator reports no error because nothing
has failed, it is only waiting.

**People-also-ask amnesia.** The beat sheet's gap list gets used and the question list gets
silently dropped. Symptom: a draft that fills every gap and answers four of six captured questions,
which is the most common near-miss in the whole procedure.

**Self-grading.** The writer evaluates its own draft. Symptom: a PASS in every single run, and
verdict prose that describes the draft's intentions rather than comparing it to a named URL.

**The unaudited claim.** Publishing with no record of which three URLs the verdict was made
against. Symptom: four months later, nobody can reconstruct what "better" meant, and the same
keyword gets targeted again by somebody who assumes it was never tried.

**Using the table stakes as the outline.** Covering the union and nothing else. Symptom: a draft
whose heading tree is a merge of the three competitors' heading trees, which reads as thorough and
gives no reader a reason to prefer it.

**Auditing against a stale capture.** The capture is three months old and the results page has
turned over. Symptom: a confident head-to-head against a page that is now at position eleven.

**Padding to beat the median.** Length treated as the target rather than a consequence. Symptom:
sections that restate the previous section in different words, and a word count comfortably above
the median with a subtopic list that is not a superset.

## What this skill does not do

- It does not fetch anything. It needs a browser session or a results API, and it will state that
  the capture could not be completed rather than reconstructing a plausible results page.
- It does not judge link authority. A draft can pass on content and still lose to a weaker page on
  a stronger domain, and nothing here surfaces that.
- It does not estimate traffic. It can note that a generated answer block suppresses the ceiling,
  and any specific number after that would be invented.
- It does not do the technical or on-page checks. Indexability, canonicals, structured data and
  internal linking are a separate pass after the draft exists.
- It does not decide whether the artifact should be an article at all. R0 refuses the obvious
  mismatches, and a proper format decision is a different job done before this one starts.
- Applied to a query with no real competition it over-analyses. If the top three are thin and
  unclaimed, record that, write the page, and move faster than this procedure suggests.
