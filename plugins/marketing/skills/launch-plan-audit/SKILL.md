---
name: launch-plan-audit
description: Audits a launch plan for sequencing defects rather than missing activities. Rebuilds the plan backwards from the date by lead time, finds long-lead items started too late and one-shot moves spent before their preconditions are true, checks that the goal decomposes into per-channel inputs with stated conversion rates, tests for single-channel dependency, runs a structured pre-mortem, and checks for a freeze point and a day-after plan. This skill should be used whenever a launch plan, GTM timeline or release runway exists as a document and a date has been set.
---

# Launch plan audit

## The claim this skill is built on

A launch plan is usually written as a list of things to do, ordered by how important each one feels. It should be written as a dependency graph, ordered by lead time. Launches that underperform mostly fail for one of two reasons, and both are ordering errors rather than effort errors.

**The first: work with a long lead time was started too late.** A review queue, an approval, a translator, a printer, a security questionnaire, a writer's editorial calendar. None of these move faster because you are enthusiastic, and none of them can be compressed by working a weekend.

**The second: moves that only work once were spent too early.** The announcement email to your whole list. The post to the community you actually belong to. The first-look offer to a publication. The founder's personal network ask. Each can be used once per launch. Spent at a moment when the page did not convert, the product was not stable, or the tracking was not live, it is gone, and it does not come back because you fixed the page afterwards.

So the audit does not ask whether the plan contains the right activities. It nearly always does. It asks whether each activity starts at a time set by its own lead time, and whether each one-shot move fires after the things it depends on are true.

## The order to run the audit in

Run these in order. Steps 1 and 2 change what the rest of the audit is even about, so running them late wastes the pass.

1. **Goal arithmetic.** Does the goal decompose into inputs?
2. **Lead-time sort.** Rebuild the plan backwards from the date.
3. **One-shot inventory.** What can only be spent once, and does it fire after its preconditions?
4. **Channel concentration.** What share of the goal rides on the single largest channel?
5. **Pre-mortem.** Assume it failed. Does the plan address any of the three most likely causes?
6. **Freeze point.** Is there one, is it dated, and is "new" defined?
7. **The day after.** Does the plan continue past launch day at all?

## Step 1. Goal arithmetic, not aspiration

A goal that does not decompose is a wish. Ask for one number and one date, then work backwards through every conversion step until you reach a quantity of raw inputs somebody has to actually produce.

Take a goal of 500 paying customers within 90 days of launch, at a price of £20 per month.

- 500 paying customers, at an assumed trial-to-paid rate of 25%, requires **2,000 trials**.
- 2,000 trials, at an assumed visitor-to-trial rate of 3%, requires **around 67,000 visits**.
- 67,000 visits in 90 days is **roughly 740 visits per day, every day, including the eighty-nine days that are not launch day**.

The point of writing it out is not the final number. It is that every assumed rate is now on the page where it can be challenged. If nobody in the room can say where 25% came from, that is the finding. If the site has never seen 740 visits in a day, the plan needs either a channel that produces that or a smaller number.

Then decompose the 67,000 by channel, and sanity-check each line against something already known:

- Search: how many pages ranking, for what, at what current click volume? A site with no rankings does not produce 20,000 search visits in 90 days.
- Email: list size times an assumed click rate. If the list is 800 people, it produces hundreds of visits, not tens of thousands, however good the email is.
- Community and social: reach times an assumed click rate, and reach is a number you can look up rather than hope for.
- Paid: budget divided by cost per click. This one is honest arithmetic and it is usually the line that reveals the goal needs money nobody has allocated.
- Partners and press: this line is nearly always the fictional one. If it carries 30% of the goal and no partner has yet agreed to anything, the goal is not decomposed, it is decorated.

**Write the assumed rates into the plan document.** A rate that lives only in somebody's head cannot be found to be wrong later, which means the post-launch analysis will blame execution.

## Step 2. Rebuild the plan backwards by lead time

Take every item and ask: what determines its start date? For anything with a third party in the loop, the answer is the third party's queue, not your preference.

Items that usually have a queue, with the question to ask about each:

