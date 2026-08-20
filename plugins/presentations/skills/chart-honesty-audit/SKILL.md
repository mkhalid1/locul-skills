---
name: chart-honesty-audit
description: Audits every chart in a presentation that already exists and returns one of three verdicts per chart: accurate, misleading by omission, or misleading by construction. Checks baselines against the encoding, dual axes, pie slice counts, area scaling, palette family, denominators, time windows and completeness of the final period, chart junk and 3D distortion, and whether the title states a finding with a checkable source. This skill should be used when a deck carrying charts is about to go to anyone who will make a decision from it.
---

# Chart honesty audit

## The claim this skill is built on

This skill does not generate charts or slides. It runs on a deck that already exists.

Most misleading charts in professional decks are not fraud. They are defaults. Charting software truncates axes automatically to fill the plot area, offers a dual axis as a convenience, offers 3D as a style, and picks a palette without knowing whether your data is categorical or ordered. Somebody clicks accept and the resulting chart overstates the case by a factor nobody intended.

That is why the useful audit is not a search for dishonesty. It is a comparison of two things: what the chart's encoding says, and what the chart is being used to claim. When those differ, the chart is misleading regardless of anybody's intent, and the reader has no way to detect it because they see the encoding rather than the data.

Three verdicts, and the distinction between them decides the cost of the fix:

- **Misleading by construction.** The encoding itself misstates the data. Truncated bar baseline, dual axis, area scaled by radius, 3D perspective. The chart has to be rebuilt.
- **Misleading by omission.** The encoding is sound and the reader cannot check or contextualise it. No denominator, no source, no date, an unexplained window, an incomplete final period, unlabelled units. The fix is usually additive.
- **Accurate.** The encoding matches the claim and a sceptical reader could check it. It may still be improvable.

## The perceptual ranking, which settles most arguments

William Cleveland and Robert McGill published controlled experiments in 1984 on how accurately people judge values from different visual encodings. The resulting ordering, most accurate first:

1. **Position along a common scale.** Dots or bar tops against one shared axis.
2. **Position along identical, non-aligned scales.** Small multiples with the same axis repeated.
3. **Length, direction and angle.** Ranked together in the original paper, not separately: bar lengths that do not share a baseline, the direction of a slope, the angle of a pie slice.
4. **Area.** Bubbles, treemaps, scaled icons.
5. **Volume and curvature.** 3D solids, curved surfaces.
6. **Shading and colour saturation.** Heat maps, choropleth fills, density shading.

Widely reproduced versions of this list split length from angle, drop direction and curvature, and move colour saturation around. Those are later adaptations. The six ranks above are the ones Cleveland and McGill published, and it is worth knowing which one you are quoting when somebody disagrees with you.

The operating rule: **use the strongest encoding available for the quantity the reader is meant to judge, and reserve weaker encodings for context.** That one sentence resolves most chart choice arguments without appeal to taste.

What it decides in practice. A grouped bar chart beats a stacked one when the reader must compare the non-baseline segments, because in a stacked bar only the bottom segment sits on a common scale and everything above it floats. A dot plot beats a bar chart when values are similar and the differences are what matter, because position resolves more finely than length and the bars waste ink. A pie loses to a bar for ranking, because angle is weaker than position. A bubble chart is the wrong choice whenever precise comparison is the point, because area is near the bottom of the list. A heat map is fine for pattern and poor for reading values, so label the cells if the values matter.

## The truncated axis, stated as a rule rather than a ban

The blanket instruction to always start at zero is wrong, and enforcing it produces its own dishonesty: a line chart forced to a zero baseline can flatten a movement that genuinely matters, which misleads by suppression instead of exaggeration.

The real rule follows from the encoding.

- **A bar or column encodes value by length.** The ratio of lengths is read as the ratio of values, so the baseline must be zero. Truncating a bar chart is not a presentation choice, it breaks the encoding. The same applies to anything whose size carries the value: area charts, bubbles, scaled icons, filled shapes.
- **A line or dot plot encodes change by slope and position.** Neither requires the origin, so a non-zero baseline can be legitimate and is often correct. The relevant range is the one where the variation lives.

