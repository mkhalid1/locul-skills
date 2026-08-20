---
name: reach-drop-recovery-plan
description: Diagnoses a sudden collapse in organic social reach, the thing usually called a shadowban, and produces a dated recovery plan rather than a critique. Covers the rate-versus-volume arithmetic that separates withheld distribution from content decay, a four-branch decision rule including a real procedure for an account with too little baseline data to tell, a ranked cause list with confirm and rule-out tests for each, a four-phase recovery sequence with the reason the order matters, an expected-recovery table scaled to the account's own pre-drop level, and the tripwire dates that reopen the diagnosis. This skill should be used when impressions have fallen sharply on a social account, when a suppression event or shadowban is suspected, or when someone is about to edit, delete or out-post their way through a reach drop.
---

# Reach drop recovery plan

## The claim this skill is built on

When organic reach collapses, almost everyone starts with the cause and almost nobody starts with the shape of the data. That order is backwards and it is expensive, because the first three actions people take on a suspected suppression event, editing the offending posts, deleting them, and publishing more to make up the volume, are each either useless or actively harmful, and all three are taken inside the first 48 hours.

The shape comes first because one division settles the largest question for nothing. Impressions are the amount of distribution you were granted. Engagements divided by impressions is how the people who did see it behaved. If the behaviour is unchanged and the volume has collapsed, the material is doing its job and it is being shown to far fewer people. If the behaviour has collapsed while the volume held, the platform is still handing out distribution and your posts are wasting it, which is a writing problem wearing a suppression costume.

Say the rest plainly too. No major social platform publishes its ranking system in a form you could check a penalty against, none of them treat the word shadowban as a defined feature of the product, and every penalty percentage in circulation comes from vendor blog studies and from individual accounts reading their own analytics. So this file states mechanisms as mechanisms and labels magnitudes as estimates. A ranked list of causes you can confirm or eliminate from your own export is worth more than a precise-looking number nobody can source.

## Part one. The decision rule, run before any theory

Pull two exports before writing a word of diagnosis.

1. Per-post impressions for the last 12 months, which is what tells you whether the chart is a step or a slope.
2. Per-post impressions and engagements for every post in the drop window and for the ten posts before it.

Then compute three quantities.

```
V_before = the middle value of impressions across the ten ordinary posts before the drop
           (ordinary means excluding your two or three outliers, which wreck a mean)
V_after  = the middle value of impressions across the posts in the drop window
V_ratio  = V_after / V_before

R_before = total engagements / total impressions across those same ten posts before
R_after  = total engagements / total impressions across the drop window
```

Pool the engagements and the impressions before dividing. A rate computed on a single post with 110 impressions moves a full percentage point when one person reacts, and that noise is exactly where people find the pattern they were hoping for.

**Branch A. Rate steady, volume collapsed.** R_after within roughly a factor of two of R_before, V_ratio below about 0.25. The people reached behaved as they always did and there were far fewer of them. This is the case the rest of this file is written for. Go to part two.

**Branch B. Rate collapsed, volume roughly intact.** V_ratio above about 0.7 while R_after is well under half of R_before. Distribution is still arriving and the posts are not converting it. This is a content problem, this is the wrong procedure, and running the recovery sequence anyway costs you three weeks of reduced publishing to fix something that was never broken.

**Branch C. Both collapsed together.** Volume down hard and rate down hard, usually stepping at the same post. That combination points at an account-level or policy action rather than at ranking. The correct first move is the platform's own support and appeals route, plus a careful read of every notice attached to the account, not a change to your content calendar. Content changes cannot lift a restriction, and the weeks spent making them run down whatever appeal window exists.

**Branch D. You cannot tell.** This is the honest state for more accounts than admit it, and it gets a procedure rather than a shrug. You are in branch D if any of these is true: fewer than about eight posts in the 90 days before the drop; a posting gap longer than 60 days anywhere in that window; an export that does not reach back past the break; or pooled impressions under about 300 in either window, which is too few for a rate to carry meaning.

Do this instead of diagnosing.

- **Run a calibration fortnight.** Publish exactly three posts across 14 days, at least 72 hours apart, every one free of outbound links, all on a single topic, all in the same format, with nothing else about the account changed. You are not recovering yet. You are manufacturing three comparable data points with the obvious confounders held still.
- **While it runs, do the two checks that need no baseline.** First, read every notice the platform has attached to the account, then confirm that a logged-out search for your own name still returns your profile and that your latest post is visible to a logged-out viewer. Second, ask three people who follow you but are outside your daily circle whether the post appeared in their feed, not merely on your profile. A post visible on the profile and absent from every feed is a distribution finding you can act on. A post nobody can reach at all belongs in branch C.
- **Then pool the three posts and compute one rate.** Pooling is the whole point: it is what gets you past the small-numbers problem that put you in branch D.
- **If those three posts together still total under about 300 impressions, stop treating this as a recovery problem.** At that size reach is set by who happened to open the application that morning, and the work in front of you is audience building. This is a real answer, it is frequently the true one, and it is the one most likely to be resisted.

