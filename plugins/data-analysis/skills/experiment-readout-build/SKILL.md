---
name: experiment-readout-build
description: Builds the readout document for a finished A/B test in a fixed order, running the validity checks before the effect is allowed to be computed. Carries the sample ratio mismatch test and its published failure rate, the peeking numbers, the difference between a mean metric and a ratio metric whose denominator varies per user, the segment and multiple comparison position, and a decision rule with an explicit branch for when the experiment cannot be read at all. Produces one document with the checks, the effect with an interval, the guardrails and the decision against a rule written before the test ran. This skill should be used when an online experiment has finished and somebody needs a readout that decides whether to ship.
---

# Experiment readout build

## What this produces, and why the usual order is backwards

The output is one document with six blocks in a fixed order: the decision rule as it stood before the first user was assigned, the validity checks with their results, the primary metric with an interval, the guardrails, the decision, and a closing statement of what this experiment cannot tell you.

The instinct is to compute the lift and its significance first. That is the last step, and it is worthless if the assignment was broken, because a difference between two groups that were never comparable is not an effect. It is a symptom of whatever made them incomparable.

Assignment breaks more often than people expect. Fabijan, Gupchup, Gupta, Omhover, Vermeer, Dmitriev and Kohavi, "Diagnosing Sample Ratio Mismatch in Online Controlled Experiments" (KDD 2019), reports approximately 6 per cent of experiments at Microsoft exhibiting a sample ratio mismatch, drawn from a study base of over 10,000 experiments across four companies. The same paper observes that a product running ten thousand experiments a year can expect at least one mismatch per day. LinkedIn separately reported about 10 per cent of triggered experiments affected (Chen, Liu and Xu, arXiv:1808.00114, 2018).

So the order is the method. Validity, then effect, then decision. Nothing below is restated from general analytical hygiene: base rates, mix shift and survivorship are threats to observational findings and belong elsewhere, and how the result becomes a chart is somebody else's job.

## Step 1. Recover the rule before you look at any number

Write down four things exactly as they stood before assignment began.

- **The primary metric.** One. If two are named, one of them is a guardrail and you have to decide which before you read either.
- **The effect size that matters practically.** The number below which shipping is not worth the cost, which is a business judgement, not a threshold from statistics.
- **The guardrails and their thresholds.** Metrics you are not trying to move but refuse to damage: latency, refund rate, support contacts, unsubscribe rate, error rate.
- **The stopping rule and the horizon.** Planned duration, planned sample size, and whether interim looks were allowed.

If these exist in a document dated before the start, paste them in and move on. If they do not exist, write them now and label the block **reconstructed after the result was seen**. That label is not decoration. A reconstructed rule cannot justify shipping, because it was written by somebody who already knew the answer. It can only improve the next test.

Then recover what the design could have detected. The rule of 16 gives the per-arm sample size as sixteen times the variance divided by the square of the effect you want to detect, at a two-sided alpha of 0.05 and 80 per cent power (Larsen, Stallrich, Sengupta, Deng, Kohavi and Stevens, *The American Statistician*, 2023, arXiv:2212.11366). Run it backwards on the sample you actually got. If the achieved size per arm is far below what the practically meaningful effect required, a flat result means the test was blind to the thing you cared about, and the readout must say that rather than "no effect".

One prior worth writing into the document. Kohavi, Deng, Frasca, Walker, Xu and Pohlmann, "Online Controlled Experiments at Large Scale" (KDD 2013), reports that only one third of ideas tested at Microsoft improved the metric they were designed to improve. Kohavi and Thomke, writing in *Harvard Business Review* in the September to October 2017 issue, put positive results at Google and Bing at roughly 10 to 20 per cent, and Microsoft overall at roughly one third positive, one third neutral and one third negative. A readout process that produces a win every time is describing itself, not the product.

## Step 2. The validity block, before anything else

Six checks. Each has a detection method and a consequence, and the consequence is the part that gets skipped.

### Sample ratio mismatch

Compare the observed count of assigned units in each arm against the intended split using a Pearson chi-square goodness-of-fit test, with one degree of freedom for two arms. Report the observed counts, the intended ratio, the chi-square value and the p value in the document, always, including when it passes. A check whose result is only reported when it fails is not evidence that it ran.

The threshold is deliberately much stricter than the experiment's own alpha, because at a large sample size a genuine mismatch produces a very small p value and you want almost no false alarms on a check that stops the whole readout. A value of 0.0005 is widely quoted in practice. It does not appear in the KDD 2019 paper it is usually attributed to. The threshold traceable to a primary source is the 0.01 used by the SRM Checker browser extension published by Lukas Vermeer, which was retired in later years once platforms began shipping the check themselves. So the honest instruction is this: pick a value between 0.01 and 0.0005, write it into the pre-registered rule, and state in the readout which one you used. Never pick it after seeing the p value.