**The decision rule:**

- Does the mark's size encode the value? **Baseline must be zero.** No exceptions, no visual break glyph pretending to make it acceptable.
- Does the mark encode position or change on a continuum? **A non-zero baseline is allowed**, subject to disclosure.
- **Disclosure** means the axis carries real values at both ends, the range is visible without hunting, and the caption states the range if the movement is small relative to the level. "Note the axis starts at 92%" is a one line fix that converts an omission into an accurate chart.
- **You cannot tell whether the baseline is zero**, because the axis is unlabelled or cropped. Verdict is **misleading by omission**, not accurate. Absence of the evidence is the finding.

One more rule that catches a common accident: never place two charts side by side with different truncations of the same measure. The reader compares them visually, and the comparison is meaningless.

## The dual axis

Two series, two vertical axes, one plot area. It is the single most common construction that a reader cannot check.

The specific dishonesty: each axis has an independently chosen range. Choose them one way and the two lines track each other beautifully. Choose them another way and they diverge. Choose a third way and they cross exactly where you want to point. Nothing in the chart tells the reader which choice was made, and no amount of care in reading it recovers the answer. **A dual axis chart is unfalsifiable by construction**, and every apparent relationship on it is partly a property of the axis ranges rather than of the data.

The honest alternatives, in order of preference:

1. **Index both series to a common base.** Set both to 100 at the first period and plot them on one axis. Now the comparison is about relative change, which is what people were trying to show anyway, and the reader can check it.
2. **Plot the derived measure you actually mean.** If the point is revenue per user, plot revenue per user rather than revenue and users on two axes.
3. **Two panels stacked with a shared horizontal axis.** This is the "position along identical, non-aligned scales" case from the ranking, which sits second on the list and is far stronger than most people expect.
4. **Plot the rate and annotate the counts.** When one series is a count and the other a rate, the rate is usually the finding and the count is context.

The case that looks like a dual axis and is not: two series in the same unit with very different magnitudes. That is a scale problem, not a two-axis problem, and it is solved by indexing or a logarithmic axis with the log clearly labelled.

## Pie charts

The problem is not the pie, it is angle comparison, which sits fourth on the perceptual ranking, and the number of slices a person can hold and compare.

Practical ceiling: **about three to five slices**, and only when the parts sum to a meaningful whole and the reader's question is "what share" rather than "which is bigger".

Where a pie is still the right choice:

- One dominant share against everything else, supporting a single stated number in the headline.
- A part-to-whole where there are few parts and the precise ranking does not matter.
- Where the audience expects the form and the pie is effectively an illustration of a number already stated in text.

Where it never is: time series of any kind, two pies side by side, since angle comparison across two circles is worse than angle comparison within one, any case where two slices are close enough that the ranking matters and cannot be seen, and any case with more slices than can carry external labels. A pie with a legend and eight slices asks the reader to make eight lookups and six angle comparisons, and they will do neither.

## Area, and the squared error

Scaling a shape by its linear dimension when the reader perceives its area is the most reliable way to overstate a difference without noticing.

If radius is set proportional to value, then area grows with the square of the value, so a doubling looks like a quadrupling. **Scale by area: radius proportional to the square root of the value.** Most charting libraries do this correctly and most hand-built graphics do not.

The same error with pictorial icons. An icon drawn three times taller and three times wider to represent three times the value carries nine times the ink. Either scale the icon by area or, better, use three icons.

## Colour

Three palette families exist and using the wrong one is an error rather than a preference.

- **Categorical.** Unordered groups. Distinct hues at roughly similar lightness. Maximum about seven or eight before hues stop being reliably distinguishable, and five is the honest working limit on a projector.
- **Sequential.** Ordered magnitude. One hue, varying lightness. The lightness ordering does the work, so a sequential palette that varies hue without varying lightness is not sequential.
- **Diverging.** Ordered with a meaningful midpoint. Two hues meeting at a neutral centre. **The midpoint must be a real value**, such as zero, the target, or the prior period, not the middle of the observed range. A diverging palette centred on an arbitrary point invents a meaning that is not there.