**The shape test, run alongside.** A suppression event is a step: two adjacent posts, days apart, an order of magnitude apart in impressions. Ordinary decay is a slope: each post 10% to 20% below the one before across a couple of months with no single break. If your chart is a slope, close this file. A slope is a content and audience trend, and the sequence below will do nothing for it except reduce how much you publish.

## Part two. The ranked cause list

Run these in order. The ranking is by how often each one has explained a case in the material this file was built from, not a measured frequency, and it matters for two reasons: the first cause accounts for a large share of cases, and causes two and four interact in a way that punishes a wrong guess.

Each cause gets three things. What it looks like from outside, how to confirm it, and how to eliminate it. Elimination matters as much as confirmation, because a plan carrying four unresolved candidates is not a plan.

The taxonomy is written from LinkedIn, where the community material on this is thickest and where the figures below were gathered. The mechanisms generalise to any ranked feed. The magnitudes do not, and where a figure is specific to LinkedIn it is named as such.

**1. Outbound links in the post body. Critical.**
The mechanism is not in dispute: a feed is ranked for time spent on the feed, and a post that sends the reader elsewhere competes with the platform's own objective. The magnitude is very much in dispute. Vendor studies of LinkedIn commonly cite the reach cost of one in-body external link across a wide band, roughly a fifth to a half, and they disagree with each other because each was measured on a different set of accounts. Treat the direction as reliable and the number as a hypothesis to test on your own posts. The first-comment workaround, treated for years as free, is now widely reported to carry a smaller penalty of its own, magnitude unverified.
Confirm: table the last ten posts, mark each one link or no link, and compare against impressions. If the surviving high-reach post is the one without a link and the suppressed ones all carry one, that is your candidate.
Eliminate: if the high-reach posts before the break also carried in-body links, this is not what changed, and it should not appear in the plan at all.

**2. Frequency and spacing. High.**
Mechanism: your posts compete with each other for the same followers, and a post published before the previous one has finished being distributed splits its own audience. For LinkedIn the commonly cited safe band is two to three posts a week with at least 24 hours between them, and the version most often blamed is a second post inside about three hours of the first. Community-derived, documented by nobody.
Confirm: plot the timestamps in the drop window against the timestamps in the good window. Two posts hours apart inside a suppressed week is the signature.
Eliminate: identical cadence on both sides of the break.

**3. Topic-authority reset after dormancy. High.**
Mechanism: routing depends on an association between your account and a subject, and that association is built by repetition and decays without it. A returning account is re-evaluated before it is trusted with reach again. Commonly cited at 60 to 90 days of consistency to build the association and roughly a fortnight of minimal distribution on return. Both figures are estimates.
Confirm: a gap of more than about a month immediately before the drop, especially where the return was at high frequency.
Eliminate: unbroken publishing through the window.

**4. Compounding negative feedback from the seed cohort. Medium, and it is the cause that makes the others worse.**
Mechanism: a post is shown first to a small initial group and the signal it gets there decides whether it travels further. A weak result lowers the floor the next post starts from, so a bad week compounds into a bad month with nothing else changing. Seed cohorts of 100 to 200 people are commonly cited and are published by no platform.
Confirm: a staircase rather than a cliff, four or five posts each meaningfully below the last, usually beginning at one identifiable bad post.
Eliminate: a clean step followed by a flat floor.
This cause is the reason the recovery sequence opens with silence rather than with a better post.

**5. Topic shift. Medium.**
Mechanism: your reach history was built in one subject and the recent posts are in another, so routing has nothing to work from even though nothing is being penalised.
Confirm: classify the last 20 posts by topic and compare the mix before and after.
Eliminate: the same mix on both sides.

**6. Publishing tool. Low, and usually eliminated.**
Mechanism: tools publishing through a platform's official interface are sanctioned. Tools that drive a logged-in browser session or hold your password are against most platforms' terms, and a study circulated in the community puts a substantial share of accounts using automation of that kind under some restriction within 90 days. Single source, unverified, sample unknowable.
Confirm: check how the tool authenticates. An official integration sends you to a consent screen owned by the platform and names the permissions it wants. A scraping tool asks for your credentials or a session cookie.
Eliminate: official authentication clears the tool outright.
Distinguish carefully. "The tool published the post" is a tool question. "The post linked to the tool's own website" is cause one in a costume.

