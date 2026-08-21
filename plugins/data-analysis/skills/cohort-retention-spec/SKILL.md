---
name: cohort-retention-spec
description: Writes a retention specification and the cohort table it defines. Fixes the cohort key, the cohort grain, the activity event, which of the three retention calculations is in use and why, the right-censoring rule, and the minimum cohort age at which a cell may be plotted. Names the traps that bend a retention curve for reasons unrelated to user behaviour, including partially observed cohorts and a shrinking denominator. This skill should be used when a retention curve, cohort table or churn number is about to be built, or when an existing one is being rebuilt or disputed.
---

# Cohort retention spec

## The claim this skill is built on

The output is two things: a short specification, and the cohort table that specification defines. The spec fixes seven decisions. The table is the triangle those decisions produce. Neither is a critique of somebody else's chart.

Asked for a retention curve, a strong model produces a plausible one. That is the problem. The word retention is underspecified in a way the word revenue is not, and the same events yield materially different curves depending on which of three standard calculations you use. All three are legitimate, all three ship in major analytics tools under their own names, and each answers a different question.

The evidence is published and easy to check. Amplitude's retention chart documents three types: Return On or After, which "tells you how many of your users triggered your return event on a specific day or after they triggered your starting event"; Return On, which gives "the percentage of users that came back to trigger your return event on a specific day"; and Return On (Custom) brackets. Mixpanel's retention report offers On, On or After, and On or Before as separate criteria, with On or After as the default, plus a streak mode. Google Analytics 4's cohort exploration exposes a different cut of the same ambiguity, with Standard cells ("all cohort users who meet the return criteria for that individual period"), Rolling cells ("that period as well as all previous periods") and Cumulative cells ("in any period in the exploration"). All three vendor pages verified 21 August 2026.

So the number does not exist until the calculation is named. The second problem is worse because it is invisible: the most common retention chart in the wild quietly mixes a fourth thing into the trend, which is that recent cohorts have not been alive long enough to have a late-period number at all. So the spec comes before the query.

## What the spec fixes, in order

Seven lines. Each gets a written choice and one sentence of reason. An unanswered line stays in the document marked open, because a visible open decision is safe and an invisible one is next quarter's argument.

1. **Cohort key.** The event whose timestamp assigns a user to a cohort.
2. **Cohort grain.** Day, week or month, and the calendar rule that defines the boundary.
3. **Activity definition.** The event or event set that makes a user count as retained in a period.
4. **Calculation.** Bucketed, unbounded, or range, named explicitly with the reason.
5. **Censoring rule.** The extraction timestamp, and the minimum cohort age at which a cell is allowed to be plotted.
6. **Denominator.** Cohort as originally sized, or net of some named class of removal.
7. **Reporting unit.** What the published number is called, carrying its grain and calculation in the name.

The day boundary and the definition of the activity event are metric-definition problems rather than retention problems, and they are covered properly in the [metric definition spec](/skills/metric-definition-spec/). Settle them there and reference the answer here rather than deciding twice.

## The three calculations, and which curve each draws

**Bucketed retention**, also called N-day retention, counts a user as retained at period N if they were active in exactly that period. Day 7 means day 7, not day 6 and not day 8. Amplitude's Return On, Mixpanel's On.

**Unbounded retention**, sometimes called classic retention, counts a user as retained at period N if they were active in period N or any period after it. Amplitude's Return On or After, Mixpanel's On or After, which is Mixpanel's default.

**Range retention**, also called rolling or bracket retention, counts a user as retained if they were active at any point in a stated window: days 7 to 13, or days 28 to 34. Amplitude's custom brackets, and what most weekly and monthly cohort tables are actually doing even when labelled daily.

**The ordering is not a matter of opinion.** For the same events, cohort and period N, bucketed is less than or equal to range over a window starting at N, which is less than or equal to unbounded at N. Every user counted by the narrower definition is counted by the wider one, because the wider one's window contains it. So three curves drawn from one dataset sit in a fixed order, and the gap between the top and bottom widens as the product's natural usage interval gets longer relative to the bucket.

**Two properties worth knowing before you choose.**

The unbounded curve is non-increasing by construction. The set of users active on day N or later shrinks monotonically as N grows, because it is a nested sequence. An unbounded curve can therefore never slope upward, so if a chart labelled unbounded, or classic, or "on or after" rises anywhere, either the label is wrong or the query is.

The unbounded number for a given cell is never final. Bucketed retention at day 30 is settled once day 30 has passed for that cohort, and re-running the query in six months returns the same figure. Unbounded retention at day 30 counts activity on day 30 or later, so it keeps rising as the observation window lengthens. Any unbounded figure has to be quoted with its extraction date attached or it is not reproducible, and this is the most common reason two people cannot reconcile a number they both computed correctly.