Using a categorical palette on ordered data destroys the ordering the reader needs. Using a sequential palette on categories implies an ordering that does not exist. Both are misreadings created by the chart.

**Colour blindness.** Red-green colour vision deficiency affects roughly 8% of men and around 0.5% of women of northern European descent. In any audience of reasonable size, somebody cannot separate your red series from your green one. Two consequences:

- **Colour must never be the only encoding.** Add direct labels, distinct positions, distinct shapes, dashed against solid lines, or ordering. If removing colour destroys the chart, the chart is broken for part of the room.
- **Red to green is the most common diverging scale and one of the worst choices available.** It fails for the affected readers, it collapses in greyscale print, and it smuggles in a good-and-bad judgement that the data may not support. Blue to orange, or purple to orange, carries the same directional meaning while remaining separable for nearly everyone.

## Three separate omissions

They get grouped together and they have different effects.

- **The missing zero.** A size encoding without a zero baseline. Effect: differences are exaggerated, sometimes by an order of magnitude. Fix is structural.
- **The missing denominator.** Raw counts where the underlying population changed. More incidents may mean more users. Effect: conclusions reverse, not merely soften, which makes this the most dangerous of the three. Fix: plot the rate per unit of exposure and state the denominator on the chart.
- **The missing time period.** No date range, no "as of" date, no indication of whether the final bucket is complete. Effect: the reader cannot tell a real decline from a partial month. The specific pattern is the **partial final period**, where the last bar is always shorter because the period has not ended, and every such chart appears to show a collapse in progress. Either exclude the incomplete period or mark it explicitly.

## Cherry-picked windows

A chart's time window is a choice, and the choice can carry the entire finding.

The check: **extend the range in both directions and see whether the story survives.** A two year uptrend that begins exactly at the historic low is a window, not a trend. A series that ends exactly at a peak is the same trick reversed.

The rule: the window must be justified by something outside the data. A policy change, a product launch, a system migration, the date the data starts. That reason belongs in the caption. If the only justification available is "this is where the story is", the verdict is misleading by construction, because the window is doing the work the data will not.

## Chart junk and 3D

Chart junk, a term from Edward Tufte's work on statistical graphics, is decoration that carries no data and competes with the data that does.

3D deserves separate treatment because it is not a style choice, it is a distortion of the encoding.

- **A 3D bar has a top face that is a parallelogram**, so the reader must decide which edge to read against the axis, and the two edges give different values.
- **Perspective enlarges what is near the viewer**, so front bars and front pie slices read larger than equal values behind them.
- **A 3D pie is the worst case**: it distorts angle, the encoding it already relies on, and it does so unevenly around the circle.

Related, less obvious: drop shadows extend apparent bar length, gradient fills imply a value change within a single bar, and heavy gridlines compete with the data marks. None of these are stylistic quibbles. Each changes what the reader reads off the chart.

## Labelling and sourcing

- **The title states the finding, not the variable.** "Support volume rose by two fifths while headcount held flat" rather than "Support volume by month". The title is the claim the chart must support, and writing it as a claim is what exposes charts that do not support one.
- **Axis labels carry units and multipliers.** Currency, percentage of what, thousands or millions. An unlabelled axis is an omission finding on its own.
- **Direct labels beat a legend** where they fit, because a legend is a lookup and lookups are the cost the perceptual ranking exists to minimise.
- **Source and date.** Where the data came from, when it was pulled, and any filter that materially narrows the population. Without these the chart is not checkable, and an unchecked chart should not carry a decision.

## The audit procedure

For each chart, in order:

1. **State the claim.** What is this chart being used to say? Take it from the slide headline or the sentence it supports. **If you cannot state the claim, that is the first finding**, and it usually means the chart is there because the data existed.
2. **Check the encoding against the ranking.** Is the strongest available encoding being used for the quantity the reader must judge?
3. **Check the baseline** against the encoding rule.
4. **Check denominator, window, and completeness of the final period.**
5. **Check the palette family** and whether colour is load-bearing on its own.
6. **Check labelling, units, source and date.**
7. **Return a verdict.** Construction defects outrank omission defects, and a chart with both is reported as misleading by construction with the omissions listed under it.

