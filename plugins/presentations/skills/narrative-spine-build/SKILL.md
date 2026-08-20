---
name: narrative-spine-build
description: Builds the argument of a presentation as a text spine before any slide exists, working from a brief, an audience and an evidence pack. Writes the ask first, derives the claims a decider must accept, orders them by the objection they answer, attaches evidence with a three-valued status, then tests the sequence with a read-aloud connective pass and cuts it to a slot budget in a fixed order. Produces one line per slide, each line stating the claim that slide must earn, and never produces slides. This skill should be used when somebody has been asked for a deck and has not yet opened the deck tool, or when a slot has been shortened and the argument has to survive the cut.
---

# Narrative spine build

## The claim this skill is built on

The artifact this produces is a spine document: a numbered list where each line is one slide, and the line is the claim that slide has to earn, plus the evidence that claim needs. It is a page of text. It is not slides, it does not become slides by itself, and the deck tool stays closed until the last step.

The obvious approach is to open the deck tool and start making slides, on the reasoning that you think better with something on the screen. It fails for a mechanical reason rather than a moral one. Once a slide exists it has a cost, and the cost is visible: somebody typed the headline, found the chart, aligned the boxes. Structure then becomes an emergent property of what happened to get built, and every subsequent structural decision is argued against sunk work. Deck reviews stall at exactly this point, because the person defending slide 14 is not defending an argument, they are defending an afternoon.

The second obvious approach is to write an outline first, and it fails differently. An outline is a list of topics: Background, Market, Our proposal, Costs, Risks, Next steps. It is a table of contents in a text editor, and it will produce a deck with exactly those headlines. The [deck narrative audit](/skills/deck-narrative-audit/) covers the reviewing side of this in detail, including the finding that a slide headline stating the slide's conclusion beats a headline naming the slide's topic, attributed there to Michael Alley's assertion-evidence work. This file takes that as settled and builds forward from it. If the finished headline has to be a claim, then the spine line has to be a claim, written in the finished form, at the point where changing it costs nothing. The spine is therefore a draft of the headline set, not notes towards one.

## What a spine line is

Four fields. Keep them on one line each or in a four column table, whichever your editor makes cheap. The format works on any machine and in any text editor, which matters, because a spine that lives in a proprietary format has already started becoming a deck.

```
S04  CLAIM     Two of the three contracts renew in March, so this is a decision for this quarter.
     EVIDENCE  Renewal dates from the contract register, one table, three rows.
     STATUS    have
     ROLE      turn
```

**CLAIM.** A full sentence with a subject, a verb and, wherever the claim is quantitative, a number. The test is contestability: if nobody in the room could disagree with the sentence, it is not a claim. "This section covers pricing" is a sentence and not a claim. "Our pricing is the reason we lose the mid market" is a claim.

**EVIDENCE.** What the slide will actually show. Name the artefact, not the topic: one chart of monthly active accounts by tier, a three row table, a quote from a support transcript, a photograph of the packaging. If the evidence field says "data on this", the field is empty.

**STATUS.** Exactly one of three values, and the third one is the point of the format:

- **have.** The artefact exists, you have seen it, and you know what it says.
- **need.** It can be produced with work you are willing to do before the deck ships.
- **cannot get.** It does not exist, cannot be produced in time, or the analysis will not support it.

A line marked cannot get must be resolved before the deck is built, and there are only three resolutions. Weaken the claim until the evidence you do have supports it. Cut the line and check whether the argument still stands without it. Or change the ask so that this claim is no longer load bearing. Doing none of these and building the slide anyway is the most expensive failure in deck work, because it is discovered in the room, by the one person who reads carefully.

**ROLE.** One of setup, turn, support, ask, or appendix. Roles are what the cut order operates on, so a line with no role cannot be cut correctly.

## Write the ask first, and the setup last

Two ordering rules, and they are the two that change the output most.

**The ask line is written before spine line one.** It sits at the top of the document as the target, and it names four things: what is being asked for, from whom, by when, and what happens if the answer is no. Everything below it exists to make that sentence acceptable. A spine written forwards from context arrives wherever the context leads, which is usually a summary. A spine written backwards from the ask has a test for every line: does the decider need this to say yes.

If you cannot write the ask line, stop and check what you are building. A deck with no ask is an informational deck, which is a legitimate artefact, and its spine has a different target: the one sentence you want the audience to repeat to somebody else afterwards. Write that sentence at the top instead and carry on. What you must not do is leave the target blank and start listing slides.

**The setup lines are written last, and there are at most two.** Context expands to fill available space, because every fact about the background feels necessary to the person who learned it. Written last, setup is written to a known requirement: you can see which claims depend on context the audience does not have, and you write only that. Two is a cap rather than a target. Many decks need one. Almost none need five, and a spine that reaches line six before making a claim has already lost the room.

## The build procedure

Run in this order. The order is the method.

