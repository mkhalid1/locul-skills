---
name: data-story-build
description: Turns a dataset and a set of findings into an ordered chart sequence, one chart per claim, choosing each encoding from the type of claim it has to support and fixing the scale, the sorting, the binning and the annotation as construction rules. Carries a five-way mapping from claim type to encoding, the baseline rule that follows from the mark, thresholds for trend lines, overplotting, category counts and pie slices, the partial final period rule, and an ordering for the sequence itself. Produces a written specification per chart and never produces slides or images. This skill should be used when a dataset or an analysis has to become evidence somebody else will read, and no charts have been drawn yet.
---

# Data story build

## The claim this skill is built on

This produces a chart sequence specification: an ordered list, one entry per chart, each entry naming the claim, the encoding, the fields mapped to each channel, the scale decisions, the annotation, and what is deliberately not shown. It is text. It does not draw the charts and it does not produce slides.

The obvious approach is to look at the dataset and plot what is in it, picking chart types that fit the shape of each field. It produces one chart per column, which is an inventory. An inventory has no order, so the reader assembles their own argument from it, and about half of them assemble a different one from yours.

The second obvious approach is worse and more common: make the charts first, then write the titles. A title written over a finished chart is a caption, and captions drift towards claiming whatever the author hoped, because the chart is already made and nobody wants to have made it for nothing. That is how a title asserting that one thing caused another ends up sitting over two lines that merely move together.

The order that works is claim first, encoding second, because the same two columns support at least three different claims and each one wants a different mark. Monthly revenue by region can support "the north is our largest region", which is a comparison, "the north is two-fifths of revenue", which is a composition, and "the north has been shrinking since spring", which is a change over time. One dataset, three claims, three encodings, and no amount of staring at the columns tells you which one you meant.

**What this file deliberately does not carry.** The perceptual ranking, the catalogue of manipulations, and the per-chart verdicts belong to the [chart honesty audit](/skills/chart-honesty-audit/), which is the reviewing side of this material. That ranking is used here as a tie breaker and is not restated: when two encodings both fit a claim, take the one the ranking puts higher. Where a rule below looks like the audit's, the difference is direction. The audit asks whether a drawn chart still tells the truth. This asks what to draw so the question does not come up.

## Step 1. Inventory the dataset before writing any claim

Do this first, and do it as a list rather than in your head. For every field you intend to use, record:

- **Type.** Categorical, ordinal, temporal or quantitative. Ordinal matters because it removes your freedom to sort.
- **Cardinality.** How many distinct values. This decides whether a category can be an axis at all.
- **Unit and denominator.** Currency, count, minutes, percentage of what. A rate with no stated denominator cannot be charted honestly, and a field named rate is a percentage in one system and a count per day in another.
- **Coverage.** First period, last period, and any gap. Note whether the last period is complete.
- **Definition changes.** Any point at which the metric's definition, the instrumentation or the filter changed. This is the defect no chart can show and no reviewer can catch.
- **Extraction facts.** When the extract was taken and which filters were applied.

Two rules come out of this step. A field whose unit you cannot state does not go on an axis. And a series that crosses a definition change is either split into two series with the change annotated, or it is not charted.

## Step 2. Write the claims as sentences with numbers in them

A claim is a sentence somebody could be wrong about. Write each one out in full, with the number in it, before any chart is chosen.

The test is that you can name the single query that would falsify it. If you cannot write that query, the sentence is a feeling and it does not get a chart.

Then verify each claim against the data, still before choosing any encoding. The ordering matters more than it looks. An encoding chosen for a claim that turns out to be false is wasted work, but that is the small cost. The large cost is that a half-built chart creates pressure to soften the claim rather than drop it, and softened claims are how a sequence ends up asserting nothing while occupying nine pages.

## Step 3. Classify the claim

Sort each surviving sentence by the words in it. This is mechanical and it is meant to be.

