---
name: deck-narrative-audit
description: Audits the argument of a presentation that already exists, working from exported slide text and speaker notes. Runs the headline-only read, converts topic headlines to assertion headlines, finds slides carrying two messages, locates the ask, checks the opening three slides, and reports whether the deck is built to be read or to be presented. This skill should be used when a deck is finished or nearly finished and someone needs to know whether it says anything, rather than whether it looks good.
---

# Deck narrative audit

## The claim this skill is built on

This skill does not generate a deck. It runs on a deck that already exists.

A deck's argument lives in its headlines and nowhere else. Everything else on a slide is evidence for a headline, or it is decoration. That is not an aesthetic position, it is a consequence of how decks get consumed: an audience sees each slide for between thirty seconds and three minutes, reads the largest text first, and either follows the thread or loses it there. A reader skimming a forwarded deck does the same thing faster.

So the usual review fails structurally. Reviewing a deck slide by slide, improving each one, cannot reveal a missing argument, because the argument is a property of the sequence rather than of any slide. Forty individually good slides in the wrong order remain forty slides in the wrong order, and every hour spent on typography before the sequence is settled is an hour that makes the rewrite more expensive, because nobody wants to throw away work that looks finished.

Run the structural pass first. It is cheap, and it is the only pass whose findings invalidate the others.

## The headline-only read

The diagnostic, in full:

1. Extract the headline of every slide, in order, into a plain list.
2. Read only that list. Do not look at the slides.
3. Classify what you have.

Three outcomes:

**An argument.** Each line follows from, qualifies, or advances the one before it, and the last line is a conclusion or an ask. Somebody who read only this list would know what you think and roughly why. The deck has a narrative.

**Disconnected assertions.** Each line is a sentence stating something true, but the lines do not connect. This is the state a deck lands in after someone converts topic headlines into sentences mechanically, and it feels like an improvement while fixing nothing. The fix is not more rewriting of individual headlines, it is deciding what the argument is.

**A table of contents.** Background. Market overview. Our approach. Case study. Timeline. Next steps. The deck has a structure, which is not the same thing as an argument, and no amount of slide polish converts one into the other.

Report the extracted list verbatim in the audit. It is the single most persuasive artefact a deck review can produce, because the author usually has never seen their own deck in that form and the failure is self-evident once they do.

## Assertion-evidence

The structure the headline read is testing for is assertion-evidence, developed and taught by Michael Alley in the context of technical and scientific presentations. The rule: the slide headline is a complete sentence stating the slide's claim, and the body of the slide is the evidence for that claim, preferably visual. It replaces the conventional pattern of a topic phrase headline followed by bullets, where the headline names a subject and the bullets are the content.

Two things follow from it that make it worth the effort. The headline forces the author to decide what the slide is for, which is the work most decks skip. And the evidence has to actually support the claim, which surfaces slides where a confident assertion sits above a chart that does not show it.

**Be honest about the evidence base.** Assertion-evidence has been studied, mostly in engineering and science education settings, with results favouring it on comprehension and retention against topic-and-bullet slides. The research is real and it is modest: a handful of controlled studies rather than a large replicated literature. Present it as a well-reasoned structure with supporting evidence, not as a settled empirical finding, and it will survive contact with a sceptical audience better.

Two practical constraints. An assertion headline should fit in one or two lines at headline size, which in practice means roughly ten to sixteen words. Longer, and it stops being read as a headline and starts being read as body text, which defeats the purpose. And an assertion headline can still fail by stating a topic in sentence form: "This section covers our pricing approach" is a sentence and it is not an assertion. The test is whether someone could disagree with the headline. If nobody could, it is not a claim.

## Answer first, and the case for building

Barbara Minto's Pyramid Principle, developed for consulting work in the 1970s, gives the shape most decks should use. The introduction establishes the situation, the complication that makes the situation a problem, and the question that follows, and the body answers it. Beneath the answer sit the supporting arguments, grouped, each group supported in turn.

The part most decks get wrong is where the answer goes. The pyramid puts it at the top: state the recommendation, then support it. Most decks build to it, holding the conclusion for slide 28 as though the audience were reading a mystery. That fails for a specific reason: senior audiences interrupt, meetings overrun, and the person with authority frequently leaves after twenty minutes. A deck that builds to its conclusion is a deck that risks never delivering it.

**The decision rule:**

