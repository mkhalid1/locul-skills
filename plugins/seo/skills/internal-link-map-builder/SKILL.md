---
name: internal-link-map-builder
description: Builds an ordered internal link map from a supplied page inventory: which page links to which, in which direction, with what anchor text, in which section, and in which numbered batch the change ships. It resolves the crawl and canonical preconditions first, because a link pointed at a disallowed or non-canonical URL does nothing, and it separates what Google documents about internal links from the click-depth and anchor-ratio rules the field repeats without a source. This skill should be used when a set of pages needs linking together deliberately, when newly published content has nothing pointing at it yet, when a template-wide link block is about to be switched on, or when someone proposes using robots.txt to remove pages from the index.
---

# Internal link map builder

## The claim this skill is built on

An internal link map is a directed graph you author, not a property of the site you observe and report back. The product is a table in which every row is one link: a source URL, a target URL, the exact anchor text, the section of the source page the link sits in, a one-sentence reason the direction runs that way, and the numbered batch in which it ships. If what comes out is a list of orphan pages with severity scores, the work has not been done, because nobody can deploy a finding.

The obvious approach fails in three checkable ways. It builds the map before establishing whether the links can do anything, so rows point at disallowed URLs or at URLs whose canonical Google has selected as some other page. It decides existence rather than direction, when every link runs one way and a related pair very often justifies exactly one of the two. And it ships in one deploy, a change with no attribution and one remedy, reverting all of it.

The order below is load bearing: the preconditions decide which URLs are legal targets, the grouping decides which pairs are candidates, the direction test decides which half of each pair survives, and the batching decides whether anyone can say afterwards what the work did.

## Part zero. Preconditions, which void the map if they fail

Do these before writing a single row. Each is a documented mechanism, and each silently invalidates rows built on it.

**The disallow and noindex deadlock.** Google's documentation is explicit, in a callout carried at least since its update of 10 December 2025: "For the `noindex` rule to be effective, the page or resource must not be blocked by a robots.txt file ... If the page is blocked by a robots.txt file or the crawler can't access the page, the crawler will never see the `noindex` rule, and the page can still appear in search results, for example if other pages link to it." These are two separate mechanisms and they cancel each other out. To remove an already-indexed URL: allow crawling, serve `noindex`, wait for the recrawl, and only then consider a disallow. In the other order the URL stays indexed indefinitely. For the map, the rule is simpler: a disallowed URL is never a link target, and finding one puts a repair in batch 0.

**The robots.txt status code, counterintuitive in both directions.** All 4xx responses except 429 are treated as though a valid robots.txt did not exist, so Google assumes there are no crawl restrictions. A 404 is harmless on its own account, the documented equivalent of "crawl everything", which also means every restriction you wrote is gone the moment the file starts 404ing. Google separately warns against using 401 and 403 to throttle crawling. A persistent 5xx does the opposite and is staged: 12 hours of no crawling while retrying, then 30 days on the last cached copy, then either behaving as if there is no robots.txt or, if the site has general availability problems, stopping crawling. The file is cached for up to 24 hours, so an edit is not instantly effective, which is why batch 0 ships a day ahead. Last updated 8 July 2026.

**The canonical you declared is not necessarily the one Google chose.** Google's own words: "indicating a canonical preference is a hint, not a rule." The documented strength ordering puts redirects and `rel="canonical"` annotations as strong signals and sitemap inclusion as a weak one, and the methods stack. Record, for every page, the Google-selected canonical from URL Inspection alongside the URL you intended. Where they differ, the link target is the URL Google selected, or batch 0 stacks signals to change its choice. Do not use `noindex` to resolve a duplicate inside one site: the documentation says to avoid that, since it blocks the page from Search entirely.

**Links a crawler cannot follow.** Google only extracts links that are `<a>` elements with an `href`, and states that "most links in other formats won't be parsed and extracted". Its crawlers do not click buttons, so a "load more" control with no href is a discovery dead end. Check the rendered HTML, not the source: a link injected by JavaScript is fine provided the rendered result is an `<a href>`.

