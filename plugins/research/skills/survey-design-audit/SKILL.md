---
name: survey-design-audit
description: Audits a questionnaire before it is fielded, covering the named wording defects (leading, loaded, double-barrelled, assumed premise, undefined term, unavailable recall, socially desirable), scale design including odd versus even points and where "not applicable" belongs, question order effects, response option coverage, sampling and non-response, sample size arithmetic, length and abandonment, piloting, and the analysis plan that decides which questions survive. This skill should be used whenever a survey, questionnaire, poll, or feedback form is being drafted, reviewed, or added to.
---

# Survey design audit

## The claim this skill is built on

Almost all the damage a survey does is done before a single response arrives, and almost none of it is visible in the results. A biased question produces clean data, a tidy chart, a plausible number, and a decision. Nothing downstream flags it. The analysis cannot recover what the instrument did not ask, and no sample size fixes a question that two respondents understood differently.

That is why survey review is worth doing as an audit with a fixed order rather than a read-through. The order matters because the early tiers can make the later ones irrelevant: there is no point perfecting the wording of a question that should be deleted, and no point deleting questions if the people who receive the survey are the wrong people.

Five tiers, in this order.

## Tier 0. What decision does this change?

For each question, write three things: the chart or table it produces, the comparison it feeds, and the action that differs if the answer is A rather than B.

**The cut rule.** If no possible answer would change any action, the question is deleted. Not shortened, not moved to the end. Deleted. Every question costs completions from every respondent, so a question that informs nothing is paid for by the questions that do.

Pre-specify the segments you will break results by, and check each has a plausible chance of enough respondents to report. A segment you decide on after seeing the data is exploratory, and the write-up has to say so.

## Tier 1. Who will actually answer?

**A sample is not "everyone who answered".** Respondents are a self-selected group, and the selection is on effort, opinion strength, tenure and engagement, all of which correlate with the answers.

**Non-response bias is the dominant threat in practice**, and it is not measured by the response rate. A 12 per cent response rate is not automatically fatal and a 60 per cent rate is not automatically safe. What matters is whether responding correlates with the answer, and it usually does, because people with a strong recent experience are the ones who bother.

**The statement people dislike: a large self-selected sample is worse than a small representative one.** Increasing the sample shrinks the interval around the estimate without moving the estimate towards the truth, so a biased estimate with 10,000 responses is a wrong number reported with confidence. Size fixes noise. It does nothing to bias.

**Checks.** Compare respondents to the full population on variables you already hold: tenure, plan, usage band, region, role. Where the gap is large, either weight or disclose. Where the frame itself excludes people, say so out loud: a survey emailed to users who logged in during the survey week cannot see the users who stopped logging in, and those are the ones whose answers would change the conclusion.

## Tier 1b. Sample size, as arithmetic

What determines the number you need: the size of the difference you must detect, the variability of the answer, the confidence required, and, for small finite populations, a correction. Not, in the usual case, the size of the population.

For a proportion at 95 per cent confidence the margin of error is 1.96 times the square root of p times (1 minus p) over n. At p of 0.5, the worst case, that is 0.98 over the square root of n, which is why the useful shortcut is 1 over the square root of n:

- n = 100 gives about 10 points
- n = 400 gives about 5 points
- n = 1,000 gives about 3.1 points
- n = 2,500 gives about 2 points

**Where extra responses stop helping.** The error falls with the square root, so quadrupling the sample halves the interval. Going from 400 to 1,600 halves it, and from 1,600 to 6,400 halves it again. Past roughly a thousand responses for a whole-population estimate, the bias term is almost always larger than the sampling error, and the next unit of effort belongs on chasing non-respondents, not on volume.

**Subgroup arithmetic decides the real number.** If results must be reported for six segments, the smallest segment sets the size. A segment with 40 respondents carries a margin around 15 points, which cannot support any statement more precise than "most" or "few".

## Tier 2. The questions themselves