**7. The null cause, which belongs in every diagnosis.** Nothing was withheld, and the last three weeks of material were worse than the three weeks before. This is the most common true answer to the question this file exists to answer, and a procedure that cannot arrive at it is a horoscope. It is why branch B exists, why the deliverable carries a null-hypothesis line, and why the second tripwire in part four is written the way it is.

## Part three. The recovery sequence, and why the order is the method

**Phase 0, days 1 to 3. Silence.** Publish nothing for 48 to 72 hours. It is first because of cause four: every post published while the floor is low drags the floor lower, and every post published during the diagnosis is a data point taken under changed conditions, which destroys the measurement you are about to depend on. Instead: two to three genuine comments a day on other people's posts, which keeps the account active on a different surface without adding to the chain. Audit the last ten posts for in-body links and write down what you find, changing nothing. Review the third-party applications holding permissions on the account and revoke the ones you no longer use.

**Phase 1, days 4 to 14. Controlled resumption.** Two posts a week, at least 48 hours apart. Zero links anywhere, first comment included. If a link is genuinely unavoidable, put it in a comment at least 12 hours later or ask people to message you. Cap hashtags at three. Hold one topic. Lead with your most reference-grade material, the frameworks, checklists and numbers people save and forward, rather than opinion, because with little reach you need the strongest signals from the few people who do see it. It comes second because the pause established a floor and these are the first clean readings from it.

**Phase 2, days 14 to 90. Consistency.** Lock two or three topics and do not deviate for 90 days. Run roughly five substantive comments for every post you publish. Causes three and five are only repaired by time under a consistent signal, and any change of subject restarts the clock you are trying to run down.

**Phase 3, days 30 to 90. Ramp, conditionally.** Two posts a week in weeks one and two, three in weeks three and four, four to five from week five, and each step taken only if the previous step held its numbers. It is last because raising frequency before reach recovers re-triggers causes two and four at the same time, which is the loop that dug the hole in the first place.

**Why the order matters, stated once.** Pause, then diagnose, then publish, then scale. Each phase is the precondition for the measurement in the next one. Scale first and nothing is attributable: you will have changed frequency, spacing, topic and link behaviour inside a single fortnight, and whatever happens afterwards is uninterpretable in both directions, including the happy one.

## Part four. The expected-recovery table and the two tripwires

Publish this inside the plan, scaled to the account's own `V_before` rather than to anyone's absolute numbers, and with real calendar dates attached.

| Window | Expected per post, as a share of `V_before` |
| --- | --- |
| Week 1 | zero, paused by design |
| Weeks 2 to 3 | roughly 5% to 25% |
| Weeks 4 to 6 | roughly 25% to 60% |
| Weeks 7 to 10 | roughly 60% to 100% |
| Week 12 and after | prior level, or a decision that it is not returning |

This curve is assembled from one account's own recovery plus community reports, so use the bands to tell progress from wishful thinking, not as a forecast. The value of writing it down is that it happens before you have an emotional stake in the answer.

**Tripwire one, the diagnosis tripwire.** If week six is still at week two levels, the diagnosis was wrong. Do not extend the plan. Return to part two, take the next cause down the list, and write a new plan with a new tripwire date.

**Tripwire two, the null tripwire.** If weeks two and three come back at 60% or more of `V_before` almost immediately, that is evidence against a suppression event. The pause changed nothing a ranking system could see except what you published. Record that the likely cause was the material rather than distribution, and stop attributing it to the platform.

## Part five. What the deliverable contains

The output is a dated document, not a critique. These sections, in this order:

- **Header.** Account, date the diagnosis was run, drop window with start and end dates, source of the data and the date it was exported.
- **The arithmetic.** `V_before`, `V_after`, `V_ratio`, `R_before`, `R_after`, and which branch they land in.
- **Causes.** Each of the seven marked confirmed, eliminated or unresolved, with the evidence beside it and one named primary cause.
- **The plan.** Four phases with real calendar dates, not day offsets, because a plan in offsets is never actually followed.
- **Expected recovery.** The table above with your own numbers substituted.
- **Tripwires.** Both dates written down, plus the name of the next cause to take if tripwire one fires.
- **The null hypothesis.** One line stating what would show there was no suppression event, written before the plan starts.

## Worked example

An independent consultant who sells incident-response workshops to engineering teams. Numbers invented for illustration.

**The arithmetic.** Over the prior 12 months, ordinary posts drew 4,000 to 9,000 impressions with three outliers at 12,000, 28,000 and 41,000. Excluding the outliers, `V_before` is 5,800. In the drop window, four posts across seven days drew 620, 240, 155 and 110, so `V_after` is about 198 and `V_ratio` is 0.03. Engagements on those four were 14, 6, 4 and 2, giving pooled `R_after` of 26 on 1,125, or 2.3%. The ten posts before pooled to 1,180 engagements on 58,000 impressions, so `R_before` is 2.0%.

