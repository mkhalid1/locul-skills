---
name: pricing-audit
description: Audits pricing and packaging from the value metric outward: choosing what you charge per and rejecting metrics that shrink as the product succeeds, the legitimate and illegitimate ways to fence tiers, good-better-best design including where the free ceiling should bind, willingness-to-pay research with the actual mechanics and weaknesses of Van Westendorp, Gabor-Granger and conjoint, per-account margin arithmetic for usage cost sitting under a flat subscription, the batch-and-churn pattern, price increase mechanics with and without grandfathering, and discount discipline. This skill should be used when setting or reviewing prices, designing tiers, planning a price increase, or diagnosing falling gross margin or early churn.
---

# Pricing and packaging audit

## The claim this skill is built on

Almost every pricing discussion starts at the number, and the number is the last decision, not the first.

Before the number there is a structural choice that everything else inherits: what you charge per. Get that wrong and no amount of tier design, page layout or discount policy recovers, because you have built a machine that charges less as it delivers more, or one whose heaviest users are its least profitable customers. Both faults are invisible on the pricing page and obvious in the margin report eighteen months later.

So this audit runs in a fixed order: metric, then fences, then tiers, then the number, then the mechanics of changing it. Each layer is only worth auditing if the layer above it survived.

## Layer one: the value metric

The value metric is the unit you charge per. Seats, workflow runs, documents processed, monitored endpoints, gigabytes stored, transactions cleared, sites covered.

**The hard filter, applied before any scoring: reject any metric that shrinks as the product succeeds.** This is the named failure of per-seat pricing on a product whose value does not scale with seats. If the product automates work, then success means fewer people doing that work, which means fewer seats, which means your revenue falls precisely as your value rises. You have made your customer's success your own revenue problem, and you will eventually respond by slowing the thing that helps them, which is a bad position to be arguing from internally.

The second shape of the same fault is subtler: a product with three power users and forty occasional readers. Per-seat pricing on that population produces shared logins, a permanent argument about read-only seats, and a customer who has quietly capped their own adoption to control the bill.

**The selection procedure.** List four to six candidate metrics. Score each from zero to two on five criteria.

| Criterion | The question | Why it matters |
| --- | --- | --- |
| Value alignment | Does it grow as the customer gets more out of the product | Determines whether growth is automatic or has to be sold |
| Predictability | Can a buyer forecast next year's bill within a sensible band | Unpredictable bills fail procurement, not the buyer |
| Verifiability | Can the customer see and check the count themselves | An unverifiable meter turns every invoice into a support ticket |
| Meterability | Can you count it accurately and cheaply, today | A metric you cannot instrument is a plan, not a price |
| Adoption neutrality | Does it avoid charging for the behaviour you want more of | Charging per user, per project or per stored record all tax adoption |

Take the highest total. Break ties on predictability, because a metric that wins on alignment and loses on predictability is the one that dies in procurement.

Two repairs are available when the winner is imperfect. An unpredictable metric becomes predictable with an included allowance plus a published overage rate, which converts a variable bill into a fixed bill with a known tail. An unverifiable metric becomes verifiable with a usage dashboard that shows the same number your invoice uses, which is cheap to build and removes an entire class of dispute.

## Layer two: value fences

A fence is the reason someone on a cheaper tier cannot simply take the more expensive tier's benefit. There are five legitimate kinds and one illegitimate one.

| Fence | Example | Why it holds |
| --- | --- | --- |
| Volume | 500 documents, then 2,500, then unlimited with a fair-use rate | Larger customers self-identify by consuming more |
| Capability | Approval workflows and audit trails only in the tier bought by regulated buyers | The capability is genuinely useless to the smaller buyer |
| Support and service level | Named contact, response time commitments, an uptime credit | The cost to you is real and scales with the promise |
| Deployment | Region choice, dedicated capacity, private network install | Real infrastructure cost, and a real procurement requirement |
| Commitment | Annual or multi-year term at a lower effective rate | You are paying for certainty, which has a value you can compute |

The illegitimate one is crippling something that costs you nothing to provide, where the buyer knows it costs you nothing. The canonical case is putting single sign-on behind a large multiple, which is disliked enough that a public list of vendors who do it exists and gets cited in procurement threads. It is also a strange thing to sell, because single sign-on reduces breach risk for both parties, and the tier that cannot afford it is the tier with the worst password hygiene.