Seven named defects. Each is given as the defect, an example, and the repair.

**Leading.** Presumes the direction of the answer. "How much did the new dashboard improve your workflow?" There is no answer available for "it made things worse". Repair: make both directions available and symmetric. "Since the new dashboard, has your workflow become easier, harder, or stayed about the same?"

**Loaded.** Carries a charged word or a contested premise inside it. "Do you support the wasteful spending on the reporting rebuild?" Repair: strip the evaluative term and describe the thing neutrally.

**Double-barrelled.** Asks two things and permits one answer. "How satisfied are you with the speed and reliability of the service?" A respondent who finds it fast and unreliable has no honest option, and the resulting number cannot be interpreted in either direction. Repair: split into two items. This is the most common defect in internal surveys, and the giveaway is the word "and" inside the question stem.

**Assumed premise.** Asserts something about the respondent. "How often do you use the reporting feature?" assumes they use it at all. Repair: a filter question first, with the following item shown only to those who pass it.

**Undefined term.** "Regularly", "recently", "often", "engaged", "active". Each respondent supplies their own definition, so the aggregate mixes several different questions. Repair: specify in units. "On how many of the last 7 days did you open it?"

**Recall the respondent does not have.** "How many times did you contact support in the past 12 months?" People do not know, so they estimate, and estimation errors are directional rather than random. Repair: shorten the window to something recallable, offer bands rather than an exact count, or take the number from your own systems and stop asking.

**Socially desirable answer.** "Do you read the documentation before contacting support?" Repair: a face-saving preamble that makes the undesirable answer normal ("Many people go straight to support, which is often quicker"), an indirect framing, or dropping the question, because the answer you will get is already known.

Two more to watch: **double negatives** ("Do you disagree that sync should not be limited?"), which produce reliable misreadings, and **absolutes** ("always", "never"), which push respondents off the position they actually hold.

## Tier 3. The answer space

**Odd versus even points.** An odd scale has a midpoint, which serves two purposes at once: a genuine neutral position, and a parking space for anyone answering without reading. An even scale forces a side, which manufactures an opinion from people who do not have one. Rule: odd when neutrality is a real position on this subject, even when you specifically want a lean and can tolerate the artefact.

**Five and seven point scales are not interchangeable.** Seven points offer finer discrimination and a different distribution of responses. Means from the two are not comparable, and rescaling a seven onto a five after fielding is destructive. The practical consequence: if you hold historic data on a five point scale, changing to seven costs you the trend line, and the reason to change had better be worth more than the trend.

**Labelling every point versus only the ends.** A fully labelled scale reduces drift between respondents, since each point carries the same meaning for everyone, and it survives translation. An end-anchored numeric scale invites respondents to impose their own interpretation of the middle numbers. Label every point when results will be compared across groups, sites, or languages.

**Unbalanced scales.** "Excellent, Very good, Good, Fair, Poor" offers three or four positive options and one negative, and it will produce a positive result on any subject whatsoever. Repair: equal numbers of positive and negative points around a clear neutral, with parallel wording on each side.

**Where "not applicable" belongs.** Outside the scale, visually separated, and stored as a distinct code. If "no opinion" or "have not used it" is collapsed into the midpoint, every mean moves towards neutral and the size of the movement depends on how many people had no view, which nobody will remember to check.

## Tier 3b. Response options

- **Exhaustive and mutually exclusive.** Test both directions: can a respondent honestly tick two, and can a respondent honestly tick none. "Daily, Weekly, Rarely, Never" fails the second, because monthly falls in a gap.
- **An escape option is required**, and the three common ones are different things: "None of these" is a substantive answer, "I do not know" is an admission, and "Prefer not to say" is a refusal. Collapsing them loses information you will want.
- **The "other, please specify" trap.** If nobody has committed to reading and coding the free text, the box is a discard bin that also makes the percentages misleading, because "other" absorbs a real category you failed to list. Rule: either name the person who codes it and when, or replace it with a fixed list plus "None of these" and accept the loss.
- **Option order.** Long visual lists suffer primacy effects, read-aloud lists suffer recency. Randomise the order where the list is unordered, record the order shown as a variable, and never randomise an ordinal scale.

