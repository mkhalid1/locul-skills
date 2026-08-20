---
name: paid-acquisition-gate
description: Decides whether paid acquisition can work before any money is spent, using arithmetic rather than judgement. Computes the site conversion rate required by your price, margin and click cost, the budget floor needed for a test to be readable, the learning volume the platform optimiser requires, the seed quality needed for lookalike audiences, and the kill rule to write down in advance. Also covers retargeting segmentation by intent and how to approximate an incrementality test cheaply. This skill should be used before a first or renewed paid budget is committed, and whenever platform-reported conversions are being used as the source of truth.
---

# Paid acquisition readiness gate

## The claim this skill is built on

Most paid acquisition failures are decided before the first ad runs. By the time the campaign is live, the price is set, the margin is set, the landing page converts at whatever rate it converts at, and the click costs what the auction says it costs. Those four numbers determine whether the campaign can work. Creative, targeting and bidding decide where inside that range you land.

This is why "the ads did not work" is nearly always the wrong diagnosis. The ads did what ads do. The arithmetic was never checked.

So this is a set of gates. Each one is a calculation with a threshold, run before spending. Failing a gate does not mean you are bad at advertising. It means the money should currently go somewhere else, and the gate tells you where.

Run them in this order. Gate 1 can end the exercise, and everything after it is wasted if gate 1 fails.

## Gate 1. The conversion gate

**The question: what site conversion rate would paid traffic need for this to work, and how far is that from the rate you actually have?**

Three steps.

**Step 1. Compute the contribution per customer.** Not revenue. Revenue is not what pays for the ad.

`contribution = price × gross margin × expected number of payments`

For a £40 per month subscription at 80% gross margin with an average retained life of 12 months: `40 × 0.80 × 12 = £384`.

**Step 2. Compute the CAC ceiling.** Pick a payback multiple and be explicit about it. A common target for a subscription business is a 3:1 ratio of contribution to acquisition cost, which leaves room for everything the ad does not pay for. So:

`CAC ceiling = 384 / 3 = £128`

For a one-off purchase there is no second payment, so the honest multiple is close to 1:1 unless you have measured genuine repeat behaviour. This single difference is what makes the gate bite so much harder on one-off products.

**Step 3. Compute the required conversion rate.**

`required visitor-to-customer rate = cost per click / CAC ceiling`

At a £2.50 cost per click: `2.50 / 128 = 1.95%`.

Now compare with what your site actually does on cold traffic. If the measured rate is 0.4%, the real cost per acquisition is `2.50 / 0.004 = £625`, which is 4.9 times the ceiling.

**The rule of thumb, with the reasoning rather than as received wisdom:**

- **Required rate is at or below your measured rate.** Spend. You have headroom.
- **Required rate is up to about twice your measured rate.** Spend, carefully and with a bounded budget. Media work realistically halves a cost per acquisition on a new account through better targeting, better placements and better creative, so a gap of this size is a gap media work can close.
- **Required rate is more than about twice your measured rate.** Do not spend on ads. Spend on the page, the offer or the price. The reason for the 2x line is not superstition: media optimisation operates on the numerator, the click cost, and there is a floor under that set by the auction. The page and the offer operate on the denominator, and the denominator is where multiples of improvement are available. Nobody optimises a 5x gap away with better headlines in the ad.

**The counter-intuitive case, worked.** A £15 one-off product at 70% gross margin has a contribution of £10.50 and, with no repeat purchase, a CAC ceiling of about £10.50. At a £2.50 click, the required conversion rate is `2.50 / 10.50 = 23.8%`. Cold traffic does not convert at 24% at any competence level. This is not a warning, it is a proof: **cheap one-off products cannot buy clicks at typical click prices.**

Turn the same equation around to find what would have to be true:

`maximum viable cost per click = CAC ceiling × conversion rate`

At a plausible 3% conversion rate, `10.50 × 0.03 = £0.32`. So the entire question becomes whether any placement anywhere sells clicks at 32 pence. Sometimes broad-reach social or display does, and that reframes the search usefully. Usually it does not, and the correct answers are raising the order value, adding a subscription, or finding a channel that is not priced per click.

## Gate 2. The budget floor

**The question: can this budget produce a readable result in a reasonable time?**

