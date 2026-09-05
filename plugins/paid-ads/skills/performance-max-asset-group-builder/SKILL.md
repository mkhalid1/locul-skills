---
name: performance-max-asset-group-builder
description: Produces one or more fully specified Performance Max asset groups from a product or service line, an audience and a set of creative constraints. It decides how many asset groups the campaign needs before any asset is written, then writes every headline, long headline and description, specifies the image, logo and video slots, sets the business name and call to action, lists the search themes, composes the audience signal and sets the exclusions. This skill should be used when a Performance Max campaign is about to be built or restructured, when a single asset group needs splitting, or when search themes and audience signals are about to be treated as targeting.
---

# Performance Max asset group builder

## The claim this skill is built on

Performance Max is steered, not aimed. It has no manual keyword targeting, and almost every disappointment with it follows from operating it as though it had. Search themes and audience signals are inputs to a system that decides, not filters that constrain it. Google says so: "Performance Max may show ads to relevant audiences outside of your signals if they have a strong likelihood of converting" (answer/14530785, checked 31 August 2026).

Three failures follow from that, and all three are checkable before launch. The first ports an exact match keyword list into search themes, against Google's own advice. The second leaves the campaign on one asset group, since one is all you can create before publishing. The third launches uninstrumented, then finds the reporting cannot answer the questions asked of it.

Order matters mechanically: assets are written per asset group, and an asset group cannot be shared between campaigns. Decide the split first and each set is written once.

## Part one. Decide the split before writing a single asset

Google publishes the bounds and nothing in between: a campaign takes 1 to 100 asset groups (Google Ads API system limits, last updated 2026-08-19 UTC), and only one can exist before publishing.

Google prefers more groups inside one campaign to more campaigns, "if the asset groups have similar bidding, location, and language targets", because "The more you split up traffic, the more you restrict Smart Bidding from finding the most conversions at your target." The documented basis for splitting is content: "category, theme, language, or target audience".

**The rule this skill applies, which is the author's and not Google's.** Google names the basis for a split but publishes no test for when a difference is big enough, so this file sets one: a candidate earns its own asset group only if it changes at least two of four things, the words, the images, the landing page, the audience signal. One changed dimension is a variation and belongs in the existing group as extra assets. Two or more is a different offer, and deserves its own Ad Strength.

**When splitting starves the groups instead.** Google publishes no recommended number of asset groups and no minimum spend or conversion volume per group, so "three to five groups" and "thirty conversions a month" are third-party figures. An asset group has no budget of its own either; budget sits at campaign level. The cost of a split is therefore not divided money, it is a second complete, non-duplicated asset set and a second Ad Strength to feed. **Fill the second group by copying the first group's headlines and you have built two copies of one group.**

## Part two. The field specification you emit

Every number below is Google's own, checked 31 August 2026, from the canonical specifications page (answer/17091269) unless another page is named.

**The qualifier almost everyone drops.** Google defines the Recommended column as "Minimum: required to publish an asset group that does not have a Google Merchant Center feed attached." These are publish gates for non-feed groups, not universal requirements: a feed-backed group launches with no creative, at the cost of "a Poor Ad Strength rating".

| Field | Limit | Min | Recommended | Max | Required |
| --- | --- | --- | --- | --- | --- |
| Headline | 30 chars, one of 15 or fewer | 3 | 11+ | 15 | yes |
| Long headline | 90 chars, aim 30+ | 1 | 2+ | 5 | yes |
| Description | 90 chars | 2 | 4+ | 5 (conflict below) | yes |
| Business name | 25 chars | 1 | not stated | 1 | yes |
| Call to action | automated, or pick from a list | 1 | 1 | not stated | yes |
| Final URL | 1 URL | 1 | 1 | 1 | yes |
| Display URL path | 15 chars each | 1 | 2 | 2 | no |

Characters in Korean, Japanese or Chinese count twice, and the business name must match your domain or a legally verified business name, with no symbols or promotional text.

| Image slot | Recommended | Minimum | Count | Required |
| --- | --- | --- | --- | --- |
| Horizontal 1.91:1 | 1200 x 628 | 600 x 314 | 4+ | yes |
| Square 1:1 | 1200 x 1200 | 300 x 300 | 4+ | yes |
| Vertical 4:5 | 960 x 1200 | 480 x 600 | 2+ | no |
| Square logo 1:1 | 1200 x 1200 | 128 x 128 | 1 | yes |
| Horizontal logo 4:1 | 1200 x 300 | 512 x 128 | 1 | no |

**The 20 and the 5 are totals across all ratios, not per ratio.** The specs table repeats "Add up to 20 images" on three rows, which reads as sixty. A second page settles it: "Maximum total number for all images: 20", "Maximum total number for all logos: 5" (answer/10724748). JPG or PNG, content inside "the center 80% of the image".

**Video is not required, which does not mean absent.** All three rows, 16:9, 1:1 and 9:16, are optional, each "10 seconds or more", 15 maximum across all types. With none uploaded, "one or more videos may be auto-generated from the assets in your asset group". There is no documented opt-out, and the remedy is coverage: "you must upload your own custom videos in all required formats (horizontal, square, and vertical)" to "prevent the system from auto-generating AI videos". One vertical video of 10 to 60 seconds keeps the group Shorts-eligible.