## Tier 4. The instrument as a whole

**Order effects are real and large.** Earlier questions prime later ones. The specific case worth knowing: a general satisfaction question placed after a block of specific items is answered as a summary of those items, while the same question placed first is answered as an overall impression, and the two produce different distributions. Asking about a recent outage before general satisfaction lowers the general score.

Mitigations: put the general question first when you want an unprimed overall; randomise item order within a block and store the order; and hold the order fixed across waves when tracking a trend, because changing it breaks comparability in a way that looks exactly like a real movement.

**Length and abandonment.** Every additional minute costs completions, and the loss is not random. The people who abandon are the busiest and least invested, so the questions at the end are answered by a different, more engaged population than the ones at the start. Two consequences: never place a decision-critical question last, and never compare an item at position 40 with an item at position 3 as though they came from the same sample. Put critical items in the first third, demographics near the end unless one is needed for weighting, and remember that a matrix grid reads as one question and costs like the number of rows in it.

## Tier 5. Piloting

The smallest pilot that catches the worst problems: five to eight people from the actual target population, answering while thinking aloud, plus one full data pass on the pilot rows.

Three things a pilot catches that a review never will: a question that two people interpret differently from each other, a question nobody can answer without guessing, and a branch or skip rule that sends someone down the wrong path.

**Run your planned analysis on the pilot rows** even though the sample is tiny. If the chart you promised cannot be produced from the pilot data, the instrument is wrong, and finding that out for the cost of eight responses is the cheapest discovery available in this whole process.

## The special case of a satisfaction or recommendation score

Treated honestly, a single recommendation number measures a summary attitude at one moment, dominated by recent experience, from the people who chose to answer. It is comparable to itself over time only if wording, scale, order, sampling and send timing are all held constant, and comparable across organisations essentially never.

What it does not measure: what to fix, whether anyone will actually recommend anything, or the state of the accounts that did not respond, which is where churn lives.

**The arithmetic problem with bucketing.** Collapsing an eleven point scale into three buckets and subtracting one from another throws away most of the information and makes the result jumpy at realistic sample sizes. With 100 respondents, moving a single person from the bottom bucket to the top moves the headline by two points. A four point quarter-on-quarter change on that base is two people, and it will be presented as a trend.

**The misuse to watch for.** When the number becomes a management target, the instrument gets optimised rather than the product: the send is timed after a good interaction, unhappy segments quietly drop out of the frame, and staff learn to ask for a specific rating. The check is simple. If the person whose objectives depend on the number also controls who is surveyed and when, the trend is not evidence about anything.

## Decision rule

- **A question fails on wording only.** → Rewrite it, keeping the same thing being asked, and note that any trend from previous waves is broken.
- **A question fails on premise or has no filter.** → Add the filter question and route around it, rather than adding "not applicable" to the scale.
- **A question changes no action.** → Cut it, whoever asked for it.
- **The frame excludes a population whose answers would differ.** → Fix the frame if you can, and if you cannot, publish the exclusion in the same sentence as the headline number every time it is quoted.
- **You cannot tell whether a question is understood as intended.** → That is what a pilot is for, and it is the one defect class that reasoning cannot settle. Do not resolve it by argument in a review meeting. Five think-aloud responses settle it in an afternoon.

## Worked example, compressed

**A six-question survey for a team scheduling tool. All figures invented for this example.**