- The audience can decide, and time is limited or uncertain. **Answer first.** State the recommendation in the first three slides, then support it.
- The recommendation reverses a decision this audience made, or the news is bad enough that stating it first triggers rejection before the reasoning is heard. **Build**, but declare the destination type up front: "this is a recommendation to stop the programme, and here is how we got there". The audience needs to know what kind of thing is coming, even when they do not yet get the conclusion.
- The deck is a teaching artefact or a conference talk where discovery is the experience. **Build.** The headline read should still form a story rather than a topic list.
- **You cannot tell which of these it is.** Answer first. A deck whose audience you cannot characterise is a deck that will be forwarded, and a forwarded deck has no presenter to manage the reveal.

## One message per slide

State what the slide is for in one sentence. If that sentence contains "and", it is two slides.

The test is cruder than it looks and it works. "This slide shows that support volume grew and that the team did not" is two findings, and the audience will take away whichever one you happen to emphasise out loud, which means the other one was decoration. Split it, or decide which one matters and cut the other to the appendix.

The second half of the test: if you cannot write the sentence at all, the slide has no message. That is fine for a section divider and it is fine for an appendix exhibit. It is not fine for a slide in the main line, and slides that fail this test are usually the ones inherited from a previous deck.

## The read deck and the presented deck

These are different artefacts. Treating them as one is the most common structural error in professional decks, and it is invisible until the deck is forwarded.

- **Density.** A presented slide carries the assertion plus one exhibit, because the audience is listening. A read slide carries the assertion plus enough body to stand alone, because nobody is talking.
- **Notes.** In a presented deck the speaker notes carry the argument, the caveats, and the transitions. In a read deck the notes are not read. Most readers never open the notes pane, and a PDF export drops them entirely. Any argument that lives only in the notes is lost the moment the deck moves.
- **Headlines.** A presented deck can survive a fragment headline, because the presenter supplies the verb. A read deck cannot. Nothing else supplies it.
- **What happens next.** Almost every presented deck gets forwarded. It gets attached to an email to somebody who was not in the room, and that person makes a judgement from the slides alone. The deck you built for a room is then doing a job it was not built for, and the person forwarding it does not know that.

The practical resolution is one of two things. Either build the read version and present from it, accepting slightly denser slides in exchange for a deck that survives forwarding, or build the presented version and export a separate read version with the notes folded in. What does not work is building the presented version and hoping it never leaves the room.

## The opening three slides

The first three slides have to establish three things, and they have roughly ninety seconds to do it: what this is about and why now, what decision or action is on the table, and what you are recommending.

Four openings waste them:

- **The agenda slide.** It tells the audience the order of a document whose length they can already see, and it consumes the single most valuable slide in the deck. Put the agenda in the notes, or state it in one sentence out loud.
- **The company history slide.** It assumes relevance rather than earning it. Nobody has yet been given a reason to care when it appears.
- **The team slide, placed early.** Credentials answer a question the audience has not asked yet. Move it after the ask, or into the appendix, unless the audience is specifically deciding about the team, which is the one case where it belongs at the front.
- **A definition of a term the audience already knows.** It signals that the author did not find out who was in the room, which is a worse first impression than the wasted minute.

## The ask

Every deck asking for something must say what. Specifically, one findable sentence naming: what is being asked for, from whom, by when, and what happens if the answer is no.

The failure is precise and it is common. The deck presents an analysis, ends on a slide headed "Next steps" listing activities rather than decisions, and the meeting closes with everyone agreeing that it was interesting. Nobody has said no. Nobody has said yes. The work gets rescheduled, and the deck gets rebuilt for the next meeting.

The diagnostic: find the sentence. If you cannot point at one, quote the closest thing and report that the deck has no ask. Do not soften this. It is usually the most valuable finding in the audit, and it is the one the author is most likely to have talked themselves out of.

## Slide count arithmetic

A slide carrying one assertion and one exhibit takes about ninety seconds to land at a normal speaking pace. Any slide that draws a question takes two to three minutes. Audiences ask questions.

So for a thirty minute slot, plan for roughly twenty minutes of your own material, because the rest goes to discussion whether you planned for it or not. That is ten to fourteen slides. Most people build thirty to forty for the same slot, discover the problem at minute twenty-two, and rush the last third, which is where the recommendation lives.

The cut material goes to the appendix, not the bin. That distinction matters more than the count. An appendix should hold the detailed exhibit behind every number quoted in the main line, the analysis somebody will challenge, and the version of the argument you decided not to lead with. A deck with twelve main slides and twenty-five appendix slides is well built. The appendix is where you win the question you could not have anticipated, and a presenter who can jump straight to the supporting table gains more credibility in ten seconds than the whole deck earned in twenty minutes.

