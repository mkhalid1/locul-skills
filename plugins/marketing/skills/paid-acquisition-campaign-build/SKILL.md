---
name: paid-acquisition-campaign-build
description: Builds a paid acquisition campaign from an empty account and scales it. Covers the readiness threshold measured against branded organic conversion, the objective staged by company maturity, the required event set, the operational budget floor and what to run below it, audience seeds and lookalike minimums, a three-creative test against a single cold audience, winner detection inside a stated spend band, the inverted scaling structure of one creative across many audiences, creative production rules, the retargeting ladder, message matching by stage of audience awareness, and an ordered fault tree for diagnosing an underperforming campaign. This skill should be used when a paid budget has been approved and the account structure has not been decided, when a first test needs to be designed, or when a running campaign is underperforming and the cause is being guessed at.
---

# Paid acquisition campaign build

## The claim this skill is built on

Two claims, and the second one is the reason this file exists.

**The first: ads cannot fix a page.** People who search for your company by name are the warmest traffic you will ever receive. They already know who you are, they have chosen to look for you, and they arrive with intent that no targeting can manufacture. If that traffic does not convert, cold traffic will convert worse, and every pound spent on ads is being poured into a bucket with a hole in it. The advertising is not the experiment. The bucket is.

**The second: the natural way to scale a winning ad is backwards.** When one creative works, the instinct is to make more creatives, tailored per audience, so each segment sees something written for them. This is intuitive, it is what a thoughtful marketer proposes, and on platforms where engagement attaches to the ad object it usually loses. Every reaction, comment and share sticks to one specific object. Run the same object across ten audiences and the tenth audience sees an ad carrying the accumulated social proof of the first nine. Duplicate the creative ten times and you have created ten objects with zero proof each, and you have thrown away the compounding asset in exchange for wording that fits the segment slightly better. Concentrated proof beats per-audience customisation more often than not.

Everything below is arranged so that neither of those two mistakes is available to you.

## What you produce

1. **A readiness statement**: the measured branded-organic conversion rate against the threshold, with the decision it implies.
2. **A staged objective**: what this spend is for, given where the company is.
3. **An event map**: the six events that must fire before launch, and how each was verified.
4. **A test structure**: three creatives, one cold audience, one ad set, one optimisation event, and a stated spend band for the read.
5. **A scale structure**: one creative, many audiences, plus the retargeting ladder underneath it.
6. **A diagnosis procedure**: the fault tree, in order, with the metric that identifies each branch.

## Step 1. The readiness prerequisite, before any spend

Measure the conversion rate of branded organic traffic, meaning people who arrived by searching your name or typing your domain, and who then did the thing you sell.

**For products under a mid price point, roughly 3 percent is the working threshold.** Below it, do not advertise. Above it, you have a page that converts warm traffic and a plausible chance of converting cold traffic at a lower but usable rate.

Three things to be careful about with this number.

- **Segment properly.** Branded traffic means branded. Mixing in a blog post that ranks for a generic phrase will drag the rate down and tell you to fix a page that is not broken.
- **Expect cold traffic to convert at a fraction of it.** A third to a half of the branded rate is a reasonable planning assumption, so a 4 percent branded rate implies planning at roughly 1.5 to 2 percent cold.
- **This is a readiness check, not the full arithmetic.** Whether the resulting cost per acquisition can be afforded at your price, margin and retention is a separate calculation with its own thresholds, and it belongs to the companion readiness gate. This step only answers whether the destination works at all.

If the threshold fails, the money goes to the page, the offer or the price. This is not a delay, it is a reallocation, and the reason it is first is that everything else in this file becomes unmeasurable when it is skipped.

## Step 2. Stage the objective by company maturity

The single largest source of confusion in paid acquisition is running an early-stage campaign and judging it by a late-stage metric.

**Early.** Ads are a validation instrument. You are testing the offer, the price, the variant, the customer type and the hook, and you are buying observations rather than customers. Return on ad spend is not the metric here, and reporting it as though it were produces an account that gets switched off two weeks before it would have told you something. The output of this stage is a list of things now known: which of five hooks stops the scroll, which of three prices people click through on, which segment engages at all.