Start from the noise. The relative standard error on a count of `N` conversions is approximately `1 / √N`.

- At 100 conversions, that is 10%.
- At 400 conversions, that is 5%.

When comparing two arms, the standard error of the difference is roughly `√(e₁² + e₂²)`, so two arms at 10% each give about 14%. To call a difference confidently you want it to be roughly 2.8 times that standard error, which is the usual rule of thumb for 95% confidence with reasonable power. So:

- **100 conversions per arm resolves a difference of roughly 40%.** Not 10%. Not 5%.
- **To resolve a 20% difference you need roughly 400 conversions per arm.**

Most people running a first test believe they are measuring a 10% improvement with 30 conversions. They are measuring nothing.

Now convert to money and time.

```
clicks per arm  = conversions needed / conversion rate
cost per arm    = clicks per arm × cost per click
days            = (conversions needed × arms × CPC) / (daily budget × conversion rate)
```

At a 2% conversion rate, £2.50 per click, and a target of 100 conversions per arm across two arms:

- Clicks per arm: `100 / 0.02 = 5,000`
- Cost per arm: `5,000 × 2.50 = £12,500`, so £25,000 for the test
- At £150 per day total: `(100 × 2 × 2.50) / (150 × 0.02) = 500 / 3 = 167 days`

167 days is not a test, it is a subscription to uncertainty, and the market will have moved before it finishes.

Rearranged, to finish in 30 days you would need `(100 × 2 × 2.50) / (30 × 0.02) = £833 per day`.

**The decision:** either fund the floor, or do not run a comparison. If the budget cannot reach the floor for even one arm, run one thing with no comparison and judge it against the gate 1 ceiling on blended cost per acquisition. That is a weaker conclusion honestly labelled, which beats a strong conclusion drawn from twelve conversions.

**The escape hatch, with its cost.** Optimise and measure on an event that happens ten to twenty times more often than a purchase, such as a trial start or a qualified lead. The arithmetic then works at a tenth of the budget. The cost is that you are now measuring a proxy, and proxies diverge: the variant that produces more trials can produce fewer paying customers. Use the proxy to steer and check the real event periodically, and never declare a winner on the proxy alone.

## Gate 3. The learning budget

Platform optimisers need a volume of conversion events before their delivery model is stable, and they publish thresholds of this kind. The long-standing published guidance on one major platform is on the order of **50 optimisation events per ad set per week**. Treat the exact figure as something to verify in the platform's current documentation rather than as a constant, but treat the existence of a threshold as certain.

The consequence is arithmetic:

`weekly budget needed per ad set = 50 × your cost per acquisition`

At a £60 cost per acquisition, one ad set needs roughly £3,000 per week, about £430 per day, purely to stay out of the learning phase. So:

`number of ad sets you can actually afford = weekly budget / (50 × CPA)`

At £2,000 per week and a £60 cost per acquisition, that is `2000 / 3000 = 0.67`. You cannot afford even one. The account structure drawn on the whiteboard, with six ad sets for six audiences, is not affordable, and running it anyway means six ad sets all permanently in learning, all delivering badly, and a conclusion that the platform does not work.

Two further consequences that cost people whole months:

- **Material edits reset learning.** Changing the optimisation event, the audience, the creative, or the budget by a large step restarts the process. A campaign edited every two days never leaves learning, and its performance history is uninterpretable. Set a change cadence in advance, no more often than every three or four days, and hold to it.
- **The learning budget is large relative to most first budgets.** Before you can identify a winner you have to fund the floor from gate 2 and the learning volume here, simultaneously. Most first budgets are an order of magnitude below that. Decide in advance how much of the first month's spend you are treating as tuition rather than acquisition, write that number down, and stop pretending it is a performance campaign.

## Gate 4. Audience seed quality

Lookalike and similar-audience targeting builds a model from a seed list. Platform minimums are commonly around 100 matched people in a single country, and the practical quality floor is far higher, on the order of **1,000 to 5,000 people who performed the event you actually care about**.

But size is the lesser question. **The seed event determines what the model finds.**

Rank your available seed events, best first:

1. Customers retained past 90 days
2. All paying customers
3. Trial starters or qualified leads
4. Newsletter or free-download signups
5. Add-to-cart or pricing page viewers
6. All site visitors
7. Video viewers and social engagers