**The embarrassment test.** Can you explain the fence, out loud, to a buyer standing on the wrong side of it? "The dedicated instance costs us money" survives. "Audit logs are for regulated customers, and you told me you are not one" survives. "You would pay for it" does not survive, and the buyer hears it as a penalty rather than a price, which is recovered from you at renewal.

If you must fence a security control, put it in the lowest paid tier, never the highest.

## Layer three: good, better, best

Three tiers work for three separate reasons, and only the first is about psychology.

1. It converts a yes-or-no decision into a which-one decision. The buyer's internal question changes shape.
2. The compromise effect, documented in consumer choice research by Simonson in 1989, means an option gains share when it sits in the middle of three rather than at the top of two.
3. Three tiers let one page do three jobs at once: acquire, serve the target buyer, and anchor.

The specific mistakes:

**A middle tier that is not obviously the answer.** The middle is the intended purchase and it should be designed that way, not derived by splitting the difference. The test: can a buyer in your target segment say, in one sentence, why the middle one is theirs? If your salespeople spend the call explaining the difference between entry and middle, the fence between them is not doing its job.

**A top tier priced to be bought.** The top tier's first job is to make the middle look reasonable. If a large share of customers buy the top tier straight off the page with no conversation, it is underpriced, or you are missing a tier above it. Treat this as a design heuristic and check it against your own mix rather than against any published constant.

**A free tier whose ceiling binds at the wrong moment.** Two failures, opposite in direction. Bind too early and the user hits the wall before they have felt anything worth paying for, which reads as a bait and kills activation. Bind too late and they never need to pay. The rule with the most content in it: the ceiling should be reached by success rather than by the calendar. A fourteen-day trial measures how busy someone was that fortnight. A limit of twenty-five processed documents measures whether the product worked.

Finally, the number of tiers should equal the number of genuinely different buyers you can name and describe. Three is a good default and a bad law. Add a fourth only when you can name who it is for.

## Layer four: the number, and the research that produces it

**Van Westendorp's Price Sensitivity Meter.** Four questions, asked in this order about a product the respondent has just had described to them:

1. At what price would this be so expensive that you would not consider buying it?
2. At what price would it be so cheap that you would question the quality?
3. At what price does it start to feel expensive, but you would still consider it?
4. At what price would it be a bargain?

Plot the cumulative distributions and read the intersections. Too cheap crossing too expensive gives the optimal price point. Bargain crossing expensive gives the indifference price point, which frequently sits near the market leader's price. The acceptable range is bounded below by the point of marginal cheapness, where too cheap crosses the inverse of bargain, and above by the point of marginal expensiveness, where too expensive crosses the inverse of expensive.

The weaknesses are not footnotes, they are the reason people misuse it. It measures stated preference, so it records what people say about hypothetical money. It is powerfully sensitive to the description read out immediately beforehand, so the same product framed two ways yields two different ranges, which means you cannot compare studies fielded with different copy. And it returns a range with no volume attached, so it tells you nothing about how many people buy at each point and therefore nothing about revenue. Respondents who have never used the product are guessing, and small samples produce curves whose intersections move if three people change their minds.

**Gabor-Granger.** Show a series of prices, ask purchase intent at each, and derive a demand curve and from it a revenue-maximising point inside the tested range. Better than Van Westendorp at anything involving quantity: elasticity, revenue, the shape between two candidate prices. Worse in three ways: it only reveals the prices you chose to show, the order and anchoring of those prices moves the answers, and stated purchase intent reliably overstates actual purchase, so the absolute conversion numbers are not usable without deflation.

**Conjoint, in one paragraph.** In choice-based conjoint, respondents repeatedly pick between bundles that vary in features and price, and the analysis recovers a utility value for each attribute level, which yields willingness to pay for individual features and lets you simulate share against competing bundles. It is the only method here that answers the packaging question, which is which features belong in which tier. It is overkill when fewer than about four attributes are genuinely in play, when you cannot field a few hundred respondents, or when the real decision is whether to charge 29 or 39. It is the right answer when a packaging mistake would cost more than the study.

**The honest ranking for a small company.** Observed behaviour beats all of them: a live price test on new traffic, or your own record of which deals closed at which price. Ten structured conversations with recent buyers and recent leavers come second. A badly fielded survey with sixty noisy responses comes last, and it is dangerous precisely because it produces a number with a decimal point.

## Layer five: the modern failure, usage cost under a flat subscription

A flat monthly price sitting over a variable per-request cost is a fixed price on a variable cost base. The result is structural: your heaviest users, who are also your most engaged and most referenceable customers, are your least profitable, and the effect is invisible in the blended figure because the typical account uses a small fraction of what the heavy account uses.