**The consequence.** A failed ratio check invalidates the experiment. It is not a caveat, a footnote or a reason to add the word "directionally" to the summary. The readout stops and becomes a diagnosis instead. The same KDD 2019 paper gives a taxonomy of 25 root causes grouped into 5 categories, which is worth reading in full, because the causes are spread across assignment, triggering, telemetry and downstream processing and the natural instinct is to search only the first.

A useful search order when one fires: does the mismatch appear at assignment or only after a filter is applied, does it appear in every day of the test or start on one day, does it appear in every platform and browser or only one, and does the deficit correspond to a population that would have been dropped by a redirect, a crash, a bot filter or a late-arriving log.

### Pre-period comparability

Compute the primary metric for both arms over a window that ends before assignment started, using the same users. There is no treatment yet, so any difference is either chance or a bug in bucketing. A gap that is close to the size of the effect you are about to report is the strongest available evidence that the effect is not real.

### Assignment leakage

Check that no unit appears in both arms, that assignment is stable across sessions and devices, and that the arms cannot influence each other. Leakage is common in three shapes: a logged-out user rebucketed on login, shared resources such as a leaderboard or a marketplace inventory where the treatment changes what the other arm sees, and users who share an account.

### Instrumentation parity

Compare the event vocabulary per arm, not the counts. If the variant emits a click event the control does not, every metric computed from that event is uncomparable regardless of what the ratio check says. This is the check that most often explains a ratio mismatch that appears in analysis but not in assignment.

### Coverage and triggering

Confirm the experiment fired for the population it was meant to fire for. Compare the triggered population against the eligible one, and check the trigger condition is symmetric: if the variant triggers on rendering a new component that the control never renders, the two arms contain different people. Analysis should be restricted to triggered users in both arms, and the readout should state the triggered fraction.

### Duration and whole weeks

Confirm the test covered whole weeks so each arm contains the same number of each weekday, and state the calendar window. Note any holiday, outage, marketing campaign or release that landed inside it. Both arms experience the same shock, so it rarely biases the comparison, but it changes the variance and it changes whether the result generalises.

## Step 3. Peeking, stated honestly

Repeated significance testing inflates the false positive rate, and the magnitude surprises people. Evan Miller, "How Not To Run an A/B Test" (18 April 2010), gives the actual false positive rate under continuous monitoring at a nominal 0.05 as 26.1 per cent. Holding the real rate at 5 per cent requires a reported significance of about 2.9 per cent for a single interim look and about 1.0 per cent for ten. Johari, Pekelis and Walsh (arXiv:1512.04922) show Type I error can easily increase fivefold at 10,000 samples under continuous monitoring.

There are exactly two legitimate answers, and both are decided before the test runs.

1. **Fix the horizon in advance** and do not act on the result until it arrives. Looking is fine. Deciding is not.
2. **Use a method built for continuous monitoring and name it in the readout.** The always-valid alternative in the Johari, Pekelis and Walsh work is the mixture sequential probability ratio test.

A sequential method is not a free lunch. It buys the right to stop at any time by widening the interval at every point, so an experiment that would have been called at the horizon may not be callable early. Reporting a fixed-horizon interval on a test that was stopped early because it looked good is the one thing neither answer permits.

**Branch.** If the test was stopped early, the design was fixed-horizon, and the stated reason was that the numbers looked convincing, the readout records the outcome as a hypothesis and names the next test. It does not report a p value as though the horizon had been reached.

## Step 4. Compute the effect for the metric you actually have

Two kinds of metric, and they need different arithmetic.

A **mean metric** has one value per randomised unit: revenue per user, sessions per user, conversion as a binary per user. The unit of randomisation is the unit of analysis, the observations are independent by construction, and the ordinary variance is correct.

A **ratio metric** has a denominator that varies per unit: orders per session, clicks per pageview, minutes per visit. If you randomised by user and the denominator counts sessions, the sessions inside one user are correlated, and treating them as independent understates the variance. In the simulation in Deng, Knoblich and Lu, "Applying the Delta Method in Metric Analytics" (KDD 2018, arXiv:1803.06336), the naive variance came out at roughly a third of the true value. An interval built on it is far too narrow and the p value is far too small, which manufactures wins from noise.

The test is one question: **is the denominator the randomisation unit?** If yes, ordinary arithmetic. If no, correct the variance with the delta method as described in that paper, or bootstrap at the randomisation unit, and say in the readout which you used.

Three reporting rules follow.

