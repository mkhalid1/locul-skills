---
name: paid-social-turnaround-plan
description: Diagnoses a paid social account that is already running and losing money, then produces an ordered remediation plan and a verdict on whether the channel should continue. Carries a catalogue of fourteen named failure shapes with the dashboard signature and the discriminating observation for each, the arithmetic for how much data a decision needs before it is a decision rather than noise, a one-change-per-cycle remediation order, and an explicit stop test written as checkable conditions. This skill should be used when an ad account has been spending for weeks with poor results, when somebody is about to restructure or switch off an inherited account, or when the response to a bad month is about to be a discount or a bigger budget.
---

# Paid social turnaround plan

## The claim this skill is built on

A failing ad account is almost never failing for the reason discussed in the meeting about it. The meeting is about targeting. The dashboard, read properly, is usually pointing at delivery structure, at a creative that has aged out, at a page that contradicts the ad, or at economics that were never going to work.

The obvious approach is to change several things at once and see if next week is better. It fails for a reason that has nothing to do with which things you changed. Every change resets the platform's optimisation, so the following week is not a measurement of your fix, it is a measurement of a system relearning. Change five things and the account gets slightly better or slightly worse for reasons nobody can attribute, and the next decision is made on that.

So the order here is fixed: establish whether anything can be concluded at all, run an elimination sweep that maps the account's numbers onto a named shape, then change one thing per cycle in a ranked order, and hold a stop test that can actually fire.

**Read this before any number below is used.** Every constant taken from the source retrospective is one large advertiser's experience in an earlier platform era, around 2020, and is marked as such where it appears. Click prices, click-through rates and the cost of a readable test have moved since, generally upward, and they vary by country, placement, vertical and season. The arithmetic is the durable part. The constants are not. Replace each one with your own account's trailing figure before any decision rests on it, and where this file gives a mechanism without a number, that is deliberate, because the mechanism survived and the number did not.

## Part one. The readability gate, or what you are allowed to conclude

Most accounts are killed on data that could not have supported the conclusion. Three constraints run at once and the binding one is whichever is largest. Compute all three before opening the catalogue.

**Constraint one, rate resolution.** The relative uncertainty on a count of `n` events is roughly `1 / sqrt(n)`. Nine clicks carries about 33 per cent uncertainty, 25 carries 20 per cent, 100 carries 10 per cent. Two arms at 100 clicks each can therefore separate a difference of roughly 30 to 40 per cent between them, and nothing finer. This is the arithmetic behind the working figure of about 100 clicks per arm reported in the source retrospective, and it is why a 10 per cent difference in click-through between two creatives at 40 clicks each is not a finding, it is the same number twice.

**Constraint two, the calendar.** The earliest honest read date is not a preference:

```
earliest read = date of last material edit
              + the platform's learning period after a material edit
              + one full seven-day cycle, because weekday and weekend behaviour differ
              + the click attribution window, because the most recent days are incomplete
```

A material edit means the optimisation event, the audience, the creative, or a large budget step. The attribution term is the one people forget: with a seven-day click window, the last seven days of any report are still filling in, and a report read on Monday about the week just gone is systematically pessimistic.

**Constraint three, money.**

```
clicks needed  = clicks per arm x number of arms
spend needed   = clicks needed x your own trailing cost per click
days at budget = spend needed / daily budget
```

At 100 clicks per arm across three arms and a click price of 1.85 in your currency, that is 555 of spend, which is under five days at a budget of 120 a day. So on that account the calendar binds, not the money, and raising the budget buys nothing except a faster arrival at the same wait. At a budget of 25 a day the same test takes 22 days and money binds, and the correct response is to cut to two arms rather than to run three badly.

**The thresholds reported by the source, all labelled.** A single large advertiser working around 2020 reported that a clear creative winner usually emerged after 500 to 1,000 US dollars of test spend, that a fair test needed one to two weeks and roughly 100 clicks, that around 2,000 to 4,000 US dollars of learning spend was appropriate for a product under 100 dollars before concluding the channel itself was wrong, and that roughly twenty ads in a hundred proved scalable, "if that". Treat all four as that account's experience in that era. The useful thing about the last one is the expectation it sets: a low hit rate is the normal condition and not evidence of incompetence.