## Part one. Group pages by role, not by topic

Topic groups produce meshes. Role groups produce directions. Sort every page into one of four roles.

- **Entry.** The page earning the broadest demand, decided from 28 days of Search Console impressions rather than from what you intended.
- **Depth.** A narrower page answering one question inside the same group.
- **Destination.** A product, pricing or signup page. A target for a content group, rarely a source.
- **Utility.** Login, privacy, tag archives, paginated pages beyond the first. Navigation reaches these, so no editorial links.

Two pages belong in the same group when a reader who has finished one has an obvious next question the other answers. The empirical version is overlapping query sets in Search Console. Where two pages surface for substantially the same queries they are not a group but a duplication problem, handled in part two rather than by a link.

Use impressions and clicks here, not average position. Google documents that average position is the topmost position your property occupied, averaged across impressions, and that a link must get an impression for its position to be recorded at all, which is why the number can improve when weak impressions disappear and nothing ranks better.

## Part two. Decide direction, which is the actual work

For each candidate pair, in order.

**Test 1, the reader's next question, mandatory.** Write, in one sentence, the question a reader finishing the source page has that the target answers. If you cannot write it there is no link. Put the sentence in the map row; it is also the raw material for the anchor.

**Test 2, evidence direction.** Where both directions pass test 1, ship from the page with more impressions towards the page with fewer first: it reaches more readers sooner and its effect is the half you can watch. A destination page is the end of a path, so its outbound links belong to another job.

### The decision rule

Given a pair A and B, run these branches in order and stop at the first that matches.

1. **Either page is disallowed in robots.txt, or its Google-selected canonical is a different URL.** Do not link to it. Retarget at the URL Google selected, or fix the mismatch in batch 0 and re-enter the rule. Clicks, impressions and position are attributed to the canonical, so a link at the non-canonical URL is credited somewhere you are not looking.
2. **The target is a utility page.** No editorial link.
3. **You can write the reader's next question in both directions.** Link both ways, one link each, in different sections of the two pages. Two separate sentences, not a reciprocal block at the foot of each page.
4. **One direction only.** Link that way, and record that the reverse was considered and not writable. The absence is a decision, and recording it stops the pair being reopened every quarter.
5. **Neither direction.** No link. Belonging to the same group is not a reason on its own.
6. **You cannot tell.** A real state with three named causes. If the target is not yet indexed, the missing input is the URL Inspection result. If neither page has 28 days of impressions, the missing input is time. If both pages surface for substantially the same queries and you cannot say which answers better, this is not a linking problem but a decision to consolidate or differentiate: Google's site diversity system generally shows no more than two listings from one site in top results anyway, so linking them will not make both rank. In every case, put the pair on a deferred list with the missing input named and a recheck date. Never resolve a cannot-tell by adding the link.

## Part three. Anchor text, to the only documented standard

Google documents a qualitative bar and no numeric one: "Good anchor text is descriptive, reasonably concise, and relevant to the page that it's on and to the page linked to." Its documented bad examples are "Click here", "Read more", and linking a generic word such as "website" or "article". Its documented test is the useful part: read the anchor alone, out of context, and if you cannot tell what the target is about it needs to be more descriptive. Apply that to every row before it ships.

Two mechanics come from the same page. Google falls back to the `title` attribute as anchor text when there is no link text, and uses an image's `alt` text as the anchor for an image link, which is why an image-only link needs descriptive alt text rather than a filename.

There is no documented anchor text ratio, exact-match percentage or safe distribution, in either direction. Every circulating figure comes from agencies and tool vendors, and they disagree with each other. Write the anchor that describes the target inside the sentence it lives in, and let it vary because the sentences vary, not to hit a number.

