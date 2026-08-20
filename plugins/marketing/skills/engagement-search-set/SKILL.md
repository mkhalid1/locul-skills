---
name: engagement-search-set
description: Builds a reusable set of five to eight validated saved-search queries that surface the specific posts where a person's buyers are already commenting, so a daily engagement routine has a target list instead of a feed. Covers the targeting inversion from posts your buyers write to posts your buyers comment on, a three-part query grammar, the filters and sort orders that silently collapse a result set, a live three-question validation gate, a named rejected-query register, geography handled honestly, and the conditions that trigger a rebuild. This skill should be used when a daily commenting or social-selling routine is being set up, when a search has just been narrowed with an author filter, or when months of consistent commenting have produced no inbound interest.
---

# Engagement search set

## The claim this skill is built on

Your buyers barely post. They comment.

The obvious move is to filter for posts written by people who match your ideal customer: set the author title to Founder, or Head of Operations, or Practice Owner, and read what comes back. It looks like precision. It is the single most reliable way to destroy a result set, and the damage is invisible from the inside, because every poster now matches your description perfectly.

The reason is structural rather than incidental. That filter intersects two conditions: this person is a decision-maker, and this person published a post about your topic in the last week. The second condition is rare in the first population. Decision-makers are busy, are a small group to begin with, and post irregularly. What survives the intersection is the long tail: the posts that reached almost nobody, carrying a handful of reactions and no comments. You have found the right people in the emptiest room on the platform.

The population you want is one step sideways. Your buyers read and comment on a small number of practitioners who serve them: the consultants, the specialists, the operators one rung ahead. Those people post constantly, because posting is how they get work. Their comment sections are where your buyers congregate. Target those posts and your comment sits in a room full of buyers, none of whom you had to find individually.

So the unit of targeting is not the author. It is the comment section.

The second claim follows from the first. This targeting layer is a build-once artefact, and almost nobody builds it. The default is to open the feed each morning and improvise, which is why daily commenting routines die in the second week: not because the writing is hard, but because the finding is, every single day, forever. A set of saved URLs converts a daily research problem into a daily writing problem, and only the second one is worth your attention.

**A standing caveat, which belongs at the top rather than in a footnote.** No major professional network publishes how its content search ranks, sorts or filters. Every filter behaviour described here was observed on an account rather than read in documentation, the behaviour changes without notice, and some of it may already have changed by the time you read this. Part four gives you a ten-second test for each claim. Run it before you trust anything below.

## Part one. What the set actually is

The deliverable is not a list of keywords. It is a small, dated, self-documenting artefact with five parts.

1. **Five to eight saved query URLs.** Fewer than five and the rotation staleness compounds. More than eight and nothing gets run often enough to notice when it dies.
2. **A record per query:** the query string, the filters applied, the sort, the date it was validated, what kind of person was posting, the engagement band observed, and one line saying why the query exists.
3. **A rejected register:** every candidate that failed, with the named reason it failed.
4. **A rotation:** which query is primary on which day, with one rest day.
5. **The operating rules** that travel with the URLs, because a list of links with no rules attached gets abused within a fortnight.

Save queries as URLs rather than as a list of people. A person list decays the moment somebody changes jobs or stops posting. A query keeps working on new posts by new people, which is the whole point of building a query in the first place.

## Part two. The three query archetypes and how to choose

**Archetype A, identity-anchored.** A quoted cluster of the nouns your buyers use for their own business, plus topic tokens, plus a role qualifier. Use when your buyers share a self-applied business-type noun.

**Archetype B, problem-anchored.** The symptom in the buyer's own words, plus a role qualifier. Use when there is no shared noun for the business but there is a shared complaint.

**Archetype C, practitioner-anchored.** Name the service class your buyers already read, and accept that the poster is not your buyer. Use when a recognisable category of adviser, specialist or agency already gathers your market.

**Archetype D, moment-anchored.** Words that only appear around a trigger event: a funding round, a hire, a migration, a renewal, an audit. Use when your product attaches to a discrete moment rather than a standing condition. Narrow, low volume, high value.

**The decision rule.**