Rate steady, volume at three percent of before. **Branch A.** The shape test agrees: the post 12 days earlier drew 6,400 and the next drew 620, which is a step and not a slope.

**Causes.** Post one carried an in-body link to a landing page. Posts two and three carried first-comment links. Post four carried none but was published two hours after post three. The 6,400 post carried no link at all. Cause one confirmed. Cause two also confirmed, four posts in seven days with two of them two hours apart. Cause three eliminated, publishing was continuous all year. Cause five eliminated, the same two topics run through both windows. Cause six eliminated, the scheduler authenticates through the platform's own consent screen. Cause four unresolved and plausible as a consequence of the other two.

**Verdict: withheld distribution, primary cause in-body and first-comment links, secondary cause spacing, and no evidence for an account action.** The plan runs a pause to day 3, resumption at two link-free posts a week from day 4, consistency locked on the two existing topics, and no ramp before day 30. Scaled to `V_before` of 5,800, weeks four to six should land between roughly 1,450 and 3,500 impressions per post. Tripwire one is set at day 42: if posts are still under 600 then, the diagnosis was wrong and the next cause to take is cause four, which implies a longer pause rather than more posts.

**And the null line, written into the plan on day 0.** All four posts in the drop window were product announcements, while the ten before were teaching posts. If reach returns to 60% of `V_before` inside weeks two and three, the honest conclusion is that the material changed and distribution did not, and the link finding was a coincidence that happened to be true.

## About every number in this file

None of it is platform documentation. Link penalties, seed cohort sizes, safe cadence bands, authority windows and the automation restriction figure are aggregated from vendor blog posts and from individual accounts reading their own analytics, and several cannot be verified at all. They are here because the mechanisms are real and the direction is consistent, not because the magnitudes are known. Before you act on any of them, re-derive what you can from your own export, which is the only data set in this whole procedure that you can actually see.

## Failure modes

**The edit.** Going back into the suppressed posts to strip the links. Editing a post shortly after publication is widely reported to cost further reach, magnitude unverified, and the cost of the original link has already been taken. From the outside it looks like decisive action, and it converts one damaged post into two.

**The delete.** Removing the offending posts. It destroys the evidence the diagnosis runs on, removes whatever residual distribution they still had, and returns nothing, because the signal they generated was recorded when they ran. The account is left with a gap in its history exactly where it needed data.

**The volume response.** Concluding that fewer impressions per post means you need more posts. This is the single most common reaction and it feeds causes two and four simultaneously. Reach per post falls further, which reads as confirmation that the suppression is worsening, which produces still more posts.

**The engagement blitz.** Reacting to fifty or more posts inside an hour to look active. Rapid bursts of identical actions are what automated tools produce, on an account that may already be under scrutiny. It looks from outside like someone working hard on a recovery, right up to the point where a real restriction arrives.

**The pivot.** Changing subject because the old one "stopped working". This resets cause three and creates cause five in one move, and it guarantees the next 60 to 90 days go on rebuilding an association you already had.

**The pod.** Joining a reciprocal engagement group. Engagement arriving from the same accounts on every post, in the same order, within minutes, is a pattern any ranking system can see, and the reach it buys comes from people who were never going to buy anything.

**The single-post diagnosis.** Declaring suppression from one bad post. Post-level variance is large and the shape test needs a run of posts on both sides of the break. It looks like a fast diagnosis and it is a coin toss with a document attached.

**The unfalsifiable plan.** A recovery plan with no tripwire date and no null hypothesis. It cannot fail, so it runs for months: every improvement confirms it and every disappointment extends it.

**The blended change.** Altering links, cadence, topic and format in the same week. Reach recovers, nothing is learned, and the next drop starts from the same standing start as this one.

## What this skill does not do

- It has no access to your analytics. Every figure has to be exported by a person and handed over, and without the two exports named in part one the decision rule cannot run.
- It cannot confirm that a suppression event occurred. No platform documents the mechanism or acknowledges individual cases, so the strongest available conclusion is that your export is consistent with distribution being withheld.
- It cannot resolve an account restriction or a policy action. That is the platform's appeals route, and content work is not a substitute for it at any length.
- It cannot judge whether your recent posts were simply worse. It gives you branch B and the null tripwire. The answer comes from reading the posts, and a person who knows your audience does that better and faster.
- Its magnitudes are community-derived and need re-checking against your own numbers, because the platform publishes none of them and several are unverifiable in principle.
- It does not write the posts. The resumption phase asks for your strongest reference-grade material, and it cannot supply it.
