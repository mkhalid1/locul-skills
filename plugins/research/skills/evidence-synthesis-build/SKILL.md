---
name: evidence-synthesis-build
description: Produces one defensible synthesis from many sources without averaging away the places where they conflict. Extracts a structured row per source rather than a summary, clusters sources by origin so a number retold eight times counts once, normalises quantities before comparing them, classifies every remaining disagreement against a fixed eight-cause taxonomy, weights sources on independence and design fit rather than reputation, resolves the case where the weight of evidence and the single best study point different ways, and writes the output in a fixed slot order that ends with a required section stating what would change the conclusion. This skill should be used when several sources have been gathered on a contested question and one answer has to be written that someone will act on.
---

# Evidence synthesis build

## The claim this skill is built on

Where credible sources conflict, the conflict is the finding. A synthesis that smooths it into a confident middle has destroyed the most valuable thing it had, and it has done so invisibly, because the smoothed version reads better than the honest one.

This is not a plea for caution. It is a claim about where the information sits. If four studies say a thing helps and two say it does not, the useful output is almost never the average of the six. It is the reason the six split, because that reason usually names the condition under which the thing helps, and the condition is what the reader needs in order to decide anything.

## Why the obvious approach fails

Three specific failures, all common, none of which look like failures on the page.

**Vote counting.** Treating the number of documents on each side as evidence about which side is right. Documents are not independent. Eight pages carrying the same figure are often one measurement retold eight times, and the retellings are correlated with how quotable the figure is rather than with how sound it is.

**The synthetic middle.** Averaging two estimates that were never measuring the same quantity. If one study counts every reported fault and another counts only faults that reached a customer, the mean of the two is a number that describes nothing in the world.

**Consensus laundering.** Writing a paragraph that reports agreement by omitting the sources that disagreed, usually the older ones, the smaller ones, or the ones that were harder to read. This produces a confident document with no trace of the choice that made it confident.

## The method, in order

The order is not arbitrary. Each step removes a class of fake disagreement so the next step is not wasted on it.

### Step 0. Write the question and the decision it feeds

One sentence for the question, in a form that can be answered with a direction, a quantity or a yes and no. One sentence for the decision that will be made differently depending on the answer.

Without the second sentence there is no stopping rule, and a synthesis with no stopping rule expands until the deadline. It also tells you the precision required. A decision that flips at "any reduction at all" does not need a point estimate, and spending a day narrowing one is waste.

### Step 1. Extract, do not summarise

One row per source, with fields, not prose. The minimum set:

- The claim exactly as stated, quoted, not paraphrased.
- The quantity, its units, and its stated bounds or interval if it has any.
- The population and the sample size.
- The method: what was actually done to produce the number.
- The measurement definition: what counted as the thing being measured.
- The time window and the publication date.
- Who paid for it and who benefits from the direction of the result.
- Provenance depth: does this document contain the measurement, or does it cite it.

Summaries hide definitional disagreement, which is the single most common cause of an apparent conflict. Two sources cannot be seen to be measuring different things if you have written down what each of them concluded rather than what each of them measured.

### Step 2. Deduplicate by origin, not by document

Follow each claim back to the earliest document that actually contains the measurement, and cluster every source that traces to the same origin. What you want at the end of this step is a count of **independent evidence lines**, which is almost always much smaller than the count of documents.

**The reporting rule.** If more than two thirds of your documents trace to a single origin, the document count is not reportable at all, and every statement about weight of evidence must use the count of origins instead. Write both numbers in the output: fifteen documents, four independent lines.

This step is where most source piles lose their apparent strength, and it has to happen before weighting, because weighting an unclustered pile counts the same evidence several times over and produces false confidence that no later step can undo.

### Step 3. Rate each source once, and elsewhere

Each source needs a credibility rating: what kind of evidence it is, whether the venue is what it appears to be, whether it has been retracted or superseded, whether the methodology is visible. That is a separate pass with its own criteria, and this method consumes its verdict as a single field on the extraction row rather than repeating it. Where the rating comes back as "cannot assess", carry that word forward literally. It is not the same as low, and collapsing it to low is a decision you have not earned.

### Step 4. Normalise before you compare

Put every quantity on the same footing before deciding anything:

- Same units, and the conversion recorded on the row.
- Same denominator. Per user, per session, per month and per thousand requests are four different claims.
- Same time base, since a rate over a quarter and a rate over a year are not comparable without a stated assumption.
- Same population definition, or a note that they cannot be aligned.

**Two sources that cannot be normalised onto one footing are not in conflict.** They are answering different questions, and the correct output is to say so and to report both, separately, with their scopes attached. Recording that as a disagreement invents a controversy and then spends effort resolving it.

Most apparent conflicts die here. Doing this before classification is the highest-value ordering decision in the method.

### Step 5. Classify every remaining disagreement

Each surviving conflict gets exactly one label from this list. The label is the finding.

