---
name: promotion-case-build
description: Builds a promotion packet from an employer's own career ladder text: a one sentence level claim, an evidence table with one row per ladder criterion, an artifact and a named corroborator per row, a proposed reviewer set, and a written answer to the objection most likely to be raised. Carries the dimensions published ladders share, the operating-at-level rule, the scope inflation tells a calibration room reads for, and a go or no go rule with a branch for when the ladder is vague or unwritten. This skill should be used when a promotion cycle or a self assessment is open, or when someone is deciding whether to go up this cycle and the evidence has not yet been mapped to the ladder.
---

# Promotion case build

## The claim this skill is built on

The output is a promotion packet: a level claim in one sentence, an evidence table with one row per ladder criterion, the artifact list, the proposed reviewer set, and a written answer to the objection most likely to be raised. It is a document. It is not talking points and it is not a plan for a conversation.

The obvious approach is to write a strong summary of a good year. That document loses to a thinner-looking one whose every claim sits next to the criterion it satisfies, and the reason is structural rather than literary. Your manager presents you to a group of other managers. Most of them have never seen your work, none of them have read your code or your documents, they are working through a list under time pressure, and they are holding a written standard in one hand and other candidates in the other. Two things survive that room. Evidence a stranger can check without asking you, and a claim that somebody who is not you can restate in one sentence.

Employers call the meeting different things: calibration, promotion committee, moderation, review board, talent review. The mechanics that matter are the same in all of them. The reader was not there, the reader is comparing, and the reader is reading fast.

Everything below is written for that reader.

## Step 1. Get the ladder text before you write a word

Ask for the document, and ask for its version and date in writing. You need three things.

- **The criterion text for your current level.** This is what a sceptic will say you are already being paid to do.
- **The criterion text for the next level.** This is the entire spine of the packet.
- **The process document.** Who decides, when packets are due, whether there is a word or page cap, whether peer reviewers are nominated by you or chosen by your manager, and whether the committee sees your self review verbatim or a summary written by your manager. That last question changes what you write: if it gets summarised, the sentences that survive are the ones that are already short.

Quote the criteria verbatim in your packet. Paraphrasing costs you the exact phrase a tired reader is scanning for, and it invites the response that you have restated the standard in terms that suit you.

If the ladder exists only as a slide, get the slide. If it exists only in a manager's head, go to the cannot-tell branch at the end.

## The dimensions published ladders actually share

Use your employer's words, always. This list is for interrogating a thin ladder, not for replacing one. It is drawn from public frameworks, checked in August 2026.

- **Scope.** The size of the thing you are responsible for. Public ladders express it as ownership: a well-scoped component of somebody else's project at the bottom, technical design for a project of moderate complexity in the middle, strategic effect across a large team or a deep problem at the top. The engineering ladder Kickstarter published as a public gist is written almost entirely in this vocabulary, across eleven positions on technical, data and people paths.
- **Autonomy.** How much steering you need. The UK government's Digital and Data Profession Capability Framework makes this its whole proficiency scale, with four ascending levels named awareness, working, practitioner and expert, where working means applying a skill with some support and expert means leading and teaching others. That framework was first published on 23 March 2017, was renamed from the Digital, Data and Technology framework on 1 December 2023, was last updated on 29 May 2026, and is published under the Open Government Licence, so it can be adapted rather than only read.
- **Influence.** Whether your effect extends past your own output. It appears as a named axis in the Engineering Ladders framework published on GitHub under Apache-2.0, which uses five axes across seven levels, and as the phrase about influencing without reporting authority in several company ladders.
- **Impact.** What changed for the business or the user. Dropbox's public engineering career framework carries this as one of four pillars, alongside direction, talent and culture, across IC and management tracks.
- **Ambiguity.** How well defined the problem is when it arrives. This is the one least often given its own column. It is usually folded into scope, or into a pillar named something like direction. If your ladder has no ambiguity language, look for it in the verbs: given a problem, given a goal, and identifies the problem are three different levels wearing similar clothes.

Two structural facts about ladders themselves matter for the packet. Frameworks differ in whether every line must be met or whether strength on some axes can offset gaps on others: Medium's published growth framework, licensed CC BY-SA 4.0, splits seniority into multiple tracks precisely so a person can be spiky, while a single-column ladder implies every line is required. Read which kind yours is before you decide what counts as coverage. And these documents are versioned: CircleCI's competency matrix, published under CC BY-SA 4.0, is a six-level matrix created in 2018, and the collection at progression.fyi shows roughly seventy-five organisations' frameworks with licences ranging from CC BY-SA and MIT to none stated. Your employer's is versioned too. Cite the version you were assessed against.

## Step 2. The operating-at-level rule

At most employers with a written ladder, promotion is recognition of behaviour already sustained, not a bet on future behaviour. Check whether your own document says this, because the phrasing is usually explicit: already operating at, consistently demonstrates, has been performing.

Three practical consequences follow, and they decide the shape of the evidence table.