1. **Write the ask line.** Four elements. If you cannot, switch to the informational target above.
2. **Write the budget.** Convert the slot into a line count before writing any lines. For a presented deck, assume roughly a third of the slot goes to discussion whether you plan for it or not, and that a slide carrying one claim and one exhibit takes about ninety seconds. A thirty minute slot is therefore about twenty minutes of your material, which is ten to fourteen lines. A twenty minute slot is six to nine. Write the number at the top of the document. A budget set after the lines exist is not a budget, it is a regret.
3. **List the preconditions.** What must be true for the decider to say yes. There are usually three to five. These become the main line claims, and this is the step that separates a spine from an outline, because preconditions are things somebody can refuse to accept, and topics are not.
4. **Order the preconditions by objection, not by logic.** The objection this specific audience raises first goes first. Where two compete, the objection that kills the ask outright goes before the objection that resizes it. "Why not just fix the existing system" comes before "why two people rather than one", because an unanswered blocking objection makes the sizing conversation pointless.
5. **Attach evidence and set status.** Resolve every cannot get now.
6. **Add support lines** beneath any precondition that a single exhibit will not carry. Support lines are the only place parallel siblings are legal.
7. **Write the setup lines.** At most two.
8. **Run the read-aloud pass.**
9. **Run so what and how do you know.**
10. **Cut to budget** using the cut order.
11. **Freeze the spine**, then open the deck tool.

## The read-aloud pass: the connective test

Read the CLAIM fields only, aloud, in order, at speaking pace, with nothing between them. Do not read the evidence fields. Do not read it silently, because silent reading lets you supply the connective tissue from your own memory of what you meant, and reading aloud at speaking pace does not: you hear the gap before you can patch it.

Then run the joint test. Between every adjacent pair of claims, insert exactly one of these five words or phrases and check the pair is still true:

- **and so**, for consequence
- **because**, for support running backwards
- **but**, for a genuine turn
- **which means**, for interpretation
- **for example**, for an instance of the line above

If the only thing that fits is **also**, **next**, **separately**, **in addition** or **meanwhile**, that joint is a list joint, not an argument joint. Record it.

List joints are legal in exactly one position: between parallel siblings inside a support group, where two or three exhibits support the same parent claim. They are a defect anywhere on the main line. The quick version of the same test: swap the two lines. If the sequence reads exactly as well backwards, they are siblings. If it reads worse, they are a chain, which is what you want.

The count that matters: on a spine of ten to fourteen lines, more than two list joints on the main line means you have written a topic list in sentence form. Two consecutive list joints on the main line means an entire section is a list, and that section is usually the one inherited from the last deck.

## So what, and how do you know

Two questions, run in opposite directions, on every line.

**So what** runs down the spine. Ask it of each claim and see where the answer lives.

- The answer is the next line. The spine is chained. Move on.
- The answer is a line further down. The order is wrong. Move one of them.
- The answer is nowhere in the spine. The line is context nobody asked for, or the deck is missing its own consequence. Demote or add.

The terminal case is the one to check first. Ask so what of the final claim. The answer must be the ask. If the answer is "so we should probably look at this", there is no ask, whatever the last slide is titled.

**How do you know** runs sideways into the evidence field. Ask it of each claim, and the answer must be that line's own evidence. If the answer is "slide 11 shows it", then the claim and its evidence have been separated across two slides, and the fix is to merge them or to move the exhibit. A claim on one slide supported by a chart three slides later is read by an audience as an unsupported claim followed later by an unexplained chart.

## The budget, and the cut order

Spines run long. The first draft is routinely twice the budget, and this is fine as long as the cut happens here rather than in the deck. Cut in this order, not by trimming everything evenly, because even trimming removes the specifics from every claim and leaves you with the same number of weaker slides.

1. **Setup beyond two lines.** Every time. Do not negotiate with yourself about it.
2. **Parallel siblings, down to two per parent claim.** Three examples of the same point move a sceptic barely further than two and cost a third more of your slot. Keep the two a sceptic would attack. Cut the two that are there because they were easy to produce.
3. **Merge claims that differ only in degree.** "Support volume grew" and "support volume grew fastest in the enterprise tier" are one claim with a qualifier, which is one line.
4. **Demote to appendix, do not delete.** Any line whose evidence is strong but whose claim is not load bearing for the ask goes to the appendix pack, in full. The appendix is where you win the question you could not have anticipated, and material that was deleted rather than moved is the thing you will wish you had at minute twenty-four.
5. **Only now cut a support a sceptic would attack.** If you reach this step you are out of room for the ask you have written. The honest move is to narrow the ask so it fits the slot, and say in one line that the larger version exists and is a later conversation.

**The decision rule for a line you cannot classify.** If you cannot tell whether a line is setup or support, remove it and reread the ask. If removing it changes what you are asking for, or makes the ask unanswerable, it is support and it stays. If removing it only makes the ask feel less justified to you personally, it is setup and it goes. If you still cannot tell, mark the line **unresolved** and leave it in. Then apply the cap: at most two unresolved lines survive into the deck. Three or more means the argument itself is not settled, and building slides will not settle it. Go back to step 3 and rewrite the preconditions.