The standing branch: **anything you cannot determine from the chart and its caption is an omission finding, not a pass.** A reader in the room is in exactly your position and has less time.

## Worked example

Three charts from a quarterly review deck. All figures below are invented for this example.

**Chart A. Column chart, revenue by quarter, four columns.** The vertical axis runs from 40 to 60 with no break marker and no caption. The claim in the headline is that revenue grew sharply. The actual growth is from 47 to 52, roughly one tenth, and on that axis the last column stands about 1.7 times the height of the first: a tenth more revenue drawn as most of a doubling. Size encodes value, so the baseline must be zero. **Verdict: misleading by construction.** Fix: rebuild with a zero baseline, and if one tenth of growth looks unimpressive at that scale, that is the honest picture and the headline needs rewriting rather than the axis.

**Chart B. Dual axis line, support tickets on the left, satisfaction score on the right.** The two lines converge over eight months and the headline claims the two moved together. The right axis runs from 3.9 to 4.4 on a five point scale, a range chosen after the fact. **Verdict: misleading by construction.** No reader can check the relationship because it is a property of the two chosen ranges. Fix: index both series to 100 at the first month and plot on one axis, or plot tickets per active account, which is what the claim is really about.

**Chart C. Stacked area, monthly active usage by plan tier, twelve months.** The encoding is defensible: the total is meaningful and the reader's question is composition. But there is no source, no pull date, and the final month is the current one, which is nine days old, so the total appears to fall off a cliff. Tier colours are red, amber and green. **Verdict: misleading by omission, with a palette error.** Fixes: drop or explicitly mark the incomplete month, add source and pull date, replace the traffic light palette with a categorical scheme that survives colour blindness, and label the bands directly instead of using a legend.

**Overall: two charts must be rebuilt and one needs three additions.** The two construction defects both sit under headlines making growth claims, so the headlines change with the charts.

## Failure modes

**The blanket baseline ban.** Forcing every chart to zero, including line charts, and hiding the movement that mattered. The rule is about the encoding, not about charts in general.

**Legend hopping.** A chart with five series and a legend in the corner, so the reader makes five lookups before reading anything. Direct labels remove the cost entirely.

**The wrong palette family.** A rainbow scheme applied to ordered magnitude, or a sequential ramp applied to unordered categories. Both invent or destroy an ordering, and both look deliberate.

**The radius bubble.** Bubbles scaled by radius rather than by area, so every difference is squared. Almost nobody catches this by eye, which is why it needs to be on a list.

**The partial final period.** The last bar is short because the month is not finished. Every chart of this shape appears to show a collapse, and somebody in the room reacts to it before anyone explains.

**The denominator swap.** A rate on one chart and a raw count on the next, on the same slide, describing the same thing. The two point in opposite directions and the audience trusts whichever appeared second.

**The honest chart with a dishonest title.** The encoding is clean and the title states a causal claim the chart cannot support. Two series moving together is not a chart of one causing the other, and the chart will be quoted by its title.

**The reformatting audit.** A review that fixes fonts, colours and gridlines and leaves the dual axis untouched. The deck now looks more credible and says the same wrong thing, which is worse than where it started.

**3D as house style.** A template with 3D bars applied across an organisation, so every chart in every deck overstates whatever is at the front. Nobody owns the defect because it arrived with the template.

**The unsourced dashboard screenshot.** A chart pasted from a live tool with the filters, date range and segment invisible. It cannot be reproduced, including by the person who pasted it, a fortnight later.

## What this skill does not do

- It does not have the underlying data and cannot verify any number. It compares the encoding to the claim, which is a different and narrower question.
- It cannot rebuild a chart. It names the defect, the class, and the fix, and somebody else makes the chart.
- It cannot read a chart that is a flat image unless the axes, labels, legend and colours are described to it, and screenshots are exactly the case where charts go unexamined.
- It does no statistics. Significance, noise, seasonality, confounding and causal inference are all outside it, and a chart can pass every rule here while supporting a conclusion the data does not.
- It cannot compute contrast or verify colour blind safety without real colour values, so palette findings are structural unless you supply hex codes.