1. **The packet must show next-level behaviour over a period, not a peak.** As a working rule, evidence spanning more than one review cycle. One cycle of strong work is a good cycle.
2. **A single large project is the weakest possible case.** It cannot distinguish a person operating at the next level from a person who was handed the right assignment. If your whole packet is one project, the room cannot tell those apart and it defaults to the safer reading. The fix is not to shrink the project, it is to add two smaller, unrelated pieces of evidence beside it.
3. **Work in flight is not evidence.** A migration that lands next quarter belongs in the next packet. Putting it in this one invites a comparison with the candidate whose equivalent work has already shipped.

## Step 3. Build the evidence table

This is the artifact. One row per criterion at the next level, in the ladder's order, with these columns.

| Column | What goes in it |
| --- | --- |
| Criterion | The ladder's own words, quoted, with the version and date of the document |
| Evidence | One sentence. What you did, with the number and its denominator, and the period |
| Artifact | Something a reader can open without asking you, named precisely enough to find |
| Corroborator | A person, their relationship to the work, and the specific thing they saw |
| Period | Which review cycles this evidence spans |

**The hard rule: a row with no artifact and no name is not evidence.** It is an assertion. Assertions go on a separate list which is useful to you and never enters the packet. This rule is what turns the packet from prose into something a stranger can verify, and it is the rule people delete first.

**What counts as an artifact.** A design document or an RFC with your name on it. A decision record. A launched change with a date. A runbook or an operations document other people now follow. An incident review you wrote. A written review somebody else produced about your work. A document from another team that cites yours. Meeting notes recording a decision you drove.

**What does not count, however much it feels like it should.** A slide you made for this packet. A private chat thread. A dashboard filtered to your date range that nobody else can reproduce. A verbal thank-you. Your own summary of a meeting, unless the notes were shared at the time.

**Coverage targets.** At least one artifact per criterion. At least two distinct corroborators across the table, and preferably not all from your immediate team. Evidence spread across more than one cycle, per the operating-at-level rule.

## Step 4. State the contribution boundary before somebody asks

Calibration rooms are practised at detecting scope inflation, and they detect it with a small number of specific tells. Each one has a correction, and every correction reads as stronger rather than weaker, because a person who can describe the boundary of their own contribution is describing something they understand.

- **Tell: led, where the artifact shows one contributor among many.** The document has nine authors and one of them is you. Correction: name your slice. Owned the migration plan and the rollback design, while two other engineers implemented the backfill.
- **Tell: an impact number with no denominator.** Reduced latency by 300 milliseconds. Of what, measured how, on what share of traffic, over what period. Correction: the figure, the baseline, the denominator, the window, and who measured it.
- **Tell: a team outcome in the first person singular.** I grew adoption to forty per cent. Correction: name the team, then name what was yours.
- **Tell: ownership by proximity.** You were adjacent to something significant. Correction: delete the row. It is the row a stranger asks about.
- **Tell: a title claimed rather than a count given.** Ran incident response. Correction: was incident commander on two of the eleven incidents in the period.

## Step 5. Treat the self review as an input, not a second document

The self assessment most employers ask for is the raw material of the packet, and writing them as two separate exercises is how people end up with a warm narrative and an unevidenced table. Write the evidence table first, then draw the self review out of it.

Four rules make one useful.

- **Write for someone who was not there.** Expand every project name on first use. One sentence of context per item, stating what the thing is and who it is for, before what you did.
- **Name the counterfactual.** What would have happened had you not done it. This is where impact actually lives, and it is the sentence that survives being summarised by your manager.
- **Organise by the ladder's headings, not by chronology.** A chronological self review forces the reader to do the mapping, and under time pressure they will not.
- **Include one thing that went badly, and what changed as a result.** A self review with no failure in it is discounted by experienced readers, who read it as either low self-awareness or low scope. It also takes the framing of that failure away from whoever would otherwise raise it.

If there is a word cap, treat it as the design constraint rather than an inconvenience. Cut the weakest rows entirely rather than compressing every row into something unverifiable.

## Step 6. Choose the reviewer set for coverage

Peer reviewers are chosen for what they can corroborate, not for how much they like you. Map each proposed reviewer to the specific rows they witnessed, and check the map for two things.

If the next level's criteria name influence beyond your own team and every reviewer sits inside it, the reviewer set is itself evidence for the objection you are about to face. Add someone outside. And if every reviewer worked with you on the same project, the room sees one project reviewed five times.

When you ask, give each reviewer the rows, the artifact names and the dates. A vague request produces a vague review, and a vague review is a neutral data point at best. Ask early, because reviewers usually have an earlier deadline than you do.

## Step 7. Write the objection down before a stranger says it

Every case has one. Name yours, then put a short paragraph in the packet that answers it. The taxonomy below covers most cases, and for each the packet needs a specific thing rather than an argument.