**The arithmetic to run, per account, not blended.** Monthly revenue minus units consumed multiplied by your unit cost, sorted ascending. Look at the bottom decile. If the worst accounts are negative, you do not have a pricing question, you have a metering question, and no price rise fixes it because the same customers will consume proportionally more at the higher price.

Design responses, in the order they are usually correct:

- Meter the cost-driving unit and publish an allowance with an overage rate, or a hard cap the customer controls. The customer's ability to control the cap is what makes this acceptable rather than frightening.
- Move to a two-part tariff: a platform fee for access plus usage above the included allowance. This keeps the predictable revenue and stops the bleed.
- Route the bulk path to a cheaper model and reserve the expensive path for the tier that funds it. That makes model access a fence with a genuine cost basis, which passes the embarrassment test.
- Set a gross margin floor per account with a named owner and an alert. Any account below it gets a commercial conversation rather than a quiet subsidy.

**Batch and churn.** The second failure of this shape: a customer signs up, clears a two-year backlog in three weeks, and cancels in month two. Common in migration, bulk generation, cleanup and catch-up work. The symptom is a first-month usage figure several times the steady-state, followed by cancellation, and it hides in aggregate retention while new signups are growing.

Responses:

- Sell the burst as its own thing. A backlog or migration SKU, priced per unit with a minimum, and a subscription priced for the ongoing work.
- Make the allowance monthly rather than total, so extraction is rate-limited by design and the value takes months to collect.
- Require a minimum term or annual prepay on the plan that permits the burst.
- Move the recurring product towards the part that is genuinely recurring: monitoring, updates, alerts, the thing that decays without you.

## Layer six: raising the price

**Grandfathering, and its long-term cost.** A permanent legacy price splits your base into cohorts forever. Every future packaging change has to be modelled against every legacy plan, the billing system accumulates special cases and the bugs that come with them, support has to know which rules apply to whom, and the customers most likely to be badly underpriced are the oldest and largest, which is exactly where the money is. It also creates a resentment event on the day it ends, and it will end.

Better instruments than a permanent promise: time-boxed grandfathering for twelve months, a price lock sold as a benefit of annual prepay, or a contractual uplift cap that limits annual increases for existing customers to a stated percentage. All three give the customer certainty without giving you a permanent second product.

**Notice.** For monthly plans, at least thirty days before the renewal that carries the new price. For annual plans, sixty to ninety days before renewal, because procurement needs a cycle to process it. Never mid-term. Read your own contracts first, because many specify a notice period and some cap the uplift, and breaching one converts a pricing exercise into a legal one.

**Increasing without a migration.** The mechanics, in order: the new price applies to new customers from a stated date and existing customers are untouched; existing customers move at their first renewal after a later stated date, with the notice period; a window is offered in which existing customers can prepay annually at the old price, which converts churn risk into cash and gives objectors something to do other than leave; one reason is given plainly rather than three; and exceptions are governed by a written rule with a budget, so account managers hold price for named reasons up to a fixed number of accounts rather than improvising.

## Layer seven: discounting

A discount teaches four things, and it teaches them permanently: the list price was fiction, waiting is rewarded, asking is rewarded, and every renewal is now a negotiation. The discounted price also becomes that account's reference price, so the increase you plan in two years starts from the lower number.

Annual prepay is the discount worth giving, when three things hold: cash now is worth more to you than the discount, churn is front-loaded so a year of commitment removes real risk, and the discount is smaller than the churn you avoid plus your cost of capital. The common band is the equivalent of one to two months free, roughly eight to seventeen percent. Above about twenty-five percent you are no longer buying commitment, you are funding a pricing problem.

**The rule that matters most.** A discount may buy a longer term, a case study, a reference call, a payment schedule, or a logo. It may never buy belief. A deal lost on positioning does not become a won deal at thirty percent off, it becomes a customer who did not want the product, consumes support, churns at renewal, and leaves behind an internal precedent that the price is negotiable.

## The audit procedure

Run the layers in order and stop at the first one that fails, because the layers below inherit the fault.