**The reading cadence.** The same source reported reviewing daily below roughly 2,000 dollars of cumulative spend as a leading cause of killing eventual winners at three hours on one lucky or unlucky conversion. The durable version drops the dollar figure: do not read a cell more often than the cadence at which it accumulates the events your resolution arithmetic requires, and for most accounts that is weekly, not daily.

## Part two. The elimination sweep

Run these eight reads in order. Each one eliminates part of the catalogue, so by the end you are choosing between two or three shapes rather than fourteen. This is deliberately not a stop-at-the-first-failure tree, because a failing account usually carries three shapes at once and the ranking matters more than the first hit.

1. **Reconcile.** Platform-reported conversions against your own order or signup table for the same window. Confirms or clears shape 14.
2. **Delivery.** Is the full budget actually being spent, at what cost per thousand impressions against the account's own earlier figure, and are the ad sets out of the learning phase. Shapes 1, 2, 11.
3. **What the money buys.** Click-through rate against the account's own figure from six to eight weeks ago, and frequency over the same window. Shapes 3, 7, 8, 9.
4. **After the click.** Paid landing page conversion against the same page's conversion on organic or email traffic, then each funnel step separately. Shapes 4, 5.
5. **Composition.** What share of spend sits on cold prospecting against warm and retargeting, and what happened the last time the budget went up. Shape 6.
6. **The test log.** What has actually been tested, at what spend each, and whether any of it could have resolved at that spend. Shape 10.
7. **Promotion history.** Revenue by week against the discount calendar. Shape 12.
8. **Economics.** True cost per acquisition from your own table against contribution per customer. Shape 13.

## Part three. The failure catalogue

Fourteen shapes. Each carries what it looks like from the dashboard, what separates it from the shape it is most often confused with, and the remedy.

### Structure and delivery

**1. The boosted account.** Spend sits on promoted posts rather than on campaign structure. Signature: healthy engagement, an almost empty conversion column, and results that do not survive a budget increase. Discriminator against weak creative: engagement rate is fine, so the creative is working and the container is not. Remedy: rebuild as campaigns in the ads manager, reusing the existing post identifiers so accumulated engagement carries. The source retrospective calls this the single biggest mistake it saw.

**2. The over-stacked audience.** A broad similar audience with interests layered on top, one narrower similar audience excluded, and recent site visitors excluded as well. Signature: delivery far below the daily cap, cost per thousand impressions well above the account's own norm, frequency climbing fast against a small reach, and an ad set that never leaves the learning phase. Discriminator against fatigue: reach was small from day one rather than decaying from a healthy start. Remedy: remove one exclusion or one layer per cycle, never all at once, because you want to know which layer was doing the suffocating.

**11. The daily-read account.** Signature: high ad-object churn, many objects with tiny lifetime spend, most ad sets never out of learning, and account-level results with no trend at all because nothing ran long enough to have one. Discriminator: this is visible in the change log rather than in the metrics. Remedy: freeze the account for one full read cycle from part one and change nothing, which is harder to get agreed than any other item here.

### What the money buys

**3. The thumb-stop failure.** Signature: impressions plentiful, click-through rate far below the account's own earlier figure, cost per click above the account's norm, frequency low. Discriminator against targeting: click-through is weak on warm audiences too, and warm audiences already know you, so this is the creative. Remedy: three genuinely different concepts, not three crops of the same image.

**7. Dispersed social proof.** The same idea shipped as many separate ad objects. Signature: a dozen creatives each carrying trivial comment, share and reaction counts, none accumulating, and cost per thousand impressions drifting upward. Discriminator: the account has plenty of creative and none of it has any visible proof attached. Remedy: consolidate onto one post identifier reused across ad sets so engagement accrues on a single object. One operator in the source ran a single ad across dozens of campaigns and several hundred ad sets for this reason. The spend and return figures attached to that account are unverifiable and are deliberately omitted here.