- Buyers share a noun they apply to themselves, and you can write it without hedging: **A**, with **C** as the secondary.
- No shared noun, but a shared complaint you have heard in the same words from three different customers: **B**.
- A recognisable service class already serves them and posts about it: **C**, with **A** as secondary.
- Your product attaches to a discrete, nameable event: **D**, and never as the only archetype, because volume will be too low to sustain a daily routine.
- **You cannot tell, because you do not yet know your buyer's vocabulary.** This is the common case and guessing at it produces a plausible set that is precisely wrong. Do not build the set yet. Run a vocabulary harvest first, and time-box it to one hour.

**The vocabulary harvest, which is the real first move.** Take ten to fifteen people you already know are buyers: customers, live pipeline, anyone who has replied to you with intent. Open each person's recent activity and read their comments, not their posts. Their posts are performance. Their comments are vocabulary. For each person write down three things in three columns: the noun they use for their own business, the noun they use for the problem, and the category of person whose post they were commenting under.

Those columns are your archetypes. Column one is your A cluster. Column two is your B cluster. Column three is your C list, handed to you by the people you are trying to reach. Two seed queries out of an hour is a good outcome.

**If you have fewer than five such people to harvest,** stop. You do not have a search problem, you have a customer problem, and no query will substitute for five conversations. Book the conversations.

## Part three. Query construction, three parts, and the one everybody omits

Every query is built from three parts.

**(a) A quoted identity cluster, two to four terms, joined with OR.** Quoting is what stops the engine matching the words separately across the post. Keep each phrase to two or three words: a five-word quoted phrase is an exact-match request and usually returns nothing.

**(b) Unquoted topic tokens.** Two or three. These are the words that would appear naturally in a post about the problem, not your category name for it. Your category name returns vendors.

**(c) An unquoted role-qualifier cluster, joined with OR.** This is the part that gets left out, and it is the difference between a query that returns practitioners of your topic and a query that returns the people who buy it. On its own, a topic like inventory forecasting returns inventory forecasters talking to each other. The same topic plus owner OR director OR founder returns posts where somebody who signs the cheque is in the conversation.

Keyword operators are conventionally uppercase, and a lowercase or is commonly treated as an ordinary word rather than an operator. Write OR in capitals.

**Invented generic shape,** for a service sold to owners of small building firms:

```
"building firm" OR "construction company" OR "trade business"  cashflow invoices  owner OR director
 ^ (a) identity cluster, quoted                                 ^ (b) topic         ^ (c) role qualifier
```

**Percent-encoding.** A saved URL stores the encoded form: `%22` for a double quote, `%20` for a space. The example above becomes:

```
%22building%20firm%22%20OR%20%22construction%20company%22%20OR%20%22trade%20business%22%20cashflow%20invoices%20owner%20OR%20director
```

**Do not hand-write the URL.** Parameter names are undocumented and have been renamed before. Set the filters you want in the interface, copy the address bar, then edit only the keywords value. As of August 2026 a content search on the largest professional network takes roughly this shape, and it is worth keeping only as a sanity check that you copied a content search rather than a people search:

```
https://www.linkedin.com/search/results/content/?keywords=<encoded>&sortBy=%22relevance%22&datePosted=%22past-week%22
```

## Part four. Filters: what to apply, what to never apply, and the ten-second test

| Filter | What it does to the set | Verdict |
| --- | --- | --- |
| Content or posts scope | Removes people, companies and jobs from the results. Large reduction in volume, no loss of relevant posts. | Always |
| Date posted, past week | Cuts volume by roughly the ratio of a week to the index depth. Everything removed was uncommentable anyway. | Always |
| Sort by relevance or top match | Reorders rather than filters. Volume unchanged, top of set transformed. | Always |
| Author title | Collapses the set to the rare intersection of decision-maker and recent poster. Volume falls hard and the surviving posts have almost no audience. | Never, for discovery |
| Author industry, set to your own | Excludes your buyers by construction, since they are in their industry, not yours. | Never |
| Author industry, set to the buyer's industry | Legitimate but redundant if the identity cluster is doing its job, and it suppresses archetype C entirely because practitioners sit in a different industry from their clients. | Rarely |
| Author company | Turns discovery into named-account monitoring, which is a different job with a different cadence. | Only for account work |
| First-degree connections only | Returns the room you are already standing in. Useful for maintaining relationships, useless for reaching new people. | Never, for discovery |
| Author location | Does not exist on content search. See part six. | Not available |
| Language | Worth applying when your operating language is not the dominant one in the result set. | Situational |

