---
name: google-search-rsa-writer
description: Produces a complete, pastable responsive search ad asset set from a keyword theme, an offer and a landing page: up to 15 headlines and 4 descriptions mapped to a coverage matrix of distinct jobs, both display paths, a pinning map costing each pin in lost combinations, and the asset list. This skill should be used when a Google Search ad group needs its ad written or rebuilt, when a disclaimer or brand mention must appear in every impression and someone is deciding where to pin it, or when an existing responsive search ad needs its remaining headline slots filled without duplicating the ones already there.
---

# Google Search RSA writer

## The claim this skill is built on

A responsive search ad is not an ad. It is a parts bin plus a combination engine, and the engine's only raw material is how different the parts are from one another. Google's documentation, checked on 31 August 2026, lets you supply up to 15 headlines and 4 descriptions for one ad. Its API says these ads "are displayed with three headlines and two descriptions", while the help centre is more careful: the second headline, third headline and second description "aren't guaranteed to appear in your ads", and "a minimum of one headline and one description will be selected to show". Treat three-and-two as a ceiling, not a promise.

The engine spends its life choosing which three of your fifteen to assemble against a query, so the common failure is handing it fifteen versions of one claim. Asked for fifteen headlines, a competent writer returns fifteen good ones about the same benefit, all true, all inside 30 characters, none adding a dimension the others lacked. That set starves the mechanism that made the format worth adopting.

The fix is not better writing. It is a coverage matrix built before a word is written. The matrix is the asset; the headlines are what fill it.

## The container, with the published numbers

All documented by Google and read on 31 August 2026.

- **Counts and lengths.** Up to 15 headlines and 4 descriptions, minimum 3 and 2 to publish. Headlines 30 characters, descriptions 90, each of the two path fields 15. Limits do not change per language, but "each character in double-width languages like Korean, Japanese, or Chinese counts as two toward the limit instead of one".
- **Display paths.** `path1` and `path2` in the API, appended to the domain from your final URL, which is not itself editable.
- **The cap is not a display guarantee.** Google "may need to shorten your text, usually with an ellipsis", and text in wide glyphs "may be wider than the space available on some browser sizes".
- **Ads per ad group.** A hard cap of 3 enabled responsive search ads, with Google recommending at least 2 rated Good or Excellent, each with a unique final URL. The familiar advice to run three or four per group traces to a Google page about expanded text ads, uncreatable since 30 June 2022, so four is now impossible.
- **Assets can leave the ad.** Under what Google calls Enhanced Flexibility, headline text "may show at the beginning of the description", up to 2 unused headlines can serve as link-based assets in space previously reserved for sitelinks, and unused assets from another ad in the same ad group can serve pointing at that ad's final URL. So an asset written for headline duty may end up doing sitelink duty, and has to stand alone.

## Part one. Fill the coverage matrix before writing

Assign the jobs first, then write into them. The order matters: it is far easier to write into an empty benefit slot than to notice at headline twelve that you have six benefits and no objection handler.

| Slot | Job | Carries |
| --- | --- | --- |
| H1 | Query echo, literal | The theme, near verbatim |
| H2 | Query echo, variant | A second phrasing, same intent |
| H3 | Primary outcome | The result, not the feature |
| H4 | Secondary outcome | A different result |
| H5 | Named feature | One checkable capability |
| H6 | Second named feature | A different job to be done |
| H7 | Proof | A number, credential or count |
| H8 | Objection, money | Price, contract, refunds |
| H9 | Objection, effort | Setup, migration, integration |
| H10 | Offer | The term on the table |
| H11 | Brand | The name, alone |
| H12 | Audience qualifier | Who it is for |
| H13 | Speed | Time to first value |
| H14 | Category or comparison | What it replaces |
| H15 | Call to action | The next step, as a verb |

Descriptions get four jobs at 90 characters: **D1** the pledge you would pin if you had to, since Description position 1 is the only description slot Google guarantees will show; **D2** the outcome plus its proof; **D3** an objection answered and the risk reversed; **D4** offer terms plus the call to action. These labels are jobs, not positions. Nothing puts the asset you wrote as D1 into Description position 1 except a pin, which is the next section.