**8. Creative fatigue.** Signature: click-through falling week over week on an ad that used to work, frequency climbing inside the same audience, cost per thousand impressions roughly stable. Discriminator: the decline tracks time and frequency, not budget. Remedy: new concepts. Refreshing the copy on the same image usually buys days.

**9. Audience exhaustion and overlap.** Signature: several ad sets degrading at the same time, cost per thousand impressions rising across all of them at once, and delivery lurching between ad sets day to day as they bid against each other. Discriminator against fatigue: new creative also underperforms, and more than one ad set is affected simultaneously. Remedy: consolidate overlapping ad sets, or move to a genuinely different audience pool.

**10. Trivia testing.** Signature: a test log of pack colours, button labels and minor variants, every result inside the noise band, and no single test funded to the resolution its own arithmetic required. Discriminator: compute the clicks each test received and compare against constraint one. Remedy: test the offer, the price, the hook and the format. A price test moves revenue. A colour test does not, and the store's existing data usually answered it anyway.

### After the click

**4. The message-match break.** Signature: click-through healthy, bounce high, and landing page conversion on paid traffic far below the same page's conversion on organic or email traffic. Discriminator against a weak offer: the page converts other traffic perfectly well. Remedy: make the page's first screen repeat the ad's promise in the ad's words, or change the ad.

**5. The checkout leak.** Signature: the early funnel steps look normal and the final step does not, with completion far below the rate the same funnel used to achieve, often concentrated on one device class or one browser. The source reported 40 to 50 per cent of people reaching the cart completing checkout as its own working figure around 2020. Use the shape of that claim, which is that the final step should be compared against the funnel's own history, rather than the number. Remedy: fewer fields, one page, wallet payment options, faster hosting, a visible refund line, and a test on the device where the drop concentrates.

### Composition, economics and measurement

**6. The harvesting account.** Spend sits almost entirely on retargeting and existing-customer audiences. Signature: excellent reported return, small absolute volume, a retargeting pool flat or shrinking week over week, and performance that degrades the moment the budget rises. Discriminator against fatigue: the decline is a function of budget rather than of time. Remedy: fund the top of the funnel, and expect the blended return to fall while total contribution rises. Judge it on total contribution or the arithmetic will tell you to stop doing the correct thing.

**12. Discount dependence.** Signature: revenue spikes aligned exactly to promotional weeks, near-flat weeks in between, average order value falling across quarters, and repeat purchases clustered in discount periods. Discriminator: overlay the discount calendar on the revenue chart and the account explains itself. Remedy: stop answering weak performance with a price cut, which trains the audience to wait, and recompute cost per acquisition excluding code-assisted orders before judging anything else.

**13. Structurally impossible economics.** Signature: click-through healthy, page conversion healthy, delivery healthy, and cost per acquisition still above contribution per customer. Nothing in the account is broken. Discriminator: every operational metric is at or above the account's own norms and it still loses money. Common with single-product stores, low-priced one-off items and thin margins. Remedy: this is not an ads problem, and it is a legitimate finding rather than a failure of diagnosis. It belongs to price, order value, retention or a channel that is not priced per click.

**14. The measurement mirage.** Signature: platform-reported conversions exceed the orders in your own table, or the sum across platforms exceeds your total order count, or a large share of conversions are view-through, or the pixel fires twice. Discriminator: your own database disagrees with the dashboard. Remedy: fix the instrumentation, then manage every ratio from the internal table. Until this is done, every other diagnosis is being graded against a flattering number.

## Part four. Remediation order and the one-change rule

**The order is not by expected gain. It is by what makes the rest readable.**