## Worked example

A platform team at a mid-size software company wants to fund a second on-call rotation. Thirty minute slot with an executive group. Everything here is invented.

**Ask line.** We are asking this group to approve two additional platform engineers in the next hiring round, so a second on-call rotation starts before the winter peak. If there is no decision by the end of the month, we hold the current rotation through peak and accept the attrition risk.

**Budget.** Thirty minutes, so about twenty of our material, so ten to fourteen lines.

**First draft.** Nineteen lines. Five of them setup, including a slide on the history of the platform team and a definition of on-call.

**Preconditions, ordered by objection.** The first thing this group will say is "fix the alerts instead", so that objection is answered second, immediately after the strain is established. Sizing comes after that, because arguing about two versus four before anyone accepts there is a problem wastes the middle of the slot.

**The spine after cutting, nine lines:**

1. Setup. One rotation of five engineers has covered every out-of-hours page for two years.
2. Setup. Page volume has risen with the number of services, from twelve services to thirty-one.
3. Turn. The rotation now pages someone on two nights in three, which is above the level we told people to expect when they joined it.
4. Support. Alert tuning has already removed a large share of the noise, so the remaining pages are mostly real, and tuning further will not return the nights.
5. Support. Two of the last three leavers named on-call load in their exit interview, and we cannot separate that from pay.
6. Claim. A second rotation halves the nights per engineer without adding process, which is the only lever that does not depend on the service count falling.
7. Claim. Two engineers is the smallest number that makes a second rotation viable, because a rotation below five people fails the moment one person takes leave.
8. Claim. Peak begins in eleven weeks and hiring takes eight, which is why this is a decision for this month rather than next quarter.
9. Ask. Approve two platform engineers in the next round, decision by the end of the month.

**Connective test.** Joints 1 to 2 take and so. 2 to 3 takes which means. 3 to 4 takes but. 4 to 5 is a list joint, and it is legal, because 4 and 5 are parallel siblings supporting line 3. 5 to 6 takes and so. 6 to 7 takes because. 7 to 8 takes but. 8 to 9 takes and so. One list joint, inside a support group. The spine passes.

**Cannot get.** Line 5 started as "on-call load is driving attrition". The evidence status came back cannot get: exit interviews are self-reported, the sample is three people, and pay was mentioned in two of them. Rather than build a slide on it, the claim was weakened to what the evidence actually supports, and the sizing argument was moved onto line 7, which does not need it.

**Verdict.** Nine main lines against a budget of ten to fourteen, two setup lines, the ask stated at the top of the document and again at line 9, ten exhibits demoted to an appendix pack including the full alert tuning history and the rotation viability arithmetic, and one claim weakened before it was ever drawn. The deck tool can now be opened, and the person who opens it has nine headlines to write rather than a deck to invent.

## Failure modes

**The outline in disguise.** Every line is a noun phrase. From the outside it looks finished and reads as competent, and the tell is that no line could be disagreed with by anyone.

**The retro-fitted spine.** Written by extracting headlines from slides that already exist. Recognisable because it has exactly as many lines as the deck has slides, and it never once fails the budget, which no honest first draft does.

**The also chain.** Every joint is a list joint. Symptom in the room: the presenter says "another thing worth mentioning" between slides, repeatedly, and the audience cannot say afterwards what the deck argued.

**Evidence written as a topic.** Every status says have, and no evidence field names an artefact. This survives all the way to the build, where somebody discovers that the chart everyone assumed existed was a conversation in a meeting.

**The context avalanche.** Setup written first and defended line by line. Symptom: the spine reaches line six before a claim appears, and the author can explain why each context line is necessary, one at a time, which is exactly how five of them got there.

**The unresolved argument shipped anyway.** Four lines the author cannot defend, carried on the assumption that a well-made slide will make them look settled. Symptom in the room: the presenter says "we can go deeper on that offline" three times, and the group stops asking.

**The orphaned support.** A claim on one slide, its chart three slides later. The audience reads an assertion with nothing behind it, then a chart with no reason to exist.

**The activity ask.** The final line names an action the team will take rather than a decision the audience must make. Nobody says no, nobody says yes, and the same spine comes back in three weeks with better formatting.

**The deleted appendix.** Cut material dropped rather than demoted. The first hard question lands on an analysis that was done thoroughly two weeks ago and can no longer be shown.

## What this skill does not do

- It does not produce slides, a deck file, or a design, and it is not a step towards a generator. It ends with a frozen text document, and something else builds the deck.
- It cannot check whether any evidence is real. The status field records what you tell it, and a spine with a clean connective pass can rest entirely on a number nobody verified.
- It cannot see the room, so the objection ordering in step 4 is a default. Anyone who has sat in the meeting will beat it, and should be asked.
- It does not settle the final wording of headlines, which have to survive at headline size and in about ten to sixteen words. That is a separate editing pass on the frozen spine.
- It is the wrong tool for a deck that already exists. Reconstructing a spine from finished slides describes what was built rather than what was meant, and the narrative audit is designed for that case.
