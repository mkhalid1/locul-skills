---
name: google-ads-negative-keyword-list-builder
description: Builds tiered, paste-ready Google Ads negative keyword lists from a business description, a campaign and ad group structure, and a search terms export where one exists. It produces an account-level list of terms that can never be relevant, campaign-level lists for terms wrong for one intent but right for another, and ad group lists that stop your own ad groups competing for the same query, with an explicit match type on every single line and a conflict check against the active positive keywords. This skill should be used when a search, shopping or Performance Max account needs query control written or rebuilt, when a search terms export has just been pulled, or when an ad group's impressions have collapsed for no visible reason.
---

# Google Ads negative keyword list builder

## The claim this skill is built on

Positive keywords expand and negative keywords do not. Google publishes both halves and never reconciles them. On close variants: "By default, all keyword match types are eligible to match to close variants. There's no way to opt out." On negatives: "Negative keywords won't match to close variants or other expansions. For example, if you exclude the negative broad match keyword 'flowers', ads won't be eligible to serve when a user searches 'red flowers', but can serve if a user searches for 'red flower'." Both checked on Google's Help Centre on 31 August 2026, and the negative statement appears in four separate articles in near-identical wording.

A negative blocks a literal string in a defined arrangement and nothing merely similar to it. So the usual output, a long list of sensible words with no match types and no scope, fences in a fraction of what its author believed. Google states the consequence itself: "The main difference is that you'll need to add synonyms and singular or plural versions if you want to exclude them."

The order matters. Enumeration comes before tiering, because you cannot place a term until you know how many entries it is. Tiering comes before the conflict check, because scope decides which positive keywords a negative can reach. The conflict check comes last, because Google documents no surface that performs it afterwards.

## Part one. The four exceptions, which cut both ways

Before enumerating anything, know what is already handled, or you will pad the list with entries that do nothing.

**Casing and misspellings are automatic.** "Negative keywords automatically account for casing and misspellings, so you don't need to add those separately." The same sentence appears independently on the search terms report page. So the blanket claim that negatives match nothing but the literal string is wrong. Misspellings are covered. Plurals and synonyms are not.

**The 16-word cut-off.** "Your ad might still show when someone searches for a phrase that's longer than 16 words, and your negative keyword follows that 16th word." Google's example uses the negative discount against a 17-word query ending in discount. No number of extra entries fixes it.

**Display campaigns generalise.** On Display, negatives are excluded as an exact topic and do expand: Google's example is "women's trousers" blocking a page about women's jeans, but not skirts or men's slacks. Never reason about Search behaviour from Display behaviour.

**Accents and ampersands split.** "Negative keywords with accent marks are considered two different negative keywords." Adding the unaccented form does not block the accented one, and "socks & shoes" is a different negative from "socks and shoes". This is the mirror image of positive exact match, where accents are an all-languages close variant.

**The enumeration rule that follows.** For every family you block, write out the singular and the plural, the obvious synonyms, both the ampersand and the spelled-out form, and every accented rendering used in your markets. Do not write misspellings. That is the only one of the four you get free.

## Part two. The three negative match types, with Google's tables

Negative broad is the default, and the rules apply whether the negative is one word or several. The grid below merges Google's three separate tables, all built on the same negative keyword `running shoes`, read from the Help Centre on 31 August 2026. Google renders each cell as a tick or a cross image rather than as text.

| Search | Negative broad | Negative phrase | Negative exact |
|---|---|---|---|
| blue tennis shoes | shows | shows | shows |
| running shoe | shows | shows | shows |
| blue running shoes | blocked | blocked | shows |
| shoes running | blocked | shows | shows |
| running shoes | blocked | blocked | blocked |

**Negative broad**: blocked when the search contains all your negative terms, in any order. "blue tennis shoes" shows because only some terms are present. "running shoe" shows because the singular is a close variant.

**Negative phrase**: blocked when the search contains the exact terms in the same order, extra words allowed. Read Google's final sentence carefully: "The search may also include additional characters to a word and the ad will show even when the rest of the keyword terms are included in the search in the same order." The plural leak is a stated rule, not an edge case.

**Negative exact**: blocked only on the terms, in order, with no extra words.

**Selection rule, which is this file's own.** Negative broad for a term that can never be relevant in any combination: jobs, salary, torrent. Negative phrase for a multi-word intent string where order carries the meaning, such as `"how to build"` on a campaign selling the finished thing. Negative exact for one specific query you want stopped where longer versions may still be worth buying.

## Part three. Defaults, coercions and punctuation

**The same term added by two routes gets two different match types.** Negatives added from the search terms report arrive as negative exact by default. Negatives typed into a campaign arrive as broad. Display and video coerce everything to broad with no way to change it. Shopping accepts all three, and Google spells out the expansion burden there directly, telling advertisers to add "the synonyms, singular version, plural version and other variations". So the deliverable carries syntax on every line, always: brackets for exact, quotes for phrase, bare text for broad, chosen deliberately rather than inherited from wherever the term was clicked.