- **App store or marketplace review.** What is the current published review time, and what is your own worst case from last time? Submit with a buffer that assumes at least one rejection and one resubmission.
- **Legal, security or compliance review.** Take the stated internal turnaround and treat it as a floor. Whatever the process is, it has a person in it who has other work.
- **Customer case studies and quotes.** The long pole is the customer's own legal or communications approval, and it is out of your control entirely.
- **Anything printed or physical.** Production plus shipping plus a reprint if the first proof is wrong.
- **Translation and localisation.** Including the second pass after the copy changes, which it will.
- **A new sending domain or a large increase in send volume.** Sending reputation is built over weeks of increasing volume, not switched on. A cold domain used for a large launch send is a launch send in the spam folder.
- **Directory, marketplace and integration listings** with human review.
- **Writers and editors.** Publications with any planning at all are working weeks ahead. A pitch in launch week is a news pitch and needs actual news to survive.

The rule: **schedule by lead time, not by desire.** If an item takes six weeks and you want it live at T-2, it starts at T-8, and if T-8 has already passed, that item is not in this launch. Say so now rather than discovering it in week ten.

## Step 3. The twelve-week runway

A twelve-week shape, with what each item depends on and what it blocks. Adjust the absolute weeks to your runway, but do not reorder them.

### T-12 to T-10: measurement, positioning and arithmetic

- **Analytics and event tracking installed, then verified with a real test conversion end to end.** Depends on: nothing. Blocks: every paid campaign, every landing page test, and every claim you will later make about which channel worked. This is first for one reason: a campaign that ran before tracking existed produces a number nobody can act on, and you cannot retrofit it. You will be left arguing about attribution for a quarter over a week of spend.
- **The goal decomposed, from step 1.** Depends on: nothing. Blocks: budget allocation and channel selection, because you cannot choose channels before you know how much each must carry.
- **Positioning and the one-sentence claim settled.** Depends on: nothing. Blocks: every asset, every pitch and every landing page, all of which have to say the same thing.
- **The list, started now.** Depends on: a page that can collect an address. Blocks: the launch email. Build the list before there is anything to announce to it, because a list assembled in launch week is a list of strangers with no relationship and poor deliverability behind it.

### T-9 to T-7: long-lead submissions and the first contact

- **Everything with a queue enters its queue.** Depends on: a build or an asset that is good enough to submit. Blocks: launch day itself for anything user-facing.
- **The relationship list is built: name twenty to thirty writers, community moderators, and potential partners who plausibly care.** Depends on: positioning. Blocks: all outreach.
- **First contact, with no ask in it.** Depends on: the list. Blocks: the ask at T-2. This is the single most commonly inverted item in the whole plan. Outreach that begins in launch week is a cold ask from a stranger. Outreach that began seven weeks earlier is a note between two people who have spoken before. The mechanism is not politeness, it is that the second one gets read.
- **Content that needs to accumulate starts publishing.** Depends on: positioning. Blocks: any search traffic in the goal decomposition, because indexing and ranking are not launch-week activities.

### T-6 to T-4: assets, and the honest version of the stability problem

- **Second contact with the relationship list: send something useful with no ask.** Depends on: first contact. Blocks: the ask.
- **Demo video, screenshots, and documentation.** Depends on: a product stable enough that the recording will not be obsolete. This is the dependency that gets fudged.

**The branch for when the product is not stable at T-4:** do not record the full demo. Record only the parts that are frozen, and script the rest so it can be captured in a single afternoon later. Then set a hard decision date at T-2: if the unstable parts are still moving at T-2, the launch asset ships showing only what is frozen, and the plan drops any claim that depends on the rest. Re-recording a full demo in the final week is the classic way to lose the final week.

- **Landing page built and, if you have any traffic at all, tested.** Depends on: measurement, positioning. Blocks: every one-shot move.
- **Pricing and checkout tested with a real transaction, including a refund.** Depends on: nothing but the build. Blocks: everything, quietly.

### T-3 to T-1: cashing in, and the freeze

- **The ask.** Now the outreach converts into commitments: a review, a mention, a co-marketing slot, a moderator's blessing to post. Depends on: two prior contacts. Blocks: launch-day coverage.
- **Embargoed briefings, if any.** Depends on: the ask, and a product that will actually be live.
- **Warm-up sends to the list.** Two or three emails before the announcement, so the announcement is not the first thing they have heard from you in three months. Depends on: the list existing since T-10.
- **The freeze point, at T-1.** After this date nothing new enters the launch: no new feature, no new page, no new channel, no new asset. Define "new" explicitly, because everyone will argue: new means anything not already written down in the plan on the freeze date. Fixes to things already in the plan are allowed. Additions are not. Without a stated freeze, the final week is spent on the thing somebody thought of on Tuesday, and the things that were planned go out unrehearsed.

