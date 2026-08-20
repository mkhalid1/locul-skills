---
name: pricing-packaging-build
description: Diagnoses which of four monetisation problems a company actually has before any price changes, then builds the tier structure that follows from the diagnosis. Covers the value cascade run in order, the value metric test stated as one falsifiable question, a symptom-to-branch routing tree with a named remedy class per branch, an explicit cannot-tell branch that requires cohort instrumentation first, fence design that maps to value perception rather than to company size, and the ratios to compute instead of borrowed benchmarks. This skill should be used when setting or restructuring pricing and packaging, when churn, flat revenue or persistent discount requests are being read as a price problem, or when a new tier is about to be added.
---

# Pricing and packaging build

## What is established here, and what is ours

Read this before you use the tree.

**Established and public.** The vocabulary in this file belongs to the value-based pricing discipline and is not our invention: the value cascade from features through to price drivers, the value metric, value fences, good-better-best packaging, the win-keep-grow framing of monetisation, and the economics of price discrimination, whose first, second and third degree taxonomy dates to 1920. The two-part tariff, a platform fee plus usage above an allowance, was formalised in the economics literature in 1971. Van Westendorp's price sensitivity meter dates from 1976. All of that is checkable in public sources.

**Ours, and unvalidated.** The four-way routing tree in step 3, which maps an observed symptom to a named remedy class, is a construction. It was built by us from a philosophy statement about value capture, not from a documented procedure, and it has never been scored against outcomes. There is no dataset behind it, no set of resolved cases, no before-and-after. Use it to order an investigation and to stop the conversation jumping to the number. Do not cite it as evidence, and where your own data contradicts a branch, the data wins and the tree is wrong.

**No invented benchmarks.** This file quotes no churn threshold, no willingness-to-pay figure and no healthy tier mix, because a fabricated number on a page about honest value capture would be the exact failure it is arguing against. Where a number is needed, it names the ratio you compute from your own data, or points at a dated public source and tells you to read the current edition yourself.

If a pricing page already exists and the question is what is wrong with it, that is an audit and a different job. This file starts from a symptom in the business and ends with packaging.

## The claim this skill is built on

Almost every pricing discussion starts at the number, and the number is the last decision.

Two things go wrong before it. First, the symptom is not diagnostic. Churn, flat revenue and discount pressure each have more than one cause, and the remedies are mutually destructive: raising the price against batch-and-churn accelerates precisely the exit you were trying to stop, and cutting it does the same thing more cheaply. Second, most teams cannot yet tell which cause they have, because the distinguishing evidence is cohort behaviour and nobody instrumented it.

So the order is fixed: cascade, then metric, then branch, then packaging, then communication. Each stage is only worth running if the one above it survived.

## Step 1: run the value cascade, in order

Four steps, and the order is the method.

1. **Features.** What the product does. Product-side language is allowed here and nowhere else.
2. **Benefits.** What that lets the customer do.
3. **Value drivers.** The customer-side outcome that changes a number in their world: a cost that falls, a revenue that arrives sooner, a risk that shrinks, a person who is freed.
4. **Price drivers.** The subset of value drivers that different customers value differently. A driver everyone values identically supports a price. Only one that varies supports a structure.

An invented walk-through. Feature: the tool detects broken internal links on every build of a documentation site. Benefit: broken links never reach readers. Value driver: support contacts caused by dead links fall. Price driver: teams with a high support cost per contact and a large site get far more of that outcome than a team with forty pages, so they will pay differently.

**The exit condition.** If the cascade cannot reach a price driver, you have a positioning problem rather than a pricing problem, and nothing downstream will work. The recognisable symptom is that every candidate value driver is a sentence about your product rather than about the customer's numbers.

## Step 2: choose the value metric

The value metric is the unit you charge by. The test is single and falsifiable.

**As the customer succeeds, does the meter move?**

Three possible answers, and only one of them is a metric.

- **It moves with their success.** Candidate. Growth is now automatic rather than something sales has to go and get.
- **It stays flat while they thrive.** This caps you at your worst customer's willingness to pay, because the account getting ten times the value pays the same as the one barely using it. Every expansion is free, and you will feel it later as flat revenue.
- **It moves with your cost rather than their success.** You have turned yourself into a reseller of your own inputs, and you will end up competing at the commodity price of whatever you are passing through.