**Why the recency window is not a preference.** The distribution window on a feed post is short, and a comment arriving after it closes is read by the author and nobody else. The mechanism is well established; the magnitude is not published by any platform. A past-week window is the widest setting that still returns posts worth commenting on, and it exists so that yesterday's post appears rather than last month's.

**The one observation worth stating carefully.** On one account, on one date, adding an author-title filter to an otherwise working query moved the top of the result set from posts carrying reactions in the low hundreds to posts carrying single-digit reactions. That is one person's testing, not a platform rule, and it is an order of magnitude observed once rather than a measurement. The structural argument in part one is the part to trust. The number is the part to re-check.

**The ten-second test, and run it before trusting any row above.**

1. Run your query with the filter off. Read the reaction count on result one and result five.
2. Apply the filter. Read the same two numbers.
3. If the top-of-set engagement falls by roughly an order of magnitude, the filter still behaves as described here and belongs in the Never column. If the two readings are broadly similar, the platform has changed and the filter is now safe to use.

Do the same for sort order. Switch from relevance to latest and read the first five posters. If they are company pages and scheduled output with no reactions, the described behaviour holds. Re-run both checks whenever the search interface visibly changes, and note the date next to the query.

## Part five. Sorting, and the consequence nobody plans for

Relevance sorting only. Chronological sorting returns company-page output and zero-engagement noise, because recency alone selects for whoever posted most recently rather than whatever anybody read.

Relevance sorting also appears to weight network proximity, which has two consequences.

**The set self-improves.** As you connect with the right people, the same saved URL surfaces posts closer to that part of the network. You do not have to rewrite the query for it to get better.

**The set is not portable and not stable.** Two people running the identical saved URL see different posts, so a query a colleague recommends may be worthless to you, and a query you validated in March is a different query in September. This is the mechanism behind most of part seven.

## Part six. Geography, handled honestly

Content search has no author-location filter and there is no workaround. Say that plainly rather than inventing one.

**What does not work:** putting a country or city name in the query text. That returns posts about the place, written by anyone anywhere, mostly news commentary and relocation content.

**What actually shifts the mix:**

- **Vocabulary that only exists in that market.** The name of the local statutory scheme, the regulator, the tax term, the qualification. A post using it was almost certainly written for that market.
- **Language.** The strongest available lever where the market is not English-speaking, and the one most people skip because their own material is in English.
- **Network proximity compounding.** As you connect with people in that market, relevance sorting does the geographic work the filter cannot.

Frame the goal correctly: the right people in the comments, not the right flag on the poster. A practitioner based anywhere whose audience is in your target market is a better target than a buyer in that market whose post nobody saw.

## Part seven. Validation, and the rejected register

**Run every candidate live before it enters the set.** Read the first twenty results and answer three questions.

1. **Are the posters the right kind of people?** Owners, partners, or practitioners who serve your buyer. Not students, not motivational accounts, not company pages, not people selling what you sell.
2. **Is the engagement real?** A working floor from one account's testing is ten or more reactions and five or more comments on the majority of the top twenty. Adjust it to your market before you apply it: in a small technical niche a strong post may carry twelve reactions, and an unadjusted floor will delete your best query.
3. **The room test, which is the one that decides it.** Open the comment sections of the top three posts and classify the first ten commenters on each. Would commenting here put you in front of your buyer even if the poster is not your buyer? This is the only reading that measures the thing you actually want.

A query failing any one question is discarded, not tuned. Expect roughly half your candidates to die: in one person's build session, six of more than twelve candidates survived. Budget for that, so a rejection reads as the process working rather than as a failure.

**Keep the rejected register, because this is the part everyone throws away and then rebuilds six weeks later.** One line per rejection: the query, the date, and the named pattern.

- **Wrong-seniority jargon.** Internal process vocabulary returns the people who run the process, not the people who own the outcome. Delivery and operations language is the usual offender.
- **Identity term too broad.** A generic self-description returns motivational and aspirational content with enormous engagement and no buyers.
- **Topic only, no role qualifier.** Returns practitioners of your topic talking shop.
- **Ambiguous industry noun.** One word covering several unrelated industries drags in all of them. Test any single-word industry term against its other meanings before committing.
- **Negative-sentiment phrasing.** Phrasings like "X does not work" return almost nothing, because that is not how people write about their own problems.
- **Vendor-category naming.** Searching your own product category returns your competitors and their marketing.
- **Over-quoting.** A quoted phrase longer than about three words is an exact-match request and returns an empty or near-empty set.
- **Filter collapse.** Author title or author industry applied to a discovery query.
- **Chronological sort.** Company pages and zero-engagement scheduled output.