Placement follows from the same standard: the anchor must be relevant to the page it is on as well as the page it points at, which an identical block of eleven links at the foot of every page cannot satisfy. Put the link where the reader's question arises, and record the section heading.

## Part four. Sequence the changes into dated batches

This ordering is a working practice, not a documented threshold, and is stated as such. Its purpose is attribution afterwards.

- **Batch 0, repair.** robots.txt status code, any disallow conflicting with a noindex, canonical mismatches using the documented stacking of redirect plus annotation plus sitemap, and any link that is not an `<a href>` in the rendered HTML.
- **Batch 1, discovery.** Every page with zero internal links pointing at it gets one, from the most relevant page in its group. First, because discovery via crawlable links is the one mechanism Google documents. "Orphan page" is not Google's terminology; the mechanism under it is.
- **Batch 2, the highest-demand group only.** Entry to depth first, then depth to depth where test 1 passed.
- **Batch 3 onwards, one group per batch. Final batch, destination links.**

One batch per deploy, each dated in the map. Do not start the next batch on the same group until the source pages have been recrawled, read from the Last crawl date in URL Inspection. IndexNow notifies participating engines sooner and Bing participates, but Google is not a participant and IndexNow appears nowhere in its documentation, so it changes nothing there.

**Crawl budget is out of scope for most sites, and that is documented rather than an opinion.** Google's guide is aimed at large sites of 1 million or more unique pages changing moderately often, about weekly, and at medium or larger sites of 10,000 or more unique pages with very rapidly changing content, daily. Google then tells everyone else not to read it: "If your site doesn't have a large number of pages that change rapidly ... you don't need to read this guide." It is set per hostname, not per registered domain, and crawling does not imply indexing. Below those thresholds, drop crawl efficiency from the reasoning entirely.

## The map row format

One row per link, as CSV or a markdown table, so it can be diffed:

`source_url | target_url | anchor_text | placement | reader_question | batch | applied_on | verified_on`

Plus a second table of deferred pairs: `page_a | page_b | missing_input | recheck_on`.

Verify each row after its batch ships: fetch the rendered source page, confirm an `<a href>` whose href is exactly the target URL with no redirect hop, and confirm the anchor matches the row character for character. Then fill in `verified_on`. A row without a verification date is not finished.

## Worked example

A shift-scheduling tool for restaurants, 240 pages. One group, overtime rules, with 28-day impressions:

| Page | Role | Impressions |
|---|---|---|
| /guides/overtime-pay-rules/ | Entry | 4,100 |
| /guides/overtime-pay-rules/california/ | Depth | 900 |
| /blog/calculating-overtime-for-split-shifts/ | Depth | 120 |
| /blog/overtime-alert-checklist/ | Depth | 0, published 9 days ago |
| /features/overtime-alerts/ | Destination | 260 |

Preconditions turn up three things. `/tag/overtime/` carries a `noindex` and is also disallowed, so the directive has never been read and the URLs are still indexed: batch 0 removes the Disallow and leaves the noindex to be crawled. URL Inspection reports the guide's Google-selected canonical as an older `/blog/overtime-pay-rules/`, so the annotation alone did not win: batch 0 stacks a 301 from the old URL, keeps the annotation and adds the guide to the sitemap. And the checklist page has nothing pointing at it, reporting as discovered but not indexed.

Directions. Entry to California passes test 1 one way only, "does this apply in my state?", so branch 4. Entry to the checklist passes both ways, but it is the checklist's only inbound link, so the forward link ships in batch 1 and the reverse in batch 2, branch 3. Entry to the destination page passes on "how do I stop this happening without checking by hand?" and ships last. California and split shifts fail test 1 both ways, and 120 impressions will not break the tie, so branch 6: deferred, missing input recorded as 28 days of impressions, with a recheck date. Anchors go through the documented test: "overtime rules in California" passes read alone, "read more about state rules" does not.