- **The metric fails the hard filter or scores badly on value alignment.** Fix the metric. Treat the current tiers as scrap. Repricing on a broken metric compounds the error and makes the eventual migration harder.
- **The metric is sound and the margin floor is breached in the top consumption decile.** The fix is packaging, an allowance plus overage, not a price rise.
- **The metric and the margins are sound but nobody can say why the middle tier is theirs.** Repackage. Do not reprice.
- **All of the above are sound and the open question is the number.** Now run research, choosing the instrument by what you need: a range from Van Westendorp, a revenue-maximising point from Gabor-Granger, a packaging answer from conjoint, or an actual answer from a live test on new traffic.
- **You cannot tell.** This is the common case, and it means you have no per-account cost data, no cohort retention split by first-month usage, or no idea what the buyer would otherwise use. The output is a research plan, not a price. In order: instrument the cost-driving event and name it; pull twelve months of cohort retention split by first-month consumption decile; hold ten to fifteen conversations with recent buyers and recent leavers. Fielding a willingness-to-pay survey before you can meter the cost driver produces a confident number attached to the wrong unit, which is worse than having no number at all.

## Worked example

An invented case: a contract review assistant sold at 60 per user per month, unlimited reviews, with a per-review inference cost of about 0.35.

**Metric.** Per seat fails the hard filter outright: the buyer's stated goal is to need fewer reviewers, so success reduces revenue. Scoring the candidates on the five criteria, per seat totals seven, per contract reviewed totals nine, per contract page totals seven and loses on predictability. Winner: contracts reviewed, with an included monthly allowance and a published overage rate to repair predictability.

**Margin.** The heaviest accounts run about 4,000 reviews a month on five seats. That is 300 of revenue against roughly 1,400 of delivery cost. The blended margin on the whole base reads as healthy and hides this completely. First finding, and it is not the price.

**Fences.** Single sign-on sits in the top tier at a large multiple and fails the embarrassment test. Move it to the entry paid tier. Replace it as a fence with deployment region and dedicated capacity, both of which cost real money.

**Tiers.** The middle tier differs from entry only by seat count, so nobody can say why it is theirs. Repackaged: entry at 500 reviews, standard at 2,500 with audit trail and single sign-on, enterprise on committed volume with private deployment.

**Free tier.** Currently fourteen days, which measures how busy the evaluator was. Changed to twenty-five reviews with no time limit, so the ceiling is reached by success.

**Batch and churn.** One cohort ran 3,000 reviews in month one clearing a litigation backlog and cancelled in month two. Response: the monthly allowance now rate-limits extraction, and the backlog is sold separately as a per-review project with a minimum.

**Verdict: do not field the willingness-to-pay study this quarter.** It was the thing the team wanted to do first, and it would have produced a defensible range for a per-seat price on a metric that is being abandoned. Ship the metering, repackage onto reviews, run the margin floor for one quarter, and research the number after that, at which point a live price test on new signups will be available and better than a survey.

## Failure modes

**Auditing the number while the metric is broken.** Recognisable because the pricing discussion is entirely about whether it should be 49 or 59, and nobody has said what a unit is.

**Blended margin.** The company reports a healthy gross margin and the top decile of accounts is underwater. Symptom: margin drifts down as revenue grows and nobody can name the cause.

**Per-seat pricing on a product that removes seats.** Symptom: a successful customer's bill falls, and your team starts quietly hoping adoption stays uneven.

**A fence with no cost basis.** Symptom: the fence comes up in procurement as a complaint rather than as a decision, and your salespeople have a rehearsed defensive answer for it.

**A calendar-bound free tier.** Symptom: trials expire with almost no usage, and reactivation requests arrive weeks later asking for an extension.

**Grandfathering by reflex.** Symptom: three legacy plans, a billing system full of exceptions, and a repackaging project that stalls because nobody can model the migration.

**Discounting a positioning loss.** Symptom: discounted accounts churn at renewal at a visibly higher rate, and the discount is quoted internally as the real price.

**A survey fielded to settle a packaging question.** Symptom: a price range with two decimal places and no answer to which features belong in which tier, which was the actual question.

## What this skill does not do

- It does not field research, recruit respondents, or analyse survey data. It designs the instrument and tells you which one fits the question.
- It cannot see your costs, margins, cohorts or contracts. Every number in its conclusions comes from you, and a margin finding is only as good as the unit cost you supplied.
- It does not read your customer agreements, so it cannot tell you whether a planned increase breaches a notice period, an uplift cap or a most-favoured-nation clause.
- It does not model tax, currency, regional purchasing power or payment method economics, all of which change the effective price materially in some markets.
- It will not give you a number on the strength of reasoning alone. Where the honest answer is that the evidence is missing, it outputs a research plan instead, which is sometimes an unpopular deliverable.