1. **Measurement first**, always. Costs no media spend and changes the denominator of every other judgement.
2. **Anything capping the data rate second**, meaning suffocated delivery or unstructured spend. You cannot buy resolution from an ad set that will not deliver.
3. **Then the leg with the largest gap to its own history**, comparing the click-through leg against the account's own earlier figure with the landing page leg against the same page's organic conversion. Take the larger shortfall.
4. **Economics last**, because the economics decide whether the account can ever work and the other three decide where inside that range it lands.

**One change per cycle, and a cycle is one earliest-read-date from part one.** The single permitted relaxation: you may bundle changes inside one leg, for example three new creative concepts at once, and you must then accept that you will learn about the leg and not about the change. Never bundle across legs. New creative and a new landing page in the same week produces a number that belongs to neither.

**Bid strategy, as a family taxonomy rather than product names**, because the names and the availability change and must be checked in current platform documentation. Automatic or lowest-cost bidding is hands-off, spends the full budget, gives no cost control, and gets more expensive as cheap inventory is consumed. A bid cap sets a ceiling on the bid and may leave budget unspent. A cost cap seeks volume within an acceptable acquisition cost and pays for it with a longer and more volatile learning phase. A target cost holds cost steady and declines cheaper results. Value optimisation with a minimum return floor under-delivers when the floor is set too high. Choose the family from the diagnosis: an account with a suffocated ad set does not want a tighter cap. One practitioner trick from that era, useful and not scalable: when a cost-capped ad set refuses to deliver at all, raise the target by one unit of currency per day until it starts spending.

## Part five. The decision rule, three branches

**Branch one, fix.** You have a named shape, the account cleared the readability gate, and at least one untested remedy could plausibly move cost per acquisition by the multiple required. Execute one cycle, one change, with the read date written down before the change is made.

**Branch two, you cannot tell yet.** Triggered by any of: fewer clicks than constraint one requires, less elapsed time than constraint two requires, more than one variable changed inside the window, a learning reset inside the window, an attribution window that has not closed, or a dashboard-to-database disagreement above your stated tolerance. This branch is only useful if it is priced, so it has four required outputs and is incomplete without all four.

- **The ambiguity, named.** Two catalogue entries that both fit the current signature. Not "we need more data".
- **The discriminating observation, decided in advance.** The observation, and what each outcome means, written before it runs. Useful ones: run the current creative unchanged to a fresh non-overlapping audience, where recovery means exhaustion and no recovery means fatigue. Send an equal number of email or organic visitors to the same page, where their converting and paid traffic not converting means message match and neither converting means the offer. Pause a defined geography for a defined period and compare your own order table, which tests measurement against reality.
- **The price.** Clicks needed times your own click price, plus the days at the current daily budget. State both, because one of them is usually the binding one and it is often the days.
- **The refusal clause.** If the discriminating observation costs more than the learning budget you have left, you do not buy it. You take the cheaper of the two remedies, mark the other as untested, and move on. Without this clause the branch is procrastination with a rule attached.

**Branch three, stop the channel.** All five conditions below, checked and written down. Any one of them unmet keeps you in branch one or two.

1. Cumulative spend since the turnaround began has passed the learning budget you computed in part one, which is clicks needed times your own click price times the number of ranked hypotheses.
2. At least two complete remediation cycles have run, each a single change, each read on or after its earliest honest read date.
3. Trailing four-week cost per acquisition, computed from your own order table and not from the dashboard, is flat or worse across those cycles.
4. The best-performing cell in the account, not the account average, still has a cost per acquisition above contribution per customer divided by your chosen payback multiple.
5. No catalogue entry with an untested remedy remains that could plausibly move cost per acquisition by the multiple you need.

**The single strongest indicator inside that test** is the size of the gap on condition 4. Media work operates on the click price, and the auction puts a floor under that. A gap above roughly twice your ceiling is not closed by better targeting or better creative, and the reasoning behind that line sits in the readiness gate skill rather than being repeated here.

