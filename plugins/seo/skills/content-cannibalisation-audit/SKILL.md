---
name: content-cannibalisation-audit
description: Finds pages on one site or across several that compete for the same search intent, using the content inventory rather than ranking data. Runs three independent signals over titles, research files and planned rows, applies the de-noising rules that stop drafts and translations firing as false findings, and returns at most five ranked actions with exactly one recommended move each. Reads and reports only, never edits. This skill should be used before scheduling a batch of drafts, after a site merge, or on any established site quarterly.
---

# Content cannibalisation audit

## The claim this skill is built on

Two pages aimed at one intent is the most common self-inflicted ranking injury there is, and it is almost always found too late, in a performance report, after both pages have accumulated links and neither can be retired cheaply.

Most of it is visible earlier than that, in the inventory itself. The reason it is not seen is that comparing titles as strings finds nothing. Consider "The complete guide to expense categories (2026)" and "Expense categories: a complete guide". No useful substring in common. Same page.

So the audit is three signals, each independently weak and jointly strong, plus the de-noising that makes the output readable. A candidate is a pair that fires on one signal. A **finding** is a pair that fires on two, or on one with severity attached. Acting on candidates is how legitimate pages get merged.

## Signal A. Normalised keyword collision

Seven steps, in this order.

1. **Lowercase.** First, or capitalised stopwords survive the next steps.
2. **Tokenise on any run of non-alphanumeric characters.** This splits hyphens, slashes, colons, apostrophes and brackets. "Founder's guide" becomes "founder" and "s", which the stopword list will discard.
3. **Drop stopwords.** The conventional list: a, an, the, to, for, of, and, or, in, on, with, your, you, how, what, why, is, are, that. Fold trivial plurals in the same pass, so "timelines" and "timeline" collapse. Without the plural fold you miss roughly half of the real collisions on any site that mixes singular and plural titles.
4. **Drop topic-noise tokens and bare year numbers.** Topic noise is the words that appear in nearly every title on the site and therefore carry no discriminating signal. Compute it per site rather than from a fixed list: any token appearing in more than about 40 per cent of titles is noise. On a site about email, "email" is noise. Bare years go too, because a year is a freshness marker rather than a topic, and keeping it makes the 2025 and 2026 editions of one guide look like two subjects.
5. **Deduplicate** the remaining tokens.
6. **Sort** them alphabetically.
7. **Join** with a single separator.

**Why the sort matters.** It makes word order irrelevant, which is correct. Two titles with the same words in a different order are the same page. "Expense categories for small business" and "small business expense categories" collapse to one key, and any method that preserves order treats them as unrelated.

Any normalised key shared by two or more items is a candidate. Not a finding. Signal A has a known weakness: it is an exact match on a set, so it fails on derived forms. "Estimate" and "estimation" produce different keys and no light stemmer safely folds them. That is what the other two signals are for.

## Signal B. Shared results page

If two of your items list the same competitor URL in their research, they were built against the same results page. That is a stronger statement than similar wording, because it means somebody, twice, independently decided the same page was the thing to beat.

Normalisation rules, and they are as load bearing as the title ones:

- Strip the protocol and a leading www.
- Strip the trailing slash.
- Strip query strings and fragments.
- Lowercase the whole thing and accept the rare collision on a case-sensitive path.
- **Keep the path. Do not collapse two different paths on the same host into one.** A site can legitimately rank two different pages for two different intents, and collapsing to the host turns every research file that cites a large publication into a match with every other one. This single rule is the difference between Signal B being useful and being noise.

Threshold: two or more shared normalised competitor URLs between a pair is a candidate. One is weak, because in most niches there is a dominant page that appears in every results page you will ever capture.

## Signal C. Title token overlap

Jaccard similarity of the two title token sets, taken after stopword removal, at or above **0.6** is a candidate.

Jaccard is the size of the intersection divided by the size of the union: 1.0 means the sets are identical, 0 means they share nothing.