**Scaling.** Now the objective is efficient acquisition, and the structure is a balance between cold prospecting and retargeting. Prospecting fills the pool. Retargeting converts it. Neither works alone: prospecting with no retargeting pays full price for every conversion, and retargeting with no prospecting exhausts a finite pool and reports a magnificent return while total new customers fall.

**Established.** Reach and recall, not direct return. At this stage a meaningful share of demand arrives through channels that will never be attributed to the ad that created it, and measuring by last-click return systematically underfunds the thing generating the demand.

Write down which stage you are in, in the campaign brief. It determines the success metric, and a metric chosen after the fact is chosen to flatter.

## Step 3. Instrument before launch

Six events, defined and verified with a real end-to-end test before a single impression is bought:

1. Page view
2. Content view, meaning a meaningful page such as pricing or a product page
3. Lead, meaning an identified contact
4. Add to cart or trial start
5. Checkout started
6. Purchase

Verify each one fires exactly once, in a real browser session, on both Windows and macOS, and once more in a private window with a default content blocker enabled, since a proportion of your audience is browsing that way and an event that only fires for unblocked users under-reports in a shape that looks like poor performance.

Two notes with dates on them. Browser-side tracking has degraded steadily since the mobile tracking-permission changes of 2021 and the successive changes to third-party cookies through 2024 to 2026, so server-side event forwarding is now the default choice rather than an advanced one. And a double-firing purchase event is the single most common instrumentation fault, because it makes a failing campaign look like a working one for exactly as long as it takes to reconcile against your own database.

## Step 4. The budget floor, and what to run below it

**Roughly 50 a day is the working floor for running a proper prospecting campaign**, in pounds, dollars or euros, and the magnitude is similar in each. Below that the delivery system does not have enough budget to explore, the campaign spends its life in the least efficient part of its own model, and the results are noise wearing the costume of data.

This is an operational floor, not a statistical one. The far larger budget needed to read a comparison with confidence is a separate calculation, and it is usually an order of magnitude above this. Fifty a day is simply the point below which the machinery stops behaving.

**The floor covers retargeting first.** If the budget is tight, warm audiences come before cold ones, because they convert at multiples of cold rates and because they are the only part of the funnel where a small budget produces a readable number of conversions.

**Below the floor, do not run a smaller version of the same campaign.** Run three small parallel campaigns instead:

- Content shown to remarketing audiences, meaning people who already visited.
- Cart or checkout abandoners, on a short recency window.
- Your best-performing organic content shown to interest-based audiences, as a cheap way to build the remarketing pool the first campaign needs.

Notice what those three have in common: two of them convert an audience that already exists, and the third manufactures more of that audience at content prices rather than conversion prices. That is what a sub-floor budget can actually do.

## Step 5. Audience seeds, and the lookalike minimum

**Do not build a lookalike or similar audience until the seed contains at least 500 to 1,000 people who performed the event you care about.** Below that the model is extrapolating from noise, and it will produce an audience with a confident name and no signal in it.

Two refinements that matter more than the number.

**Prefer a smaller, better seed to a larger, worse one.** A seed of 800 retained customers produces an audience shaped like retained customers. A seed of 40,000 free-guide downloaders produces an audience shaped like people who download free guides, which will look superb on cost per lead and convert to revenue at close to nothing. Where the seed event is a purchase, the practical quality floor is higher than the minimum stated above, on the order of 1,000 to 5,000, and the companion readiness gate sets out the full ranking of seed events.

**Rebuild on a rolling window.** An all-time seed is dominated by whatever spike is inside it, which is usually a launch, and it slowly stops resembling your current customer. A rolling window of the last 180 days, rebuilt on a schedule, keeps the model pointed at the present.

Seed minimums and matching behaviour are platform mechanics and they move. The figures here are as of August 2026 and should be checked in the platform's current documentation before you build.

## Step 6. TEST. Three creatives, one cold audience, one ad set

The test structure is fixed, and each part of it is doing a job.