A seed of 50,000 people who downloaded a free guide produces an audience of people who like free guides. A seed of 800 customers who stayed six months produces an audience that looks like customers who stay. **The second is smaller and better, and choosing the first because it is larger is the single most common way this gate is failed.**

Two traps:

- **Circularity.** If the last six months of converters were bought through badly targeted campaigns, a lookalike built from those converters faithfully reproduces the bad targeting with a statistical veneer on top. Where possible, seed from customers acquired through a channel you did not target.
- **Staleness.** An all-time seed is dominated by whatever spike is in it, often a launch. Rebuild from a rolling window, such as payers from the last 180 days, on a schedule.

## Gate 5. One creative, many audiences

**The structural finding: testing one creative across many audiences usually beats testing many creatives against one audience.** Three mechanisms, and they compound.

**Social proof accumulates on the object, not the campaign.** Reactions, comments and shares attach to the specific ad object. On platforms that expose this, running the same underlying post across many ad sets by referencing its existing identifier pools every interaction onto one object, so the tenth audience sees an ad with visible engagement on it. Duplicating the creative instead produces a fresh object with zero engagement, and all the accumulated proof is thrown away. This is invisible in the interface and it is the reason two identical-looking setups perform differently.

**Optimisation events are counted per ad and per ad set.** Splitting a fixed budget across ten creatives divides the conversion events ten ways, which from gate 3 means every one of them sits below the learning threshold. One creative concentrates them.

**The statistics from gate 2 apply per arm.** Ten creatives on one audience means each arm gets a tenth of the data, so you need roughly ten times the budget to reach the same certainty. One creative across ten audiences is ten arms too, but the arms are audiences, and audience differences are usually much larger than creative differences, so the same data resolves a real difference more often.

The caveat that stops this becoming an excuse: **creative fatigue is real.** As a rule of thumb, refresh a cold-audience creative when frequency in a seven-day window rises above roughly 3, or when click-through rate falls to about half its first-week level, whichever comes first.

The resulting sequence: **find the audience first with one creative, then test creatives inside the winning audience.** Doing it in the other order tests the cheap variable with the expensive data.

## Gate 6. Retargeting segmented by intent

An unsegmented retargeting pool pays the same price for someone who abandoned a checkout yesterday and someone who bounced off the blog five months ago. Rank and separate:

1. **Started checkout, did not complete, last 1 to 7 days.** Highest expected value per person by a wide margin. Small pool.
2. **Trial started, not converted, last 1 to 14 days.**
3. **Pricing page viewed, last 1 to 14 days.**
4. **Product or feature page viewed, last 1 to 30 days.**
5. **Any page viewed, last 1 to 30 days.**
6. **Social or video engagement with no site visit.** This is prospecting wearing a retargeting label. Budget it as prospecting.

**Past purchasers are not a retargeting segment. They are a different business.** Three reasons, and the first is the one that matters:

- The ad is not buying a first conversion. It is buying a repeat purchase, an upgrade, or a renewal. That means a different offer, a different message, a different frequency cap, and a different success metric. Measuring it as cost per acquisition is measuring the wrong thing.
- Including purchasers in an acquisition pool inflates every number in the report, because they were going to buy again anyway and the ad collects the credit.
- Advertising heavily to existing customers has a cost that never appears in the ad account, which is irritation, and irritation shows up in retention instead.

So: **exclude purchasers from every acquisition campaign by default**, and run them as a separate retention campaign whose target is repeat revenue.

Two more practical points. **Recency decay is steep**, and a 180-day catch-all pool performs respectably only because a small recent subset inside it does all the work, which is invisible until you segment. And **segments must be large enough to deliver**: if the 7-day checkout-abandon pool is 200 people, it will not spend the budget, and the right response is to widen the window on that segment rather than merge it with a colder one.

Finally, the honesty note: **retargeting's reported return is the most inflated number in any ad account**, because the audience is defined by having already demonstrated intent. It is usually worth running. It is almost never worth as much as it reports.

## Gate 7. Attribution, honestly

**What a platform-reported conversion actually is:** a conversion that the platform has claimed, under an attribution window the platform chose, using matching the platform performed, according to rules the platform wrote. The platform is paid on the basis of the outcome it reports. That is not fraud, and it does not require anyone to behave badly. It is simply a structural conflict, and it should be treated the way you would treat any other interested party's self-report.

