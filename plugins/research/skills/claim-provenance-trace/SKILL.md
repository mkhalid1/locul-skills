---
name: claim-provenance-trace
description: Traces a statistic or a factual claim back through its citation chain to the earliest document that actually contains it, rather than to the most cited retelling. Names the specific distortion that occurred at each hop, including lost bounds, an upper limit read as a typical value, a projection reported as a measurement, a population substitution, a modelled figure reported as observed, an unstated currency or unit conversion, and a stripped date. Produces a provenance note that records the origin, the exact sentence, the hops and the verdict, and states plainly when the chain terminates in a document that does not support the claim. This skill should be used before repeating any number in published work, a deck, a board paper or a submission.
---

# Claim provenance trace

## The claim this skill is built on

A number can become untrue while every link in its chain is a real, working, honestly made citation.

This is the part that makes the failure hard to see. Nothing in the chain is a lie. Each author cited the source they read, and each source cited the source it read. At every hop something small happened to the claim: a range lost one end, a qualifier was dropped for length, a population was generalised, a forecast was written in the past tense. After four hops the sentence in front of you has a number, a citation, and no relationship to anything anyone observed.

Checking the citation does not catch this, because the citation is real. Only walking the chain catches it, and only if you know what to look for at each hop.

## The mechanism, stated plainly

Article C says: *businesses lose an average of £1.2m a year to poor internal documentation [17]*.

Reference 17 is article B, which is real, reputable, and says: *industry analysis suggests losses of up to £1.2m for the largest organisations surveyed [9]*.

Reference 9 is article A, a press summary, which says: *modelled annual costs ranged from £120,000 to £1.2m across organisations above 5,000 staff, based on a 2021 survey*.

The press summary references a conference presentation. The presentation slides are no longer online. The archived copy contains one bar chart, no method, no sample and no definition of the cost being measured.

Nothing was fabricated. What happened, in order, is: a range became its upper bound, an upper bound became an average, a specific size band became all businesses, a modelled figure became a loss, and a 2021 figure became a present-tense fact. The whole chain is above board and the claim is unsupported. Every figure in this section is invented to illustrate the shape.

## The distortions, named, one per hop

You cannot notice a missing bound unless you know that bounds go missing. Learn the list.

- **Debounding.** A bounded estimate loses one or both bounds. *Between 5 and 30 per cent* becomes *up to 30 per cent* becomes *30 per cent*. This is the single most common hop distortion.
- **Ceiling drift.** An upper limit becomes a typical value. *Savings of up to 8 hours a week* becomes *saves 8 hours a week*. The words that carried the limit were doing all the work.
- **Tense slip.** A projection becomes a measurement. *Forecast to reach 4 million units by 2027* becomes *reached 4 million units*, usually because a later writer summarised in the past tense out of habit.
- **Population creep.** A finding in one population becomes a general fact. A result in enterprises over 5,000 staff becomes a result about businesses. A result in one country becomes a result about the world.
- **Causal upgrade.** A correlation becomes a cause. *Teams that do X report higher Y* becomes *X increases Y*. Frequently accompanied by the original paper's explicit warning against exactly that reading, which nobody carried forward.
- **Model laundering.** A modelled or estimated figure becomes an observed one. *Our model estimates* becomes *research found*. Once laundered it is indistinguishable from a measurement.
- **Silent conversion.** A figure in one currency or unit is converted with no rate, no base year and no note. The number changes, the citation does not, and the conversion cannot be checked or reversed.
- **Date stripping.** A figure from a specific year is repeated with no date and silently ages. A 2019 measurement quoted flatly in 2026 is presented as current, and the reader has no way to know it is seven years old.
- **Sampling amnesia.** A survey of self-selected respondents becomes *research shows*. The recruitment method is the first thing dropped and the only thing that decided whether the result means anything.
- **Precision inflation.** A rounded estimate acquires decimal places on the way through, usually via a conversion or a per-capita division, and the false precision is then read as rigour.
- **Aggregation inflation.** A per-organisation figure is multiplied up to an economy-wide total, with the multiplier chosen by whoever needed the bigger headline.

## Numbers that are structurally suspicious

Trace these first. Each of these shapes is produced by a distortion far more often than by a measurement.