**Verdict.** Batch 0 is three rows of repair, one a deletion rather than an addition. Batch 1 is a single row, the checklist page's first inbound link. Batch 2 is five rows. One pair is deferred with a named missing input and a date. Nine link rows across three dated deploys, each carrying a reader question and a verification date. Crawl budget appears nowhere in the output, because 240 pages is two orders of magnitude below the smaller of Google's documented thresholds.

## What the field repeats that Google does not document

Name these as unevidenced when they come up, and do not build the map on them.

- **Click depth and the three-clicks rule.** Neither "click depth" nor "crawl depth" is Google terminology, and neither phrase appears in the Search Central documentation checked as of 30 August 2026, including the crawl budget guide, the links guide, the starter guide and the pagination guide. No primary source.
- **Link equity flowing in measurable quantities.** Google documents that PageRank exists and remains part of its core ranking systems, and that link analysis helps determine what pages are about. It has never published a model of internal distribution, a per-link decay or a per-page budget. The plumbing vocabulary is a practitioner metaphor built on a 1998 paper.
- **A required or maximum number of links per page, and anchor text ratios.** No documented figure for either, in any direction. The old "under 100 links" line was removed years ago and was a crawler practicality note, not a ranking rule.
- **PageRank sculpting with `nofollow`.** Changed in 2009 so nofollowed links stay in the divisor and the value evaporates rather than being redistributed; in September 2019 `nofollow`, `sponsored` and `ugc` became hints rather than directives.

## Failure modes

**The disallow and noindex deadlock.** From the outside: URLs stay indexed for months with no description, and every recheck of the source shows the `noindex` sitting there apparently doing nothing. It is doing nothing, because the crawler is not permitted to read it.

**The robots.txt 404 fire drill, and the 5xx nobody noticed.** A 404 gets escalated as an emergency when it is documented as harmless, while a persistent 5xx that halts crawling for 12 hours and then runs on a cached copy gets logged as a blip. From the outside: crawl stats fall away with no content change and no robots.txt edit in the log.

**The map pointing at non-canonical URLs.** From the outside: URL Inspection reports a Google-selected canonical you did not choose, and the Performance report attributes clicks to a URL that appears nowhere in your map.

**The invisible link.** The related-posts widget renders through a click handler or a button with no href. From the outside: it works for people and is visible in the browser, never appears in a crawl of the rendered page, and the target keeps reporting as discovered but not indexed.

**Anchors engineered to a ratio.** From the outside: sentences get rewritten around the anchor rather than the other way round, and the same four words appear as the anchor on nine pages regardless of what each was saying.

**Crawl-budget reasoning on a site that does not qualify.** Pages pruned and pagination nofollowed on a 900-page site. From the outside: the pruned URLs 404, links to them break, crawl stats do not move, and the site was never within an order of magnitude of the documented thresholds.

**The reciprocal mesh.** From the outside: an identical block of eleven links at the foot of every page in a group, nobody clicking any of them, and the one useful next step buried in the middle.

## What this skill does not do

- It does not crawl your site. It needs an inventory, and one built from a sitemap will not contain the pages nothing links to, which are the ones batch 1 exists to fix. Run a crawler first.
- It cannot see which URL Google selected as canonical, or whether a page is indexed. Those come from URL Inspection, and several branches wait for that input rather than guessing.
- It does not predict ranking change. No published model exists for how internal links redistribute anything inside a site, so the map is justified by discovery and by the reader's next question, never by a promised position.
- It does not write or repair the target pages. If the target does not answer the question its anchor promises, the anchor is a lie and the fix is a content job.
- It does not handle external links, link acquisition or disavowal, and does not restructure navigation beyond flagging links a crawler cannot follow.
- It cannot tell you what Google's documentation says today. Every fact here was checked on 30 August 2026 against Google Search Central, whose robots.txt, block-indexing and canonicalisation pages each carry a last-updated stamp and do change. Recheck before betting a migration on one.