Specific things to know:

- **View-through conversions** count someone who was served an impression and did not click. Whether they belong in your reporting is a decision, and it should be a deliberate one.
- **Attribution windows differ between platforms and have changed repeatedly.** Two platforms can and do claim the same sale.
- **The standard tell:** add up the conversions reported by every platform and compare with the total number of orders in your own database. If the sum exceeds the total, the excess is double counting, and you now know the scale of it.
- **Branded search is the canonical non-incremental spend.** Someone who was already going to type your name types it, sees an ad, clicks it, and buys. The ad reports a conversion and produced nothing.

**Incrementality is the only question that matters:** would this conversion have happened anyway? Three approximations of a holdout, cheapest first:

1. **Scheduled blackout.** Turn everything off for a defined period, two weeks is usually enough, and compare total new customers in your own database against the preceding period and, where seasonality allows, the same window a year earlier. Costs the revenue forgone during the blackout. Costs nothing to build. Confounded by anything else that changed in those two weeks, so pick a quiet fortnight.
2. **Geo holdout.** Split comparable regions into test and control, run in test only, and compare total orders per region from your own data. Better than a blackout because time is held constant. Needs enough volume per region to clear gate 2's noise floor.
3. **Platform-native lift studies**, where offered. Free and well designed, but run and reported by the interested party, so treat the result as directional rather than as evidence.

**The operating rule: choose one source of truth before you spend, and make it your own database.** Manage to blended cost per acquisition, total marketing spend divided by total new customers in the period. It is blunt, it attributes nothing, and it cannot be inflated by a window. Use the platform numbers to steer within a campaign and the blended number to decide whether the channel exists.

A quick credibility check: divide platform-claimed conversions by your own new-customer count for the same period, and compare that with the share of sessions that came from paid. If paid clicks are 10% of sessions and the platform claims 80% of your new customers, the claim is not credible and you have just measured how not credible.

## Gate 8. The kill rule, written before the first ad runs

Write it down before spending, in this exact shape: **metric, threshold, date, action, and a minimum-data condition.**

> By 21 September, if at least 60 conversions have been recorded and blended cost per acquisition exceeds £180, we stop and move the remaining budget to the landing page. If fewer than 60 conversions have been recorded by that date, the test did not run, and we stop by default.

The minimum-data condition is the part everyone omits and it is the part that does the work. Reaching the date without enough conversions is not a failure and it is not a success. It means the test did not happen, and the honest response is a single decision, made once: fund it to the floor from gate 2, or stop. **Stopping is the default, because a test you cannot afford to finish is a test you cannot afford.**

Also write down who is permitted to override the kill rule. If nobody is named, the person who overrides it is whoever is looking at the dashboard late at night, and they will always find a reason.

## The go or no-go decision rule

- **Required conversion rate is at or below the measured rate.** Spend.
- **Required rate is between one and two times the measured rate.** Spend a bounded amount, on one variable, with the kill rule set at the required rate and the budget floor funded for a single arm.
- **Required rate is more than twice the measured rate.** Do not spend on ads. The money goes to the page, the offer, the price, or the retention that lengthens the contribution.
- **The budget cannot reach the floor for even one arm.** Do not run a test. Run one thing, judge it on blended cost per acquisition against the ceiling, and label the conclusion as weak.
- **You cannot tell, because there is no measured conversion rate at all.** Do not start on paid. Paid traffic is the most expensive way to learn a conversion rate, because you are paying per observation. Get the first several hundred visitors from any channel that is not priced per click, measure, and come back. If no such channel genuinely exists, cap the learning spend in advance at a figure you would be content to write off entirely, and call the line item measurement rather than acquisition, so that nobody later reports it as a failed campaign.

## Worked example, compressed

A document collaboration tool for small professional firms. £29 per month, 75% gross margin, average retained life 10 months. Organic visitor-to-paid conversion is measured at 1.1%. A quoted cost per click in the category is £3.20. Proposed budget: £3,000 per month for three months.

**Gate 1.** Contribution is `29 × 0.75 × 10 = £217.50`. At a 3:1 target, the CAC ceiling is `217.50 / 3 = £72.50`. Required conversion rate is `3.20 / 72.50 = 4.4%`.

