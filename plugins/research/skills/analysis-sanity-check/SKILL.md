---
name: analysis-sanity-check
description: Checks an analysis, a chart or a reported metric movement against the named ways a confident result reverses: base rate neglect, a mix shift that flips an aggregate, survivorship, self-selection, regression to the mean, a missing denominator, an unlike-for-like window, multiple comparisons, an average that describes nobody, and composition effects. Carries the arithmetic under each name and the specific check that catches it. This skill should be used whenever a number, chart or experiment result is about to be believed, presented, or acted on.
---

# Analysis sanity check

## The claim this skill is built on

Wrong analyses are rarely sloppy. They are usually careful, well presented, internally consistent, and reversed. The arithmetic is right and the conclusion is backwards, because the thing that flipped it happened before the arithmetic started: in how the population was chosen, in what the denominator was, in which window was compared, or in which rows never made it into the table.

That is why "check the maths" finds nothing. The maths is fine. What is needed is a list of the specific reversals, each with its own tell, because each one leaves a visible mark in the description of the analysis even when you cannot see the data.

Ten of them follow. Run them in the order given, because the first four can make the rest irrelevant: if the population is wrong, no amount of correct arithmetic on that population helps.

## 1. Base rate neglect

**The arithmetic, because the general statement never lands.** Take a test that is 99 per cent accurate in both directions, for a condition present in 1 in 10,000 people. Apply it to 1,000,000 people.

- 100 people have the condition. The test catches 99 of them.
- 999,900 do not. At a 1 per cent false positive rate, 9,999 of them test positive.
- Total positives: 10,098. True positives among them: 99.

So a positive result carries a 99 in 10,098 chance of the condition, which is 0.98 per cent. The test is 99 per cent accurate and a positive result is wrong 99 times out of 100. Almost nobody predicts that number before seeing it, including people who know the rule.

**The general form.** When the thing you are looking for is rare, the false positives from the enormous negative population swamp the true positives, however good the classifier is. The controlling quantity is the ratio of the base rate to the false positive rate, not the accuracy.

**Where it appears outside medicine.** Fraud detection: a model with a 1 per cent false positive rate on a population where 0.05 per cent of transactions are fraudulent hands the review team twenty false cases per real one. Lead scoring: "high intent" flags applied to a list where 1 per cent will ever buy. Security alerting: the reason an alert queue gets ignored is arithmetic, not laziness. Screening of any kind, including hiring filters.

**The check.** Ask for the base rate before you ask for the accuracy, and compute the positive predictive value rather than accepting the accuracy figure. If nobody knows the base rate, that is the finding.

## 2. Simpson's paradox: every subgroup moves one way, the total moves the other

**Worked numbers, invented for this example.** Two checkout variants, measured on desktop and mobile.

| Segment | Variant A | Variant B |
|---|---|---|
| Desktop | 300 of 1,000 = 30.0% | 64 of 200 = 32.0% |
| Mobile | 20 of 200 = 10.0% | 120 of 1,000 = 12.0% |
| **Total** | **320 of 1,200 = 26.7%** | **184 of 1,200 = 15.3%** |

B wins on desktop. B wins on mobile. B loses overall by 11 points.

**The mechanism.** Nothing paradoxical happened. The segments convert at very different rates, and the two variants got very different segment mixes: A's traffic was 17 per cent mobile, B's was 83 per cent mobile. The aggregate is a weighted average, and the weights changed. The aggregate is measuring the mix, not the variant.

It runs the other way just as often and is less noticed in that direction: an aggregate that improves while every segment worsens, because the mix shifted towards the better-performing segment. Average revenue per account rises while every plan tier's average revenue per account falls, because more of the accounts are now on the expensive tier.

**The check.** Before believing any aggregate movement, look at the segment mix over the same period. If the mix moved materially, report the segments and treat the aggregate as uninterpretable. In an experiment, an unequal mix between arms means the randomisation is broken or the assignment was not random, and that is a bigger finding than the result.

## 3. Survivorship bias: the missing data is the finding

**The general form.** You are analysing the units that made it to the point where they could be measured, and the units that did not are absent from the table rather than marked as failures. Every conclusion is then a conclusion about surviving, dressed as a conclusion about performing.

**Two modern examples.** First, product analytics on current customers. "Our most engaged accounts all use the reporting module" is compatible with the reporting module causing engagement, and equally compatible with everyone who tried it and hated it having already churned. The churned accounts are the missing rows, and they are the ones carrying the answer. Second, performance tables of any kind that quietly drop dead entries: a list of strategies, funds, or products ranked over five years, where the ones that closed are not in the list. The average of the survivors is not the average of the population, and the gap is systematically in one direction.

A third, common inside companies: studying high performers to learn what predicts performance, while never seeing the equal number of people who did exactly the same things and were not promoted, or were never hired.