**Which one is for what.** Bucketed answers a habit question: is this product part of the user's day. Unbounded answers a permanence question: have we lost them for good. Range answers a commercial question: were they there at all during the interval we bill for. An episodic product measured with a daily bucket gives a curve that falls off a cliff for reasons that are not churn, because the user is behaving exactly as intended and is simply not due back yet.

## Cohort construction

**The cohort key moves the curve more than most people expect, and it moves the day-0 cell most of all.**

If the cohort key is the same event as the activity definition, day 0 is 100 per cent by construction and carries no information, because every user is active on the day they first became active. That is fine, as long as nobody reads the flat column as a finding.

If the cohort key is signup and the activity definition is something else, day 0 is not 100 per cent, it is the activation rate, and the curve is now a compound of two things: how many people ever started, and how many of those kept going. A curve built this way can improve because activation got worse, since a higher bar at the front leaves a more committed population behind it.

If the cohort key is a first paid event, the population is self-selected and the curve sits higher than any signup-keyed curve on the same product. That is not a better product, it is a different denominator, and quoting it beside a signup-keyed number from last year is the commonest accidental version of definition drift.

**Grain follows the product's natural usage frequency, not the calendar you like.** The rule: the grain must be at least as long as the typical gap between consecutive active periods for an engaged user. If people use the product about once a fortnight, a weekly grain guarantees roughly half of the engaged population is absent from any given cell, and the curve shows a decline that is really an alternating pattern.

Grain boundaries are less obvious than they look. GA4 documents its own: daily is "from midnight to midnight in the property timezone", weekly is "from Sunday to Saturday included, not on a rolling 7 days", and monthly runs start to end of the calendar month, verified 21 August 2026. A calendar-week grain also makes cohort sizes depend on the weekday a campaign launched on. If size stability matters more than calendar alignment, anchor a rolling window on each user's own key event instead, and say so.

**Alignment.** Where signup and first use are different events, the two clocks disagree. A user who signs up on the 1st and first uses the product on the 9th sits in week 1 by signup and week 2 by first activity, and their day 7 is either a day before they started or the day after. Pick one clock, write it down, and never let a single table mix them. The tell for a mixed table is a day-0 cell that is neither 100 per cent nor the known activation rate.

## Right-censoring, which is the biggest trap

A cohort that is 6 days old cannot have a day-30 number. Not a low one. Not a zero. It has no number, because the period in question has not happened yet.

This is right-censoring, a standard problem with a standard name. The lifelines documentation defines it as individuals who "have not been subject to the death event" and are therefore labelled right-censored, meaning the rest of their history could not be observed, and notes that excluding censored observations underestimates the true average while naively averaging only the observed durations underestimates it too. Kaplan and Meier's 1958 paper in the Journal of the American Statistical Association, "Nonparametric Estimation from Incomplete Observations", exists precisely because you cannot fix this by dropping rows.

A cohort table is a crude non-parametric survival table and inherits the problem exactly. The triangle is ragged: a cohort that is A periods old has cells for periods 0 through A and nothing beyond. Three things then go wrong.

**Averaging down a column.** The day-30 column is averaged across every cohort in the table, including the ones that are 6, 12 and 19 days old. Those contribute a zero or a null, and either way the column mean drops, so the chart shows day 30 getting worse while nothing about the product changed. The narrower the grain and the longer the horizon, the more of the table this affects.

**Plotting a partially observed curve.** The newest cohort's curve is drawn to day 30 with its last several points computed over a window that has not fully elapsed, so it bends down at the right. Any curve that bends down harder at its right-hand end than anywhere else should be suspected of this before it is explained.

**The partial final bucket.** A day-30 cell needs 31 full days to have passed if day 0 counts as a day, and a week-4 cell needs the whole of week 4, not the Tuesday of it. Off-by-one here produces a cell that is consistently and mildly low, much harder to spot than one that is obviously broken.

**The rule.** The spec names an extraction timestamp, in the same timezone as the day boundary. A cell at period N is plotted only when every cohort contributing to it has had all of period N fully elapse as of that timestamp.

**What to do with the incomplete cells.** Two acceptable answers, and one forbidden one.

- **Show them, marked.** Render the cell with a visible flag and never include it in a column aggregate. Amplitude does a version of this in custom bracket analyses, where "results for days with incomplete data show an asterisk", verified 21 August 2026.
- **Exclude them.** Leave the cell blank and let the triangle be visibly ragged. Nobody can then read a partial number by accident, and the ragged shape is itself a truthful picture of what has been observed.
- **Never average them in silently.** A zero says nobody came back. A blank says we do not know yet. Any pipeline that coalesces null to zero before aggregating has converted the second claim into the first, and does so consistently and invisibly.