The measured 1.1% is organic, and organic traffic is warmer than a cold ad click, so the honest planning assumption is roughly a third to a half of it: call it 0.5%. Implied cost per acquisition is `3.20 / 0.005 = £640`, against a ceiling of £72.50. **The gap is about 8.8x. Gate 1 fails, decisively.**

**Gate 2, for completeness.** Even setting gate 1 aside, reaching 100 conversions at £640 each costs £64,000. The proposed budget is £9,000 in total, which buys roughly 14 conversions, which resolves nothing. **The budget floor fails too, by about 7x.**

**What would have to change, and by how much.** Turning the gate 1 equation around, the maximum viable click price is `72.50 × 0.005 = £0.36`. So one path is finding placements at 36 pence, which in this category is implausible on search and only conceivable on broad-reach social with an unqualified audience, which would in turn lower the conversion rate again.

Testing the levers one at a time:

- **Double retention from 10 months to 20.** Ceiling rises to £145, gap falls to about 4.4x. Still fails.
- **Raise cold conversion from 0.5% to 2%**, which is a landing page and onboarding project, not an ads project. Gap falls to about 2.2x. Still fails, but now it is in media-work range on the next lever.
- **Both together.** Ceiling £145, conversion 2%, implied cost per acquisition `3.20 / 0.02 = £160` against £145. Gap about 1.1x. Borderline, and worth a bounded test.

**Verdict: do not spend.** Paid acquisition here is downstream of two changes that have nothing to do with advertising, and realistically at least two quarters away. The constructive finding is that the £64,000 a valid test would cost buys a great deal of landing page, pricing and onboarding work, and that work is on the critical path to paid working at all. Revisit the gate when cold conversion is measured above 1.5% and retained life above 15 months, and not before.

## Failure modes

**Blaming the optimiser for the offer.** Six weeks of creative iteration on a campaign that failed gate 1 by 5x. The creative was never the binding constraint and no amount of it will be.

**Declaring a winner on thirty conversions.** The observed difference is inside the noise, the loser is switched off, and the account is now optimised on a coin flip that everyone believes was data.

**Restructuring weekly.** Every restructure resets learning, so the account is permanently in its worst-performing state, and the performance history cannot be read because no configuration ever ran long enough.

**Ten creatives against one audience.** The budget is divided ten ways, nothing exits learning, the engagement that would have accumulated on one object is scattered across ten, and the test resolves nothing.

**The lookalike built from the wrong event.** A seed of free-guide downloaders produces an audience that downloads free guides, spends well on cost per lead, and converts to paying customers at close to zero. The account looks efficient the whole time.

**Purchasers left inside the retargeting pool.** Reported return on ad spend is excellent because existing customers keep buying, and the campaign is scaled on the strength of a number that was measuring loyalty.

**Platform-reported conversions used as the source of truth.** Three platforms claim 140% of the actual orders, the budget is allocated according to who claims hardest, and the channel that gets scaled is the one with the most generous attribution window rather than the one producing sales.

**No kill rule, so the kill decision is emotional.** The campaign is stopped in a bad week and restarted in a good one, and every restart resets learning, which guarantees the next week is bad too.

**Testing the cheap variable.** Headline tests while the audience, the offer and the price go untested. Headlines move results by a modest amount. The offer moves them by multiples, and it is the thing nobody wants to reopen.

## What this skill does not do

- It does not know your cost per click or your cold conversion rate. Those are the two inputs everything else rests on, and it will ask for them rather than estimate them.
- It does not write ads, choose keywords, build audiences in the interface or manage bids. It decides whether to spend, on what scale, and when to stop.
- It does not run statistics properly. The square-root arithmetic here is for the meeting. Use a real sample size calculator before committing a large budget.
- It cannot see the account, so it cannot tell you whether learning has reset, whether the pixel is firing twice, or whether the conversion event is even wired to the right thing. Those need someone inside the account.
- It does not restate current platform specifics, because attribution windows, learning thresholds and audience minimums change. It tells you which thresholds exist and to go and check the current values.
- It does not evaluate whether the product is worth advertising. A campaign that passes every gate here can still sell something nobody wants twice.