Three tests make the matrix real, and they are the author's construction rather than anything Google publishes.

1. **The duplicate test.** Strip stop words from any two headlines. If the remaining content words are the same set, one is not doing a distinct job however differently it reads.
2. **The triple read.** Read any three headlines in a random order as one ad. Unpinned assets have no guaranteed order or adjacency, so a headline that only works beside one particular other headline is broken.
3. **The length spread.** Do not push every headline to 29 characters. Three compete for a finite width, and Google truncates with an ellipsis.

## Part two. Pinning, priced

Google publishes no combination formula and no quantified performance penalty for pinning, so what follows is derived from its published counts, and counts arrangements the engine could assemble rather than claiming anything about performance.

Fifteen headlines into three ordered slots gives 15 x 14 x 13 = 2,730 arrangements. Four descriptions into two ordered slots gives 4 x 3 = 12. Together, 32,760.

- Pin one asset to Headline position 1: 1 x 14 x 13 = **182** headline arrangements. One fifteenth of what you had, a fall of about 93 per cent.
- Pin three different assets to that same position, Google's documented workaround: 3 x 12 x 11 = **396**. The three pinned assets can no longer fill positions 2 or 3, because Google documents that pinning an asset "causes it to show only in that specific position", so the pool behind them drops to 12. That is roughly a seventh of the unpinned figure and more than double what a single pin leaves, which is the arithmetic behind Google's own wording: "if you must pin an asset, pin several unique versions to the same position". It warns that near-identical text in one position "blocks A/B testing and lowers your Ad Strength".
- Pin one asset each to Headline positions 1 and 2: 1 x 1 x 13 = **13**, under half of one per cent.
- Pin every position: Google states unpinned headlines and descriptions "won't show" at all.

**The asymmetry that decides where a pin goes.** Google documents that content pinned to Headline position 1, Headline position 2 or Description position 1 "will always show", while content pinned to Headline position 3 or Description position 2 is "not guaranteed to show in every ad". Its instruction is explicit: "If you have text that should appear in every ad, then you must pin it to either Headline position 1, Headline position 2, or Description position 1." A disclaimer pinned to Description position 2 looks locked in the interface, but Google guarantees nothing about that slot appearing, so impressions can serve without it. Several assets pinned to one position rotate between themselves.

Two Google pages sit in tension here. The RSA page says the second headline and second description "aren't guaranteed to appear in your ads", while the pinning rule says content pinned to Headline position 2 "will always show". Reading the pinning page as the more specific one is the author's judgement, so for genuinely mandatory text prefer Headline position 1 or Description position 1, which no page contradicts.

**The decision rule.**

- **A regulator, a contract or a legal reviewer requires exact text in every impression.** Pin, to Description position 1 for a disclaimer, which is Google's own example, or Headline position 1 or 2 for a mandated brand or licence mention. Write two or three unique compliant phrasings and pin all of them there, so the slot rotates.
- **A mandatory brand mention with no wording constraint.** Pin to Headline position 2 rather than 1. Google names both as always showing when pinned, and this leaves the highest-value slot free for the asset most likely to echo the query. That preference is the author's reasoning, not Google's.
- **Somebody wants control of the message.** Do not pin. Google's best-practice page answers this directly: "For general ad campaigns, no."
- **You cannot tell, because nobody can name the document.** If no one can point to the clause, regulation or approved-copy list requiring the text, you do not have a pinning case, you have a preference wearing one. Ship unpinned and ask for the source. A pin added on a maybe costs 93 per cent of the arrangement space permanently, and nobody goes back to check.

## Part three. Keyword insertion, and how it produces nonsense

The syntax is `{keyword:default text}`. Google's capitalisation table is more granular than the interface suggests: `keyword` gives "dark chocolate", `Keyword` gives "Dark chocolate", `KeyWord` gives "Dark Chocolate", `KEYWord` gives "USA Chocolate", `KeyWORD` gives "Chocolate Made In USA".