- **Report an interval, never a point estimate with a verdict.** "Up 2.1 per cent" is an assertion. "Up 2.1 per cent, interval plus 0.9 to plus 3.3 per cent" is a result, and it is the form that makes the underpowered case visible.
- **Report absolute and relative.** A 40 per cent lift on a metric at 0.2 per cent is worth knowing about in absolute terms before anybody celebrates.
- **State n per arm next to the number.** Not in an appendix.

## Step 5. Segments and multiple comparisons

The segment breakdown is where most false wins are manufactured, and the mechanism is arithmetic rather than dishonesty. Twenty segment comparisons at a 0.05 threshold produce an expected one apparently significant result per readout when nothing is happening at all. Slicing by platform, country, tenure, plan and traffic source crosses twenty comparisons quickly.

- **Segments named before the test** carry the weight the pre-registered rule gave them, and they are reported whether they are flattering or not.
- **Segments found after the numbers arrived** are adjusted, using Bonferroni for a handful or Benjamini-Hochberg for many, and the adjustment is stated. Adjustment reduces the false positive rate. It does not turn a post hoc finding into a pre-registered one.
- **The honest position, which is the one to write into the document:** a post hoc segment finding is a hypothesis for the next experiment, not a result. It goes in the readout under that label, and it is never the reason to ship something the primary metric did not support.

If the primary is flat and one segment is up, the sentence for the readout is "the experiment did not move the primary metric, and one segment is worth testing next", not "it works for mobile".

## Step 6. Novelty and primacy

Both are defined in the 2023 *American Statistician* review cited earlier. A **novelty effect** is an early inflated response to the change because it is new, which decays as the novelty wears off. A **primacy effect** is the reverse: an early suppressed response because existing users are practised at the old design, which recovers as they adapt.

Detection is a plot, not a test. Compute the daily effect, not just the cumulative one, and look at the shape. A daily effect that starts large and decays towards zero is the novelty shape. One that starts negative or flat and rises is the primacy shape. A cumulative curve hides both, because it averages the early days into every later point.

Kohavi, Deng, Longbotham and Xu, "Seven Rules of Thumb for Web Site Experimenters" (KDD 2014), recommends running for two weeks to detect these effects, and notes that in practice they turn out to be uncommon. Both halves of that matter. A one-week test on a visible interface change cannot distinguish a real effect from a novelty effect at all, and equally, "it is probably just novelty" is not a way to dismiss a result you dislike without the daily plot to support it.

A second view helps: compute the effect for users who first appeared during the test. They have no old design to be practised at, so a gap between new and existing users is the signature.

## The decision rule

Work down this list and stop at the first branch that matches.

1. **Any validity check failed.** The result does not exist. Output a diagnosis of the assignment, not a hedged recommendation. Name the check, the numbers, and the search order for the cause. Do not report the lift anywhere in the document, including as context, because it will be quoted.
2. **Primary metric clears the pre-registered practical effect, the interval excludes zero, no guardrail breached.** Ship.
3. **Primary is flat and the interval still contains the effect you cared about.** The test was underpowered for the question. This is not "no effect", it is "we could not see". Either extend to the sample size the rule of 16 calls for, or record it as inconclusive and say what it would have cost to answer.
4. **Primary is flat and the interval is tight enough to exclude the practical effect.** A genuine null, and a valuable one. Do not ship, and write down that the idea was tested properly and did not work, because that sentence is what stops it being proposed again next quarter.
5. **A guardrail is breached and the primary is flat.** Do not ship. No further analysis required.
6. **Primary clears and a guardrail moves against you.** This is a trade, not an analysis. The readout does not decide it. Put both numbers with both intervals on one page, name the person who owns the trade, and state what each unit of gain costs in guardrail terms.
7. **Primary clears but the effect is smaller than the practical threshold you designed for.** Significant and not worth shipping is a normal outcome. Report the point estimate, the interval and the threshold together, and default to not shipping unless the cost of the change is genuinely zero.
8. **You cannot tell.** If the metric definition changed mid-test, if instrumentation parity failed on the primary metric's own events, or if the trigger was asymmetric, then the numbers are not comparable and no amount of adjustment repairs them. The correct output is the diagnosis plus a specification of the re-run, and an explicit line saying the original question remains unanswered.

## Worked example

An online retailer tests a shortened checkout: three steps collapsed into one. Everything here is invented.

**The rule, dated before the start.** Primary metric: orders per session. Practically meaningful effect: plus 1.0 per cent relative. Guardrails: refund rate, support contacts per order, checkout error rate. Horizon: 14 days, whole weeks, no interim decisions. Randomisation by user at a 50/50 split. Ratio threshold written in as 0.001.

**Validity block.**