### Launch week

- **Order the one-shot moves so the highest-value one fires when the page is known to work.** If the plan sends the whole-list email at 9am on day one and the checkout breaks at 9:20, that move is spent. Where you can, fire a smaller move first, watch the funnel for an hour, then fire the big one.
- **Somebody is on support, not on the launch.** A launch-day support queue that goes unanswered converts an interested visitor into a cancelled trial.

### T+1 to T+4: the part almost every plan omits

Most plans end at launch day. This is where most of the remaining value is, because a launch produces a one-off spike that decays within days, and the whole question is what is left after it.

- **The follow-up sequence to everyone who arrived and did not convert.** They are the largest and most qualified group you will have all quarter, and after two weeks they are strangers again.
- **The retargeting audience that only now exists.** Before launch you had nobody to retarget. Now you do. If this was not set up before launch, the audience is not being collected during the spike, which is when it is cheapest to build.
- **The second wave.** Everyone who said "send me something when it is live" gets that, in the week after, and this converts better than launch-day noise because it is not competing with launch-day noise.
- **A retrospective against the arithmetic from step 1**, comparing the assumed conversion rates to the observed ones. This is the only moment those assumptions can be corrected while anyone still remembers them.
- **The baseline comparison, which is the real result.** Not the launch-day number. Compare the week before launch to week four after launch. If they are the same, the launch produced a spike and no channel. That is a normal outcome and it is worth knowing rather than celebrating.

## Step 4. The single-channel rule

For each channel, compute the share of the goal it carries, from the step 1 decomposition.

If one channel carries more than roughly half the target, it is a single point of failure. The arithmetic that makes this a rule rather than a feeling: if a channel carrying 70% of the goal underdelivers by half, you miss the total by 35%, and no other channel has the headroom to absorb that in the launch window because the others were sized for their own share. If the largest channel carries 30%, the same underdelivery costs 15%, which is recoverable.

The channels most often over-weighted are the ones you least control: a single launch-day aggregator, one publication, one large partner's audience, one platform's algorithm. Ask of each: what happens if this simply does not happen? If the answer is "the launch does not happen", the plan has one channel and a lot of decoration.

## Step 5. The pre-mortem

Do this as a written exercise, not a discussion, because the first person to speak sets the answer for everyone else.

1. State the premise: it is four weeks after launch and the result is 20% of the goal.
2. Everyone writes down, **independently and before any discussion**, the three most likely reasons.
3. Collect them. Cluster. Take the three most frequently named.
4. Go through the plan line by line and identify which line addresses each of the three.
5. For any cause with no line addressing it, either add one or write down explicitly that you are accepting the risk and why.

Step 5 is the whole exercise. A pre-mortem that produces a list of fears and no changes to the plan has cost an hour and bought a feeling.

## Step 6. The decision rule for cutting scope when the date is fixed

Sort every remaining item into three tiers:

- **Tier 1, makes the promise true.** Without it, the sentence you are going to say on launch day is false.
- **Tier 2, makes the promise credible.** The claim survives without it, but a sceptical visitor has no reason to believe you.
- **Tier 3, makes it pleasant.** Polish.

Then apply the rule:

- **The item is tier 3.** Cut it. Do not discuss it further.
- **The item is tier 2.** Cut it, and write down what the launch now cannot prove, so that the post-launch conversion rate is read with that in mind.
- **The item is tier 1.** Do not cut it. **Move the date.** Cutting a tier 1 item ships a launch whose central claim is not true, which costs more than a delay, and costs it in the one place you cannot buy back, which is the first impression of the people who cared most.
- **You cannot tell which tier it is.** Apply the announcement test: write the launch sentence out, with the item removed, and ask whether it is still true. If it is, the item is tier 2 or 3. If you still cannot tell after that, **treat it as tier 1 and move the date**, because the cost of being wrong in that direction is a delay, and the cost of being wrong in the other direction is a false claim in public.

## Worked example, compressed

A plan for a small business invoicing tool, twelve weeks out, goal stated as "a strong launch and good early traction".

**Step 1, goal arithmetic. Fail.** "Strong" is not a number and there is no date beyond launch day. Forced into arithmetic, the team lands on 300 paying customers in 90 days at £15 per month. That decomposes to 1,200 trials at an assumed 25% trial-to-paid, and 40,000 visits at an assumed 3% visitor-to-trial. Neither rate has ever been measured, which is now written into the plan as an assumption rather than left as an alibi.