Three practical gates sit under the test. The metric must be meterable today rather than in principle, verifiable by the customer against their own records, and forecastable by them a year ahead, which is a procurement requirement rather than a preference. A metric that fails forecastability can usually be repaired with an included allowance and a published overage rate.

One implementation detail bites here: the meter must count the same event wherever the customer is. Instrument it in the desktop client on one operating system and not the other, or in the web app and not the mobile one, and you will under-bill part of your base and then lose the argument when you correct it. Count server-side where you can, and prove the totals match across Windows and macOS clients before you price on them.

## Step 3: take the symptom to the branch

This is the constructed part. Four branches, plus a cannot-tell branch in step 4, plus one condition that can co-occur with any of them.

| Symptom you can see | Branch | Remedy class | What not to do |
| --- | --- | --- | --- |
| Heavy use in period one, then cancellation | Batch and churn | Value metric plus retention design | Do not change the price in either direction |
| Nearly everyone buys the cheapest tier | Packaging | Rebuild good-better-best, install fences | Do not add a fourth tier |
| Flat revenue while shipping features | Expansion | Make the meter move for a succeeding customer | Do not treat it as a roadmap problem |
| Persistent discount requests, long cycles | Ambiguous | Disambiguate before acting | Do not lower list price |

**Branch one: heavy use in period one, then cancellation.** This is batch and churn, and it is a value-metric and retention problem rather than a price problem. The customer extracted a finite thing, a backlog cleared, an archive migrated, a library translated, and then left, because there was nothing left to extract. You delivered a one-time transformation and billed it as a subscription.

The remedy has two halves and both are required. Re-cut the metric so it charges for ongoing use rather than one-time extraction, which usually means charging for the thing that keeps being true rather than the thing that was done once. Then add anti-batching, meaning genuine ongoing reasons to return, of which there are three kinds worth building: **freshness**, where the output decays and has to be renewed; **visible progress**, where the customer can see a state improving and would notice it stalling; and **accumulated state**, where the customer has built something inside the product that is costly to abandon, such as a glossary, a rule set, a history or a set of exceptions.

**Do not raise the price.** A higher price on an extractable product accelerates the same exit, because the customer is running a project budget rather than a subscription decision, and a bigger number makes the project shorter and the departure faster. Cutting it is worse: it widens the population who will extract and leave.

The confirming evidence is a ratio you compute yourself, defined in the thresholds section below.

**Branch two: nearly everyone buys the cheapest tier.** This is a packaging problem. The usual cause is that the tiers are the same problem at different volumes, so there is nothing to climb toward, only more of what the customer already has and does not yet need.

The remedy is to rebuild good-better-best so each tier answers a qualitatively different question, then install value fences, meaning the specific attribute that makes a segment self-select upward. This is second-degree price discrimination: the buyer sorts themselves, which is the only sorting that survives contact with a sales team. Fences are also what makes a discount survivable, because discounting without one trains every segment to wait and the list price becomes fiction.

**Branch three: flat revenue while shipping features.** This is an expansion problem, the grow leg of win-keep-grow. New value is being handed to existing customers at the existing price, so value created is not being captured anywhere.

The diagnostic is small and brutal. Name the last three features you shipped. For each, say which meter reading changed for a customer who adopted it. If the answer is "none" three times, the metric is the defect and not the roadmap, and shipping harder will not fix it.

Two legitimate repairs. Put the new value on the existing meter, so adoption grows consumption. Or add a second meter for a genuinely different job, which is a real structural change and should be treated as one. Adding a tier is the third option and the worst, because it re-opens a packaging question you have not answered.

**Branch four: persistent discount requests and long cycles.** Ambiguous, so disambiguate before acting. Run it as an experiment on a small number of live deals with a stated end date, not as a policy.

- **They take the discount and buy.** This is a fence problem. The discount was doing the segmentation your packaging should be doing. Build the fence, then withdraw the discount.
- **They take the discount and still do not buy.** This is a value-communication problem. They do not believe the value driver, and a price they do not believe in is not too high, it is unexplained. Go back and re-run the cascade, and test it by asking a buyer to restate your value driver in their own words. If they cannot, you have found the defect.

Never treat the second case as a price-level problem. Lowering list price against disbelief buys you a cheaper version of the same disbelief.