## Part eight. The operating rules that ship with the set

A URL list with no rules attached gets abused. These travel with it.

- **Target posts in the ten to seventy-five comment band.** Below ten there is not enough audience present. Above seventy-five your comment is buried and the effort is invisible. This band comes from one person's testing and is a starting point to check, not a law.
- **Comment within the first two to four hours**, for the distribution-window reason in part four.
- **Three to five sentences minimum, adding a framework, a counter-example or a specific number.** Never agreement. Agreement is attendance.
- **Never pitch.** The comment section is not a channel.
- **Expect to appear three or four times before anyone recognises you and ten or more before you are top of mind.** Widely repeated in practitioner writing, unverified anywhere, and directionally consistent with how recognition works.
- **Fifteen to twenty minutes per session, hard stop.** The stop is what makes it survive.
- **Rotate.** One primary and one secondary query per day, one rest day.

What a comment should actually contain is a separate job. This file gets you into the right room.

## Part nine. Staleness and the rebuild trigger

A search set goes stale in five ways, and only one of them announces itself.

1. **Network drift.** Relevance sorting follows your network, so the same URL narrows over time. Helpful, until it becomes an echo chamber of the eleven people you already comment on.
2. **Poster attrition.** The practitioners anchoring a query stop posting or change subject. Nothing errors, the URL still loads, and the results are quietly worse.
3. **Vocabulary drift.** The market renames the problem and your identity cluster stops matching.
4. **Platform change.** A filter changes behaviour or disappears.
5. **Saturation.** You have commented under the same people so often that the marginal new reader is near zero.

**The maintenance rule.**

- **Weekly, ten minutes.** Run each query, check the top ten against the three validation questions, and write the date on the record. A query that fails the engagement floor twice in a row is suspended, not deleted, and its record goes to the rejected register with the reason.
- **Monthly, one query rebuilt from scratch.** One, never all of them. A whole-set rebuild throws away the accumulated knowledge of what already failed, which is the expensive part of the artefact.
- **Re-run the ten-second filter test** whenever the search interface visibly changes.

**Trigger a full rebuild when any two of these are true:** three or more queries suspended within one month; you have added several hundred connections since the set was built; a filter you depend on has changed behaviour; your engagement sessions have stopped producing profile visits for four consecutive weeks.

**Trigger it immediately, on one condition alone: a change in who you sell to.** A repositioning invalidates the entire vocabulary layer, and a stale set after a repositioning is worse than no set, because it keeps producing well-targeted comments in front of people who can no longer buy from you.

## Worked example, compressed

An invented generic business: a subcontractor invoicing service sold to owners of small building firms, five to fifty staff, in one English-speaking market.

**Archetype choice.** The buyers apply a noun to themselves without hedging, so archetype A, with C as secondary. The vocabulary harvest across twelve known customers returns three nouns for the business, two for the problem, and one dominant practitioner category in the third column.

**Nine candidates run live. Five die.**

- `"subcontractor payment" retention` dies. Topic only, no role qualifier. Returns commercial managers and payment-practice consultants talking to each other.
- `"small business owner" invoices` dies. Identity term too broad. Two thousand reactions on the top result, and the comment section is congratulation.
- `"application for payment" valuation certification` dies. Wrong-seniority jargon. Every commenter is a quantity surveyor, which is a practitioner of the process, not the person who owns the cash position.
- `"contractor" late payment` dies. Ambiguous industry noun. Catches independent software contractors, defence contractors and freelance consultants in one set.
- `"retention doesn't work"` dies. Negative-sentiment phrasing. Eleven results, most of them years old.

**And one more dies for a different reason.** Candidate one, run with an author-title filter set to Owner, moved the top of the set from a post with reactions in the low hundreds to a post with six. Filter removed, candidate reinstated. Logged in the register as filter collapse so nobody tries it again in November.

**Four survive, each with its reading recorded.**