**Stopping properly is a procedure, not a switch.** Export ad-level reporting before anything is archived. Leave the pixel and the conversion events firing, because that history is the seed for any future campaign and it decays. Do not delete audiences or ad objects. Keep a genuinely profitable retargeting cell running at a small floor if, and only if, it clears true break-even on your own numbers with branded search traffic excluded. Write a re-entry condition as one measurable statement, for example that you will retest when price rises above a stated figure, or when retained life passes a stated number of months, or when the page converts organic traffic above a stated rate. Then move the budget. Email, content, organic search, partnerships and community are legitimate destinations, and plenty of businesses reach scale without paid social at all.

**On return on ad spend as a threshold:** the source's numeric targets from that era are omitted deliberately, because a return figure only means anything against true break-even computed from all costs, meaning goods, wages, returns, processing and refunds, rather than against ad spend alone. Compute your own break-even multiple and use that. A related mechanism worth keeping without its numbers: shares divided by reach is an underused signal, because a share is a free impression, and the highest-share creative that also clears break-even is the one to fund. Share ratio falls as spend rises, so only compare creatives inside the same spend band.

## Worked example, compressed

A subscription service posting loose-leaf tea. Price 22 a month, gross margin 62 per cent, average retained life seven months, so contribution is `22 x 0.62 x 7 = 95.48`. At a 3:1 payback target the acquisition ceiling is `95.48 / 3 = 31.83`. The account has spent 120 a day for nine weeks, so 7,560 in total. Trailing cost per click 1.85.

**The sweep.**

1. **Reconcile.** The dashboard claims 118 new subscribers. The internal table shows 71. True cost per acquisition is `7,560 / 71 = 106.48`, against a dashboard figure of `7,560 / 118 = 64.07`. Shape 14 confirmed, and every ratio the team has managed to for nine weeks has been about 66 per cent flattering.
2. **Delivery.** Full budget spent, ad sets out of learning, cost per thousand impressions up 8 per cent. No suffocation, so shape 2 is cleared.
3. **What the money buys.** Click-through 0.74 per cent now against 1.30 per cent six weeks ago, a 43 per cent shortfall, while frequency in the main prospecting ad set has gone from 1.4 to 3.9. Two shapes fit: fatigue and exhaustion.
4. **After the click.** Paid landing page conversion 1.1 per cent against 3.6 per cent for the same page on branded organic traffic. Shape 4 is a third live candidate.
5. **Composition.** 78 per cent of spend is on cold prospecting, so shape 6 is cleared.
6. **Test log.** Three tests in nine weeks, all pack-colour variants, each under 200 of spend, none capable of resolving anything. Shape 10 confirmed as a process fault.
7. **Promotions.** Two discount codes ran in the period and 44 of the 71 subscribers used one. Shape 12 flagged.
8. **Economics.** `106.48 / 31.83 = 3.3x` over the ceiling.

**Readability.** Two arms at 100 clicks is 200 clicks, `200 x 1.85 = 370`, about three days of budget. But the calendar needs one full week plus the seven-day click window after the last edit, so the earliest honest read is fourteen days. The calendar binds. Raising the budget buys nothing.

**Branch two, priced.** Ambiguity: fatigue against exhaustion. Discriminating observation: run the current creative unchanged to a fresh non-overlapping audience for seven days, decided in advance as click-through at or above 1.2 per cent meaning exhaustion and click-through near 0.74 per cent meaning fatigue. Price: 370, taken from the existing budget, no incremental spend, seven days. Result: 0.81 per cent. **Fatigue, not exhaustion.**

**Ranked remediation.** Measurement first and it is already done. Then the larger gap, which is the click-through leg at 43 per cent below the account's own figure. Cycle one is three genuinely different creative concepts. Say the expected outcome out loud before running it: if click-through returns to 1.30 per cent at a stable cost per thousand impressions, cost per acquisition falls by roughly the same proportion, from 106 to about 61, which is still nearly twice the ceiling. **Cycle one cannot succeed on its own, and stating that in advance is the point of ranking.** Cycle two is the landing page leg, 1.1 per cent against 3.6 per cent. Closing even half that gap, to 2.3 per cent, roughly halves cost per acquisition again, from about 61 to about 31, against a ceiling of 31.83.

