---
name: decision-memo-build
description: Writes the one-page decision memo that gets forwarded instead of a deck, working from a decision, an audience and an evidence pack. Puts the recommendation in the first sixty words, fills nine slots in a fixed order with word budgets that add up to a single page, separates what you recommend from what you need the reader to do, sorts material between the body and an unbounded appendix, and enforces that uncertainty appears as a threshold rather than as an adverb. Carries a hedge detection rule and three shapes for the case where a recommendation is not yet possible. It produces a memo and never produces slides. This skill should be used when a decision has to be made by people who will not sit through a presentation, or when a finished deck needs a document that can stand without its author.
---

# Decision memo build

## The claim this skill is built on

This produces a one-page memo. It does not produce slides, and the deck, if one exists, becomes an attachment to it.

That inversion is the whole idea. The deck is built for a room, and the room is not where most decisions get made now. The memo is what gets forwarded, read on a phone in a scroll with no attachments opened, quoted back in a message, and pasted into a thread by somebody explaining to a fourth person why this is happening. A deck cannot do any of that. It cannot be skim read in ninety seconds, it cannot be quoted, and without a presenter its headlines are fragments over pictures.

The obvious approach is to write the memo after the deck, as a summary of it. That fails in a specific way rather than a vague one: a summary inherits the deck's order, which is context, then analysis, then conclusion, so the recommendation lands in the last paragraph of a document most people stop reading at the fold. It also inherits the deck's caution, because the sentences were written to be spoken by somebody who could add the missing conviction out loud.

The second obvious approach, writing a clear one-pager from scratch, fails less obviously. Clarity of sentences is not the problem. The problems are order, omission and hedging, and all three are structural. A memo can be beautifully written, entirely honest, and still leave a group unable to decide, which is the failure this file is about.

## The slot order, with word budgets

One page means about 550 to 700 words of body text at a readable size on either A4 or Letter with normal margins. The budgets below add up to that. If you go over, the fix is never smaller type or narrower margins, it is the body and appendix rule in the next section.

1. **Title line.** Twelve words or fewer, naming the decision rather than the topic. "Approve consolidating support onto one tool before the March renewals", not "Support tooling review". A reader should be able to tell from the subject line alone whether they are the decider.

2. **Recommendation. 25 to 60 words, first, with no preamble.** One option, one number, one date. No sentence before it, not even a line saying what the memo covers. This is the slot that gets read, and everything below it exists to support it.

3. **Decision requested. One or two sentences.** Who decides, what they are deciding, the deadline, and the default that applies if nobody decides by then. The default is the sentence people skip and it is the most valuable one on the page: there is always a default, it is usually that the current arrangement continues, and naming it converts deferral from a free action into a choice somebody made. This slot is separate from the recommendation because what you think and what you need from the reader are different things, and memos routinely contain one without the other.

4. **Why now. 40 to 80 words.** The forcing function: a renewal date, a contract, a seasonal peak, a dependency that expires. If there is no forcing function, write that there is none. A memo that manufactures urgency gets caught, and a memo with a real deadline stated plainly is very hard to defer.

5. **The case. Three to five claims, 150 to 250 words.** One short paragraph or one bullet each. Every claim carries a number and the source of that number, in the body. These are the same claims as the main line of a deck's spine, so build them once and use them in both places.

6. **What we considered and rejected. 40 to 80 words.** Two or three options, each with the single reason it lost. This is the slot most often missing and the one that most reliably shortens the meeting, because the first instinct of any competent reader is to ask whether you thought about the obvious alternative.

7. **Risks, and what would change our mind. 40 to 80 words.** The two largest risks with their mitigations, then one observable with a threshold that would reverse the recommendation. That sentence is what separates a memo from advocacy.

8. **Cost and resourcing. One or two lines.** Money, people, time. If it is uncertain, give a range and the basis of the range.

9. **Where the workings are. One line.** Name the appendix, the model, the deck, and where they live.

**Two ordering rules that change the output.** Write the recommendation before anything else, because it bounds every slot below it and because a recommendation assembled at the end inherits the hedging of whatever came before. Write the rejected options before the case, because the strongest rejected option defines what the case has to beat, and a case written without that constraint argues against nobody.

## Body or appendix

The body is capped at one page. The appendix has no limit at all. That asymmetry is the mechanism, and the three questions below decide where anything goes:

- **Does the reader need this to say yes or no?** Body.
- **Would a sceptical reader ask for it before saying yes?** Appendix, referenced by name in the body so they know it exists.
- **Is it here because it took work?** Appendix, and sometimes nowhere.

Concretely to the appendix: methodology, the full option comparison, vendor quotes, the model and its assumptions, sensitivity analysis, the transcript, and the deck. Concretely in the body: every number the recommendation depends on, and a named source for each of those numbers.

That last rule has a reason that is easy to miss. A number whose provenance lives only in the appendix reads as unsourced to the majority of readers, who will never open the appendix. "Support costs rose by a fifth last year, from the finance system's cost centre report" is eleven words longer than the version without the source and it is the difference between a claim and an assertion.

