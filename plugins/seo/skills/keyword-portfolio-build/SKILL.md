---
name: keyword-portfolio-build
description: Builds a validated 50-keyword portfolio from a product description, using an ordered research pipeline that puts competitor ranked keywords and results-page ownership above generic idea generation, then composing the survivors into a fixed 10/15/15/10 mix with numeric thresholds per bucket and four explicit rejection rules. Includes the category-association failure mode of keyword idea endpoints, the compound informational-plus-commercial intent case, the quick-wins predicate that decides which articles get written first, and a documented fallback when no data source is available. This skill should be used whenever a content programme needs a target keyword list before anything is briefed or written.
---

# Keyword portfolio build

## The claim this skill is built on

A keyword list assembled from whatever the tool returned is not a portfolio. It is an assortment,
and its composition is an accident of which endpoint was called and how the export was sorted.

The obvious approach is to type the product category into a keyword tool, sort by volume, take the
top 50, and start writing. It fails in two ways at once. The first is that the top of a
volume-sorted list is, by construction, the part of the market you have the least chance of
winning, so the list front-loads the impossible. The second is subtler and it is the reason this
skill exists: **generic keyword-ideas endpoints associate by category tag, not by meaning.** Seed
one with a phrase describing your product and it will return software that shares an app-store
category with something in your seed, with entirely unrelated meaning and entirely plausible
volumes. It looks like data. It is noise with numbers attached, and it is unusable without a
manual pass.

So the fix is an ordering and a composition. The ordering puts the sources that cannot bleed across
categories above the one that can. The composition fixes how many of each kind of term the list
contains before anyone sees the data, which is the only way to stop the list becoming whatever the
tool happened to be good at that day.

## Why the order is what it is

Every step below is placed by how trustworthy its output is, not by how convenient it is to run.

Idea generation is the least grounded source, so it runs first and is treated as a candidate
generator rather than as a result. Related-keyword endpoints are derived from the results pages
themselves, so they are more semantically grounded and they run second, on the survivors.
Competitor ranked keywords are the most grounded of all, because a term only appears if a real page
really ranks for it, so they run late and they override. Results-page ownership runs after that,
because it tells you which competitor list to mine next, and you cannot know that until you have
candidates to look up.

Running competitor mining first sounds tempting and produces a narrower list, because you inherit
somebody else's strategy including its blind spots. Running it last lets it correct the noise
without defining the scope.

## Step A. Seed

Write 3 to 5 tightly scoped phrases describing what the product actually does for someone, not
one-word category terms. "Time tracking" is a category tag and will bleed. "Track billable hours
across client projects" is a value proposition and will not.

Request about 200 ideas per seed. Pre-filter to volume above 50 and difficulty below 85, ordered
by volume.

Then do the manual pass, which cannot be skipped: **discard every returned term a real buyer of
this product would never type.** Expect to throw away a large fraction. If you are throwing away
almost nothing, the filter is not being applied.

## Step B. Expand

Take your 2 or 3 best surviving terms and expand them with a related-keywords endpoint at depth 2.
Related keywords are derived from what results pages actually contain, so this is a materially
different signal from idea generation and it is worth its own step.

Depth 2 is the practical limit. Depth 3 returns volume without relevance and doubles the manual
pass for very little.

## Step C. Score difficulty in one batch

You should now have 80 to 100 survivors. Score them all for difficulty in a single bulk call, not
one at a time. This matters for a reason beyond cost: **every difficulty number in the portfolio
must come from the same source in the same pass**, because scores are vendor specific and are not
comparable across tools or across weeks.

## Step D. Classify intent, and watch for the compound case

Classify the ambiguous terms as informational, commercial, transactional or navigational. Intent is
the single strongest signal for which type of page to write, and it decides bucket membership
below.

**The compound case.** A term whose primary intent is informational and whose secondary intent is
commercial both educates and converts, and it is the most valuable class of term on the list. A
strict "commercial intent only" filter deletes exactly these rows. The long-tail commercial bucket
therefore admits them explicitly.

## Step E. Mine competitors' ranking keywords

Pick 2 or 3 competitor domains and pull the keywords they actually rank for. This is more reliable
than any idea endpoint, because a term appears only if a real page really ranks. There is no
category bleed here at all.

Filter their list by the same thresholds and merge into your candidate set, deduplicated.

## Step F. Find the real owners of the results pages

For your top 20 to 30 candidates, look up who actually holds the results pages.

They are routinely not who you expected. Marketplaces, aggregators, review sites, a documentation
site, a community forum, and one competitor nobody in the room had heard of. The two or three
domains that appear most often across your candidates are the next domains to mine, so go back to
step E and run it on them.

This loop is the single highest-yield part of the procedure and it is the part most often skipped,
because it requires admitting the competitor set was wrong.