- Contains more than, twice, largest, highest, ahead of, ranks: **comparison**.
- Contains share, of the total, made up of, accounts for, split between: **composition**.
- Contains typical, most, range, spread, outlier, varies, long tail: **distribution**.
- Contains as X rises, associated with, relationship, moves with, correlates: **correlation**.
- Contains since, grew, fell, trend, a date range, or two dates: **change over time**.

**The decision rule, including the branch you will actually hit.**

- Exactly one signal present. Take that type and go to the mapping.
- More than one signal present. The sentence is two claims, and it becomes two charts. "Enterprise is now two-fifths of revenue and has grown every quarter since spring" is a composition claim and a change claim wearing one coat. Split it, then classify each half. Combining them into one chart is possible and it is the single most reliable way to produce something nobody can read.
- No signal present. You cannot tell what kind of claim it is, which means it is not yet a claim. Do not choose an encoding. Rewrite the sentence with a number in it. If no number can be put in it, the dataset is not the evidence for it, and the honest output is a sentence in the body of the document with no chart at all.

## The mapping

### Comparison

Claim shape: "X is n times, or n units, larger than Y."

Take horizontal bars, sorted by value. Specifics:

- **Sort by value, descending, unless the category has an intrinsic order** such as time, size bands or a Likert scale, in which case that order wins. Alphabetical is a default, not a decision, and it costs the reader several seconds per chart to find the largest bar.
- **Horizontal when labels run past about twelve characters.** Rotated labels cost more than the vertical space you save.
- **Cap the categories at about twelve.** Past that, readers scan rather than compare. Aggregate the tail into an explicit other, and label it with the count it contains, or split into small multiples.
- **Zero baseline, always.** A bar's length is the quantity.
- **When every value sits within a few per cent of the others**, a zero-based bar chart is mostly empty space and the differences vanish. Switch the mark to a dot plot, which encodes by position, and the range may then be cut. This is not a loophole, it is the mark rule doing its job.
- Not a pie, not a doughnut, not a radar chart. Not a line, because a line asserts continuity between categories that have none.

### Composition

Claim shape: "X is made up of these parts" or "the mix has shifted".

Those are two different claims and they take different charts. State the whole, in the subtitle, as a number.

- **Mix now.** A single stacked bar, or a sorted bar of the parts with the total stated. Prefer the sorted bar when the claim is about a specific part, because in a stacked chart only the bottom series sits on a common baseline, so only the bottom series can be compared across categories. If the claim is about one series, that series goes at the bottom. If the claim is about the smallest slice, a stacked chart cannot show it at all.
- **Mix over time.** A hundred per cent stacked area or column, or better, small multiples showing each part's share as its own line. Small multiples cost more space and remove the reading problem entirely.
- **Pie charts.** Acceptable at two or three slices where the claim is coarse, roughly half or about a third, and only with the values printed. Past three slices a sorted bar is better at every task the reader has.
- **The parts must sum to the whole.** If they overlap, or if a row can be in two categories, this is not a composition claim. Reclassify it as a comparison.

### Distribution

Claim shape: "most X are around Y", "there is a long tail", "these two groups overlap".

- **One group: a histogram.** The bin width is a decision, so write it down. Then stress test it: halve it and double it. If the shape of the story changes qualitatively, you do not have a distribution claim, you have noise, and the correct output is to say so.
- **Two or more groups:** overlapping density curves, or a strip or beeswarm plot. Box plots only for an audience that reads box plots, and never as the only view when the group is small.
- **Below about twenty points per group, show the points.** Any summary of twenty numbers hides more than it conveys.
- **Never a bar chart of group averages when the claim involves spread.** Two groups whose averages differ by a fifth can overlap almost entirely, and the bar chart of averages is how that fact gets lost.
- **n per group is mandatory annotation**, not optional. A distribution claim without n cannot be checked by anybody.

### Correlation

Claim shape: "X moves with Y".