- Ratio check: 240,118 users in control against 239,602 in treatment, expected 239,860 each. Chi-square 0.56 on one degree of freedom, p about 0.46. Passes, and is reported.
- Pre-period: orders per session over the fortnight before assignment differs by 0.3 per cent between the arms, well inside noise. Passes.
- Leakage: no user in both arms, assignment stable across devices. Passes.
- Instrumentation parity: the variant emits one new event for the combined step. It is not used in any reported metric, and the error metric is computed from a shared event that exists in both. Passes with a note.
- Coverage: triggered on reaching the first checkout step, symmetric across arms, 31 per cent of sessions triggered. Analysis restricted to triggered users in both arms.
- Duration: 14 days, two whole weeks, one public holiday inside the window affecting both arms.

**Effect.** Orders per session is a ratio metric with a per-user varying denominator, and randomisation was by user, so the naive interval is wrong. The platform reported plus 2.1 per cent with an interval of plus 1.4 to plus 2.8 per cent. Recomputed with the delta method, the point estimate is unchanged at plus 2.1 per cent and the interval widens to plus 0.9 to plus 3.3 per cent.

That widening matters. The lower bound now sits below the plus 1.0 per cent that was declared practically meaningful, so the readout reports both numbers and says plainly that the data are consistent with an effect too small to be worth having.

**Guardrails.** Refund rate flat. Checkout error rate flat. Support contacts per order up 6 per cent, interval plus 1 to plus 11 per cent, which is a breach of the stated threshold.

**Segments.** Nine were sliced after the fact. Mobile shows plus 5.8 per cent unadjusted. After Benjamini-Hochberg across the nine it does not survive, and it is written into the readout as a hypothesis for the next test.

**Novelty.** The daily effect is flat across all fourteen days with no decay, so the novelty shape is absent.

**Verdict.** Branch 6. The primary metric moved and a guardrail moved against it, so this is a trade rather than a result. The readout hands the support director two numbers: roughly 2 per cent more orders per session, with a lower bound below the threshold that was set in advance, against roughly 6 per cent more support contacts per order. It states that the interval is wider than the platform reported because the metric is a ratio, and it names the mobile finding as the next experiment rather than as a reason to ship today.

## Failure modes

**Ratio Blindness.** The readout opens with the lift. The arm sizes appear nowhere, or appear in an appendix without a test attached. From the outside it looks complete, and roughly one experiment in sixteen at the published Microsoft rate is unreadable underneath it.

**Peeked Win.** The test was planned for two weeks and called on day nine. The document reports a fixed-horizon p value and does not mention the stopping decision. The tell is a calendar window shorter than the plan with no explanation of who decided to end it.

**Segment Mining.** The primary is flat, the summary leads with a segment, and no adjustment is stated. Recognisable by the sentence "it works for" followed by a population nobody named before the test.

**Guardrail Omission.** Only the primary metric appears. Nothing was breached because nothing was checked, and the cost lands two months later in a support queue that nobody connects back to the release.

**Novelty Ship.** A visible interface change ran for five days and produced a large effect. Only the cumulative curve was plotted, so the decay is invisible. The effect is gone the following quarter and gets attributed to seasonality.

**Underpowered Confidence.** A flat result is written up as "no effect" when the interval spans everything from a serious loss to a serious win. The tell is a conclusion with no interval next to it, and a sample size nobody compared against the effect they wanted to detect.

**Metric Switch.** The primary metric in the readout is not the primary metric in the plan. It changed after the first one came back flat. Usually visible as a metric with an unusually specific definition that exactly matches the segment where the movement was.

**Interval Absence.** Every number in the document is a point estimate. This removes the single piece of information that would let a reader tell a precise null from an uninformative one, and it makes branches 3 and 4 of the decision rule impossible to evaluate.

**Trigger Drift.** The trigger fires on something only the variant renders, so the two populations differ before any behaviour does. The ratio check on assigned users passes and the ratio check on analysed users fails, which is why both should be reported.

## What this skill does not do

- It does not design the experiment or compute the sample size before it runs. It uses the rule of 16 only in reverse, to say what the finished test could have detected. If you are here before the test, you need a design, not a readout.
- It cannot fix a broken assignment. Detecting a ratio mismatch is the end of what analysis can do, and the repair lives in the bucketing service, the trigger condition or the log pipeline.
- It has nothing to offer on causal inference without randomisation. Difference in differences, synthetic controls, instrumental variables and propensity matching are a separate discipline with separate assumptions, and none of them appear here.
- It does not cover Bayesian methods beyond naming that they exist. A posterior probability of improvement is a different quantity from a p value, it changes what stopping early costs, and reporting one under the frequentist rules above is not valid.
- It does not build the charts. The readout is text and tables, and turning it into something an audience reads is a separate step.
- A statistician reading your specific design beats this file, particularly on variance reduction, interference between units, and any experiment where the randomisation unit is not a user.