## Step G. Flag answer-engine competition

Pull competitor ranked keywords filtered to those whose results pages carry generated-answer
citations. Flag those rows in the portfolio.

These are the terms where being the cited source matters more than holding a position, which
changes how the page has to be written later. Flagging them at portfolio time is cheap. Discovering
it after twelve articles are published is not.

## Step H. Re-validate volume

Re-pull volume for the final 50 in one clean pass, from one source, on one day. The list has been
assembled from several endpoints over several calls and the numbers in it are of mixed vintage.
The published portfolio carries one set of numbers with one date on it.

## The composition, fixed before you see the data

Exactly 50 rows, in four buckets.

| Bucket | Count | Volume | Difficulty | Intent |
|---|---|---|---|---|
| **Pillar** | 10 | 1,000 or more | any | informational or commercial |
| **Long-tail commercial** | 15 | 100 to 5,000 | under 60 | commercial or transactional, or informational with a commercial secondary |
| **Long-tail informational** | 15 | 50 to 3,000 | under 50 | informational |
| **Comparison and alternative** | 10 | any | under 70 | any |

The comparison bucket is the one people argue with, because its volumes are small. Accept it
anyway: a term doing 50 searches a month where the searcher is actively choosing between two
products is worth an article, and it converts at a rate no pillar term will ever match. Volume is
the wrong axis on which to judge that bucket.

The pillar bucket admits any difficulty deliberately. These are the terms you will not win this
year and should still own pages for, because they are what the rest of the cluster links towards.

## The four rejection rules

A row is rejected outright, and the rejection is recorded so the term does not return next quarter,
if any one of these holds.

1. **Navigational queries for brands you are not.** You cannot rank for a competitor's name, and
   the traffic that arrives on it is not choosing you. Comparison terms that contain a competitor
   name are a different thing and belong in bucket four.
2. **Single generic words.** One-word terms are category tags, not queries with intent, and their
   difficulty is not the real obstacle: the absence of any statable searcher goal is.
3. **A yearly trend of minus 50 percent or worse.** The topic is leaving. By the time the page is
   written, briefed, published and indexed, it will have left further.
4. **Volume concentrated in one or two spike months.** A term showing 1,200 a month whose
   twelve-month curve is eleven zeroes and one enormous month is a dead term with a healthy
   average. This rule catches the news cycle, the outage, and the viral thread, and it is invisible
   to anyone reading only the headline volume.

## The decision rule for a single candidate

For each surviving candidate, in this order:

- It violates a rejection rule. **REJECT**, and record which rule.
- It has volume, difficulty, intent and a twelve-month trend, and it satisfies exactly one bucket
  predicate. **ADMIT** to that bucket.
- It satisfies more than one bucket predicate. **ADMIT to the scarcer bucket**, meaning the one
  furthest from its quota at that moment, and never to both. A keyword appears once in a portfolio.
- **You cannot tell.** A required field is missing, or two sources disagree on volume by more than
  a factor of two, or the intent is genuinely ambiguous between commercial and informational with
  nothing to break the tie. **HOLD.** Do not admit it and do not reject it. Put it in a HOLD list
  with the reason.

The HOLD branch has one consequence that has to be stated plainly, because it is the rule people
break: **a HOLD row never fills a quota slot.** If the composition cannot be filled from validated
rows, the portfolio ships short, and the shortfall is written on it. Fifty rows where three are
guesses is a worse artifact than forty-seven rows that are all real, because nobody downstream can
tell which three were the guesses.

## Output

One table, one row per keyword, with these columns:

number, keyword, volume, difficulty, cost per click, intent, twelve-month trend, bucket.

Then a second, shorter table: **the quick wins.** The cut is a predicate, not a ranking:

**difficulty under 40 AND intent is commercial or transactional AND volume of 100 or more.**

Those are the first articles written, in that order. Note the consequence: the cut is whatever
size the predicate produces. If seven rows satisfy it, the quick wins table has seven rows. Padding
it to ten by admitting an eighth row at difficulty 44 destroys the only thing the cut was for,
which is that every row in it is genuinely winnable soon.

## The documented fallback

If the data source is unavailable, do not stop and do not pretend.

Research manually: the questions people actually ask in community forums and support threads for
this category, the headings competitors use on their blogs, the phrases that appear in review-site
comparisons. Then price-check as many of the discovered questions as you can against any free
volume source.

Then, and this is the part that matters, **state in the artifact itself that you fell back, which
step failed, and which rows carry no verified volume.** A manually researched list presented as
validated data is the worst possible outcome of this procedure, worse than no portfolio, because
it will be planned against for a year.

## Worked example, compressed

A time tracking tool for creative agencies.