1. **Definitional.** The same word covers different constructs. One counts all faults, another counts customer-visible faults.
2. **Population.** A real effect that differs between groups. Both results are correct in their own population, and the finding is the heterogeneity.
3. **Temporal.** The world changed between the measurements. Both were right at the time, and the newer one is only more relevant if the change is the one that matters.
4. **Method.** Self-reported against instrumented, observational against controlled, laboratory against field. The gap between methods is often larger and more predictable than the gap between findings.
5. **Incentive.** A funder or an author benefits from the direction. Note the direction the error would point, not merely that a conflict exists.
6. **Analytic.** Same data, different model, different covariates, different exclusions. Common and rarely disclosed.
7. **Chance.** Small samples, and both results are compatible with one underlying value. The tell is overlapping intervals and a difference smaller than the noise either study reports.
8. **Error.** One of them is simply wrong: an arithmetic slip, a transcription failure, a bound dropped in retelling, a decimal moved.

Only 7 and 8 are resolved by picking a winner. The other six are the answer, and the output has to carry the label out loud: "the estimates differ because the two populations differ, and here is which one you are in".

### Step 6. Weighting, which is a minimum and not a mean

Score each independent line on four axes, high, moderate, low, or cannot assess:

- **Independence.** Is this a distinct measurement, or one of several views of a single origin.
- **Design fit.** Can this design answer the question that was asked. A survey cannot establish a causal effect however large it is, and no sample size fixes that.
- **Directness.** Does it measure your outcome, in your population, in your conditions, or a proxy in an adjacent setting.
- **Directional risk.** If it is wrong, which way does it point, and does that direction happen to suit whoever produced it.

**The line's weight is the lowest of its four scores, not the average.** This is the part that inverts most people's intuition. A large, well-conducted, well-funded study of the wrong population gets a low weight, because directness is low and no amount of rigour elsewhere repairs it. Averaging lets prestige and sample size buy back relevance, which is exactly the substitution the whole method exists to prevent.

Where an axis is "cannot assess", the line is reported and quarantined: it appears in the evidence table with the reason, and it does not enter any quantitative statement. It is never quietly averaged in.

### Step 7. When the weight of evidence and the best single study disagree

This is the case that has no default answer, so it gets an explicit ladder. Work down it and stop at the first rung that resolves.

**Rung 1. Is the weight real?** Count independent lines, not documents. If the many collapse to fewer than three origins, there is no weight of evidence, only a phrase, and the single strong study wins by default.

**Rung 2. Is the single study better on fit, or only on size and prestige?** Size is not fit. If it is larger but less direct, it does not beat the many. If it is the only design capable of answering the question asked, it does, even alone.

**Rung 3. Is the disagreement about direction or about magnitude?** These are not the same severity. If the many and the one agree on direction and differ on magnitude, report the direction as the finding and the magnitude as unsettled, with the range. Direction disagreements are the serious kind and cannot be reported as a range.

**Rung 4. Can you name a mechanism that explains the divergence?** If the strong study is the only one run after a change in the world, or the only one run in a population like yours, it is not an outlier, it is the most relevant thing you have and it should be weighted accordingly. If the many share a design flaw the strong study avoided, the same applies in the other direction.

**If you cannot tell.** No mechanism explains it, neither side dominates on fit, and the direction is contested. Do not average, and do not pick. Write the two-conclusion form:

> If the effect is driven by [condition A], the answer is X, supported by lines 1, 2 and 4. If it is driven by [condition B], the answer is Y, supported by line 3, which is the only measurement taken under B. The observation that separates them is [specific, findable thing], and it has not been made.

That output is more useful than either forced answer, because it tells the reader what to go and find.

### Step 8. Write it in slot order

1. **The answer**, one sentence, with a confidence word from the fixed vocabulary below.
2. **What is settled**: claims agreed across independent lines, each with the count of lines.
3. **What is contested**, with each conflict named by its cause from the taxonomy.
4. **What is unknown**: questions nobody in the set has looked at. This is short and it is the section readers most often act on.
5. **The evidence table**: one row per line, with rating, weight, and quarantine flags.
6. **What would change this conclusion.**

The order matters twice. The answer comes first because a synthesis that buries the answer gets skimmed and then misquoted from the middle. The contested section comes before the table because a table read alone looks like a tie, and a tie is the one impression the whole method is built to prevent.

### Step 9. What would change this conclusion

A required section, and the one that separates a synthesis from an opinion with citations.

For each of the top three claims, name a specific observable event that would overturn it, and where it would show up. Three rules for what counts:

- It must be something that could actually happen and be seen within a stated horizon.
- It must be specific enough to search for. "New research" is not an entry. "A study measuring the same outcome in teams under twenty people" is.
- If you cannot name one, the claim is not evidence-based, and it should be labelled a working assumption in the output rather than a finding.

The list doubles as the watch list. Put a refresh date on the synthesis and check the list on that date, because a synthesis with no expiry gets quoted for years after its strongest source was superseded.

## The confidence vocabulary

Use these five words and no others, and define them in the document:

- **Established.** Three or more independent lines, consistent direction, and a plausible mechanism.
- **Likely.** Two independent lines agreeing, or one strong line plus consistent indirect evidence.
- **Contested.** Independent lines disagree on direction, and the cause has been named.
- **Thin.** One line only, whatever its quality.
- **Unknown.** Nobody in the set measured it.

The confidence word is set by the weakest link in the chain the claim depends on, not by the strongest source that supports it.

**The factor-of-two rule.** If the highest and lowest credible estimate differ by more than a factor of two, a point estimate is misinformation. Report the range and the reason for the spread, which by this stage you have already labelled.

## Worked example, compressed

**Question.** Does requiring a second reviewer on every change reduce production incidents in a mid-size engineering organisation? **Decision it feeds:** whether to make the second review mandatory next quarter. All numbers below are invented to show the shape.

**Extraction.** Eleven documents. Two industry survey reports, one peer-reviewed field study, one controlled experiment on students, four vendor blog posts, two conference talks, one internal metrics write-up from another company.

**Origin clustering.** The four vendor posts and one conference talk all trace to the same survey report, and one of them has dropped its confidence interval along the way. Eleven documents collapse to **four independent lines**: the survey, the field study, the student experiment, and the internal write-up.

**Normalisation.** The survey reports incidents per developer per month. The field study reports defects per thousand lines changed. These cannot be put on one denominator without an assumption about change size that neither states. Recorded as not comparable rather than as conflicting.

**Classification.** The field study finds a reduction. The student experiment finds none. Label: **method plus population**, because the student cohort had no production system and no on-call consequence, so the mechanism the field study proposes could not operate there. The internal write-up finds an increase in incidents, and its own text says the review requirement was introduced in the same month as a platform migration. Label: **analytic**, a confound the authors disclose and do not adjust for.

**Weighting.** Field study: independence high, design fit moderate, directness high, directional risk low, so the weight is **moderate**, set by design fit. Survey: directness moderate, directional risk high because the sponsor sells a review tool, so **low**. Student experiment: directness low, so **low**. Internal write-up: quarantined, cannot assess, confounded.

**Rung check.** Do the many beat the one? There is no many. Four lines, of which two are low and one is quarantined. Rung 1 resolves it: there is no weight of evidence here at all.

**Verdict.** Direction is **likely** favourable in settings with production consequences, on one moderate line supported by a low one, with a magnitude that is **unknown** rather than merely uncertain. The student experiment is not counter-evidence and should stop being cited as such. Recommendation to the decision: run it as a change with a measurement rather than as a policy, since the evidence supports trying it and does not support promising a number. **What would change this:** a repeat of the field study in teams under twenty people, or any measurement that separates the review requirement from the platform migration in the internal case. Refresh date set at six months.

## Failure modes

**Consensus laundering.** The disagreeing sources are present in the bibliography and absent from the prose. Reads as a clean answer, and nobody can see what was dropped.

**Vote counting.** "Most sources say" where most sources are one source. The tell is that the same figure appears with identical wording across the set.

**The synthetic middle.** A number produced by averaging two incomparable estimates. It has units and no referent, and it is the number that gets quoted.

**Deference to the largest sample.** Size treated as relevance. Produces confident answers about a population you are not in.

**Averaging the weight axes.** Scoring a source four ways and taking the mean, which lets rigour buy back directness and reinstates exactly the error the four axes exist to catch.

**Silent normalisation.** Units converted or denominators aligned without recording the conversion, so nobody downstream can check the arithmetic and an error becomes permanent.

**Contest attributed to quality.** Labelling a definitional disagreement as one source being worse. This discards the definition, which was the useful part, and starts an argument about credibility instead.

**The unbounded range.** Reporting a spread so wide it cannot inform anything, when splitting by population would have produced two usable answers.

**Recency as a tiebreak.** The newest study wins because it is newest. Sometimes right, for a reason: a change in the world. Usually it is just a preference wearing a date.

**Confidence inflation on retelling.** Each version of the summary drops one qualifier, and four versions later "likely, in settings with production consequences" has become "reduces incidents".

**No expiry.** A synthesis with no refresh date and no watch list, still circulating after its strongest line has been superseded.

## What this skill does not do

- It does not pool numbers statistically. No effect sizes, no inverse-variance weighting, no heterogeneity test. Where the data supports meta-analysis, a meta-analysis is a better instrument and this is not one.
- It does not rate individual sources. It consumes a rating produced by a separate pass and will carry a wrong rating faithfully all the way to the conclusion.
- It cannot detect undisclosed dependence between sources. Shared datasets, overlapping authors and common funders are frequently invisible, so the independent-line count is an upper bound.
- It cannot retrieve what is paywalled or offline, and a source that was never read is recorded as unretrieved rather than excluded, which is a hole in the output and should be printed as one.
- It does not scale by hand past roughly twenty to thirty sources. Beyond that the extraction needs software and the counts stop reconciling without it.
- It cannot make a decision for you. It produces a two-conclusion output when the evidence genuinely supports two conclusions, and someone with the authority and the context still has to choose.