## Readable without its author present

The standard: someone who was in none of the meetings should be able to answer three questions from the page in under two minutes. What is being decided. What do you recommend. What would have to be true for me to say yes.

The mechanical version of that standard, which is what you can actually check line by line:

- **No reference whose antecedent is in another document.** "As discussed", "per the deck", "the approach we agreed" are all instructions to a reader who cannot follow them.
- **No unexpanded internal shorthand**, project codename or team name on first use. A codename is a private joke to everyone outside the team, including the person who has to approve it.
- **Every number carries a unit, a period and a source.** A percentage with no denominator is not a number.
- **No sentence whose meaning depends on knowing which meeting produced it.**
- **No obviously, clearly, or as we all know.** Each is an instruction to the reader not to check, and a reader who notices will check that sentence first.

Then the forwarding assumption: write as though the memo will be read on a phone, in a scroll, with nothing opened. That is not a worst case, it is the normal case, and it is the reason the recommendation has to survive being the only thing on the first screen.

## The hedging failure, and how to detect it

The named failure is the balanced memo. Every claim carries a qualifier, every option is presented with its advantages and disadvantages, and the recommendation slot contains a sentence of the form "either option could work, depending on priorities". From the outside it looks careful and it reads as rigorous. The symptom is unmistakable: the meeting opens with somebody asking the author what they actually think, which is the question the memo existed to answer.

Three detection rules, all of them countable:

- **Hedge terms in the recommendation slot.** May, could, potentially, we believe, it seems, arguably, in some cases, relatively, somewhat, fairly. The target in that slot is zero. More than one and you have not made a recommendation.
- **Options named in the recommendation slot.** Exactly one. If it names two, it is a summary of the options and belongs in slot 6.
- **The reversal test, applied to every sentence.** If the opposite of a sentence is a position nobody would ever hold, the sentence carries no information. "We should pay close attention to data quality" reverses to a sentence no one would sign, so the original tells the reader nothing and can be deleted without loss. Run this on the case slot in particular, where filler survives longest because it sounds responsible.

Across the whole memo, if more than about one sentence in five carries a hedge term, the document reads as an author who does not want to be quoted, and it will be read that way whatever it says.

**The cure is not deleting uncertainty.** It is putting uncertainty where it does work. Uncertainty expressed as an adverb is noise. Uncertainty expressed as a threshold is actionable. "We are fairly confident the migration will hold" is the first kind. "We recommend the migration on the basis of the trial. If the December close does not complete inside the reporting window, we stop and buy hardware instead" is the second, and it is a more uncertain statement carrying much more information. Where you catch yourself writing relatively, somewhat, fairly or quite, replace the adverb with a range and the basis of the range, or delete it.

## When you cannot recommend yet

There are three legitimate shapes, and one thing that is never legitimate, which is a decision memo with no recommendation slot at all.

**Shape A, the investigation memo.** You cannot recommend because you are missing evidence you could get. The recommendation slot becomes a recommendation to spend something specific on answering something specific: name the single question, the method that answers it, the cost of answering, the date, and the decision that will follow. That is still a recommendation, because approving two weeks of a named person's time is a decision somebody has to make. One caution with a number in it: if answering the question costs more than the difference between the options is worth, do not investigate. Pick, and say that you picked because the investigation was not worth its price.

**Shape B, the options memo.** You cannot recommend because the choice turns on a value judgement that is genuinely the reader's, such as risk appetite or a strategic preference you do not own. An options memo must then do three things a hedged memo does not. Name the criterion the choice turns on. State which option wins under each value of that criterion. Then say which way you would lean and why, while making clear the choice is theirs. "The choice turns on whether we will accept a two week freeze on releases. If we will, take the migration. If we will not, buy hardware and revisit in the summer. We would accept the freeze, because the freeze is visible and the alternative failure is not." Note that this is not hedging: the reader is given a rule, not a shrug.

**Shape C, the briefing.** There is no decision. Label it in the title line as a briefing with no decision requested, so nobody schedules a decision meeting for it and nobody waits for an ask that is not coming.

**The rule for telling which one you are in, including the branch where you cannot tell.** Write down the single question whose answer would make the recommendation obvious. If work can answer that question, you are in Shape A. If the question is a version of "what do we value more", you are in Shape B. If you cannot state the question at all, you are in Shape C, and calling it a briefing is the honest move rather than a retreat. If you can state the question but cannot tell whether work or judgement answers it, treat it as Shape A and let the first hour of the investigation reclassify it, because an investigation that discovers the question was a values question has cost an hour and has produced an options memo.

## The build order