**Q1.** "How satisfied are you with our new scheduling assistant?" on a five point scale reading Extremely satisfied, Very satisfied, Satisfied, Not very satisfied, Not at all satisfied. Placed first.
Defects: the scale is unbalanced, three positive against two negative, and it will return a positive result regardless of reality. It also assumes the respondent has used the assistant, and there is nowhere to say otherwise. Position first is correct and should be kept.
Repair: a filter question ahead of it, a symmetric fully labelled scale from Very dissatisfied to Very satisfied with a neutral midpoint, and "Have not used it" placed outside the scale.

**Q2.** "How much time does the assistant save you each week?" open numeric.
Defects: leading, assumes a saving, and demands recall nobody has. Every answer will be a guess anchored on a round number.
Repair: ask direction first ("more time, less time, about the same"), then a magnitude band, or drop it and instrument the product.

**Q3.** "How satisfied are you with the speed and reliability of the assistant?"
Defect: double-barrelled. Repair: two separate items.

**Q4.** "Do you agree that calendar sync should not be limited to one account?"
Defects: leading, double negative, and it asserts a limitation the respondent may not know about.
Repair: state the fact neutrally, then ask a factual question. "Calendar sync currently supports one account. How many calendar accounts do you need to keep in sync?"

**Q5.** "How often do you use the assistant? Daily, Weekly, Rarely, Never."
Defects: undefined terms, options that are not exhaustive, and it is placed after the satisfaction items where it cannot act as the filter that Q1 needs.
Repair: "On how many of the last 7 days did you open it? 0 to 7", moved to position one.

**Q6.** "Anything else?" free text, after a 34-row matrix grid.
Defects: it is answered by whoever survived the grid, and nobody has been assigned to code it.
Repair: keep it, name the person who reads it and the date, and treat it as anecdote rather than data.

**Frame.** Sent to users who logged in during the survey week, which excludes exactly the lapsed users whose answers would change the conclusion, and announced by the team that owns the feature.

**Verdict: hold.** Two questions are unusable as written. Q1 needs a symmetric scale before any baseline is set, because a baseline on an unbalanced scale locks the defect into every future wave. The frame excludes the population of interest. The minimum that unblocks fielding: a filter question at position one, symmetric scales on Q1 and on both halves of the split Q3, a neutral rewrite of Q4, and a frame that includes users who did not log in this week.

## Failure modes

**Auditing the wording and never the frame.** The questionnaire is the document in front of you, so it gets the attention, while the decision that determines the answer is the mailing list nobody attached.

**Fixing a scale mid-programme and keeping the trend line.** The chart continues across the change, the step is read as a real movement, and the instrument change is not in the footnote.

**Treating the response rate as the quality measure.** A high rate from a narrow frame is worse than a low rate from a representative one, and the rate is quoted because it is the only number available.

**Reading free-text answers as though they were a sample.** Verbatims are the most persuasive thing in any deck and the least representative, because writing one takes effort that only the strongly opinionated will spend.

**Reporting a subgroup mean from 20 respondents with no interval.** The subgroup gets a bar on the chart identical in weight to a subgroup of 900.

**Reporting a satisfaction number from a survey the feature owner wrote, timed and sent.** Every step of the process is controlled by someone with an interest in the answer, and no step of it is visible in the result.

**Adding questions because someone was curious.** Curiosity questions are indistinguishable from decision questions once they are in the draft, and they are paid for in abandonment by the questions that mattered.

**Piloting with colleagues.** They know what you meant, which is the one thing a pilot exists to test.

## What this skill does not do

- It does not analyse results. Weighting, significance testing, and handling of partial responses are separate jobs that need the data and a statistician.
- It cannot see the sampling frame, the response rate, or the non-respondents, and those matter more than anything in the questionnaire.
- It does not cover interviewer-administered or translated instruments, where cognitive interviewing and back translation carry defect classes that are not listed here.
- It cannot tell you whether a respondent understood a question. Only a pilot with real people does that, and this will tell you to run one rather than substituting for it.
- It will not defend your questions for you. The output of a good audit is usually a shorter survey, and someone will have to be told their question was cut.