State which of the first two you chose, and state the minimum cohort age as a number of periods.

## What a flattening curve does and does not mean

A retention curve that flattens has reached a point where the remaining population is leaving slowly. That is the whole of the claim. The asymptote, if there is one, estimates the fraction of the cohort with a durable habit, and it is the number most worth having, because a curve that flattens at 20 per cent and one that decays smoothly to nothing look similar for three weeks and mean completely different things about the business.

Two cautions. Flattening is not proof of an asymptote: a floor read off a 40 day old cohort is the last two points of a noisy series, and it is only credible once several cohorts have independently flattened at a similar level over a horizon several times the natural usage interval. And flattening can be manufactured: a denominator that shrinks as accounts are deleted flattens the curve mechanically, and so does a mixed grain, where later periods are computed over wider windows and catch more activity.

Flattening is also not the same as a rise. A curve that dips and then rises is the resurrection or smile shape, and it is real: dormant users come back, often driven by a seasonal event or a re-engagement campaign. It can only appear under a bucketed or range definition, because those let a user leave a cell and re-enter a later one. So if a smile appears on a chart labelled unbounded the label is wrong, and if it appears on a bucketed chart, look for the calendar event behind it before reading it as an improvement.

**On benchmarks.** Published "good retention" figures do not transfer between products, for mechanical reasons rather than cultural ones. A retention percentage is a function of the calculation, the cohort key, the grain, the activity definition and the natural usage interval, and benchmark tables almost never publish all five. Two products with identical user behaviour can report day-30 figures a factor of ten apart by choosing bucketed against unbounded. Compare your curve against your own earlier curve under an unchanged definition, and against nothing else.

## The denominator

The denominator is the cohort. The only question is whether it is the cohort as originally sized at cohort close, or the cohort net of some class of later removal: deleted accounts, merged duplicates, refunded or reversed signups, accounts that turned out to be fraudulent.

**A shrinking denominator manufactures retention.** If an account is removed from the denominator after it churns, every subsequent cell for that cohort rises, and rises most in the later periods where removals have accumulated. The curve flattens. Nothing improved. This is the most reliable way to produce an encouraging retention chart from a business that is not retaining anybody, and it usually happens by accident, because the denominator is computed by joining to a current-accounts table that no longer holds the deleted rows.

**The rule.** Freeze the denominator at cohort close and record its size as a column in the table. Removals are tracked as a separate footnote row, with a count, rather than by silently subtracting them. One legitimate exception: an erasure request under a data protection regime removes rows from both numerator and denominator whether you like it or not, so log the count of erasures per cohort and a moved denominator stays visible rather than mysterious. And never let the denominator itself be a behavioural metric such as "accounts active last month", because a rate then moves when either half moves and nobody can tell which.

## Decision rule: choosing the calculation

- **The product has a natural daily rhythm**, meaning an engaged user is expected to appear most days. Use bucketed retention at a daily grain. The curve then means what people think retention means: is this a habit.
- **Usage is episodic, or the natural interval is longer than the bucket you were going to use.** Use unbounded retention, and quote it with an extraction date. Bucketed will read as catastrophic for reasons that are not churn, because the user is not due back yet, and a curve that reads as catastrophic for structural reasons gets explained away rather than acted on.
- **The question is commercial survival rather than habit.** Use range retention over the billing interval, aligned to it: a 30 or 31 day window for monthly billing, a 365 day window for annual. That answers "were they there during the period we charged for", which is the question a renewal conversation is actually about.
- **You cannot tell what the natural interval is.** Do not pick a bucket and discover later that it was shorter than the habit. Compute the observed distribution of gaps between consecutive active days first, across users with at least three active days, and let that choose. Take the median gap, and set the grain to at least that. Amplitude ships a version of this idea as its usage interval analysis, which measures "how long users go between triggering your product's most important event, its critical event", considering return events only, verified 21 August 2026. If the gap distribution is bimodal, which happens when a product has both a daily-use and a monthly-use population, do not average them: split the cohorts by segment and produce two triangles, because one curve over two populations is a mix chart wearing a retention label.

## Worked example

A payroll application for small businesses. Everything here is invented.

**The request.** "What is our day-30 retention?" The dashboard says 3 per cent and somebody has scheduled a meeting about it.

**Decisions surfaced.**