**The check.** Name the units that could have been in this dataset and are not. If the answer is "none", say how you know. If the missing units were removed by the very outcome you are studying, the analysis is measuring survival.

## 4. Selection and self-selection

Selection is when someone decided which units enter the sample. Self-selection is when the units decided.

An opt-in survey measures who opts in. That is not a hedge, it is the literal object of measurement: the sample is the set of people for whom answering was worth the effort, which correlates strongly with having a strong opinion, usually a negative one, and with tenure, engagement and role. The same applies to reviews, support tickets, community forum posts, and interviews with people who agreed to be interviewed.

**The check.** For any dataset of people, ask what someone had to do to be in it, and whether that act correlates with the answer. Then compare respondents to the full population on a variable you already hold: tenure, plan, usage band, region. If the respondent profile differs, the estimate is biased in a direction you can usually name.

## 5. Regression to the mean

Any group selected for being extreme contains units that are extreme partly because of noise. Measure them again and the noise resolves, so the group moves back towards the average, with no intervention at all.

The specific way this manufactures an effective intervention: choose the worst-performing 10 per cent of stores, reps, pages or accounts. Apply a programme. Measure again. They improve. So would a randomly chosen half of them, left alone, because they were selected on a metric that contains noise.

It also manufactures the opposite belief about feedback: praise the best week and it gets worse, criticise the worst week and it gets better, so criticism appears to work and praise appears to backfire. Neither did anything.

**The check.** Was the group selected on the same metric it is now being measured on? If so, the analysis needs a comparison group selected by the same rule at the same time and not treated. Absent that, the honest statement is that the effect is not separable from regression.

## 6. The denominator question

A rate is a fraction and most reported changes only show the top half. "Signups doubled" with no traffic figure. "Error rate fell 40 per cent" during a week when volume fell 60 per cent. "Support tickets per user fell" while the user base grew with a population that never files tickets.

Three properties a denominator needs: it must be stated, it must be the population actually at risk of the numerator event, and it must be stable across the comparison, or its movement is the finding.

**The check.** For every rate, write out both numbers and both movements. If the denominator moved more than the rate did, the story is about the denominator.

## 7. Seasonality and the like-for-like window

Three distinct defects live here.

- **Comparing against a peak.** Any period following an unusual high looks like a decline. Any period following an outage looks like a recovery.
- **Comparing a partial period.** Month to date against a full month, or a 19-day window against a 30-day one, presented as a fall.
- **Comparing across a calendar change.** February against January is three fewer days. A 28-day window contains exactly four of each weekday and a 31-day window does not, which matters for anything with a weekday pattern, which is nearly everything business-facing. Public holidays move between quarters. A leap day exists.

**The check.** Compare equal windows containing equal numbers of each weekday, or compare against the same window in the previous year with the weeks aligned rather than the dates. State the window on the chart.

## 8. Multiple comparisons

Run twenty independent tests at the 5 per cent level with no real effect anywhere and the chance of at least one "significant" result is 1 minus 0.95 to the twentieth power, which is about 64 per cent. Something will be significant. It is arithmetic, not bad luck.

**The practical version for anyone running experiments.** The count of comparisons includes every metric you looked at, every segment you sliced, and every time you checked the result before it finished. Peeking daily at a running test and stopping when it crosses the line is a multiple comparisons problem wearing a different coat.

**The check.** Declare one primary metric and one primary population before the test starts. Everything else is exploratory and is labelled as such in the write-up, and an exploratory finding earns a fresh test rather than a decision.

## 9. Averages that describe nobody

A mean summarises a distribution with one number, and that is only honest when the distribution has a middle.

- **Bimodal data.** Two populations in one table, such as trial users and paid users, or internal staff and customers. The mean sits in the valley between the two humps, where nobody is.
- **Heavy tails.** Revenue per customer, session duration, latency, file sizes. A small number of enormous values drag the mean above the great majority of observations. Mean session duration is largely a measurement of forgotten open tabs.
- **Bounded and skewed data**, where the mean drifts towards the long side and the story sits at the ends.

**When the mean is the right statistic:** when you need a total. Mean spend times customer count is total spend, and no other summary gives you that. If a total is not what you want, the mean is probably not what you want.

**The honest default summary** is the middle value plus a spread, and for anything user-facing or latency-like, the 50th, 90th and 99th percentiles, because the 99th is the experience that generates the complaints.

## 10. Composition effects

An aggregate that moves because the population changed, not because any behaviour changed. Average salary rises after a redundancy round that removed the lowest-paid roles. Average tenure rises during a hiring freeze, mechanically, at one year per year. Churn rate falls because you stopped acquiring the segment that churns, while every existing cohort is unchanged.

This is the same arithmetic as item 2, applied to a single series over time rather than to two arms. It is separated here because it is missed in different circumstances: nobody is looking for a paradox, there is only one line on the chart, and it is going the right way.