- **Too recent at level.** The answer is dates: when the behaviour started, not when the title did, plus evidence spanning more than one cycle. If the time really is short, say so and let the artifact set carry it.
- **Scope confined to one team.** The answer is at least one artifact whose readers or users sit outside your team, and a corroborator from that team.
- **Strong delivery, no influence.** The answer is evidence of other people changing what they do because of you: a standard adopted, a document other teams cite, a named person whose work changed and how.
- **Influence with thin delivery.** The answer is one shipped thing with a date and a measured result. The advice-only case dies in every room without one.
- **A visible conflict.** A failed project, an incident, a strained relationship. Address it in one paragraph in your own words, with what changed afterwards. Unaddressed, it gets raised by somebody who was not there and your manager has to improvise.
- **Comparison.** Not as strong as the other candidate. The answer is never a superlative, it is criterion coverage, because the room is checking a standard and only then breaking ties.

## The decision: go, no go, or cannot tell

- **Go**, if every next-level criterion has at least one row with both an artifact and a corroborator, the evidence spans more than one cycle, at least one artifact has an audience outside your team, and the objection paragraph is written.
- **No go**, if two or more criteria have no artifact. Do not submit. Name the two, choose work in the coming cycle that produces the missing artifacts, and put that list in writing to your manager so it is a shared plan rather than a private resolution. A no go with a named list is a better cycle than a go that fails, because a failed case sets an anchor the room remembers.
- **Cannot tell**, if the ladder is vague, unwritten, or a single line per level. Then the packet is not the first task. Ask your manager, in writing, for the two specific things that would make the case obvious at the next calibration, and a date by which they will tell you. Treat a non-answer as its own finding: it means the decision is discretionary, and no document you write is the instrument of a discretionary decision.

For a multi-track framework, compute coverage per track rather than overall, since a spike on one track plus a hole on another can still clear the bar in a system built to allow spikes, and cannot in a system that is not.

## Worked example

A mid-size logistics company. A backend engineer going from the senior level to the level above it, under an internal ladder at version 3.2, dated January 2026. Everything here is invented.

**A criterion mapped well.** The ladder says: "Sets technical direction for work beyond their immediate team." The row reads: authored the scheduling service interface contract adopted by the routing and billing teams in the second and third quarters. Artifact: the interface RFC, plus the routing team's migration plan that cites it. Corroborator: the routing team's tech lead, who reviewed the RFC and ran the migration. Period: two cycles. Contribution boundary stated: wrote the contract and ran the review, did not implement either consuming service.

**A criterion mapped badly.** The ladder says: "Improves the effectiveness of the engineers around them." The row reads: mentored junior engineers. Artifact: none. Corroborator: none. Under the rule, that row is an assertion, so it comes out of the packet and goes on the list. The repair is available but takes a cycle: the onboarding guide he keeps meaning to write, and two named people whose ramp-up he can point at.

**The objection.** Scope confined to one team, and the reviewer set confirms it, since four of the five proposed reviewers report into the same manager he does. The interface RFC answers it partially. One artifact against a whole criterion is thin.

**Verdict: no go this cycle.** Two of seven criteria have no artifact. The named repair is the onboarding guide plus a written record of the two ramp-ups, and one piece of cross-team work already available, taking the on-call handbook rewrite that the platform team has been asking somebody to own. Both go in writing to his manager this week, with the sentence that he intends to go up next cycle and these are the two gaps. The evidence table survives as a live document, which means the next packet is an update rather than a memory exercise.

## Failure modes

**The narrative packet.** A well-written account of a good year with no criterion quoted anywhere. From the outside it reads as effort rather than evidence, and the room has to do the mapping, so it does not.

**Ladder ignorance.** The packet is written to a generic idea of seniority rather than to the employer's document. The tell is a section heading no one in the room recognises.

**Artifact drought.** Every row is a sentence about what you did, and nothing can be opened. Symptom: the only way to verify any of it is to ask you.

**First person plural.** Everything is we, so nothing is yours. The room cannot separate you from your team, and the safe reading is that the team did it.

**Impact without a denominator.** A large number floats free of what it is a share of. Somebody asks out of what, and the pause is the answer.

**The single project case.** One project doing all the work in every row. It cannot distinguish sustained level from one good assignment, and the room defaults to the assignment.

**Objection ambush.** The obvious objection goes unaddressed, so a stranger raises it, your manager improvises, and the improvisation becomes the summary of your case.

**Calibration blindness.** The packet argues you have had a good year, when the room is comparing candidates against a standard and then against each other. A packet with no criterion coverage has nothing to compare.

**The friendly reviewer set.** Five reviewers, all from one team, all from one project. It reads as one endorsement repeated, and it hands the scope objection to the room for free.

## What this skill does not do

- It cannot see your employer's ladder. Without that text it builds a packet against dimensions your committee does not use, and every criterion it quotes is quoting nothing.
- It cannot make an under-levelled case ready. Where the artifacts do not exist the honest output is a no go and a list, and better prose is not a substitute for the missing work.
- It has no view on internal politics, on how many promotions the budget allows, or on whether your manager will argue for you in the room, which is often the deciding factor and is invisible from here.
- It does not work where promotion is discretionary and unwritten. With no criteria and no committee, the document is not the instrument, and the useful move is a conversation with a date attached.
- It is not a substitute for asking your manager directly what is missing. That is free, it is faster, and it comes from someone who has been in the room.
- It stops at the decision. Compensation, banding, the negotiation after a yes, and anything to do with a job elsewhere are all outside it.
