---
name: facebook-ad-set-writer
description: Produces a single, launch-ready Facebook or Instagram ad set from a product, an offer and a funnel stage. It maps the campaign objective to that funnel stage, defines an audience against its size floor, splits a stated budget between cold and warm spend sized against a computed read floor, and writes full ad copy plus a creative brief for exactly three variants pinned to different awareness levels. It also states the numeric kill, scale or hold verdict to apply at the 72-hour mark once results come in. This skill should be used when a paid social brief exists but no ad set does yet, when a landing page's conversion rate needs checking before committing spend, or when a live ad set has just reached its first read point and needs a verdict rather than a guess.
---

# Facebook ad set writer

## The claim this skill is built on

An ad set is an allocation problem before it is a copywriting problem: one objective, one audience with a size floor, one budget that has to clear a read floor before any result means anything, and a fixed number of creative variants sharing that budget. Treat it as a copywriting problem instead, "write me three Facebook ads for this product", and the result is three ads that read fine in isolation and a set that cannot answer the only question it exists to answer: which one, if any, is worth more money.

The obvious approach fails in three checkable ways. It picks an objective by habit rather than funnel stage, so the delivery system optimises for the wrong event, cheap clicks on an audience already warm enough to buy, for instance. It writes as many variants as there are good ideas rather than as many as the budget can separate, so every variant sits below the read floor and none produces a trustworthy number. And it reads the dashboard on day one, before the ad set has accumulated enough volume to say anything, and kills a genuine winner because the first six hours looked flat.

Everything below follows the order the decisions have to be made in, because the objective and the audience are close to one-way doors: changing either mid-flight does not refine the test, it restarts it.

## Part one. The gate, before you write anything

Confirm two things before opening Ads Manager, because an ad set built on top of either failure produces a confident answer to the wrong question.

**The conversion rate gate, which is this file's own rule rather than a platform figure.** Neither Meta nor Google publishes a conversion rate benchmark, and the industry tables in circulation are one agency's client book, so treat 3 per cent as a starting line drawn here and replace it with your own history the moment you have it. For a product under roughly $250, the website has to already convert at 3 per cent or better on branded or organic traffic before a pound of paid budget goes toward acquiring strangers. A stranger from a cold ad never converts as well as someone who typed your name into a search bar, so if that group is under 3 per cent, cold traffic will be lower still. For a higher-price product or lead-generation model, substitute your historic organic-to-qualified-lead rate for the 3 per cent figure and apply the same rule: fix the page before buying it traffic.

**The pixel gate.** Confirm inside Meta's Events Manager, using its test-event tool, that Purchase (or Lead), Add to Cart and View Content all fire correctly and attribute the right value, before the first campaign launches. A broken or partial event does not fail loudly, it optimises confidently against the wrong signal, and every figure below inherits that error silently.

If either check fails, stop. No audience, budget or copy decision below repairs a leaking page or a broken event.

## Part two. Match the objective to the funnel stage, not to habit

As of August 2026, Meta groups campaign objectives into six outcomes: Awareness, Traffic, Engagement, Leads, App Promotion and Sales. Meta revises objective names periodically, so confirm the current list in Ads Manager, but the mapping below has held through every relabelling because it follows the funnel stage rather than the interface.

- **Cold, unaware or problem-aware, no meaningful pixel history.** Do not optimise for Purchase yet. With too few historic conversions, the delivery system has nothing to learn from and either underspends or spends erratically chasing a rare event. Optimise for a cheaper proxy instead, Landing Page View or a video-watch threshold, under Traffic or Engagement. This also builds the retargeting pool needed for part three.
- **Mid-funnel, solution or product aware, some pixel history.** Optimise for Lead or Add to Cart under Leads or Engagement, the point where an account usually has enough weekly events to support a mid-funnel event without starving it.
- **Hot, retargeting a warm audience that has visited, added to cart, or bought before.** Optimise directly for Purchase under Sales. The audience is small, so the account needs enough historic Purchase events, typically from an earlier cold campaign, to learn at low volume.

**The decision rule, borrowed rather than published.** Meta documents that an ad set usually exits its learning phase after about 50 results in the week following its last significant edit. It publishes nothing at all about when an account is ready to optimise for Purchase, so the rule below takes the shape of that number and is this file's own: if the pixel has fewer than roughly 50 Purchase events in the last 30 days account-wide, do not launch a cold ad set optimising for Purchase, whatever the funnel stage calls for eventually. Run proxy-event prospecting first, then switch once. Switching the optimisation event on a live ad set restarts its learning phase, so make this decision before launch, not after the first flat day.

## Part three. Define the audience, and check the size floor first

**Interest-based, for a cold audience with no usable list.** Layer two to four related interests inside one audience rather than one interest per ad set, so the delivery system can find the best-performing slice on its own.