**Three creative variations.** Not two, because two gives you a coin flip with a story attached. Not ten, because the budget divides ten ways and nothing accumulates enough events to be read.

**One cold audience.** Every variant is judged against the same population, so the difference you observe is the creative rather than the audience.

**Multiple interests held inside that single ad set**, rather than split across several. This is the part people get wrong for good reasons. Splitting interests across ad sets feels like better science, and it fragments the budget so that no ad set reaches the volume its delivery model needs. Keeping them together gives one neutral population large enough to deliver, and the audience question is answered later, at the scale step, where it is cheap to answer.

**One optimisation event**, chosen deliberately, and not changed mid-test. Changing it restarts the delivery model's learning and makes the preceding data uninterpretable.

Do not boost a post. As of August 2026 a boost creates a simplified campaign with a restricted set of objectives and no ad set structure to speak of, which means it cannot be tested, cannot be scaled, and cannot be reused as the object you later run across many audiences. Boosting is how a campaign that worked becomes a campaign nobody can repeat.

## Step 7. WIN. Reading the winner inside the spend band

**A clear winner typically emerges after roughly 500 to 1,000 in spend on a first test.** Take the highest-return variant, meaning best on the event you chose in step 6, not best on clicks and not best on the one you personally like.

Two guardrails on that band.

**If nothing separates by the top of the band, that is a result.** Three creatives producing indistinguishable numbers means the creatives are three versions of the same idea. Go back and produce three genuinely different concepts, not three crops of one photograph.

**Do not stop early because one variant leads at 200.** Early leads on small numbers reverse routinely, and switching off the eventual winner at that point is the most expensive twenty minutes in the campaign.

Then stop testing creative for now. The account has one winner and the next question is not which second creative to make.

## Step 8. SCALE. One creative, many audiences

Take the single winner and run it across many audiences. Do not build a custom creative per audience.

Three mechanisms, and they compound:

- **Proof concentrates.** As of August 2026, referencing an existing post's identifier when you build a new ad set pools every reaction, comment and share onto that one object across all of them. Duplicating the creative instead creates fresh objects with no engagement. The difference is invisible in the interface and it is why two identical-looking setups perform differently.
- **Events concentrate.** Delivery models need a volume of conversion events before they stabilise. One creative gathers them. Ten creatives divide them.
- **The variable being tested is now the larger one.** Audience differences are usually much bigger than creative differences, so the same amount of data resolves a real difference far more often when the arms are audiences.

The caveat that stops this becoming permanent: creative fatigue is real. Refresh the winner when frequency in a seven-day window climbs above roughly 3, or when click-through rate falls to about half its first-week level, whichever happens first. Refreshing means a new test, back at step 6.

## Step 9. Creative rules

- **Commission real design work.** The image or the first two seconds of video is the highest-return asset in the whole campaign, because everything downstream is multiplied by whether anyone stopped. A campaign with a professional creative budget of a few hundred and a media budget of thousands is correctly proportioned. The reverse is common and wrong.
- **Prefer short video to a still.** Not because video performs better in every case, it does not, but because video generates watch-percentage audiences. As of August 2026 the major social platforms let you build audiences from people who watched a defined share of a video, commonly tiers around 25, 50, 75 and 95 percent, which gives you a graded warm audience from a cold campaign. A still image produces clicks and nothing else.
- **Turn external validation into creative.** Customer testimonials, screenshots of unsolicited praise, and press coverage frequently outperform anything produced internally, because they are the only creative on the platform that a viewer did not assume you wrote. The best creative often originates outside the company, which means the creative pipeline includes a step for collecting it.
- **Never boost. Build.** Stated again here because it is the rule most often broken by the person with dashboard access on a Friday.

## Step 10. The warm ladder

Rank warm audiences by expected return, highest first:

1. **Past purchasers.**
2. **Email engagers**, meaning people who opened or clicked recently.
3. **Content engagers**, meaning site visitors, video watchers and social engagers.

With one important qualification on the first line: **past purchasers belong in their own campaign with their own metric.** The ad to a past purchaser is buying a repeat purchase, an upgrade or a renewal, not a first conversion, so it needs a different offer, a different frequency cap and a different success measure. Leaving purchasers inside an acquisition pool inflates every number in the report, because they were going to buy again anyway and the campaign collects the credit.