**Verdict: fix, two cycles, one change each, first read date fourteen days out.** The combined plausible outcome is borderline rather than comfortable, and it is stated as borderline. Two conditions are pre-registered now, while it is still possible to act on them. First, the stop test fires if after both cycles the best cell's cost per acquisition on the internal table is above 64, which is roughly twice the ceiling, because a gap that size is not closed by media work. Second, because 44 of the 71 subscribers arrived on a discount code and their retained life is unknown, cost per acquisition must be recomputed excluding code-assisted subscribers before either cycle is judged. The stop test was checked at the start and not met, which is why this is a turnaround rather than an exit.

## Failure modes of the turnaround process

These are distinct from the catalogue. The catalogue is how accounts fail. These are how the fix fails.

**The five-at-once fix.** Creative, audience, budget and landing page all change in one session. Optimisation restarts, next week measures a system relearning, nothing is attributable, and the account is now a worse diagnostic instrument than it was before anyone helped.

**The clean-slate restart.** The account is rebuilt from scratch because it looked messy. Conversion history, accumulated engagement and audience data are discarded, and the new account pays full price to relearn what the old one already knew. Recognisable because performance gets worse for a fortnight and everyone calls it the learning phase.

**The dashboard-only diagnosis.** Every conclusion drawn from the platform's own reporting, which is also the party being judged. The reconciliation step is skipped because it is boring, and the whole plan is optimised against a number that overstates.

**The moving read date.** A read date is set, then pushed back each time an interim number looks bad, so no cycle ever completes and no change is ever evaluated. The account has been in turnaround for five months and has completed zero cycles.

**The unranked plan.** The remediation list contains fourteen items in no order, so the team does the cheap ones first, the binding constraint stays untouched, and the plan is reported as 60 per cent complete while nothing that mattered has moved.

**No pre-registered stop.** Nobody wrote down before the cycle what result would mean stop, so every result is read as a reason to continue. This is the failure that makes the abandon branch unreachable in practice, and it is why the stop test in part five is written before cycle one rather than after cycle three.

**Remediation by discount.** The response to a weak month is a price cut. The following week looks better, the change is credited to the fix, and the audience permanently learns to wait for the next code. The damage shows up two quarters later in average order value.

**The re-diagnosis loop.** Weeks of reporting, dashboards and analysis while the account runs unchanged and spends its learning budget on nothing. Recognisable because the deliverable of every cycle is a document.

**The handoff restructure.** A new agency, freelancer or hire restructures on day one, which is understandable and destroys the previous cycle's data before anyone read it. The account is now unreadable for the length of another learning period, funded by the client.

**Crediting the season.** The fix ships in the same week as a seasonal upturn, a promotion or a press mention, and the improvement is attributed to the fix. The next account gets the same change applied confidently and nothing happens.

## What this skill does not do

- It does not decide whether paid should be running at all. That arithmetic, the contribution per customer against the conversion rate paid traffic would need, belongs to the readiness gate, and this file assumes money has already been spent.
- It cannot see the account. A double-firing pixel, an event on the wrong page, overlapping ad sets and a silent learning reset after an edit are all invisible from here, and each one is capable of producing a confident wrong diagnosis.
- Every constant in it is one advertiser's experience from an earlier platform era and several would now mislead if used directly. The arithmetic survives. The numbers must be replaced with your own account's trailing figures before any decision rests on them.
- It does not produce creative. It says which leg is failing and by how much against the account's own history. Somebody still has to make three genuinely different concepts, and the quality of those decides whether the cycle works.
- It does not cover search advertising properly. Intent arriving with a query changes which shapes are even possible, and several entries here have no equivalent on a search network.
- It cannot judge whether the product is wanted. If nothing sells organically, the account is not the subject and no remedy in the catalogue applies.
- It does not model a long consultative sale. Every signature assumes an event fires inside an attribution window, and where the sale closes in a conversation weeks later the whole diagnosis has to be rebuilt around a proxy event.