**Lookalike, once there is a seed.** Meta's own source-size minimum is low, low enough that a source sitting on it behaves close to broad targeting, since there is not enough shared signal to model a real pattern. Check the current minimum in Ads Manager rather than quoting a remembered one. The working figures here are this file's, not Meta's: treat 1,000 as the practical floor worth trusting, and 1,000 to 50,000 as the band where quality is highest. Under 1,000, build interest-based prospecting instead rather than shipping a lookalike that performs like a coin flip with a precise-sounding name.

**Retargeting, windowed by intent.** A 180-day site-visitor audience for broad remarketing, a 30-day window for product-page viewers, and a 14-day window for add-to-cart-without-purchase, each its own audience with its own discount depth, deepest closest to checkout. Exclude purchasers from all three unless the campaign is a deliberate cross-sell.

**The you-cannot-tell branch.** If the customer list size, purchaser count or site-visitor count cannot be stated without opening a report never looked at before, none of the audiences above can be built with confidence yet. Pull the real numbers first. Guessing and discovering the audience was too small only after a campaign underperforms wastes the budget the read needed.

**A named restriction.** If the product falls under a Meta Special Ad Category, the four Meta names are housing, employment, financial products and services, and social issues, elections or politics, none of the targeting above is fully available. Credit is no longer a category of its own, having been folded into financial products and services, so any guidance still naming it is out of date. Age, gender and postcode-level location are restricted by policy, detailed targeting exclusions are prohibited, and lookalikes are unavailable for the housing, employment and financial categories entirely. Check this in Ads Manager before building the audience, not after the campaign is rejected.

## Part four. Size the budget against a read floor, not against what feels affordable

Two numbers decide whether an ad set can produce a verdict at all: breakeven cost per acquisition, and the volume of results needed before any number is trustworthy.

**Breakeven CPA.** Price multiplied by gross margin, for a one-off purchase: a $45 product at 62 per cent margin has a breakeven CPA of $27.90. For a subscription, substitute expected lifetime gross profit for price times margin, since a single-purchase breakeven wildly understates what can actually be paid.

**The read floor.** Meta documents that an ad set usually exits its exploratory learning phase after about 50 results in the week after its last significant edit, and that below that pace it reads as learning limited, which Meta describes as budget not being spent effectively rather than as a penalty. Note the unit: a result is whatever the ad set optimises for, not a purchase, unless the ad set optimises for purchases. Meta publishes nothing about a three-day read, so the interim floor below is this file's pro-rata arithmetic on Meta's own weekly figure: at the 72-hour mark, three-sevenths of the way through that window, treat 20 to 25 pooled events as the honest interim floor. Fewer than that and the dashboard is showing noise wearing the shape of a verdict.

**Turning the floor into a daily budget.** Divide the read floor by the days in the test window, then multiply by the CPA expected during the exploratory phase, not breakeven. Meta documents that ad sets in the learning phase are less stable and usually carry a higher CPA, without ever saying how much higher, so the 20 to 30 per cent uplift assumed here is this file's planning allowance rather than a Meta figure. A read floor of 20 events over 3 days at an expected testing CPA of $35 gives a daily budget floor of roughly $233 for the whole ad set. Below that figure, extend the window rather than shrinking the floor: fewer events is still noise, only relabelled as a verdict.

**Splitting cold and warm spend.** Fund retargeting first, but only up to its natural ceiling: audience size multiplied by a target frequency, for which this file uses around three impressions a person a week, a working figure of its own since Meta publishes no frequency target. Spend beyond that ceiling mostly buys repetition, since the audience has run out of new people to show the ad to. Whatever remains goes to cold prospecting. A brand-new account with no warm pool runs prospecting-only, and that spend counts as pool-building, not a result judged against breakeven.

## Part five. Cap variants at three, and structure the set so it can be judged fairly

**Three variants, in one ad set, against one audience.** Meta publishes no variant count. What it does publish is a warning against high ad volumes, because the delivery system learns less about each ad when there are many of them, and the cap of three is this file's own arithmetic on top of that: a fourth variant does not add a fourth data point, it divides the same fixed budget four ways instead of three, dropping every variant further below the read floor. Three is close to the largest number a modest daily budget can still push past that floor; more variants belong in a second round funded by the budget the first round frees up.

**One audience, not one ad set per idea.** All three variants sit inside the same ad set targeting the same audience, so the delivery system compares them against a shared, neutral population rather than each variant also fighting a different audience's quirks. Splitting variants across separate ad sets multiplies the number of learning phases waiting to clear at once.