| Query | Archetype | Posters | Top-20 engagement | First-ten commenters matching buyer |
| --- | --- | --- | --- | --- |
| `"building firm" OR "construction company" OR "trade business"` + `cashflow invoices` + `owner OR director` | A | Firm owners, trade association voices | 40 to 300 reactions | 5 of 10 |
| `"family business" OR "trade business"` + `hiring growth margins` + `owner OR founder` | A | Owners, one operations adviser | 25 to 180 | 4 of 10 |
| `"quantity surveyor" OR "contracts manager"` + `cashflow disputes` | C | Practitioners serving the buyer | 60 to 400 | 4 of 10 |
| `"won a contract" OR "new site"` + `team hiring` + `owner OR director` | D | Owners, at a trigger moment | 15 to 90 | 6 of 10 |

**Rotation.** Monday Q1 primary and Q3 secondary. Tuesday Q2 and Q4. Wednesday Q3 and Q1. Thursday Q4 and Q2. Friday Q1 and Q4. Saturday rest. Sunday Q2 and Q3.

**Verdict: ship the four, not the nine, and keep the five rejections in writing.** Q3 is the highest-value query in the set and the one an author-filter instinct would have deleted first, because none of its posters is a buyer and every one of its comment sections contains several. Q4 has the best commenter composition and the least volume, so it is a secondary forever and never a primary. The floor of ten reactions and five comments held in this market and would need lowering in a narrower one. Next full validation pass in one week, next single-query rebuild in one month, and an immediate rebuild if the service is ever repositioned towards larger firms, because every identity cluster above would then be pointing at the wrong companies with excellent precision.

## Failure modes

**The author-filter collapse.** Somebody adds a title filter to make the results more precise. Every poster now matches the buyer description perfectly and every post has four reactions. From the outside this reads as a query that finally works, and the missing thing, the audience, is the one thing a result page does not show you.

**The mirror set.** The queries were written in your own industry's vocabulary, so they return your competitors and your peers. Engagement is warm, replies are friendly, relationships form, and nothing ever becomes pipeline. It takes months to notice, because every surface signal says it is working.

**The dead bookmark.** A saved URL keeps loading and keeps returning results. The anchoring posters moved on six weeks ago and the results are now week-old posts with two reactions. Nothing errors, nothing alerts, and the routine continues at full effort into an empty room.

**The four-hundred-comment post.** Somebody targets the biggest posts in the category on the reasoning that bigger is better. The comment lands at position one hundred and eighty, nobody reads it, and the effort is genuinely invisible rather than merely unrewarded.

**The discarded register.** The set is built, the failures are not written down, and six weeks later the same eight dead queries are re-tested by the same person. The tell is a candidate appearing in two separate build sessions with the same result.

**The single-query routine.** One query is better than the others, so it quietly becomes the only one. Within three weeks the same eleven people see you daily, network proximity narrows the query further each week, and your comments start reading as a claque.

**Location theatre.** A country name goes into the query text to target a market. The set fills with posts about the country rather than posts by people in it, and the geographic problem is now considered solved.

**The automated set.** Somebody scripts the queries, harvests the profiles, or routes generated comments through a tool. This breaches the terms of every major professional network, and the tell is a comment that answers a post it plainly did not read. The related version is a reciprocal engagement group, which recreates the exact failure this file exists to avoid: a guaranteed room full of people who cannot buy from you.

## What this skill does not do

- It does not tell you what to write. It gets you into a room. What you say once you are there is a separate job, and a well-targeted comment with nothing behind it is worse than silence because it is publicly empty under your own name.
- It cannot see your account or your analytics. Every threshold in it is a starting point to check against your own market, and in a small or technical niche the stated engagement floor will discard your best queries unless you lower it first.
- It cannot filter by the poster's location, and no workaround exists. It offers vocabulary, language and network proximity instead, and those shift the mix rather than fix it.
- It does not automate anything and must not be automated. Scraping result pages, harvesting profiles in bulk, posting generated comments through a tool and joining reciprocal engagement groups all breach the terms of every major professional network. The set is a target list for a person who is going to read the posts.
- It does not verify the platform behaviour it describes. None of it is documented, all of it changes without notice, and the ten-second test in part four is the reader's only guarantee. Run it rather than trusting the table.
- It cannot fix a positioning problem. If you cannot write your buyer's own noun for their own business, the set will find the wrong people with great precision, and precision is exactly what makes that failure hard to see.