**The check.** Hold the mix fixed. Recompute the current period using the previous period's segment weights. If most of the movement disappears, the movement was composition.

## The checklist to run before believing any chart

Five questions, in this order:

1. **What is the denominator, and did it move?**
2. **What is the axis?** Truncated y-axis, dual axes with independent scales, a log scale presented as linear growth, an index rebased at a convenient point.
3. **What is excluded?** The date range, the filters, the definition of "active", the rows dropped as outliers, the test accounts.
4. **What changed in the population?** New acquisition source, a launch, a price change, a segment that grew.
5. **What would this look like if the effect were zero?** If you cannot describe the null-result version of the chart, you cannot tell whether you are looking at it.

## The decision rule

- **Denominator moved more than a few per cent between periods** and the finding is a rate. → The finding is about the denominator until shown otherwise. Do not report the rate.
- **Segments exist and the aggregate moved.** → Check the mix first. If the mix moved materially, report segments and suppress the aggregate.
- **The population was selected on the metric being measured.** → Treat any improvement as unexplained until a comparison group selected by the same rule is added.
- **The finding was one of many comparisons.** → Label exploratory, and require a fresh test before it drives a decision.
- **You cannot tell**, because the raw counts are unavailable, the segments were not captured, or nobody knows what query produced the chart. → Report the finding as a question, with the exact query or table that would settle it, and say plainly that it is not yet a result. Do not report a number whose denominator you cannot see, and do not soften it into "directionally" or "broadly". An honest unknown is usable. A hedged number is quoted without the hedge.

## Worked example, compressed

**All figures below are invented for this example.**

A subscription invoicing tool redesigns its help centre. The analysis presented to the leadership team says support tickets per active user fell 30 per cent in the eight weeks after launch, therefore the redesign works, therefore roll it out everywhere.

Running the list:

- **Denominator (item 6).** "Per active user" uses monthly active users, which grew 45 per cent over the same eight weeks because a free tier launched three weeks before the redesign. The numerator, absolute ticket count, rose 4 per cent.
- **Composition (item 10).** Free tier users file tickets at a fraction of the paid rate. Recomputing the current period with the pre-launch mix of free and paid removes almost all of the apparent fall.
- **Segments (item 2).** Broken out by plan, tickets per user rose in every paid tier. Every segment moves one way, the aggregate moves the other.
- **Selection (item 5).** The redesign was rolled out first to the accounts flagged as highest ticket volume, which is a group selected for being extreme on the exact metric now being measured.
- **Window (item 7).** The eight-week pre-period contains a two-day outage that generated a visible spike in tickets, inflating the baseline.

**Verdict: the claim is not supported, and the direction is probably wrong.** The honest statement is that absolute tickets rose 4 per cent while the user base grew 45 per cent, that paid-tier tickets per user rose, and that no statement about the redesign's effect is available because the rollout group was selected on the outcome. The single query that would move this forward: tickets per user by plan tier, for accounts that existed before the free tier launch, comparing equal windows with the outage days excluded from both.

## Failure modes

**Naming the bias instead of doing the arithmetic.** Saying "watch out for base rates" and moving on. The number is the whole point, because the intuition is wrong by two orders of magnitude and saying the name does not fix it.

**Asymmetric scepticism.** Running this list only on results you dislike. A pleasant result gets one read and a shrug, an unpleasant one gets five checks, and the analysis drifts in a direction nobody chose.

**Accepting the denominator because it was in the chart title.** "Per user" is a label, not a definition. Which users, measured when, active by what rule.

**Promoting a post hoc segment finding to a result.** The segment was found by looking. It needs its own test before it means anything, and the write-up needs to say how many segments were examined.

**Reading "no significant difference" as "no difference".** An underpowered test cannot distinguish a real effect from nothing, and reporting it as evidence of no effect is a different error from the one everyone watches for.

**Correcting the analysis and keeping the headline.** The caveat is added to slide 14 and the summary slide still says the redesign worked. The summary is what gets repeated.

**Auditing the chart rather than the query behind it.** Filters, joins and date logic live in the query. A chart cannot show you a row that a join silently duplicated.

**Applying the list only to other people's work.** The analysis most likely to be wrong is the one you built and already believe.

## What this skill does not do

- It does not have your data. Every check here produces a question or a required query, and someone still has to run it.
- It does not do inference. No power calculations, no confidence intervals, no correction for clustered or repeated measures, no handling of missing data mechanisms. Those need a stats library and someone who knows which test applies.
- It does not establish causation. Ruling out ten reversals leaves a correlation that has survived ten checks, which is not the same as an effect.
- It does not cover time series decomposition, forecasting, or measurement error models, and it will not detect a defect that only exists in the instrumentation.
- It will generate work on a correct analysis. Ten named reversals produce ten things to rule out even when the answer to all ten is fine.