**Escalate the incentive with depth, not breadth.** Someone who abandoned a checkout yesterday has demonstrated more intent than someone who read a blog post last month, and the offer should reflect that: nothing or a reminder at the top of the funnel, a stronger incentive only for people who went furthest and stopped. Offering the deepest discount to everyone teaches your whole audience to wait.

## Step 11. Match the message to the stage of awareness

The five stages of awareness were set out by Eugene Schwartz in Breakthrough Advertising in 1966, and they remain the most useful mapping between an audience and a message. Each campaign gets tagged with one stage, and the tag decides what the ad says.

- **Unaware.** They do not know they have the problem. Stories and surprising information. Selling anything here fails, because you are answering a question nobody has asked.
- **Problem aware.** They feel the pain and do not know solutions exist. Benefits and the anxiety of the status quo. Name the problem better than they can.
- **Solution aware.** They know solutions exist and are choosing. Claims and proof: comparisons, evidence, specifics, testimonials.
- **Product aware.** They know you and have not bought. This is the stage where an offer or a discount does real work, because the only remaining question is timing.
- **Most aware.** They are ready. State the product and the price and get out of the way.

The mapping to campaign type is direct: cold prospecting is usually problem or solution aware, content retargeting is solution aware, checkout abandoners are product aware, and past purchasers are most aware. A discount shown to an unaware audience is the classic mismatch and it produces the metric pattern of expensive clicks and no conversions.

## The fault tree, in order

When a campaign underperforms, work down this list in order and stop at the first branch that fails. The order is the whole value, because most conversations start at the bottom.

1. **Product-market fit.** Does anything sell organically at all? If nothing sells without ads, ads are buying traffic to a proposition that does not convert, and no structural change here helps.
2. **Funnel.** Landing page conversion rate, message match between the ad and the page, and the strength of the offer. Message match is the most common defect and the cheapest to fix: an ad promising one thing that lands on a page about something else.
3. **Creative.** Are there enough genuinely different variants? Does the first frame stop the scroll? Is it video?
4. **Targeting.** Audience breadth, seed quality, interest combinations. Last, not first.

**Read the metrics in this sequence**, because the sequence tells you which branch you are on:

- **Click-through rate first.** A low click-through rate is a creative and copy problem. Nothing downstream can be diagnosed until it is fixed, because too few people are arriving to produce a readable conversion rate.
- **Healthy click-through rate with no conversions is a landing page and offer problem.** The ad did its job. The destination did not.
- **Healthy click-through and healthy landing conversion but the cost per acquisition is too high** is a pricing, margin or auction problem, and it belongs to the readiness arithmetic rather than to the campaign.

### The decision rule, including when you cannot tell

- **Click-through rate is materially below the account's own historical norm.** Branch 3. New creative, three genuinely different concepts, back to step 6.
- **Click-through rate is healthy and the landing page conversion rate on paid traffic is far below its organic rate.** Branch 2, and specifically message match. Fix the page or the ad so they say the same thing before touching anything else.
- **Click-through rate and landing conversion are both healthy and the cost is still unaffordable.** Not a campaign problem. Go back to the readiness arithmetic on price, margin and retention.
- **Nothing sells organically.** Branch 1. Stop spending. This is not solvable by advertising and continuing costs money to learn nothing.
- **You cannot tell, because the numbers are too small to read.** Fewer than a few thousand impressions and a handful of clicks per variant means every rate you are looking at is noise, and the most likely outcome of acting on it is switching off the eventual winner. Do exactly one thing: consolidate the budget onto a single ad set and a single creative until the volume is readable, and set a date to look again. Do not restructure, because a restructure resets learning and starts the clock over. Do not add a fourth creative, because that divides the very data you are short of.

## Worked example, compressed

An invoicing tool for freelance tradespeople. £19 per month. Branded organic traffic converts at 4.1 percent, so the readiness prerequisite passes and the planning assumption for cold traffic is roughly 1.5 to 2 percent. Stage: scaling. Budget £75 a day, above the operational floor. Events verified end to end, including a private-window check that revealed the purchase event was firing twice, now corrected.