**Two things moved, one never existed.** Business logos and business information were "consolidated at the campaign level" in 2025, so the business name and logo rows above are set once for the campaign rather than tuned group by group. The circulated "five descriptions including one 60-character short description" is in no current Google table; that field belongs to Responsive Display Ads.

**Where Google contradicts itself**, checked 31 August 2026: descriptions, specs page 5 against "How asset groups work" 4; videos, specs page 15 against best practices 5; file size, 5120 KB against 5 MB in the note beneath. Build to the lower figure and let the interface break the tie.

Sitelinks cap at 20, 6 or more recommended, and where the primary goal is call or lead the group reads "Incomplete" without call or lead assets.

## Part three. Search themes, and the routing question

**The cap is 50 per asset group, not 25.** Google reads "Add up to 50 search themes per asset group". Archive captures put the change between 25 July 2025, still reading 25, and 8 August 2025, reading 50.

**What a theme should be.** Google asks for themes that "offer new signals to the AI", says "Avoid duplicates or close variants", and prefers breadth: "Instead of using specific, long-tail terms like 'red running shoes size 10,' use a broader term like 'running shoes'." A pasted keyword export is the opposite.

**The routing question decides whether this campaign fights your Search campaigns.** Google publishes a four-tier ladder on two live pages. One, an exact match keyword identical to the query wins: "the Search campaign is prioritized over any other broad or phrase keyword or a Performance Max campaign". Two, phrase and broad match keywords share priority with identical search themes: "Search themes will have the same prioritization as your phrase match and broad match keywords". Three, AI-based ad group prioritisation on relevance. Four, Ad Rank. "Identical" covers spell-corrected terms but not "plurals or synonyms". Both popular claims are therefore wrong: Performance Max does not automatically beat Search, and search themes are not exact match.

**The exceptions nobody quotes.** The ladder stops protecting a Search keyword under low search volume status, when "All creatives or landing pages for the ad group are disapproved", when targeting is not met, when "The campaign is limited by budget", and where ad format differs, since "The preference rules above don't apply to any Shopping campaign or other parts of the Google inventory". A sixth covers AI surfaces: searches on "Lens, AI Mode, AI Overviews, or auto-complete searches... are not considered technically identical to a keyword", so "keywords may not automatically be prioritized". There, exact match does not reliably beat an asset group.

**What follows for composition**, inferred from the ladder rather than stated by Google. Write themes for demand your Search campaigns do not already hold with an exact match keyword, since on identical queries Search wins anyway, and keep an overlap only as a deliberate fallback for those six conditions. Exclusion is a separate object and a separate job: Performance Max negative keywords "are applicable to Search and Shopping inventory only", and display and video exclusions run through the Content Suitability Centre instead. Writing the list itself belongs to the negative keyword list builder.

## Part four. The audience signal

Compose the signal, do not treat it as targeting. Google's stated preference for its contents: "provide your data about previous purchasers, and use custom segments to provide insight into the search keywords, web URLs, and apps your customers typically engage with." Signals are "typically applied at the asset group level".

Three requirements gate a usable list: more than 1,000 active and eligible users, matching the campaign operating system type, refreshed every 540 days. Two latencies gate the reading: "up to 2 weeks" for the models "to fully integrate and optimize new audience signals", and 24 to 72 hours for a list's size to populate accurately.

Insights labels top segments "Signal" or "Optimized", the second meaning ones "you did not add yourself, but that AI helped find for you". Value appearing under Optimized is the documented mechanism working, not a targeting fault. Google publishes no proportion, though: no figure for how heavily a signal weights the model and no statement of how much of a result should sit under either label.

## Part five. Exclusions and the URL setting

**Brand exclusions** are opt-in: account-level lists applied per campaign, capped at 10 lists per campaign and 5,000 brands per list. Exclusions beat inclusions, and a brand must already exist in Google's brand library. Three live Google pages disagree on whether YouTube search inventory is covered; all three agree the exclusions cover Search and Shopping. They are not an account-wide brand block and do not reach Display, YouTube in-stream, Gmail or Discover.

**Final URL expansion is on by default.** With it on, Google "may replace your Final URL with a more relevant landing page based on the user's search query, and generate a dynamic headline, description, and additional assets to match your landing page content", which makes it a copy setting as much as a destination setting. A page feed does not restrain it; only turning expansion off does, after which the campaign "will only send users to the URLs provided in your page feed and asset groups". "Automatically created assets" is now called "Text customization", so anything using the old label describes a retired interface.

## Part six. The decision rule, including the branch where you cannot tell

The input is the number of conversions in the last 30 days on the exact action this campaign will optimise for. Google publishes no conversion floor for Performance Max at all, so the 30 used below is not one. It is Google's general guidance on reading Smart Bidding results, "we recommend you measure performance for the last 30 days, including at least 30 conversions", borrowed here as a readability threshold rather than an eligibility gate.