For a deck that is read rather than presented, budget differently: a reader moves at roughly twenty to forty seconds per slide, so a forty slide read deck is a twenty minute homework assignment. Say so in the covering email, or cut it.

## The audit procedure

Run in this order. The order matters because step 0 can end the audit.

0. **Establish what the deck is for and who reads it.** If the deck itself does not tell you, and no other context does, stop here and report it. **A deck whose purpose is not apparent to a careful reader on one pass will not be apparent to an audience seeing it once, at speed, while checking email.** That is the finding. Everything else is premature.
1. **Extract the headlines** and run the headline-only read. Report the list and the classification.
2. **Locate the ask.** Quote it, or report its absence.
3. **Check the opening three slides** against the three things they must establish.
4. **Run the one sentence test** on every slide in the main line. List the slides containing "and".
5. **Determine read or presented**, and check the deck against the one it actually is.
6. **Count slides against the slot**, and identify what moves to the appendix.
7. **Report**: the current headline sequence, a proposed headline sequence, the slides to cut or move, and the missing sentence if there is one.

## Worked example

A thirty-four slide deck proposing that a company consolidate customer support onto one tool instead of three. Thirty minute slot with the operations leadership. The deck and its contents are invented for this example.

**The headline read, as extracted:** Background. Our current support stack. Ticket volume trends. Cost analysis. Vendor comparison. Migration considerations. Risks and mitigations. Next steps.

A table of contents. Nothing in that list tells you what the author thinks. Note that slide 1 is a background slide and slide 2 is a stack overview, so the first two slides are both scene setting and the recommendation appears nowhere in the sequence.

**The ask:** none. The final slide is headed "Next steps" and lists four activities, one of which is "align with finance". No decision is named, no date, no owner.

**Proposed headline sequence:** Support answers forty per cent more tickets than a year ago with the same headcount. Three tools split the queue, so nobody can see the whole backlog. Two of the three contracts renew in March, which is why this is a decision for this quarter, not next. Consolidating on one tool saves a stated amount a year, and most of that is licence rather than headcount. One vendor meets the requirements the other two fail on. Migration is six weeks of one engineer, and the risk we cannot fully mitigate is the historical ticket archive. We are asking for approval to sign by the thirtieth so we can migrate before the March renewals.

Seven headlines. Read alone, they make an argument, and the last one is an ask with a date.

**Verdict:** restructure before any design work. Cut to twelve main slides, move twenty-two to the appendix including the full vendor comparison matrix and the cost workings, add the recommendation to slide three, and repeat the ask on the final slide. The current deck is not a bad deck that needs polish, it is a document with no argument in it, and one hour of restructuring is worth more than a week of formatting.

## Failure modes

**The table of contents deck.** Every headline names a topic. Diagnosable in two minutes, invisible if you review slide by slide.

**The assertion set with no spine.** Every headline is a sentence and none of them connect. Usually the result of a previous review that said "use full sentence headlines" and stopped there.

**The and-slide.** One slide carrying two findings. The audience keeps whichever one you emphasised aloud, and the other was wasted work.

**The buried ask.** The deck presents an analysis and closes on activities. Everyone leaves agreeing it was interesting. Nothing is decided, and the same deck comes back in three weeks.

**The agenda opener.** The most valuable slide in the deck spent listing the deck.

**The forwarded presented deck.** The argument was in the speaker notes, the deck was exported to PDF and emailed, and the recipient sees fragment headlines above charts with no explanation. They conclude the work is thin.

**The deleted appendix.** Cut material was cut rather than moved. The first hard question lands on something the author analysed thoroughly two weeks ago and can no longer show.

**The polish-first review.** Design work completed on a deck whose headline read fails. The restructure is now expensive in a way that has nothing to do with the argument, and somebody will defend the current order to protect the formatting.

**The sentence that states a topic.** "This section covers our pricing." A full sentence, a full stop, no claim. Passes a lazy check and fails the headline read.

## What this skill does not do

- It does not create, edit, or export slide files. It reads exported text and reports. Producing the deck is a different skill and Anthropic publishes one.
- It cannot see layout, images, colour, or the rendered slide, so it cannot tell you whether a slide is legible, only whether it is coherent.
- It does not verify a single number. A deck with a clean narrative built on a wrong figure passes this audit.
- It cannot know the room, which means its ordering advice is a default rather than a recommendation. Politics beats structure and the file cannot see the politics.
- It cannot judge delivery, timing, or whether the presenter can actually hold the room, and those decide more meetings than structure does.