**Punctuation, which Google documents precisely.** Three symbols are allowed: ampersand, accent marks and asterisk. Full stops are accepted then ignored, so `New St.` and `New St` are the same negative. Plus signs are usually ignored, though a trailing plus is not, which is why `C++` survives. Operators are stripped: a `site:` prefix is removed, so `site:example.com dark chocolate` becomes `dark chocolate`, and a leading `OR` is ignored. A further set, including commas, at signs, braces and pipes, is rejected outright.

**The minus trap, which is the dangerous one.** "Adding a minus (-) operator to the front of a keyword will cause this keyword to be ignored for negative keyword matching. For example, if you have a negative keyword 'dark -chocolate', it'll be considered the same as just 'dark'." A list pasted from a spreadsheet with hyphens intact therefore ships a far broader block than anyone wrote. Rewrite every hyphen before pasting.

## Part four. The three tiers, which are the deliverable

**Account level.** Google's account-level list applies "to all search and shopping inventory in relevant campaign types", named as Search, Performance Max, App, Shopping, Smart and Local. Match types are selectable and the cap is 1,000. Display and YouTube inventory uses a separate mechanism, excluded content keywords, also capped at 1,000.

The test this file applies is strict: a term belongs here only if it could never be relevant to any campaign this account will run, including ones that do not exist yet. If you can imagine a future landing page that would welcome the query, it is not an account-level negative.

**Campaign level.** Terms wrong for one intent and right for another. `free` is the canonical case: fatal in a campaign driving paid subscriptions, and the entire point of one driving a free tool. `template`, `alternative to`, `login` and `support` behave the same way. The cap is 10,000 per campaign. Lists are the efficient vehicle: 20 lists per account and 20 per manager account, 5,000 keywords per list, and one list applies to up to 1,000 campaigns in a single action.

**Ad group level.** One purpose only: stopping two of your own ad groups competing for the same query. Not a place for irrelevant terms. Take each group's distinguishing terms and add them as negatives in its siblings, then run the check in part five before pasting.

**What is not published.** No cap on negatives per ad group, so do not design a process around one; the commonly quoted 5,000 has no Google source. No limit on lists per campaign. No character or word limit for negatives specifically. And the Display and video ceiling is unsettled: the Help Centre says 1,000 "at the account level" in three articles, the Google Ads Editor Help Centre says 5,000 "for any given ad". Quote whichever scope you mean and assert neither as the number.

## Part five. The conflict check, which Google documents no tool for

Google documents the failure in one sentence, buried inside an add-negatives procedure: "Make sure that your negative keywords don't overlap with your regular keywords, because this will cause your ad not to show."

It documents no tool for finding it. There is no conflict entry in the Google Ads Editor error and warning checks, no conflict value in the API recommendation types, and no conflict state in the list of keyword statuses. The widely repeated instruction to run the negative keyword conflicts report describes a surface not documented on Google's properties today. Performance Max has a related but different feature: a predictive tool previewing up to 10 keywords at a time and reporting projected impact to conversions or conversion value, for that campaign type only.

So compute it yourself. Google publishes no algorithm for it, so the three rules below are this file's, derived from Google's match type tables. A negative that does not expand can be tested as literal tokens, so check each proposed negative against every active positive keyword in its scope, meaning the whole account, the one campaign, or the one ad group respectively:

- **Negative broad conflicts** if every token of the negative appears somewhere in the positive keyword, in any order.
- **Negative phrase conflicts** if the negative's token sequence appears contiguously and in order inside the positive keyword.
- **Negative exact conflicts** if the token lists are identical.

A conflict means the positive keyword is dead. Resolve it by narrowing the negative's match type, lengthening it so it no longer sits inside the positive, or moving it down a tier so it no longer reaches that ad group.

## Part six. The input, and what to do when it is not there

The search terms report is the mining source, and it is incomplete by design.

**Branch A, a usable export.** The Keyword column turned on, over a lookback of at least 90 days, which is this file's default and not a Google figure. That column is off by default: turn it on via the column icon, then Attributes. Sort by cost descending, work top down, build all three tiers.

**Branch B, you cannot tell which keyword matched.** If the Keyword column is absent, or the campaign includes Dynamic Search Ads or Shopping targeting, attribution is unreliable. Google documents that DSA and Shopping terms return no keyword and report "Exact" in the match type field regardless, so any filter on match type mis-buckets all of them. It also documents that the reported match type is the type of the match rather than of the keyword. In this branch build the account and campaign tiers and stop, because cross-negation requires knowing which group pulled the term.

**Branch C, no history at all.** Seed from category knowledge and label the list unverified. Google documents no seed list, so the families below are this file's own taxonomy, drawn from category patterns rather than from anything Google publishes: free and zero-cost intent; employment and salary; do-it-yourself, including tutorial, template, pdf; used, rental and repair; exit intent, including cancel, refund, scam, alternative to; piracy, including crack, torrent, keygen; wrong geography and wrong buyer scale; and the adjacent category your name collides with. Enumerate each per part one, then re-derive from real data at the first trigger in part seven.