**Pin each variant to a different awareness level.** Write a cold set to the colder end of the copywriter Eugene Schwartz's five levels of customer awareness, and reserve direct claims and discounts for retargeting audiences who already know who you are, since a discount pitched at strangers has nothing to attach itself to yet. The requirement here is only that the three variants sit at three different levels, so the set is not arguing the same way three times. Which hook belongs at which level, and the bands per funnel stage, are the Meta ads copy generator's job.

**Write inside the space that actually displays.** Meta publishes recommended text lengths per placement, and they differ by more than a factor of three across the set. It files those lengths as recommendations rather than limits, and it does not publish a truncation point anywhere, so the familiar "125 characters and then see more" is not a rule you can write to: 125 is one placement's recommendation, not a cut-off. Write the first line so it carries the proposition on its own and the break stops mattering. The per-placement field inventory, including which placements render a headline or a description at all, belongs to the Meta ads copy generator.

**Match the call-to-action button to the objective.** Learn More for Traffic or Engagement built on a proxy event, Sign Up or Get Quote for Leads, Shop Now for Sales on a direct purchase. A CTA promising an action the objective was not built to deliver adds friction at the exact point the ad was trying to remove it.

## Part six. The creative brief for each variant

**Vertical placements carry their own safe zone.** For 9:16 creative in Stories, Reels and Feed, Meta asks you to leave roughly 14 per cent of the top, 35 per cent of the bottom and 6 per cent of each side free of text, logos and key creative elements. Meta's own interface sits in exactly those bands, and anything placed there is covered or cropped the moment the ad runs, which a desktop preview will not show. The full treatment, including the deeper bottom band Reels ads carrying disclaimers need and the guardrail toggle that draws the overlay for you, is in the Meta ads copy generator.

**Standard placement sizes.** Export at the ratio each placement wants rather than letting one asset be cropped across all three, since an automatic crop routinely cuts a face or a product in half: 1:1 and 4:5 for feed, where 4:5 currently claims more vertical space and is generally the stronger default, and 9:16 for Stories and Reels. Meta's current design recommendations run to 1440 by 1800 pixels for 4:5 and 1440 by 2560 for 9:16, larger than the 1080-wide exports much older guidance still quotes.

**The first three seconds carry the video, before sound.** Most in-feed video plays muted by default, so the opening has to communicate visually with a burned-in caption, not a voiceover. Open on the product, the result or the pain point, never a logo; a viewer who has not stopped scrolling inside three seconds has already left.

**Do not write a claim that cannot be documented.** A specific efficacy figure or before-and-after result dropped into copy without substantiation on file is one of the more common reasons a health, beauty or financial ad gets disapproved after it has started running, quietly ending a test partway through its window and corrupting the read for every variant sharing that budget.

## Part seven. The 72-hour rule, and the verdict

Meta publishes the list of edits that send a live ad set back into learning: any change to targeting, any change to ad creative, any change to the optimisation event, adding a new ad to the ad set, pausing it for seven days or longer, and changing bid strategy. A budget change may or may not count, and Meta gives magnitude rather than a percentage, its own example being that $100 to $101 is unlikely to reset an ad set while $100 to $1,000 may. The 20 per cent step used below is this file's conservative ceiling, not a Meta threshold. Seventy-two hours is this file's window too, three-sevenths of Meta's own seven-day figure rather than anything Meta publishes. That is the argument for waiting: an edit made because day one looked flat adds a second learning phase on top of the first, and the ad set now has to clear the read floor twice.

Apply the verdict only once the ad set, or the individual variant, has cleared the read floor from part four. Then branch:

- **Cleared the floor, cost-per-result at or below breakeven.** Scale. Do not raise the live ad set's budget past the 20 per cent step; launch the winning variant alone into a fresh ad set at the higher budget, so it starts a clean learning phase rather than inheriting the test's uneven early delivery. Pause the variants that did not win.
- **Cleared the floor, cost-per-result more than 30 per cent above breakeven.** Kill that variant. If every variant clears the floor and misses breakeven, check link click-through rate before blaming the creative: under roughly 0.9 per cent after 1,000 impressions on feed points at the creative failing to earn the click. That figure is a rule of thumb of this file's own, since neither platform publishes benchmark tables and the ones in circulation are a single agency's client data. A healthy CTR with conversions still missing points at the landing page or offer instead.
- **You cannot tell yet, because the ad set has not cleared the floor at 72 hours.** This is the branch most verdicts get wrong, since a flat dashboard on day three feels like an answer and is not one. If spend is trending toward breakeven, extend the unedited ad set another 48 to 72 hours. If daily spend has sat below the budget floor the whole time, raise it by the safe increment instead of waiting, since an insufficient rate stays insufficient next week. Do not kill a variant that never reached the floor: that is how a genuine winner is most often discarded.
- **A retargeting audience too small to clear the floor inside any reasonable window.** Extend that ad set's verdict window to its own natural ceiling, and judge it on accumulated return on ad spend rather than forcing a CPA read that will not stabilise for weeks.