A threshold is required because overlap is continuous. On any topically focused site, every pair of titles shares something, so without a cut-off the audit reports the entire inventory paired with itself and tells you nothing. 0.6 is set so that two titles sharing three of five meaningful tokens fire and two sharing two of six do not.

One correction worth applying: require at least three meaningful tokens on both sides. A two-token title inflates its Jaccard against everything, so short titles otherwise dominate the candidate list purely by being short.

## Severity

- **High.** A collision across two different properties you own, because there is no internal linking lever, no consolidation path that does not involve losing a domain's page entirely, and you are bidding against yourself from two hosts. Also high: any three-way collision between a published page, a draft and a planned row on one keyword, because the pipeline is about to publish the same page twice more.
- **Medium.** A collision between two published pages on one site.
- **Low.** Title overlap alone, with no shared results page and no key collision.

## The five actions, exactly one per finding

1. **Consolidate.** Merge the weaker page into the stronger, redirect the weaker URL.
2. **Differentiate.** Retitle and re-aim so the intents genuinely diverge. Content changes, URLs do not.
3. **Redirect.** Retire the weaker page without merging anything, because it has nothing worth keeping but does have links or traffic.
4. **De-optimise.** Deliberately weaken one page's targeting so it stops competing: retitle away from the contested query, drop the exact-match heading, change the internal links that point at it with the contested anchor text.
5. **Leave, with a written reason.** The reason is the deliverable. Without it the same pair is re-flagged every quarter and re-argued from scratch.

**The decision rule:**

- Same intent, one page clearly stronger. **Consolidate** into the stronger.
- Same intent, both weak, neither has links. **Redirect** the newer into the older, which usually carries more history.
- Different intents, and you can state the difference in one sentence. **Leave**, and write the sentence down.
- Different intents but the titles collide. **Differentiate**, which is a retitle, not a rewrite.
- The page must exist for another reason, a product page, a policy page, a hub, and it keeps outranking the page you want to win. **De-optimise** it.
- **No redirect lever exists**, because the platform cannot serve one. Consolidate and redirect are both off the table, and differentiation is the only remaining move. Do not reach for a re-slug as a substitute: changing a live page's URL without a redirect is a self-inflicted 404 that throws away every external link the page ever earned.
- **You cannot tell which page is stronger**, because you have no ranking or traffic data. **Do not guess.** Leave both, mark the pair blocked on data, and name the exact report needed: search performance for the contested query, filtered to those two URLs. A wrong consolidation is the hardest action in this list to reverse.

## Read-only, and why

The audit produces a report. It does not edit anything, and this is a rule rather than a preference.

The correct action depends on facts the audit cannot see: which page has external links, which one converts, which one sits in the navigation. Consolidation and redirects are close to irreversible once anything points at the new location. And a bulk retitle that fails halfway leaves the inventory, the published site and the sitemap describing three different realities, which is worse than the collision it was fixing.

**Cap the report at five prioritised actions.** A report of forty findings does not get acted on. It gets skimmed, filed, and the same collisions are rediscovered next quarter by someone who assumes the last report was wrong. Everything past the cap goes into an appendix, unranked, explicitly labelled as not this quarter's work.

**The tie-break order for choosing the five:**

1. Severity, high before medium before low.
2. Number of independent signals firing. Three beats two beats one.
3. Whether a publish is imminent. A draft due Thursday outranks a two-year-old pair, because it is the only one that can still be fixed for free.
4. Value of the more valuable page in the pair, where that is known.
5. Cost of the action. At equal severity, a retitle before a merge, because a retitle is reversible.

## De-noising rules

Without these, a content pipeline reports itself and the real findings drown.

- **An item and its own descendants are one item.** A planned row, the draft written from it, and the published page are one thing at three stages. Key by a stable identity, a source row id or a slug lineage, never by title.
- **Translations under one slug are one item.** Compare only within a language. A site publishing one article in six languages generates fifteen pairwise collisions per article if language is ignored, which is enough garbage to hide every genuine finding.
- **Series pages.** Do not strip ordinal tokens: part, chapter, step, and numbers that are not years. Items differing only by an ordinal are one series, flagged once if at all.
- **Tag, category and archive pages are not articles.** Exclude them or they collide with everything.
- **Declared hub pages** legitimately share tokens with every child. Exclude them, or force them to low severity.
- **Programmatic pages** that differ only by an entity name are intentionally near-identical. Exclude the template before running.