The documented failure mode is the fallback rule, and it is not truncation. Google's own example: with `Buy {KeyWord:Chocolate}` and a triggering keyword of "gourmet chocolate truffles", the ad renders "Buy Chocolate", because the keyword is too long to fit. Over-length substitution silently reverts to the default, so the default is not an edge case, it is what every query whose keyword will not fit sees, and it has to be a headline you would have written anyway.

Three further constraints. Insertion places the **keyword, not the search term**, and behaves identically on all match types, so a broad match keyword can put text in the ad bearing little resemblance to what was typed. Trademarks restricted under Google's trademark policy will not insert. It cannot be used in the final URL, though it can be used in a display path. Google names the nonsense problem itself, listing "Nonsense: Inserted keywords need to make sense in context" alongside "Incorrect grammar", and leaves liability with the advertiser.

**The rule that follows.** Insertion is safe only when every keyword in the ad group is grammatically interchangeable in one frame. If the group mixes noun phrases with verb phrases, or singular with plural, decline it and write the headline out literally.

## Part four. Ad Strength, stated plainly

Ad Strength runs Incomplete, Poor, Average, Good, Excellent. Under a heading Google itself titles "Common misconception", it states: "Ad Strength is a feedback tool for asset diversity and combination testing. It isn't used to calculate Ad Rank, Quality Score, or auction wins." The same page adds that Ad Strength "doesn't determine whether your ad is eligible to serve."

Google does publish a performance figure alongside it: advertisers improving from Poor to Excellent "see 15% more conversions on average". The footnote reads, in full, "Source: Google Internal Data. Date range: August 15, 2025 to August 20, 2025". Five days, no sample size, no methodology, from the party that builds the metric, and the same figure appears elsewhere in Google's documentation in a form that adds clicks. Treat Excellent as a checklist, not a target.

The three inputs Google names are **Asset-Keyword Relevance**, **number of headline combinations**, and **sitelink sufficiency**, where "having 6 or more sitelinks at the account, campaign, or ad group level improves your score", counting both uploaded and dynamic sitelinks in that branch.

Ship the sitelinks with the ad, because that third input is the one nobody counts. At least 2 are needed for any to show, up to 6 appear on desktop and 8 on mobile, text is capped at 25 characters (12 in double-width languages), and two with the same or similar text will not serve together.

One mechanic explains most Poor ratings: "A keyword must be contained entirely within a single headline to improve Ad Strength. It can't be split across two headlines." For a theme too long for the field, Google's instruction is to "place them in the 90-character description fields instead". Incomplete is a different thing, with only three documented causes: a missing final URL, a missing ad group, or no keywords in the ad group.

## What you hand over

One block, pastable into the ad builder or a bulk sheet: 15 headlines, each numbered against its matrix job with its character count; 4 descriptions, same treatment; `path1` and `path2` with counts; the pinning map, one line per pin naming the slot, the document requiring it and the arrangement cost, or a single line reading no pins and why; the insertion decision, taken or declined, with the grammar test that settled it; and the sitelink count in the branch. Anything knowingly declined goes at the bottom so nobody reopens it.

## Worked example, compressed

A subscription invoicing tool for freelance designers. Theme: "invoicing software for freelancers". Offer: 14-day trial, no card. Landing page path: `/invoicing/freelancers`.

**First constraint, caught before writing.** The theme is 34 characters, so it cannot fit a 30-character headline, and splitting it across two would not count for Asset-Keyword Relevance, because Google requires a keyword to sit entirely within one headline. The documented route is the description. H1 becomes the longest true fragment that fits, "Freelance Invoicing Software" at 28, H2 takes a variant intent, "Send Invoices, Get Paid Fast" at 28, and the full phrase goes verbatim into D1.

**A sample of the filled matrix.** H3: "Paid In Days, Not Months" (24). H7: "Used By 40,000 Freelancers" (26). H8: "No Card. No Contract." (21). H9: "Set Up In Under 10 Minutes" (26). H10: "Free For 14 Days" (16). H12: "Built For Designers" (19). Note the spread, 16 to 28 characters, and that no two share a content-word set.