## Part eight. Worked example, compressed

A skincare brand sells a $45 serum at 62 per cent gross margin, has a purchaser list of 3,200 people, and branded search traffic converting at 3.4 per cent. Purchase, Add to Cart and View Content are confirmed firing correctly. The gate in part one clears.

**Objective and audience.** Two ad sets. Cold: Sales, optimising for Purchase, since the account carries well over 50 Purchase events in the trailing 30 days, targeting a 1 per cent lookalike from the 3,200-person list. Warm: Sales, a 30-day site-visitor audience excluding purchasers of roughly 9,000 people, plus a 14-day add-to-cart audience of around 640 people at a deeper discount.

**Budget.** Breakeven CPA is $27.90. The read floor is 20 pooled Purchase events in 72 hours, at an expected testing CPA of $35, giving a budget floor of roughly $233 a day. The retargeting frequency ceiling, 9,000 people at three impressions a week, works out to around $40 a day. Total available budget is $300 a day: $40 to retargeting, $260 to cold prospecting, clearing its floor with margin.

**Three variants, cold ad set.** A, unaware: opens on the specific frustration rather than the product, a curiosity headline under 40 characters. B, problem-aware: names the anxiety, teases the category of solution. C, solution-aware: leads with one documented, substantiated claim. A: 9:16 video, frustration shown silently for three seconds, product held back to second four. B: 4:5 static, ingredient close-up, caption inside the safe zone. C: 1:1 static with the claim as a sourced callout.

**Result at 72 hours.** Spend $780, 24 Purchase events pooled, clearing the floor. A: $340 spend, 3 purchases, CPA $113, well over breakeven and trustworthy individually. B: $310 spend, 14 purchases, CPA $22, under breakeven with strong volume. C: $130 spend, 7 purchases, CPA $18.60, under breakeven on paper but too little individual spend to trust alone.

**Verdict.** Kill A. Scale B into a fresh ad set at $310 to $400 a day rather than editing the live one. Hold C: it helped the pooled floor clear, but its own read is not yet trustworthy; run it unedited for another 72 hours if budget allows, or archive the angle for a dedicated test rather than declaring a second winner on 7 events.

## Failure modes

**Objective drift.** A Traffic objective on a warm retargeting audience optimises for cheap clicks rather than purchases. Click-through rate looks strong and return on ad spend is quietly poor, since the number everyone watched was never the number the campaign was buying.

**Purchase optimisation on a sparse account.** A brand-new pixel with a handful of historic conversions is set to optimise for Purchase from day one. Delivery either stalls or scatters against people who resemble almost nothing in particular.

**The four-way split.** A fourth or fifth variant is added because the team liked the idea, dividing the same fixed budget further. Every variant sits below the read floor, all show as learning limited, and the round ends with no verdict on any of them.

**The reset loop.** A dip on day one prompts a headline swap; a dip on day two prompts a widened audience. Each edit restarts a shortened learning phase, and the ad set never accumulates the volume a verdict requires.

**A lookalike built from a starved seed.** A list of 140 past customers, technically above Meta's platform minimum, produces a lookalike that behaves close to broad targeting while carrying a name that suggests precision, a data failure mistaken for a targeting one.

**Cold verdict applied to hot money.** A 900-person add-to-cart audience is held to the same 72-hour, 20-event floor as a six-figure cold audience, cannot mathematically clear it, and gets killed for underperforming, costing the account its highest-return segment to an arithmetic mismatch.

**Most-aware copy shown to strangers.** A discount-led variant is written for cold prospecting because it is the highest-converting angle in the retargeting set. Relevance and click-through both suffer, since the offer has no context for someone who has never heard of the product.

**The boosted post.** A post is boosted from the page instead of built in Ads Manager, removing objective selection, audience controls, the three-variant structure and most of the reporting this method depends on.

## What this skill does not do

- It does not build, verify or debug the Meta Pixel or Conversions API implementation. Every threshold here assumes the events already fire correctly; if they do not, the method computes a confident answer to the wrong question.
- It does not fix a landing page or an offer that fails the gate in part one. A conversion rate under the stated threshold is different work, and no audience, budget or copy decision here repairs it.
- It does not know Meta's current interface labels, character limits or minimum spend defaults on the day you read this. Meta revises Ads Manager periodically; check every figure against the live interface before launch.
- It does not produce a finished video asset, only a shot list and a hook direction. A designer or editor still has to shoot and cut it.
- It cannot see an account's own delivery history. Once ninety days of real CPM and CPA data exist, replace the category-level assumptions in part four with those numbers.
- It does not determine whether a product falls under a Meta Special Ad Category or evaluate the current policy position on a restricted category. That sits with Ads Manager and Meta's published policy.