## Worked example, compressed

Two properties under one owner: site A, a project management blog, and site B, a time tracking blog. Combined inventory of 140 items including drafts and planned rows.

Raw signals: Signal A gives 9 key collisions, Signal B gives 4, Signal C gives 22. After de-noising, which removes a six-language translation set, one three-part series and eleven draft-to-published pairs, six findings remain.

The top one:

- **Item 1**, published on site A fourteen months ago: "How to estimate project timelines".
- **Item 2**, a draft on site B: "Project timeline estimation: a practical guide".
- **Item 3**, a planned row on site A: "How to estimate timelines for a project".

Signal A fires on items 1 and 3: both normalise to `estimate|timeline` once "project" is dropped as topic noise, since it appears in over half of site A's titles. Items 1 and 2 do not collide on Signal A, because "estimate" and "estimation" are different tokens.

Signal B fires across all three: every research file cites the same two competitor URLs, on two different hosts, paths preserved.

Signal C fires on items 1 and 2: tokens {estimate, project, timeline} against {estimate, guide, practical, project, timeline} gives an intersection of three over a union of five, exactly 0.6.

Severity: high, on both counts. It is a three-way collision across published, draft and planned, and it spans two properties.

Actions. Item 3 is unwritten, so it is deleted from the plan at a cost of nothing. Item 2 is a draft on the other property, and the two properties do have genuinely different audiences, so it is **differentiated**: re-aimed at what to log during a project so the next estimate is right, which is a sentence that clearly separates it from item 1. Item 1 stays, and collects the internal links that were going to be split. Nothing is merged and no URL changes, which is fortunate, because site B is on a platform with no redirect map and consolidation across the two was never available.

**Verdict: three items resolved with a delete, a retitle and a leave. No redirect, no merge, no URL change.** That is the cheapest outcome this audit can produce, and it is only available because the collision was caught while two of the three items were still text in a spreadsheet.

## Failure modes

**Comparing raw titles.** Finds the byte-identical pairs, which nobody was going to miss anyway, and misses everything else.

**Collapsing competitor URLs to the host.** Every research file citing a large publication now matches every other one, and Signal B produces pure noise.

**Skipping the plural fold.** Half the genuine key collisions never fire and the audit looks like it found a clean site.

**Running without de-noising.** Forty findings, of which thirty-four are the pipeline seeing its own drafts, plans and translations. The report is discarded, along with the six real ones.

**No threshold on Signal C.** Every pair on a topical site is a candidate, so the output has no information in it.

**Treating a candidate as a finding.** One weak signal, acted on, and two legitimately distinct pages get merged. Undoing that costs more than the collision would have.

**Editing during the scan.** The inventory changes underneath the audit, and the second half of the report describes a site that no longer exists.

**Re-slugging instead of redirecting.** A live URL changes, nothing forwards, and every external link to it now returns a 404. This is the single most damaging thing on this page and it usually happens on the platform least able to fix it.

**Merging in the wrong direction.** The page with the links is redirected into the page without them, which throws away the only asset in the pair.

**Reporting everything.** The forty-item report that nobody actions is functionally the same as no audit, except it also consumed a day.

## What this skill does not do

- It does not confirm cannibalisation. It finds pages aimed at the same intent, which predicts it. Confirmation needs a search performance report showing two of your URLs trading positions for one query.
- It cannot see external links, traffic, conversions or navigation placement, and those are what decide the direction of a merge.
- It is blind to synonym collisions. Two pages about late payments and overdue invoices share no tokens and will never fire on any of the three signals.
- It does not execute anything. No redirects, no merges, no retitles, and on some platforms the actions it recommends are not available at all.
- It does not judge content quality. It can tell you two pages overlap and not which one is better written.
- It does not handle programmatic or templated inventories, which are near-identical on purpose and have to be excluded before it runs.