**Test.** Three creatives against one cold audience with four interests held inside a single ad set, optimising for trial start. Creative A is a product screenshot, B is a founder to camera, C is a customer showing an invoice being sent from a van.

**Read, at £900 of spend.** A: click-through 0.6 percent, 1 trial. B: 0.5 percent, 1 trial. C: 1.9 percent, 11 trials at £47 each. C is the winner, inside the expected band.

**Scale.** C runs across six audiences: three interest sets, a 1,000-person lookalike seeded on paying customers from the last 180 days, and two warm segments. The existing post identifier is reused for every ad set so the 340 reactions already on C carry across. No new creative is produced.

**Two weeks later, one audience misbehaves.** Audience 4 shows a click-through rate of 1.8 percent, healthy, and zero trials from 260 clicks.

**Diagnosis, in order.** Branch 1 passes: the product sells organically. Branch 2: click-through is healthy, so the ad is working and the destination is not. Message match is the suspect, and it is confirmed on inspection, because audience 4 was built around a different problem, chasing late payments, while the landing page talks about sending invoices faster. Branches 3 and 4 are not reached, and note that the instinct in the room was to blame targeting, which is branch 4.

**Verdict.** Keep creative C, keep audience 4, and build one landing page about chasing late payments for it, since that audience produced clicks at the best rate in the account. Do not make a new creative and do not switch the audience off. The campaign is not broken, the destination for one segment is missing, and the metric that said so was the click-through rate.

## Failure modes

**Advertising into a leaking funnel.** Branded traffic converts at 1 percent and the response is a bigger media budget. From the outside this looks like a campaign that never quite works at any spend level, and every optimisation produces a small improvement on a rate that was never going to be enough.

**Boosting instead of building.** The post that did well organically gets boosted, produces something, and cannot be structured, tested, or reused as the object that accumulates proof. Six months later nobody can reconstruct what actually worked.

**Premature lookalikes.** A seed of 120 people generates an audience with an official name and no signal. It spends, it reports plausible costs per click, and it converts at the rate of a random audience, which is what it is.

**Creative sprawl at the scale step.** One custom ad per audience, ten objects, zero accumulated engagement on any of them, and the budget divided ten ways so nothing stabilises. The account looks busy and performs worse than it did with one ad.

**Multi-variable tests.** Creative, audience, placement and offer all change between arms, so the winner cannot be attributed to anything. The result is a decision everyone believes was data-driven and nobody can reproduce.

**Prospecting before retargeting on a tight budget.** The whole budget buys cold clicks, most of which leave, and there is nothing running to convert them on the second visit. Reported cost per acquisition is roughly double what the same money would produce with the order reversed.

**Diagnosing out of order.** The click-through rate already said the creative was the problem, and the meeting is about audience overlap and interest stacking. Recognisable because the proposed fix is always a targeting change and the metric never moves.

**Judging an early-stage validation campaign on return on ad spend.** The campaign was bought to learn which hook and which price work, it is reported against a revenue target it was never designed to hit, and it is switched off in week three with the answers half-collected.

## What this skill does not do

- It does not decide whether paid can work at your price and margin. That arithmetic, the contribution per customer and the conversion rate required, sits in the readiness gate and this begins after it.
- It does not write ads, shoot video, or design anything. It specifies what the creative must be and who should produce it.
- It cannot see the account. Double-firing pixels, conversion events wired to the wrong page, learning resets after an edit and audience overlap are all invisible from here and all common.
- It does not cover search advertising properly. Intent arriving with a query changes the structure enough that the audience-first sequence here is the wrong shape.
- Its platform mechanics are dated to August 2026 and several of them change every year, including seed minimums, learning thresholds, watch-percentage audience tiers, and what happens when a post is reused across ad sets.
- It assumes a self-serve conversion that fires an event. Where the sale closes in a conversation weeks later, every read in this file needs rebuilding around a proxy event, and choosing that proxy well is the hard part it does not do.