- **A round percentage.** 70 per cent, 80 per cent, a third. Real measurements land on 68.3, not on 70.
- **A very large aggregate cost figure**, especially one denominated in billions across a whole economy. It is nearly always a small per-unit estimate multiplied by a large count, and both inputs are usually softer than the output looks.
- **Claims of the form organisations lose X per year.** Loss is not observed, it is defined, and the definition is where the number was made.
- **Anything with the phrase up to.** The bound is doing the work, and the next writer will drop it.
- **A figure attributed to a named consultancy or institute with no report title, no year and no page.** The attribution is being used as authority to stop the reader looking.
- **A figure that appears identically across many pages**, down to the wording. That is copying, not corroboration, and the count of pages tells you nothing about the count of sources.
- **A figure with no unit, no denominator or no timeframe.** *Three times more effective* at what, than what, over what period.

## The tracing procedure

Run it in this order. The order matters because the earliest form of the claim, not the most cited one, is the thing you are trying to reach, and searching by popularity walks you towards the middle of the chain rather than the start.

**1. Fix the claim.** Copy the sentence verbatim as it appears where you found it. Record the number, the unit, the population, the timeframe and any hedging words. This is your reference text and everything is measured against it.

**2. Find the attached citation.** If there is one, follow it. If there is not, search for the exact phrasing rather than the topic, because the wording travels with the claim and a phrase match usually surfaces the copies.

**3. At each hop, read the actual sentence in the cited document.** Not the abstract, not the summary, not the title. Locate the sentence that contains the claim. If you cannot find the claim in the cited document, the chain has already terminated and you can stop.

**4. Diff the sentence against your reference text.** Check four invariants explicitly, because these are where the changes hide:
 - **Population.** Who was measured. Any widening is population creep.
 - **Timeframe.** When the data were collected, not when the document was published. These differ by years more often than not.
 - **Units and currency.** Including any conversion, its rate and its base year.
 - **Hedging.** Did the original say estimated, modelled, projected, up to, associated with, or self-reported. Every one of those that disappeared is a distortion with a name from the list above.

**5. Go earlier, not more cited.** Prefer the older document, the primary report, the dataset, the filing. A highly cited paper in the middle of a chain is a hop, not an origin.

**6. Stop when you reach an observation, or when the chain terminates.** An observation is a document whose authors collected, measured or modelled the thing themselves and describe how. Everything else is a hop.

**7. Record the result** in the format below, whatever the outcome.

## When the chain terminates in nothing

This is common and it is a result, not a failure of the trace.

The chain terminates when the cited document does not contain the claim, when the cited document cites nothing, when the origin is a slide deck, a press release, a webinar or a personal communication with no underlying study, or when the final reference does not exist at all.

**The decision rule:**

- **The origin exists and says what the claim says.** Cite the origin, not the retelling. Include its date and its population.
- **The origin exists and says something different.** Restate the claim to match the origin, with the original bounds, hedging and population restored. Then decide whether the corrected claim still supports your argument, because frequently it does not.
- **The origin exists and does not contain the claim at all.** The claim is unsupported. **State that, and remove the number.** Keeping the citation because it looks respectable is the exact behaviour that built the chain in the first place.
- **The chain reaches a document you cannot open**, because it is offline, paywalled, printed only, or a dead link with no archived copy. Record it as *traced to [document], not verified*, state the uncertainty in the text, and do not present the figure as established. This is the you-cannot-tell branch and it must be visible in the output, because an unstated uncertainty is indistinguishable from a verified fact to everyone downstream.
- **No origin is reachable at any hop.** Do not use the number. Say what you looked for and where you stopped.

## The statistic that will not die

Some figures have no traceable origin and circulate anyway for decades. The mechanism that keeps them alive is worth understanding, because it explains why the number's popularity is not evidence.

Everybody who repeats it saw it somewhere credible. A person reads it in a respected publication and reasonably concludes it has been checked. They repeat it, adding one more credible-looking appearance. The count of appearances rises. The count of independent sources stays at zero, and never rises, because none of the appearances was ever a measurement.

The tells are consistent: identical wording across sources, an attribution to an institution rather than to a document, no year, and a chain that folds back on itself so that two sources cite each other or both cite a third that cites the first. When you find circular citation, the trace is finished and the answer is that there is no origin.

## Recording the result