**Step A.** Four seeds: "track billable hours across client projects", "agency timesheet approval",
"how agencies bill retainer clients", "project time reporting for clients". About 780 raw ideas
returned. Pre-filter to volume above 50 and difficulty under 85 leaves 214. The manual pass then
removes 118 of those, almost all of them consumer productivity and fitness-tracking apps sharing a
category tag with the word "track". **Category bleed, visible and quantified: over half the
filtered output was noise.** 96 survive.

**Step B.** Related keywords at depth 2 on the three strongest terms adds 140, deduplicated to 93
new. Candidate set: 189.

**Step C.** One bulk difficulty call across the set. 92 clear the working thresholds.

**Step D.** Intent classified. 31 ambiguous terms resolved by hand; 9 of them turn out to be
informational with a commercial secondary, and all 9 would have been deleted by a commercial-only
filter.

**Step E.** Two competitor domains mined, adding 60 terms, of which 22 are new and survive.

**Step F.** Results-page ownership across the top 25 candidates. Two agency-industry publications
and one freelancing marketplace own most of them, and none of the three was on the competitor list.
Mining the two publications adds 18 more terms.

**Step G.** 14 rows flagged as carrying generated-answer citations, mostly definitional queries.

**Composition.** Pillar and both long-tail buckets fill cleanly. The comparison bucket does not:
only 8 comparison and alternative terms survive difficulty under 70. **The threshold is not
relaxed.** Step E is re-run against the two publications specifically for comparison phrasing,
which yields 4 more, of which 3 qualify. Bucket filled at 10 with one spare discarded.

**Rejections recorded.** 6 navigational brand queries, 3 single generic words, 2 terms with a
minus 60 percent yearly trend, and 1 term showing 1,400 a month whose curve is a single spike in
one month following an industry outage.

**HOLD.** 3 terms where two sources disagreed on volume by more than a factor of two.

**Verdict.** The portfolio ships at 50 rows with the composition intact at 10, 15, 15 and 10, and
with the three HOLD rows listed separately and excluded rather than counted. The quick-wins cut
returns 7 rows, not 10, and ships at 7. The first three articles are named on the front of the
document, all three from the quick-wins cut, and the 14 answer-engine-flagged rows carry a note
that their pages will need to be written to be quoted rather than merely to rank.

## Failure modes

**Category bleed accepted as signal.** A quarter of the list is unrelated software with plausible
volumes. From the outside it looks like a healthy long list, and nobody questions it until the
first three articles get no impressions at all.

**Seeding too broad.** One-word seeds. Symptom: the returned ideas are recognisably about a
different industry, and the person filtering starts inventing reasons why a term is relevant.

**Volume without intent.** The list is sorted by volume and the buckets are ignored. Symptom: a
programme that gets traffic and no signups, and a quarterly review where nobody can explain the gap.

**Dead-spike keywords.** Terms with a healthy twelve-month average and eleven zero months. Symptom:
an article published on a topic whose moment passed, with an author confident the volume was
verified, because it was.

**Brand-navigational padding.** Competitor names sitting in the list as targets. Symptom: a
portfolio that looks ambitious and contains three rows that were never winnable and would not have
converted if they were.

**Quota filled by relaxation.** The threshold is quietly loosened to complete a bucket. Symptom:
the last two or three rows of the hardest bucket sit just outside their own predicate, at
difficulty 62 in a bucket that says under 60, and nothing in the document records that it happened.

**The undocumented fallback.** A hand-researched list shipped without the disclosure. Symptom:
numbers with no source column, and a plan built on them for a year.

**Difficulty compared across sources.** Rows scored in different tools or different weeks, merged
into one column. Symptom: a bucket that satisfies its predicate arithmetically and is internally
incoherent, with terms of obviously different competitiveness sitting at the same score.

**The quick-wins cut sized rather than predicated.** Ten rows because the heading says ten.
Symptom: rows in the first-to-write list that fail the difficulty or the intent condition, which
converts the most useful table in the document into another ranking by volume.

## What this skill does not do

- It does not fetch data. It needs a volume and difficulty source, and where one is missing it
  falls back and says so on the artifact rather than filling the gap with plausible numbers.
- It does not decide what to publish for any row. Intent narrows the format, the results page
  decides it, and that is a separate pass done per keyword.
- It does not check your existing site. Terms you already rank for, and terms that would compete
  with a page you published last year, are invisible to it.
- It does not estimate traffic or revenue. Multiplying volume by an assumed click-through rate and
  an assumed conversion rate produces a number with no evidence in it.
- It does not assess link difficulty beyond the vendor difficulty score, which is a proxy and
  a weak one on small volumes.
- It does not refresh itself. Volumes, difficulty and trends move, so a portfolio is a snapshot
  with a date on it and it should be rebuilt rather than edited when the date gets old.