- **A scatter.** Put the variable you could intervene on along the horizontal axis and the outcome on the vertical. State both units.
- **Both axes may be cut to the data range**, because both encode position. Label them properly and the reader is not misled.
- **No fitted line below about fifteen points.** Label the points instead. A smooth through eight points is decoration that reads as evidence.
- **Above roughly five hundred points, reduce opacity to about a third or bin into hexagons.** A solid mass of ink communicates the word many and nothing else.
- **If a line is drawn, name the method and the fit** in the subtitle.
- **Never two vertical axes to imply a relationship.** The construction that replaces it: two stacked panels sharing one horizontal axis, or index both series to 100 at a stated base period. Two axes have no non-arbitrary alignment, which is precisely why they can be made to show anything.
- **Wording constraint.** If the claim sentence contains drove, caused or led to, either the design supports a causal claim, in which case say how, or the sentence gets weakened to moves with before the chart is specified. Weakening the sentence afterwards never happens.

### Change over time

Claim shape: "X grew", "the trend reversed in March".

- **A line, time along the horizontal axis, evenly spaced.** Bars for time only when the periods are discrete, few, about twelve or fewer, and the claim is about individual periods rather than the trajectory.
- **The baseline is not required**, because a line encodes by position and slope. What is required is a range chosen for a reason. Set it so the plotted series occupies roughly the middle two thirds to three quarters of the panel height. Tighter than that manufactures drama, looser flattens a real move into a straight line, and both are decisions you should be able to defend out loud.
- **The final period must be complete.** If the extract was taken mid-period, either drop the last point or mark it and label it as partial with the number of days it covers. An incomplete final month produces a downturn in a chart of any growing metric, every time, automatically.
- **Gaps are breaks.** Never connect across a period with no data. Break the line and annotate the gap.
- **Indexing.** To compare series of different magnitudes, index each to 100 at a base period, name the base period in the axis title, and pick that base for a reason you can state. If you cannot justify the base date, do not index, because the choice of base is the whole story.
- **Seasonality.** A growth claim covering fewer than two full cycles of a known seasonal pattern is not supportable. With nine months of a seasonal business, the honest claim is about the level, not the trend.

## Ordering the sequence

The sequence follows the argument, not the dataset. The default order, which is a starting point rather than a law: the change that creates the problem, the composition that localises it, the comparison that sizes it, the distribution or correlation that supports the mechanism, and any scenario last.

Three hard rules on top:

1. **One chart per claim, and no chart without a claim.** Anything left over goes into an appendix pack, in full, where it is useful when somebody asks.
2. **Keep encodings stable across the sequence.** If a tier is a particular colour in the second chart it is that colour in the fifth, and the same category order is used throughout. Readers carry the mapping from chart to chart whether you intended them to or not.
3. **Keep the time window constant across the sequence**, or say plainly that it changed and why. Two charts of the same metric over different windows read as a contradiction.

## The annotation block

For every chart in the specification, write four things:

- **Title: the claim, containing the number.** Not the variable names. If the number in the title is not visible in the chart as a labelled mark or an annotation, the reader has to compute it, and they will not.
- **Subtitle: source, period, n, unit, and any transformation.** Indexed to a base, seasonally adjusted, excludes a segment, and so on.
- **One direct label** on the mark the claim is about. Direct labels remove the legend for the thing that matters most.
- **A not shown line.** One sentence saying what was excluded and why. It usually does not appear on the chart, and it stays in the specification, because it is the thing that makes the sequence checkable by somebody who was not there.

## Worked example

A project management tool wants to argue for investment in onboarding. The dataset is twenty-four monthly rows: signups, activated accounts where activation is three tasks created within seven days, active accounts by tier, support tickets, first response minutes, and churned accounts with reason codes. The extract was taken on the twelfth of the month. Everything here is invented.

**Inventory findings.** The final month is partial, eleven days of thirty. Activation was defined as one task created until month nine, when it changed to three tasks in seven days, so the activation series is split at month nine and annotated.

**Claims, drafted and classified.**

