---
name: competitive-teardown
description: Builds a competitive teardown as a structured procedure with evidence requirements rather than a feature grid copied from marketing pages. Covers choosing the comparison set, a five-level evidence tier system carried on every cell, honest ways into a competitor's product, the dimensions that actually decide deals, a formatting rule that bans yes and limited from any cell, pricing normalised onto one reference customer, and a required section on where you lose. This skill should be used whenever a competitor comparison, battlecard, comparison page, or teardown is being written or refreshed.
---

# Competitive teardown

## The claim this skill is built on

A feature grid assembled from marketing pages is not research. It is a transcription of what several companies want to be true, arranged in columns, and it fails in a predictable place: in a live deal, when a rep quotes a cell and the buyer, who uses the product every day, corrects them. After that the whole document is discounted, including the parts that were right.

Two things prevent that, and neither is more effort. The first is deciding who you are comparing against before you compare anything, because that choice is a positioning claim and getting it wrong produces a document that is internally consistent and useless. The second is recording, on every single claim, how it was obtained. Those are the two sections most teardowns skip entirely.

## Step 1. Choose the comparison set, before anything else

There are three defensible sets and they are usually three different lists.

1. **What a buyer would genuinely choose instead.** Includes the spreadsheet, the in-house script, the existing tool they already pay for, and doing nothing. Doing nothing wins more deals than any vendor and is missing from almost every teardown.
2. **Who you actually meet in deals.** From opportunity records and call notes, not from memory. This list is usually shorter than expected and contains at least one name nobody predicted.
3. **Who ranks for your terms.** The names a buyer sees when they search the words you are trying to own, including comparison and listicle sites that rank above every vendor.

**The gaps between the three lists are the finding.** A name in set 3 and absent from set 2 is a visibility problem, not a product problem. A name in set 2 and absent from set 1 means buyers are being shown an option they would not have found alone, which usually means someone else's sales team is doing the framing. A name in set 1 and absent from both others, especially doing nothing, means you are competing on a dimension nobody is choosing on.

Write the three lists separately, then decide which set the document is for, and say so in the first line of it. A teardown that silently blends all three is the one that gets built against an aspirational competitor while the real loss is to a spreadsheet.

## Step 2. Evidence tiers, which is the core of this file

Every claim carries a tier and a date. The tiers, strongest first:

- **E1, observed.** You did it yourself in the product, on a stated date, with the steps recorded and preferably a screenshot. This is the only tier that describes the product as it exists.
- **E2, documented.** Stated in their product documentation, API reference, changelog, status page, or terms of service. Verifiable and datable, but documentation lags the product in both directions, describing things that shipped late and things that were quietly removed.
- **E3, marketing claim.** The pricing page, the landing page, a launch announcement, a webinar. This is what they want to be true. It is legitimate evidence about their positioning and weak evidence about their product.
- **E4, third party.** A review site, an analyst note, a comparison article. Record who paid for it, whether the writer used the product, and the date, because review sites carry entries several years old alongside current ones without distinguishing them.
- **E5, hearsay.** A prospect said it on a call, a former employee mentioned it, someone read it in a forum. Useful as a lead. Never a claim.

**The rule.** Every cell in the output carries its tier and date, formatted so the tier is visible at a glance. A teardown where E3 and E1 are formatted identically is a document that will mislead your own team, and it will do so with maximum confidence, because the person quoting it has no way to know which cells are soft.

**The escalation rule.** Any E5 or E4 claim that matters to a deal must be raised to E1 or E2 before it appears in anything customer-facing. If it cannot be raised, it moves to an "unverified, do not quote" appendix, or it is cut. The appendix exists so that useful leads are not lost, and it is clearly separated so nobody quotes it by accident.

**The rule about your own product.** Your own column is graded on the same scale. Claims about your own capability are E1 only if someone actually performed them recently on the shipping version, and "we have that" from an engineer is E5 like any other hearsay.

## Step 3. Getting into the product, honestly

Sources, roughly in order of value:

- **A trial or free tier, under your own name and your own company.** The single highest-value source, and the only route to E1.
- **Public documentation and API reference.** Frequently more honest than the marketing site because it is written for people who have already paid and will file a ticket if it is wrong. Limits, quotas, supported formats and prerequisites live here.
- **Changelog and release notes.** Cadence is a signal on its own. So is silence in an area they market heavily.
- **Status page and incident history.** Reliability evidence a marketing page will never provide, with dates and durations.
- **Pricing page, plus archived versions.** Current prices matter less than the direction of travel. A tier that appeared, a limit that tightened, a feature that moved up a plan: each is a strategy decision made visible.
- **Job postings.** Roles reveal where investment is going, and engineering specs often name the stack. A sudden run of hiring in one area is a roadmap statement with a date on it.
- **Public repositories, SDKs, community forums and support threads.** The forum is where the ceiling is described, by users, in detail, for free.