**The co-occurring condition: margin falling as usage rises.** This is a decoupling problem, specific to products with a variable per-use cost underneath a flat price. It is not one of the four branches because it can appear alongside any of them and it is a question about your cost structure rather than about demand. The platform value and the variable input must be priced as separate things, or the input becomes the product and you compete at the input's commodity price. The per-account arithmetic for it is an audit task rather than a build task.

## Step 4: the cannot-tell branch, and it is a real one

Without cohort data you cannot distinguish a retention problem from a value-metric problem, because both present as churn and the aggregate number is identical in each case.

The minimum you need to see is behaviour by signup cohort across at least two renewal periods. That is two months on monthly billing and two years on annual billing, and the second number is the trap. If you bill annually you cannot wait for the evidence, so you must instrument a leading indicator instead: the per-account consumption trajectory of the value metric, month by month, and read the shape rather than the renewal.

What to instrument, at minimum, each event carrying an account identifier and a timestamp: the value-metric event itself; the tier at signup and every change to it; cancellation with a structured reason rather than free text alone; and reactivation. Four events. This is a week of engineering in most products, and it is the single highest-value week available to a company that cannot answer the question above.

**Do not change price while you are instrumenting.** Changing it destroys the only baseline you had, and you will spend the following year unable to say whether the new number caused the new behaviour. This branch is the least popular output in the file because it is not a decision, it is a delay, and it is nonetheless the correct answer more often than any of the four.

## Step 5: build the packaging

Once the branch is chosen, build the tiers.

**Write the tier sentences before the feature lists.** One sentence per tier, naming who it is for and what qualitatively different problem it solves. If two sentences differ only in a number, you have one tier written twice.

**The cover test.** Cover the price column and hand a salesperson three described buyers. If they cannot say which tier each buyer belongs in without seeing the price, the fences are not doing the work and the price is doing it instead, which is the definition of a packaging failure.

**Every tier boundary is a fence, and every fence maps to a segment's value perception rather than to a company size or an industry.** Segmenting by demographic is third-degree price discrimination and it misfires in a specific way: it assigns the wrong tier to the customer who values you most, the twelve-person team running its whole operation on you, who by headcount belongs at the bottom and by value belongs at the top. Fence on what the buyer needs, and let them sort themselves.

The fence types that hold up are volume, capability that is genuinely useless to the smaller buyer, service level with a real cost behind it, deployment, and commitment. A fence around something that costs you nothing to provide, where the buyer knows it costs you nothing, is recovered from you at renewal. The number of tiers should equal the number of qualitatively different problems you can name. Three is a good default and a bad law.

## Step 6: communicate the change

Attach the value justification and do not apologise. A rise announced with no stated reason reads as extraction, and buyers fill the silence with the worst available explanation. A rise announced apologetically invites negotiation, because an apology is an admission that the number is not defended, and an undefended number is an opening bid. Give one reason plainly rather than three, because three reasons read as none.

Notice periods, grandfathering instruments and discount exception budgets are execution mechanics and are covered elsewhere. What belongs here is the sequencing: the packaging change lands first, the price follows it, and the reason is the same sentence in both announcements.

## Thresholds, stated honestly

This skill carries no invented benchmarks. Here are the ratios to compute from your own data instead, each of which distinguishes something the aggregate cannot.

1. **Batch ratio.** First thirty days of value-metric consumption for an account, divided by that account's median monthly consumption in months three to six. Compute the distribution for cancelled accounts and for retained accounts separately. If cancelled accounts cluster at the top of the distribution, branch one is live.
2. **Expansion share.** Expansion revenue divided by total new revenue in the period. This separates a grow problem from a win problem, which look identical on a revenue chart.
3. **Cohort net revenue retention.** Starting recurring revenue plus expansion minus contraction minus churn, divided by starting recurring revenue, computed one signup cohort at a time and never blended. Blending hides the cohort that is leaving behind the cohort that is arriving.
4. **Tier mix against intent.** Share of new customers landing on each tier over the trailing two quarters, set against the share your packaging intended. The gap is the size of the packaging error, and it is your own number rather than anyone's benchmark.
5. **Discount frequency and yield.** Share of deals closed below list, plus the win rate and the renewal rate of discounted deals against undiscounted ones. If discounted deals renew worse, the discount was buying belief, which it cannot do.
6. **Margin per account.** Revenue minus variable delivery cost per account, sorted ascending, bottom decile only. Never blended, for the same reason as point three.