1. *Natural interval.* Gaps between consecutive active days, computed across accounts with at least three active days, have a median of 29 days with a wide spread. That is exactly what a payroll product should look like: people run payroll once a month and otherwise stay away. A daily bucket is about one thirtieth of the habit.
2. *Calculation.* The 3 per cent was bucketed retention at day 30 exactly. Under unbounded retention, on identical events, the same cohorts read 61 per cent at day 30. Neither query is wrong. The bucketed one answers a question nobody asked, which is whether people use a payroll tool daily.
3. *Cohort key.* The table was keyed on signup, so day 0 read 74 per cent, which is the activation rate rather than anything about retention, and a change in signup quality three months earlier had moved it. Choice: key on first completed payroll run.
4. *Grain and window.* Choice: monthly grain, with range retention over a 35 day window aligned to the billing interval, which absorbs the fact that a payroll date lands on different weekdays in different months.
5. *Censoring.* The day-30 column averaged across every cohort, including eleven daily cohorts younger than 31 days contributing nulls coalesced to zero. Restricted to cohorts at least 31 full days old at the extraction timestamp, the same bucketed figure is 4 per cent rather than 3, and the newest three weeks of the curve, the part that bent, disappears because those cells are not observable yet.
6. *Denominator.* Nine accounts refunded in their first month had been dropped from the denominator by a join to a current-accounts table, overstating month-3 retention by roughly two points. Choice: freeze at cohort close, carry cohort size as a column, log removals separately.

**Verdict.** Month-1 range retention, keyed on first completed payroll run, over a 35 day window, on cohorts at least one full window old, is 68 per cent. The 3 per cent was computed correctly against a definition that made no sense for the product, then worsened by a column average over cohorts that could not have a day-30 value. The reported unit is now `month1_range_retention_first_run`, the extraction timestamp sits on the table, and incomplete cells render blank rather than zero.

## Failure modes

**Censoring Bend.** Every retention curve on the dashboard falls away sharply at its right-hand end, at roughly the same distance from today regardless of the metric. It looks like a recent regression and it is the calendar. The tell is that the bend moves with you: come back in a week and it has moved a week later.

**Definition Drift.** The number changes because somebody re-implemented the query in a new tool and picked a different default, not because users changed. Mixpanel defaults to On or After and a bucketed implementation elsewhere does not, so a migration alone can move day-30 retention by tens of points. The tell is a step change landing exactly on a tooling migration date.

**Mixed Grain.** Early periods are computed as days and later ones as weeks or months, usually because somebody widened the buckets to make the tail less noisy. Later cells then catch more activity by construction and the curve flattens or turns up. The tell is an x-axis whose units change halfway across.

**Denominator Shrink.** Retention improves in the later periods of every cohort at once, including cohorts from two years ago that cannot have changed. The cause is a join to a live accounts table that no longer contains deleted rows. The tell is history moving when nothing was backfilled.

**Cohort Key Confusion.** Two tables for the same product disagree by a large constant factor and both are internally consistent, because one is keyed on signup and the other on first activity. The tell is a day-0 cell that is 100 per cent on one table and something like 70 on the other.

**Survivor Denominator.** Each period's rate is computed against the previous period's survivors rather than the original cohort, which turns the curve into a per-period continuation rate. Both are legitimate numbers and they are not comparable. The tell is a curve that stays high and flat for suspiciously long, because a chain of continuation rates near 90 per cent looks nothing like their product.

**Bucket Shorter Than The Habit.** The curve collapses within one or two periods and stays near zero, and the team concludes the product has no retention. Users are behaving as designed and are simply not due back inside the bucket. The tell is a near-zero bucketed curve sitting beside a healthy unbounded curve on the same events.

**Average Of A Triangle.** A headline retention number is produced by averaging the whole table, or a whole column, across cohorts of unequal age. It moves every week because the mix of cohort ages in the average changes every week. The tell is a headline number that no individual cell in the table equals.

**The Never-Final Cell.** Two people compute unbounded day-30 retention for the same cohort with the same query and get different answers, because one ran it in March and one in August. The tell is that re-running an old unbounded query always beats the old slide.

## What this skill does not do

- It does not do survival analysis or hazard modelling, and where the question is time-to-event, or which attributes shorten time to churn, that is the better tool. A cohort triangle handles censoring by refusing to plot a cell, which is honest and crude; a Kaplan-Meier estimator uses the censored observations rather than discarding them.
- It cannot tell you why anybody churned. The triangle records that people stopped and never the reason, and no change of definition converts a count into a cause.
- It draws no charts. The output is a specification and a table of numbers, and turning a cohort table into something a reader can take a conclusion from is a separate job with its own encoding decisions.
- On a very low volume product the cohorts are too small to read whatever the definition. A weekly cohort of 40 accounts moves 2.5 percentage points per person, so a two-person difference is a five point swing and any curve through it is noise with a trend line on top.
- It does not settle what active means, what timezone the day is cut in, or what happens to a late-arriving event. Those are metric definition decisions, upstream of everything here, and this file assumes they are written down somewhere.
- It does not check the query. A correctly specified definition implemented with a join that fans out returns a confidently wrong triangle, and the spec cannot see that.