**The line not to cross, stated plainly.** Do not misrepresent who you are in order to get access. No invented company, no borrowed identity, no booking a sales demo under a false name, no using a customer's login, no sharing credentials that belong to someone else. Beyond being dishonest, it usually breaches the terms you accepted on the way in, and it teaches your own team that fabricated provenance is acceptable, which is the exact habit this whole document exists to prevent.

If you cannot obtain first-hand access honestly, that is a finding and it gets recorded: "no first-hand access obtained, all capability claims E2 or lower". A document that says so is more useful than one that quietly guesses.

## Step 4. The dimensions that actually differentiate

Rarely the feature list. Six dimensions that decide deals:

1. **The job it is hired for, and the trigger.** What had to happen in the buyer's week for them to go looking. Two products with overlapping features that answer different triggers are not really competitors.
2. **What it assumes about the buyer's existing setup.** Prerequisites: a data warehouse, a particular identity provider, a minimum team size, a tagging discipline nobody has. Prerequisites are the quietest disqualifier in the market and they never appear on a feature grid.
3. **The moment it stops being useful.** The ceiling. A record limit, a seat count, a workflow it cannot express, the point at which customers graduate to something else. The support forum knows this. The sales page does not.
4. **The cost of leaving.** Export fidelity, contract length, integrations that must be rebuilt, retraining, whether historical data comes out in a usable shape. High switching cost is why a worse product keeps its customers, and it is the dimension most likely to decide whether you can win an incumbent's account at all.
5. **Who inside the buying organisation it makes look good.** The champion, and the metric the champion is measured on. Two products with identical features sell to different champions, and that decides more deals than capability does.
6. **What it replaces.** The spreadsheet, the script, the agency, the person. If the honest answer is "nothing, they carry on as they are", that is the competitor to beat.

## Step 5. The comparison table rule

**Every cell must contain a number, a plan name, or a named capability.**

Banned as cell contents: yes, no, limited, partial, varies, coming soon, and a tick. The reason is mechanical rather than stylistic. Cells get quoted separately from their headers, in emails, in decks, in calls. "Limited" survives that journey and carries nothing. A "yes" that turns out to mean a beta available on the top tier is how a team loses a deal on credibility rather than on product.

Format for a cell: **value, tier, date.** For example, "5 seats included, then a per-seat monthly charge on the Team plan (E3, pricing page, 4 August 2026)".

If the honest cell content is that you do not know, write "unknown" and add what would settle it. An honest unknown is a usable row and a candidate for next week's work. A guess is neither.

## Step 6. Pricing, honestly

Three separate problems, and most comparisons address none of them.

**List price versus what is actually paid.** Published pricing is a ceiling for any buyer with a procurement function, and annual or multi-year commitments attract discounts as a matter of routine. If you only have list, label it list, and note that the effective price in a competitive deal is lower by an unknown amount.

**The units differ, so the headline numbers are not comparable.** Per seat, per active user, per record, per event, per workspace, per environment. The only way to compare is to define a reference customer and price every vendor for that exact customer. State the reference customer in the document: for instance, 40 users of whom 12 are daily, 12,000 records, two integrations, annual billing. All of those figures are invented for the example, and the point is that they are fixed and stated, so that every column answers the same question.

**Total cost, which is rarely on the pricing page.** Implementation and onboarding fees, the mandatory support tier above a certain size, the single sign-on surcharge, overage rates, the connector sold as a separate item, the minimum contract value, the annual uplift clause, and the internal cost of whoever administers it. A cheaper list price with a paid connector and a support minimum is frequently the more expensive option.

One number is often more useful than the price itself: **the exact point at which a free or entry tier forces an upgrade.** That is where the buyer's real decision happens.

## Step 7. The section about where you lose

A required section listing where each competitor is genuinely better, in the same evidence tiers, with the buyer situations in which you would recommend them.

**The rule: a teardown with no honest losses is a sales asset, not an analysis.** It also stops working as a sales asset, because reps learn quickly that a document which never concedes anything cannot be trusted in front of a buyer who has used both products.

Two tests that the section is honest. There is at least one row where the recommendation is not you. There is at least one named buyer type you should decline to sell to. If neither exists, the section has not been written yet, whatever is on the page.

The practical value is disqualification. A rep who can recognise a bad-fit deal in the first call saves more than a marginal improvement in win rate on deals that were always going to be lost.

## Step 8. Dating and refresh

Every claim carries a date and a pointer to where it came from: the documentation anchor, the archived snapshot, or a dated screenshot in a shared folder.

Shelf life by claim type, as a default:

- **Pricing and packaging: one month**, or immediately on any observed change to their pricing page.
- **Feature availability: one quarter.**
- **Positioning, target market, and the comparison set itself: twice a year.**

A claim past its shelf life is marked stale in place, not silently retained and not quietly deleted, because the stale marker is what tells a reader which rows to distrust today.

Maintenance: subscribe to their changelog, watch the pricing page for changes, keep an eye on job postings, and put one named owner plus a review date on the document. A teardown with no owner is wrong within a quarter and still in circulation a year later.

## Decision rule

- **Claim is E1 and inside its shelf life.** → Use it anywhere, including customer-facing material.
- **Claim is E2.** → Use it, cite the documentation page and the date.
- **Claim is E3, their marketing.** → Report it only as "they state", never as fact, and never as evidence of a limitation on their side.
- **Claim is E4 or E5 and it matters.** → Escalate to E1 or E2 before it is used. If it cannot be escalated, it lives in the unverified appendix.
- **You cannot tell whether a capability exists**, because documentation is silent, the trial is gated, and the sales page is ambiguous. → Write "unknown", and name exactly what would settle it: the documentation page that does not exist, the trial you could not obtain, the customer who could be asked. Do not infer it from the absence of a mention, and do not fill the cell with "limited".

## Worked example: one row, badly and then properly

**All names and figures below are invented for this example.** The competitor is called Vendor B, a reporting tool.

**Done badly:**

| Capability | Us | Vendor B |
|---|---|---|
| Reporting | Advanced | Limited |

Everything wrong with this fits in one sentence: no number, no plan name, no capability named, no source, no date, and both cells contain a word that means nothing once separated from the header. It came from reading their pricing page, so it is E3 presented as fact. In a deal, the buyer says their reporting includes scheduled exports and a query API, and the rep has nothing to say.

**Done properly:**

**Dimension: scheduled report delivery.**

- **Us:** scheduled email delivery on any saved report, at intervals from hourly to monthly, included on all paid plans. (E1, observed in product, 14 August 2026.)
- **Vendor B:** scheduled delivery at daily and weekly intervals only, requires their Business plan, which is the second of three tiers, and delivers CSV and PDF only, with no hourly option. (E2, their documentation page on scheduled reports, read 14 August 2026.)
- **Confidence:** E2 rather than E1, because no Business plan trial was obtained. Escalating this would need a trial of that tier.
- **So what:** decisive for buyers running a daily operations stand-up, irrelevant to anyone reporting monthly. Do not lead with it outside the first group.
- **Where we lose on the same dimension:** Vendor B's PDF export preserves the customer's own branding and ours does not. (E1, observed, 14 August 2026.) This is why we lose deals where the report is forwarded to an external client, and it should be raised early rather than discovered late.

**Verdict on the two versions.** The proper row is six times longer, considerably less quotable, and it is the only one of the two a rep can take into a call. The badly done row would fit on a slide, which is exactly why it exists.

## Failure modes

**The mirror teardown.** The comparison is built from your own feature list, so every row is a dimension you chose because you win it. It reads as a clean sweep and it predicts nothing about deals.

**Tiering the sources and then formatting every cell the same.** The tiers were collected, the table was designed for a slide, and the distinction was lost at the last step, which is the only step that mattered.

**Taking the lost-deal reason at face value.** Buyers give a polite, tidy reason. It is E5, and it is systematically biased towards price because price is the least awkward thing to say.

**A stale price quoted in a live deal.** Pricing moves fastest and is the most quotable number in the document, which is a bad combination without a date on the cell.

**Comparing against the aspirational set.** The document names the large, well-known vendor the team wishes it competed with, while the actual losses are to a spreadsheet and to nothing at all.

**Counting features instead of jobs.** A count of forty against twenty-five is a statement about product surface area, not about which one gets bought, and it invites the same counting exercise from the other side.

**Treating a roadmap statement as shipped.** A conference talk about what is coming is E3 about the future, which is the weakest evidence in the whole system, and it ages into a claim that was never true.

**No owner and no review date.** The document is right the week it is written and quietly wrong for the rest of its life, while continuing to circulate because nothing marks it as expired.

## What this skill does not do

- It does not fetch anything. Trials, documentation, archived pages and job postings all have to be gathered by a person or a tool with web access, and this only governs how what you gather is ranked and recorded.
- It cannot see negotiated prices, private roadmaps, renewal rates, or why a specific deal was really lost. Recorded calls and lost-deal interviews reach those and desk research does not.
- It does not size a market, segment one, or tell you which competitor matters most commercially. That needs your own pipeline data.
- It is not legal review of comparative advertising, which carries genuine exposure in several jurisdictions when a competitor is named in public material.
- It will not keep the document current. Shelf lives and an owner are stated here, and a monitoring product enforces them far better than a written rule does.