A trace nobody wrote down gets repeated by the next person. The provenance note is short and it goes wherever your team keeps working notes, next to the claim it concerns.

Record, at minimum:

- **The claim as used**, verbatim, including the number and its wording.
- **The chain**, each hop with its document, its date, and the distortion introduced at that hop.
- **The earliest document reached**, with a stable identifier: a digital object identifier, an archived link, a report title with a page number.
- **The exact sentence from the origin**, quoted.
- **The original population, timeframe, units and hedging.**
- **The verdict**: supported, supported in a corrected form, unsupported, or unverified with the reason.
- **The date you traced it and who traced it**, because a trace ages: the origin may be corrected, retracted or superseded after you looked.

If the verdict is supported in a corrected form, write the corrected sentence out in full so the next person copies the right one.

## Worked example, compressed

The claim, found in a 2026 blog post: *businesses lose an average of £1.2m a year to poor internal documentation*. All documents and figures below are invented to show the shape of a real trace.

**Hop 0, the blog post.** Reference given as a 2024 trade magazine article. No page, no quote.

**Hop 1, the trade article.** Actual sentence: *analysis suggests losses of up to £1.2m annually for the largest organisations surveyed*. Two distortions already: **debounding**, since up to became an average, and **population creep**, since the largest organisations surveyed became businesses. It cites a 2021 press summary from a research firm.

**Hop 2, the press summary.** Actual sentence: *modelled annual costs ranged from the equivalent of £120,000 to £1.2m across organisations above 5,000 staff, based on a survey conducted in 2020*. Three more: **model laundering**, since modelled costs became losses; **silent conversion**, since the equivalent of tells you the figures were converted from another currency with no rate and no base year given; and **date stripping**, since a 2020 survey is being quoted flat in 2026. It references a conference presentation.

**Hop 3, the presentation.** The slides are no longer on the conference site. The archived copy contains a single bar chart with an unlabelled vertical axis, no sample size, no definition of the cost being measured and no method. There is no accompanying paper and no dataset.

**Where the chain terminates.** At a slide with no method. No document in the chain contains an observation.

**Verdict: unsupported.** The figure should not be used. If the argument needs a cost estimate, it needs a new source, and the honest sentence for the document is that a widely repeated figure of £1.2m traces to a 2020 modelled range for organisations above 5,000 staff, presented in slides that are no longer available, and that no observed measurement was found. That sentence is more useful to a reader than the number was.

## Failure modes

**Stopping at the first citation.** Confirming that a citation exists and resolves, then treating that as verification. The citation nearly always resolves. That is the whole problem.

**Following popularity instead of age.** Searching for the claim and landing on the most cited retelling, which is a hop in the middle of the chain, then declaring it the source.

**Reading the abstract rather than the sentence.** The abstract carries the headline, and the qualifier that the claim dropped lives in the results section.

**Comparing the claim only to the number.** The number often survives the whole chain intact while the population, timeframe and hedging around it do not. If you only check the digits, the chain looks clean.

**Accepting an institutional attribution as an origin.** A named firm is not a document. Without a title, a year and a page, the attribution has not been checked by anyone including the person who wrote it.

**Mistaking repetition for corroboration.** Fifty pages carrying the identical sentence is one source, and the identical wording is the proof of that.

**Guessing at an unreachable origin.** Reconstructing what a paywalled report probably said, then reporting the reconstruction as the finding. Unverified is the correct output and it is a useful one.

**Keeping a citation after the trace fails.** Removing the number but leaving the reference attached to a softened sentence, which passes the claim on in a form that is harder to check.

## What this skill does not do

- It does not judge the quality of the origin. A claim can be perfectly traceable to a real study that used a weak design, a tiny sample or an unrepresentative population, and that is a separate assessment.
- It cannot open paywalled reports, printed documents or pages that were never archived, so some chains legitimately end in unverified.
- It does not search for a better number. Establishing that a figure is unsupported does not produce a replacement, and often no replacement exists.
- It does not verify a quotation attributed to a person. That is a different trace with different sources, usually transcripts and recordings.
- It cannot detect a fabricated origin that looks real, such as an invented report title on a plausible domain. It will report the origin it reached, not whether that origin is genuine.
- It does not stay true. A trace records the state of the chain on the day it was run, and origins are corrected, retracted and moved.