If you want an external benchmark, read the current edition of a published benchmark survey yourself and record three things with it: the date, the segment definition and the sample size. A benchmark quoted second hand has usually lost its segment, and a churn figure for annual enterprise contracts says nothing whatever about self-serve monthly.

## Worked example, compressed

An invented case. A documentation translation tool, sold at a flat monthly fee per workspace with unlimited words.

**Reported symptoms, all three at once.** Cancellations concentrated in months two and three. Revenue flat across two quarters of shipping features. Sales reporting frequent discount requests and long cycles.

**Cascade.** It reaches a price driver: support contacts from untranslated documentation, and the timing of a market launch, both of which vary sharply between customers. Not a positioning problem, so continue.

**Metric test.** The current meter is words translated per month. Does it move as the customer succeeds? No. Success is a documentation set that is translated and stays translated, and word volume collapses to near zero once the initial set is done. The meter is flat against success and it is also the thing the customer stops needing.

**Branch.** Three symptoms, so order them by evidence rather than by loudness. The batch ratio is roughly eighteen times, and cancelled accounts sit at the top of the distribution. Branch one, batch and churn, is primary.

**Cannot-tell check.** They bill monthly and hold fourteen months of cohort data, so the branch is available. Had they been annual-billed with eight months of history, the correct output would have been instrumentation and no price change at all.

**Discount symptom, disambiguated.** Six live deals, discount offered with an end date. Four take it and buy, which is a fence problem and secondary. Two take it and still do not buy, which is a small communication problem in one segment.

**Verdict: change neither price. Change the meter.** Re-cut it from words translated to pages kept in sync per month, which moves with success rather than against it. Sell the initial bulk translation as a separate one-off project with a minimum, so the extraction is priced as what it is. Build the three anti-batching mechanisms: continuous re-sync when the source page changes, a per-page drift indicator so progress is visible, and a per-customer glossary that accumulates and is costly to abandon. Revisit packaging one quarter after the meter changes, because the current tier mix was produced by a metric being abandoned and therefore tells you nothing.

## Failure modes

**Cost-plus reasoning.** Pricing from what it costs you to serve. Recognisable because the pricing conversation is being led by whoever owns the infrastructure bill, and the ceiling on revenue is now your own efficiency rather than the customer's outcome.

**Competitor matching.** Setting price against a rival's number without knowing whether you sell the same transformation. Recognisable because the justification for the price is a screenshot of somebody else's pricing page.

**Discounting without a fence.** Recognisable because the list price has become fiction internally, everybody quotes the discounted number as the real one, and new buyers arrive already asking for the discount their peer got.

**Volume tiers dressed as good-better-best.** The same problem at three sizes. Recognisable because nearly everyone buys the cheapest tier and stays there, and because your own team cannot say in one sentence who the middle tier is for.

**The flat metric.** Charging by a unit that does not move when the customer succeeds. Recognisable because your best reference customer, the one whose logo you use, pays the same as an account that logs in twice a month.

**Diagnosing churn as a price problem.** Cutting or raising price against batch and churn. Recognisable because the change is followed by the same cancellation shape at a different volume, and because month-one usage in the affected cohort was several times steady state and nobody looked.

**Cost-coupled pricing.** Passing a variable input cost straight through. Recognisable because a supplier's price change forces a customer-facing price change, which is the moment you discover you are selling their product and not yours.

**Repricing before instrumenting.** Recognisable a year later, when nobody can say whether the retention change came from the new price, the new packaging or the seasonality, because all three moved in the same quarter and there is no baseline.

**Demographic fences.** Tiers gated by company size or industry. Recognisable because the customer who gets the most value from you is on the wrong tier, has noticed, and is now negotiating from a position of being visibly right.

## What this skill does not do

- It does not produce a number. It produces a diagnosis, a value metric, a tier structure and a list of ratios to compute, and the number comes from a test or a study afterwards.
- It cannot see your cohorts, margins or billing data, which is why one of its five branches is an instruction to go and build that visibility first.
- It carries no benchmarks, so it will never tell you whether your churn, your tier mix or your discount rate is normal for your market. That question needs a benchmark set you have to buy or field.
- The routing tree is unvalidated. It orders an investigation and it is not evidence, and a branch that disagrees with your own data is wrong.
- It does not model tax, currency, regional purchasing power, payment method economics or contractual constraints, all of which move the effective price materially and none of which it can see.