**Pinning.** The finance lead wants the trial terms in every impression, but nobody can produce a regulation or contract clause requiring them, so the "you cannot tell" branch fires: this is a preference. Ship unpinned, carry the terms in D4. Paths: `path1` = `invoicing` (9), `path2` = `freelancers` (11).

**Verdict.** Ship 15 headlines, 4 descriptions, no pins, both paths filled, six sitelinks in the branch. Ad Strength will probably raise an action item about keyword coverage, knowingly declined: the theme is 34 characters and lives in D1 by design, which is what Google instructs. Do not rewrite good assets to move a meter Google says is not an auction input.

## The first read, after it has been live

Google's named instrument is the combinations report, which shows which arrangements served most often. Two things frame it. Google publishes no number for how many impressions, clicks or conversions make a comparison meaningful; its guidance is only to "wait until you have served enough impressions to be confident in your results". And it advises against judging these ads at ad level at all, directing you to "focus on incremental impressions, clicks, and conversions" for whole ad groups and campaigns.

So set your own floor and label it as yours. One derivation: with 15 headlines and three shown slots, uniform selection would put each headline in three fifteenths, 20 per cent, of impressions, so at 500 ad impressions a uniformly chosen headline would be expected about 100 times. A headline with zero appearances by then is being declined, not unlucky. Use 500 impressions as the floor at which absence becomes informative. Google publishes no such figure; this is arithmetic, not evidence.

Then branch:

- **At or above the floor, a handful of arrangements taking most impressions.** The engine has found its preference. Replace the headlines that never served with genuinely different matrix jobs, not near-copies of the winners, which removes the diversity that let a winner emerge.
- **At or above the floor, impressions spread widely.** The set is doing its job. Leave it and read at ad group level, where Google says the signal lives.
- **Below the floor.** You have not learned anything yet, and this is the branch most people get wrong, because a combinations report with three rows looks like a finding. Do not edit. An edit replaces the asset set the report was accumulating against, so what it says afterwards describes a different ad and the count restarts at nothing.
- **Ad Strength reads Poor and you want to treat that as the reading.** It is not one. Check three causes: fewer than six sitelinks in the branch, a keyword split across two headlines, and near-duplicate pins in one position. If none apply, leave it.

## Failure modes

**The fifteen synonyms.** Every slot filled with one claim reworded. The ad reads well and the count is satisfied, so nothing flags. The tell is a combinations report where every arrangement says the same thing three times.

**The wasted pin.** A disclaimer pinned to Headline position 3 or Description position 2. The interface shows it locked, which reads as done, but Google documents those positions as not guaranteed to show, so the ad can serve without the disclaimer and nothing reports that it did.

**Insertion on a mixed ad group.** Keyword insertion left on where keywords mix noun and verb phrases. It surfaces as ungrammatical headlines on some queries and the default text on every long-tail one, which looks like a fallback working rather than headlines nobody chose.

**Chasing Excellent.** Assets rewritten to move the meter, usually by adding keyword-heavy near-duplicates. The rating rises, diversity falls, and the missing input was often sitelinks rather than anything in the ad itself.

**The split keyword.** A long theme spread over two headlines so it "appears in the ad". Asset-Keyword Relevance stays low because Google requires the keyword inside one headline, and nothing in the preview shows why.

**The stale path.** Display paths naming a section the page no longer has. Never flagged, because Google's destination mismatch policy is about domains rather than page content. It shows up as clicks that bounce.

## What this skill does not do

- It does not choose keywords, set match types or write negatives. A perfect asset set on an ad group buying the wrong queries loses money efficiently.
- It cannot see your account, open the combinations report, say which headline never served, or confirm a conversion action is firing. An ad optimising against a broken conversion event looks exactly like one that works, and diagnosing that is engineering.
- It does not rewrite the landing page, which is where most wasted spend goes. It writes the promise; making the page continue it is separate work.
- It cannot predict ad review. Trademark restrictions and restricted verticals are decided by Google at review time.
- It does not know Google's interface on the day you read this. Every number was checked on 31 August 2026, and Google leaves stale pages live, so check anything load-bearing against the live documentation.