- **30 or more on that action, by the borrowed threshold above.** Build as specified, and ship however many groups the part one rule yields.
- **Between 1 and 29 on that action, a band this file sets and Google publishes nowhere.** Ship one asset group with a complete asset set, whatever the split rule says, and add the second once the count clears 30. The published cost of splitting is restricted Smart Bidding, and it bites hardest when there is least to learn from.
- **Zero on that action, or the action was created inside the last 30 days: you cannot tell.** The campaign cannot learn from an event that has never happened, and a more careful specification does not change that. Confirm the action fires end to end, or point the campaign at one with history. If the count cannot be stated without opening a report, pull it first: it is routinely assumed and routinely wrong. If you launch anyway, treat the first fortnight as instrumentation rather than a test, on Google's own timings: up to two weeks for signals to integrate, and "2-3 weeks before deciding to replace low performing assets".

## Part seven. Instrument before launch

Performance Max reporting has holes, and Google names them itself. There is no per-channel budget control: "you can't directly control budget allocation per channel". Placement reports carry impressions only, exclude Search altogether, and are a safety tool rather than a scoreboard: they "shouldn't be used to evaluate performance". Asset-level conversions do not reconcile either: "the number of conversions across all assets will not equal total conversions for the campaign". Google documents no impression share metric.

What does exist is worth setting up beforehand. The search terms report is real, sits under Insights and reports, segments by ad format and landing page, and has a hard floor: "Data for the Performance Max search terms report is available starting from March 2023." Channel performance reporting reached all Performance Max campaigns per Google's announcement of 6 November 2025. Asset group reporting adds an "Added by" column separating your assets from ones Google created or enhanced.

So, three things before you publish, the first and third inferred from the holes above rather than published by Google. Give every asset group a distinct Final URL, since the report attributes each term to a landing page, and that is what lets you tell one group's terms from another's. Set Final URL expansion deliberately rather than inheriting the default. Record the asset inventory you uploaded, so the "Added by" column can be read against it.

## Worked example, compressed

A firm sells two things to independent clinics: a subscription rota-planning tool and a one-off setup service. No Merchant Center feed, so the minimums are publish gates. Two pages, two audiences, two vocabularies. The lead conversion action shows 22 conversions in the last 30 days.

**Split.** This file's content rule says two groups: each candidate changes the words, the page and the audience signal, three of the four dimensions. The decision rule overrides it. At 22 conversions the account is in the middle branch, so one group ships now, for the subscription tool, and the other is written and held.

**The emitted group.** 11 headlines at 30 characters or fewer, one of them "Rota software" at 13 to satisfy the short-headline requirement; 2 long headlines between 30 and 90; 4 descriptions; a 25-character business name matching the domain; a selected call to action rather than the automated default, since the offer is a trial; 2 display paths. Images: 4 horizontal, 4 square, 2 vertical, 10 against the cap of 20, content inside the centre 80 per cent, plus 1 square and 1 horizontal logo. 3 videos, one per orientation, the vertical one 45 seconds. 6 sitelinks plus lead assets, since the primary goal is lead.

**Steering.** 14 search themes of a possible 50, broad rather than long-tail, none identical to an exact match keyword the Search campaign already owns, with two deliberate overlaps kept as a fallback for the AI surfaces. Audience signal: a purchaser list above 1,000 active users plus a custom segment from the search terms and competitor URLs those customers engage with. A two-brand exclusion list, Search and Shopping only. Final URL expansion off, or the blog absorbs the traffic and generates its own headlines.

**Verdict.** One asset group published and fully specified, the second written and held. Instrumentation first: distinct Final URL, expansion off, asset inventory recorded. Re-run the split decision when the lead action clears 30 conversions in 30 days, not on a calendar date.

## Failure modes

**The ported keyword list.** Fifty search themes that are the Search campaign's exact match list with the brackets stripped. It looks thorough and adds close to nothing, since Google prefers the identical keyword in Search anyway.

**The signal read as a fence.** Somebody reports the campaign is "showing to the wrong people" and concludes targeting is broken. Insights shows value under "Optimized" rather than "Signal", which is the documented mechanism, and Google publishes no proportion that would make either share alarming.

**Sixty images commissioned.** The specs table repeats "Add up to 20 images" on three ratio rows and a shoot is briefed for sixty. The cap is twenty in total, so two thirds of it cannot be uploaded and the overspend surfaces at build time.

**Pruning on cost per acquisition.** An asset group with a higher CPA is paused, against Google's explicit advice that such groups still contribute and should not be removed on that basis. Campaign conversions fall and the cause is read as seasonality.

**The brand exclusion assumed to be account-wide.** A competitor brand list is applied and the brand's name keeps appearing in Display, Gmail and Discover reporting, because Performance Max exclusions do not reach that inventory.

## What this skill does not do

- It cannot see your Google Ads account, confirm a conversion action has ever fired, or check what your Search campaigns already capture. A campaign optimising against a broken conversion event will still look confident.
- It does not own Search campaign structure, negative keyword lists, the bid strategy or the landing page. Those are separate jobs, and this file stops at naming the Final URL.
- It cannot promise any label, cap or default here is current today. Google renames Performance Max settings regularly, so verify against the live interface first.
