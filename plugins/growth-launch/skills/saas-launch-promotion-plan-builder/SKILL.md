---
name: saas-launch-promotion-plan-builder
description: Builds a promotion plan for a SaaS launch, a net-new product, a paid feature behind an existing base, or a pricing change, a scored and ranked list of the nine channels a real SaaS launch has available (launch-day directories, developer and founder communities, lifecycle email, in-app surfaces, partners and affiliates, paid social and search, comparison pages, changelog syndication, and the founder's own audience), a dependency-ordered checklist with an owner and a trigger on each item that gates on a working trial and staffed support before any channel opens, and a Promotion Yield metric split into a projected and a confirmed reading so subscription churn cannot hide behind an early number. This skill should be used when a SaaS launch is being planned before any channel has been scheduled or funded, or when a past launch's channels need to be compared on trial-to-paid economics to decide next quarter's budget.
---

# SaaS launch promotion plan builder

## The claim this skill is built on

A SaaS launch promotion plan is not a list of channels you intend to try. It is an ordered execution schedule in which every candidate is scored before anyone commits budget, every checklist item has a named owner and a trigger condition rather than a calendar date, and every channel's result is expressed in one metric that lets you compare paid search against an affiliate relationship against a lifecycle email, even though a subscription pays back over months rather than all at once.

The obvious approach, list the channels you can think of and start with whichever one feels right, fails for four reasons specific to a SaaS launch. Without a score computed before spend, effort spreads evenly across channels that will not return it evenly. Without a stated order, channels run before the thing they depend on is ready: paid spend at an untested sign-up flow, or a partner pitch made before a single trial has converted. Without a gate on the trial and on support readiness, the launch's highest-intent traffic, the first 48 hours, arrives at a product with no onboarding sequence and an unwatched support inbox. Without a shared metric that accounts for churn, next quarter's budget call rests on a number taken too early, before a cohort that looked profitable in week one has had time to cancel.

This skill produces the plan itself: the scored and ranked channel list, the dependency-ordered checklist with an owner and a trigger on each line, and the metric that decides, once churn has had time to show itself, which channel gets more money next.

## Step 1: build the candidate list wider than the obvious three

Before ranking anything, list every channel that could plausibly carry this launch, not the two or three that come to mind first. A real SaaS launch has nine kinds of channel available, split across four categories, because each category fails differently later and needs scoring against its own kind, not a paid channel's benchmarks.

- **Owned.** Lifecycle email to the existing base or a beta waitlist. In-app announcement surfaces: a banner, a changelog panel, a product-tour step. Comparison and alternative pages you already host. The founder's or team's own audience: a personal newsletter or a following built before the product existed.
- **Earned.** Developer and founder communities you do not control. Partner and affiliate co-promotion, where another company's audience chooses to carry your message. Changelog and release-notes syndication to third-party aggregators that pick up updates on their own schedule, not yours.
- **Paid.** Paid social and paid search, bought directly against the exact intent keywords or audiences your buyer uses.
- **On-platform.** A launch-day listing on a software directory, where the audience is pre-qualified but the format, queue and rules belong to the directory, not to you.

A realistic list for a mid-size launch uses most or all of these nine before scoring starts. If your list is three items long and all three are paid, you have not searched owned and earned properly, and scoring will not fix that, since it only ranks the candidates you wrote down.

## Step 2: score each candidate with the Channel Priority Score

Score every candidate on four axes, each 1 to 5, before you spend anything against it. The bands hold at any SaaS scale, since the top band on each axis is open-ended.

**Reach**, the number of qualified prospects this channel can realistically put the launch in front of inside the promotion window, not the channel's total size. A list of 40,000 with a 3 percent open rate reaches roughly 1,200 people, not 40,000.

| Score | Realistic reach in the promotion window |
| --- | --- |
| 1 | Under 200 |
| 2 | 200 to 2,000 |
| 3 | 2,000 to 10,000 |
| 4 | 10,000 to 50,000 |
| 5 | Over 50,000 |

**Fit**, how closely the channel's existing audience intent matches this product. Score 1 where the audience solves an unrelated problem, whatever its size. Score 3 where the audience is adjacent, some overlap but a stretch for most of them. Score 5 where the audience already searches for, discusses or buys this exact category of software.

**Proof**, how much prior evidence exists that this exact channel converts for this type of launch. Score 1 if the channel has never carried a comparable launch. Score 3 for one prior attempt with an unclear or mixed result. Score 5 for three or more prior launches through this channel that each closed at a positive margin.

**Launch Cost**, the time and cash needed before the first result lands. This axis runs the opposite way to the other three: a low number is good.

| Score | Lead time and cash before the first result |
| --- | --- |
| 1 | Live the same day, near-zero cash |
| 2 | Live within a week, under $500 cash |
| 3 | One to two weeks lead time, $500 to $2,000 cash |
| 4 | Two to four weeks lead time, or over $2,000 cash |
| 5 | Over a month lead time and over $2,000 cash |

**Channel Priority Score = (Reach x Fit x Proof) divided by Launch Cost.**

The shape borrows the expected-value logic of RICE, Reach, Impact, Confidence, Effort, first published by Intercom's product team in 2016 for ranking feature work, adapted here with axes specific to a launch channel rather than a backlog item. The ceiling is 125 (5 x 5 x 5 / 1). The floor worth keeping on the list at all sits around 0.2 (1 x 1 x 1 / 5), a sign to drop the candidate rather than score it precisely.

## Step 3: the decision rule, including the branch where you genuinely cannot tell

- **If the score is 20 or above:** the channel goes into this cycle's plan at full execution, with a dedicated slot in the sequence below and a real, not token, budget line.
- **If the score sits between 6 and 19:** the channel goes in as a capped test. Cap the spend or time at a level you would be comfortable writing off entirely, and build nothing for it that cannot be torn down inside a week.
- **If the score is below 6:** do not build anything for this channel this cycle. Park it, and revisit only when Fit or Proof changes, for example a testimonial you did not have before, or a directory opening a new placement type.
- **If you cannot honestly estimate one of the four inputs**, because you have never run anything through that channel and there is no comparable benchmark, do not force a guess into the formula. A guessed input produces false precision, worse than no score, because it looks decided when it is not. Instead, run the smallest possible probe sized to answer only the missing input, for example a single small paid test to measure realistic response rate, before scoring the channel for real.

## Step 4: sequence the plan by dependency, not by convention

Order matters because several steps consume an earlier one's output. The item marked first must happen before every other item, without exception, regardless of which channels you chose in Step 2.

1. **Freeze the pricing page and the sign-up flow.** Owner: product marketing or growth lead. Trigger: pricing, trial length or free-tier limits, and the sign-up flow are final, and the trial has been tested end to end by someone who did not build it. This has to happen before every other item, because every channel below sends traffic at this exact page and flow.
2. **Load the onboarding sequence and confirm support is staffed for the spike.** Owner: whoever owns lifecycle email, with the support lead. Trigger: step 1 is frozen. The onboarding sequence must be live before the first outside sign-up, since the highest-intent traffic arrives in the first 48 hours and an empty sequence loses the sign-ups the promotion paid to acquire. Support needs a staffing plan and prepared responses ready before traffic arrives, not once a backlog has formed.
3. **Soft-release to the smallest owned audience and require a committed action, not a free sign-up.** Owner: the product or growth lead. Trigger: steps 1 and 2 are done. The test is whether early users start a card-required trial or convert to paid, not whether they register for a no-commitment trial, since an uncommitted sign-up measures curiosity, not intent.
4. **Kill or proceed.** Owner: whoever holds the budget. Trigger: enough soft-release sign-ups have had time to convert, typically three to seven days. Combine that signal with the Priority Scores from Step 2: if the soft release produced no committed conversions, stop here regardless of scores, since a score describes reach and fit, not whether the product is wanted.
5. **Launch on owned channels.** Lifecycle email, in-app announcements, comparison pages, the founder's own audience. Owner: whoever owns the base and lifecycle messaging. Trigger: step 4 passed.
6. **Activate earned channels.** Developer and founder communities, partner and affiliate co-promotion. Owner: partnerships or community. Trigger: the owned-channel launch has produced at least one testimonial or trial-to-paid story, since a partner is being asked to vouch for something an unproven product cannot yet support.
7. **Launch the directory listing and paid channels scored 20 or above.** Owner: launch coordinator or paid media. Trigger: at least one owned-channel data point sets a target trial-to-paid rate and cost benchmark, so spend is judged against real numbers, and the listing's requested assets, screenshots, a demo, a maker account, are ready, since most directories will not schedule a slot without them.
8. **Run the closing push.** Owner: whoever owns lifecycle messaging. Trigger: the launch enters its final 10 to 15 percent of its stated window, for example a founding-price deadline on the last day of a ten-day window. Stated as a proportion, not a fixed day count, so it scales with the launch's length.
9. **Collect the two questions.** Owner: whoever owns customer research or lifecycle. Trigger: within 48 hours of each paid conversion, not sign-up, while the reason is fresh. Ask what moved them from trial to paid, in their own words, and what else they would want a similar tool to solve. Both feed the next launch's copy and channel list rather than sitting unused.
10. **Measure and reallocate.** Owner: whoever owns the marketing budget. Trigger: the attribution window has fully closed, at minimum the trial length plus one billing cycle, so conversions and first-cycle cancellations are both counted rather than cut off mid count.

## Step 5: the one metric that makes unlike channels comparable

Revenue is not comparable across channels, because it hides margin and time: an affiliate channel paying out 30 percent of every sale and a direct email send with no revenue share can produce identical first-month revenue and mean very different things, and a subscription's real value arrives after several billing cycles, not on day one.

Use one figure for every channel, on every launch: **Promotion Yield equals cumulative contribution margin collected from the channel's converted cohort, divided by the fully loaded cost of running that channel.** Fully loaded cost means cash spend plus a reasonable estimate of hours put in, valued at a stated internal rate, not cash spend alone.

Because that margin arrives in monthly instalments, read it at two points, not one.

- **Projected Yield**, taken at the attribution window's close: paying customers attributed to the channel, multiplied by average monthly margin per customer, multiplied by a standard planning horizon, your measured CAC payback period if known, or six months as a default. A forecast, and it should be labelled as one.
- **Confirmed Yield**, taken once real billing data covers that horizon, so actual cancellations are counted rather than assumed away. Check it at the horizon, and again at twice the horizon, since early churn often shows up only after the second or third renewal.

A Yield of 1.0 is breakeven at the point measured. Below 1.0, the channel had not paid back its cost yet. Above 1.0, it had. Confirmed Yield is the number for the budget conversation, because Projected Yield is a forecast, and forecasts are exactly what churn is in the business of breaking.

## Step 6: the graduation rule, test budget to invested budget

A channel moves from a capped test to an invested budget line only when both of the following are true, not one.

1. **Confirmed Yield above 1.0 across at least two independent, non-overlapping launch cycles**, not one strong launch and not a Projected figure. A single good result is as likely to be a favourable early cohort as a repeatable channel, and a Projected reading taken before churn shows itself is exactly what misleads you.
2. **The spend behind those two cycles clears a stated floor**, so the result is not noise from a trivial test. As a working minimum: a cash-metered channel needs at least $500 in spend across both cycles, a sweat-equity channel such as affiliate recruitment needs at least 20 logged hours, before its yield is trusted.

If both clear, raise the budget cap, a defined step such as three times the prior test cap, and give the channel a dedicated owner. If Confirmed Yield is positive but only one cycle is confirmed, extend the test one more cycle. If negative across two confirmed cycles, cut the channel for at least one cycle, and if retried, record what changed, offer, price, audience or creative, so the retry is a real test, not a repeat.

**You cannot tell** if the two cycles used a different offer, price or audience, since the comparison is no longer apples to apples, and that does not count as two cycles. Nor can you tell if a cohort has not reached its measurement horizon: a channel run eight weeks ago cannot yet produce a Confirmed Yield against a six-month horizon, and scoring it as a loss this early mistakes not yet measured for measured and losing. Hold the variables constant, or wait for the horizon, before judging.

## Worked example

A two-person team is launching a new SaaS billing tool for freelance consultants, coming out of a private beta. Candidate channels: lifecycle email to a beta waitlist of 3,200 people, paid search on invoicing-related terms, and a launch-day listing on a software directory.

Waitlist email: Reach 3 (roughly 2,400 realistic opens on an engaged waitlist), Fit 5 (everyone signed up specifically for this tool), Proof 3 (one prior beta-invite email converted, mixed result), Launch Cost 1 (same day, no cash). Score: (3 x 5 x 3) / 1 = 45. Full execution.

Paid search: Reach 3, Fit 4 (high-intent keywords), Proof 1 (never run before for this product), Launch Cost 3 (a week of setup, roughly $800 committed). Score: (3 x 4 x 1) / 3 = 4. Below 6, parked.

Directory listing: Reach 4 (strong visibility on a good launch day), Fit 3 (a broad discovery audience, not billing-specific), Proof 2 (one earlier listing by the same founder had an unclear result), Launch Cost 2 (under a week, mostly time). Score: (4 x 3 x 2) / 2 = 12. Capped test.

Sequence: pricing, sign-up and the 14-day trial are frozen and tested end to end, onboarding and support macros are ready. A soft release to 140 beta users produces 22 card-required trial starts in four days, so the launch proceeds. The waitlist email runs at full execution; two testimonials clear the way for community and partner outreach. The directory listing runs as a capped test, paid search stays parked.

The listing costs $300 in fees plus 15 hours of founder time at $50 an hour, $1,050 fully loaded. It drives 60 trial starts and 9 paid conversions at $22 average monthly margin each. Projected Yield at a six-month default horizon: 9 x $22 x 6 / $1,050 = roughly 1.13, a pass on paper. Six months on, billing data shows 3 of the 9 churned within two months, and actual cumulative margin collected comes to $780. Confirmed Yield: $780 / $1,050 = roughly 0.74.

**Verdict.** Because graduation runs on Confirmed Yield, not Projected, and 0.74 is below breakeven on only one cycle, the directory listing does not graduate. It is cut for one full cycle. If retried, test Fit rather than Reach: the directory brought visibility, but much of that traffic browsed rather than had real invoicing pain, so a more targeted directory is the next test, not a bigger budget on the same one.

## Failure modes

**Parallel launch.** Every channel goes live on day one with no sequencing, so paid spend drives traffic at an untested sign-up flow, and the first real signal about the product arrives after the money is spent.

**Vanity reach scoring.** Reach gets scored off a raw follower count instead of a realistic response rate, inflating the score for large, low-engagement audiences and starving smaller, higher-fit ones of budget they earned honestly.

**One-cycle graduation.** A channel moves to invested budget off a single strong result, and the next cycle regresses hard, because nobody checked whether the result was the channel or a cohort that happened to convert well once.

**Payback mirage.** Projected Yield clears 1.0 at the window's close and the channel is treated as a win, then Confirmed Yield six months later comes in under breakeven once churn is counted, by which point the budget call has already repeated for a full quarter.

**Onboarding gap.** Traffic arrives at a trial with no onboarding sequence live, sign-ups never reach the point where the product proves its value, and a channel that scored well looks like a failure that was really a readiness gap.

**Support spike blindness.** A launch drives more sign-ups than support can handle because nobody staffed for the spike, and response times blow out during the exact week that decides whether early users talk about the product or churn quietly.

**Metric mismatch.** Channels get compared on raw sign-ups or revenue instead of Confirmed Yield, so a channel producing many low-intent trials looks stronger on the page than a smaller one that actually converted and stuck.

## What this skill does not do

- It cannot see your list sizes, trial-to-paid history or churn curves. Every score and yield figure is only as honest as the numbers typed into it.
- It does not judge whether the product is worth launching. That is a validation question, and this skill assumes it has already been answered before Step 1 starts.
- It does not write the pricing page, the onboarding sequence or the ad copy, only that each must exist and hold still before the channel that depends on it goes live.
- It has no view of channel-specific compliance: email consent law, affiliate disclosure rules, or a platform's listing policy. A compliance specialist or in-house counsel is better placed for that part.
- The graduation rule needs real billing data and time before Confirmed Yield means anything. A channel that looked profitable at the projected stage can still reverse once churn is known.
- It does not run the ad platforms, send the emails or manage partner relationships. It produces the plan those actions follow, not the actions.