**Step 2 and 3, lead-time sort. Three failures.**
- Analytics is scheduled for week nine, after a paid test in week seven. The paid test will therefore produce a spend figure and no conversion data. Move analytics to week one. This is the item that blocks the most.
- The marketplace listing is scheduled to be submitted in launch week, and its published review time is longer than the week itself. It cannot be live on launch day. Either submit at T-4 or remove it from the plan and stop counting its traffic in the goal.
- Press and community outreach is a single line item in launch week reading "reach out to writers and communities". This is the inverted item. Move to a named list at T-9, a first contact with no ask at T-8, a useful second contact at T-5, and the ask at T-2.

**Step 3, list. Fail.** The mailing list is scheduled to start collecting in week ten, two weeks before launch, and the launch email is expected to carry 8,000 visits. A list built in two weeks does not produce that. Start collecting at T-10, and re-do the email line of the decomposition with the list size you will realistically have.

**Step 3, assets. Conditional fail.** The demo video is booked for T-2, and the two features it is meant to show are still being rewritten. Apply the branch: record the frozen parts now, script the rest, and set the T-2 decision point.

**Step 4, channel concentration. Fail.** The launch-day aggregator post carries 60% of the projected traffic. One surface, one day, one algorithm, no control. Either build a second channel to size or restate the goal at a level the remaining channels can support.

**Step 5, pre-mortem.** The three causes written independently: nobody knows who it is for, the free tier is too generous to convert, and the launch-day post does not get traction. The plan has a line addressing none of them. Positioning work is added at T-11, a pricing review at T-6, and a second acquisition channel at T-8.

**Step 6, freeze.** No freeze point exists. One is set at T-1 with "new" defined.

**Step 7, the day after.** The plan ends on launch day. Added: a follow-up sequence to non-converters, retargeting collection switched on before launch rather than after, a second wave to the people who asked to be told, and the week-minus-one against week-four baseline comparison as the actual measure of success.

**Verdict: hold the date.** The marketplace review window and the outreach schedule are both immovable, and the plan currently assumes both are free. Either move the launch by three weeks so the queues fit, or drop the marketplace listing and the press line from the goal and restate the number the remaining plan can support. Fixing the analytics ordering is the single cheapest change here and should happen today regardless of which option is taken.

## Failure modes

**The wishlist goal.** "A strong launch." It cannot be decomposed, so it cannot be planned against, and it cannot be missed, which is the actual attraction.

**Outreach as a launch-week task.** A single line reading "contact press and communities" scheduled in the final week. It converts near zero, and the failure is invisible because nobody counted what it would have converted at if it had started at T-8.

**Lead time measured forwards from desire.** "We want it live in week ten, so we will start it in week nine." The correct question is what date the item must start given how long it takes, and for anything with a queue, the queue owner sets it.

**Spending a one-shot before its preconditions are true.** The whole-list email on the morning of a day when checkout has not been tested with a real transaction. You do not get to send it again.

**Measurement installed after the first spend.** Producing a launch retrospective that is a debate about attribution rather than a set of numbers.

**The unfrozen final week.** Something new enters the plan on the Tuesday of launch week, and the things that were planned go out unrehearsed while everyone works on the new thing.

**The single-channel launch.** Every projection depends on one surface nobody controls, and there is no line in the plan for what happens if it does not fire.

**The plan that ends on launch day.** The spike arrives, decays over roughly a week, nobody follows up with the largest pool of interested non-buyers you will have all quarter, and the baseline four weeks later is exactly where it was before.

**Optimism in the asset schedule.** Booking the demo recording against a product that is still moving, then losing the final week to a re-record.

## What this skill does not do

- It does not evaluate the product, the positioning or the price. It will tell you that a plan is well ordered. It cannot tell you that the thing being launched is wanted.
- It does not know your real lead times. Review queues, legal turnaround, print and translation vary by organisation and by country, so it will ask, and where you cannot answer it will audit against a stated default and label it as such.
- It does not check the arithmetic against reality. It checks that the arithmetic exists and that the assumed rates are written down. Whether 3% is the right rate for your site is a question for your analytics.
- It does not manage the plan. It is one read of a document. Once the plan is live, a dependency-aware tool that recalculates when a date moves is the correct home for it.
- It cannot see capacity, holidays or competing priorities, which is the most common reason a correctly ordered plan still slips.
- It does not write the assets, the emails or the pitches. It tells you when each has to exist and what it depends on.