1. Write the decision sentence: what, who decides, by when, and the default if they do not.
2. Pick the shape: recommend, investigate, options, or briefing.
3. Write the recommendation slot. One option, one number, one date, 25 to 60 words.
4. List the rejected options with one reason each.
5. Write the case as three to five claims, each with a number and a source, each of which has to beat the strongest rejected option on the criterion that decides.
6. Write the reversal condition as an observable with a threshold.
7. Add why now, cost, and the pointer to the workings.
8. Cut to one page using the body and appendix rule. The first cuts are background and methodology. The rejected options slot and the sources are never the cut.
9. Run the without-the-author checklist and the three hedge counts.
10. Attach the deck as an appendix item, and do not restructure the memo to match it.

## Worked example

A retail analytics company is deciding whether to move a nightly batch job onto a managed service. Everything here is invented.

**The draft that existed.** Nine hundred words. Two paragraphs of history, a description of the current architecture, a comparison of three options with advantages and disadvantages for each, and a closing paragraph beginning "on balance, either the managed service or additional hardware could address the issue, depending on our appetite for migration risk". Seven hedge terms in that closing paragraph. No date anywhere. Cost described as significant.

**The rebuild.**

- Title. Approve moving the nightly batch to a managed service before the year end close.
- Recommendation, 41 words. Move the nightly batch to the managed service, at a stated annual cost, with migration starting next month and completing six weeks later. This removes the batch window problem for at least three years rather than deferring it by two quarters.
- Decision requested. The engineering director decides, by the thirtieth. If there is no decision by then, we run the current job through the year end close and revisit in February, which means the window breaches during the busiest reporting period of the year.
- Why now. The batch currently finishes forty minutes before the reporting deadline, and that margin has shortened by about four minutes each quarter for the last five quarters, from the job scheduler logs. On that trend the margin is gone within two quarters, and the year end close is the point where it matters most.
- The case. Four claims, each with a source: the shrinking margin from the scheduler logs, the growth in row counts from the warehouse statistics, the two failed runs last quarter and what they cost in analyst time from the incident record, and the quoted service cost from the vendor proposal.
- Rejected. Rewrite the job in house: about three engineer months and it does not remove the window problem, only postpones it. Buy more hardware: buys roughly two quarters at a comparable annual cost with no ceiling raised. Do nothing: named, with its consequence, so that it is a choice rather than an absence.
- Risks and reversal. Two risks named with mitigations, then the reversal condition: if the trial run does not complete the December close inside the reporting window, we stop the migration and buy hardware instead.
- Cost. A stated range with its basis, which is the vendor quote at current volumes plus a fifth for growth.
- Workings. Appendix holds the full option comparison, the vendor proposal, the scheduler log extracts, and the deck.

**Verdict.** Five hundred and eighty words, one page, one option recommended, zero hedge terms in the recommendation slot, three rejected options each with one reason, a default named for inaction, and a reversal condition with a threshold. The deck that started this is now attachment four. What changed the meeting was not the writing quality, it was slot 3: naming what happens if nobody decides made deferral a visible choice rather than the easiest exit.

## Failure modes

**The deck summary.** The memo's headings match the deck's section titles and the recommendation is in the final paragraph. Recognisable from the table of contents alone.

**The balanced memo.** Every option gets a fair hearing and none gets chosen. Symptom: the meeting opens with "so what do you actually think", and the real recommendation is delivered verbally, unrecorded, by whoever is most senior in the room.

**The context ramp.** The first third is history. Symptom: the reader's first comment lands on paragraph four, and it is a question the memo answers on page two.

**The unsourced number.** Somebody asks where a figure came from and the answer is "the analysis". The number was probably right, and it now costs more credibility than it bought.

**The missing rejected options.** The meeting is spent on an alternative the author ruled out weeks ago and can dismantle perfectly out loud, having not written down a single line of it.

**The memo that needs its author.** It gets forwarded, and the recipient replies asking for a call. That reply is the failure, and it is usually caused by three or four phrases pointing at documents the reader does not have.

**The activity ask.** The decision slot names something the team will do rather than something the reader must approve. Nobody can say afterwards what was agreed, and the same memo returns with a new date on it.

**The adverb hedge.** Fairly low risk, relatively confident, broadly on track. Symptom: ask two readers what the risk actually is and they give different numbers, both of which they believe came from the memo.

**The two-page one-pager.** Font size dropped and margins narrowed to make a page. Symptom: the layout looks compressed, and the material that should have gone to the appendix is still in the body, doing nothing.

**The buried reversal.** The condition that would change the recommendation sits in the appendix, so the decision reads as permanent and nobody sets up the check that would trigger it.

## What this skill does not do

- It does not produce slides, a deck, or any presentation file. Where a deck exists it becomes an appendix item, and the memo leads.
- It does not verify anything. It requires that each number in the body names a source, and it cannot tell whether the source supports the sentence.
- It does not decide. If the recommendation is wrong, this format makes it more persuasive, which is a real risk and the reason the reversal condition is a required slot rather than a nicety.
- It cannot see the room or the politics, so it cannot tell you that the option you are recommending is owned by somebody who will block it, which is the most common reason a well-built memo fails.
- It does not fit every organisation. A place that runs on longer narratives read in silence, or on a two-line approval in a message, needs a different artefact, and the local convention beats this one.