**True in every branch.** The report lists only terms "that a significant number of people have used", and low-volume queries are omitted for privacy. Google publishes no threshold and no time window. Because negatives do not expand, a negative added from the visible head will not cover the invisible tail's variants, so the response is deliberate enumeration rather than more mining. Do not promise deep history either: data from before 1 September 2020 carried a removal date of 1 February 2022, and the 9 September 2021 restoration returned queries reaching back only to 1 February 2021.

## Part seven. The review trigger

Not "check regularly". Two triggers and one decay rule.

**Spend trigger.** Re-run the mining pass every time spend since the last pass reaches one target cost per acquisition, or 14 days, whichever comes first. Google publishes no review cadence, so that threshold is this file's reasoning: one target CPA is the smallest amount that could have bought a conversion and did not, which makes it the smallest quantity of waste worth opening the report for.

**Surface-change trigger.** Re-run whenever anything widens the query surface: adding broad match keywords, adding a Performance Max or Shopping campaign, turning on final URL expansion, or a large budget increase. Google states directly that it is "critical to use Smart Bidding with broad match". It nowhere states that the pairing removes the need for negatives, and the claim that it does is widely repeated and unsupported.

**Decay rule.** Negatives never expire. Once a year, again this file's interval, read the account-level list against the current product range. A negative left over from a discontinued line is a common cause of a new campaign with healthy quality signals and almost no impressions.

## Part eight. Worked example, compressed

A subscription scheduling tool for independent clinics. Three campaigns: Brand, Non-brand, Competitor. Non-brand has two ad groups, `"online booking software"` and `"appointment reminder software"`. A 90-day export exists with the Keyword column on, so branch A applies.

**Account list, 84 entries.** Employment family enumerated rather than assumed: job, jobs, career, careers, salary, salaries, recruiter, recruiters, internship, internships. Piracy family: crack, cracked, torrent, keygen. Wrong-adjacency family, all negative phrase so clinic-side variants survive: restaurant booking, flight booking, hotel booking, event booking. That is 84 of the 1,000 cap.

**`free` is not an account negative.** A free appointment-reminder template campaign is planned for next quarter, so it fails the never-relevant test. It goes to the Non-brand campaign list as negative broad, alongside freeware, open source, spreadsheet, template and templates.

**Competitor campaign.** The brand names are not added as negatives, because blocking a brand and every variant of it is what brand exclusion lists do.

**Ad group cross-negation.** Each group's distinguishing terms go into its sibling, enumerated per part one rather than trusted to one entry. Into the booking group, all negative phrase: `"appointment reminder"`, `"appointment reminders"`, `"sms reminder"`. Into the reminder group, all negative phrase: `"booking software"`, `"online booking"`, `"booking system"`. All six pass the conflict check, since none of those sequences appears contiguously inside the other group's positive keyword.

**Two rejections.** Someone proposes account-level negative broad `booking` to stop generic traffic. The check fires: every token of `booking` appears in the positive `"online booking software"`, so it would take that ad group to zero impressions with no warning anywhere in the interface. Rejected. Separately, a pasted line reads `double -booking`, which Google reads as `double`. Rewritten as negative phrase `"double booking"`.

## Failure modes

**The singular that got away.** You add `coupons` and next month's report still shows spend on `coupon`. From the outside it looks as though the negative never applied, so people re-add it or blame the interface. It applied exactly as documented.

**The silent broad block.** A one-word negative broad added at account level because it looked obviously irrelevant is also a token inside a valuable positive keyword. Impressions for that ad group fall to near zero, the keyword still reads as eligible, and nothing in Google Ads names the cause.

**The minus paste.** Hyphens survive a spreadsheet export and a term becomes its first word only. The account quietly blocks everything containing that word, and the list still reads correctly to anyone scanning it.

**The route-dependent match type.** The same term is added twice, once from the search terms report as exact and once typed into the campaign as broad. Identical text, very different reach, one of them unintended.

**Mining the head and declaring victory.** Only terms a significant number of people searched are visible. Negatives added from those do not cover the tail's variants, so spend leaks one invisible term at a time and the report shows nothing to fix.

**Brand mistaken for keyword.** A competitor is added as a negative and considered handled. Negatives block the literal string and its misspellings; brand exclusions save you entering misspellings, variants or versions in other languages. Only one of the two expands.

## What this skill does not do

- It cannot see your account: not your real search terms, not your real costs, not whether the conversion action your campaign optimises towards records anything. A precise negative list on top of a broken conversion action is still a campaign spending against the wrong signal.
- It does not write ad copy, choose bids or set budgets. Query control decides which searches reach the ad and nothing about what the ad says or pays.
- It cannot enumerate the queries Google withholds. No threshold is published for "a significant number of people", so the blind spot is unknown in size rather than small.
- It does not manage brand inclusion or exclusion lists, a different mechanism with different behaviour and different limits.
- It cannot confirm today's caps. Every figure was read on 31 August 2026, Google publishes a known issue admitting the 5,000-per-list limit is not always enforced, and the Display and video ceiling is stated differently on two live Google properties.