1. "Signups grew by two fifths over two years while activation fell from 52 to 38 per cent." Two signals, so two claims and two charts. Both are change over time.
2. "Nearly two thirds of accounts that churned last year never activated." Composition.
3. "Activation on the free tier is nineteen points below the paid tiers." Comparison.
4. "Support tickets drive churn." Correlation, and it contains a causal verb. Twenty-four monthly points, with tickets and churn both rising through the same two seasonal peaks. Weakened to "months with more tickets per account are also months with more churn", drawn as a scatter with no fitted line, and demoted to the appendix pack because at that strength it does not carry any part of the ask.
5. "Typical time to first task is about three days." Distribution, and the interesting one. The average is 3.4 days, which is a number that describes nobody: the histogram at one day bins shows two clusters, accounts that create a task on the first day and accounts that never create one at all. Halving and doubling the bin width leaves both clusters visible, so the shape is real. The claim is rewritten as "time to first task is not a distribution with a middle, it is two populations", and the bar chart of averages that had already been made is dropped.

**Specified sequence, four charts in the body.**

- Chart 1, change over time. Line, signups by month, split at month nine only for the activation series, partial final month excluded and stated in the subtitle. Title carries the two fifths.
- Chart 2, change over time. Line, activation rate, two segments with the definition change annotated between them, vertical range holding the series in the middle of the panel and labelled from 30 to 60 per cent.
- Chart 3, distribution. Histogram of days to first task, one day bins, bin width stated, n stated, the two clusters directly labelled.
- Chart 4, comparison. Horizontal bars, activation rate by tier, sorted descending, zero baseline, three categories, the nineteen point gap annotated directly on the chart.

**Verdict.** Six drafted claims became four body charts and three appendix exhibits. One compound sentence was split into two charts, one causal claim was weakened and demoted before anything was drawn, and one bar chart of averages was replaced by a histogram that says the opposite. Nothing has been rendered yet, and the person who renders it has a specification rather than a dataset.

## Failure modes

**The inventory sequence.** The chart count equals the field count and the titles name variables. From the outside it looks thorough, and the audience takes away whichever chart was on screen when they were paying attention.

**The caption written last.** The title asserts a cause and the chart shows two series moving together. The tell is that the title contains a verb the encoding cannot support.

**The alphabetical bar chart.** Categories in name order. Symptom: in the room, somebody has to say "the third bar from the left is the one to look at".

**The stacked chart with the story in the middle.** The claim is about a band that has no common baseline, so the presenter points at the screen and asks people to squint. Nobody can compare the middle of a stack across categories, and no amount of colour fixes it.

**The tool's bin width.** Nobody chose it, so nobody can defend it, and two people describe the same histogram differently in the same meeting.

**The partial final period.** Every trend chart in the pack ends with a dip, and the dip is always the current month. Once somebody notices it in one chart they stop trusting all of them, which is the correct response.

**The unlabelled index.** The subtitle says indexed and no base date appears anywhere. The reader cannot tell whether the crossing point is a finding or an artefact of where the lines were pinned to 100.

**The dual axis built to show a relationship.** Someone asks which axis a line belongs to, and the answer takes fifteen seconds and a pointing finger, by which time the argument has been paused.

**The chart that survived because it was hard to make.** It has no claim above it, it stayed in because it took a day, and it is the one that draws the question you cannot answer.

## What this skill does not do

- It does not produce slides, a deck or an image of any kind. The output is a written specification, and a charting tool or a person renders it.
- It does not verify the numbers. It works with the query results it is given, and a sequence that follows every rule here can be built on a metric whose definition moved.
- It does not do statistics: no significance, no effect size, no attempt at causal identification. A chart can obey every constraint in this file and still support a conclusion the data does not.
- It says nothing about colour palettes, contrast, projection legibility or how the chart behaves when printed in grey. Those belong to the accessibility and density files, and to a designer.
- It does not audit charts that already exist. Once the chart is drawn, the questions change from what to draw to whether the drawing still tells the truth, which is a different file.
