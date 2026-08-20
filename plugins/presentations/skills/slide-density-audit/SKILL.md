---
name: slide-density-audit
description: Scores the slides of a presentation that already exists for density and legibility, working from exported slide text, speaker notes, font sizes and canvas dimensions. Replaces the six-by-six folklore with an attention-based rule, checks text size as a fraction of slide height, flags redundancy between slide text and speaker notes, and returns a ranked shortlist of the worst slides with thresholds rather than a comment on every slide. This skill should be used when a deck is structurally finished and someone needs to know which slides the room will not be able to read.
---

# Slide density audit

## The claim this skill is built on

This skill does not generate a deck. It runs on a deck that already exists, after the argument is settled.

Everyone was taught a counting rule. Six bullets, six words each. Seven and seven. Five words a line, five lines a slide, five text-heavy slides in a row. None of these has an identifiable research basis or a traceable primary source, and they persist because they are easy to comply with.

They also fail on their own terms. Six bullets of twelve words is seventy-two words, which is roughly twenty seconds of silent reading, and it satisfies six-by-six the moment you interpret the second six loosely. Meanwhile a single thirty word sentence on an otherwise empty slide is perfectly fine and violates every version of the rule. The unit is wrong. Word count is a proxy for the thing that matters and it is a bad one.

The thing that matters: **a slide is too dense when the audience cannot read it and listen to you at the same time.** That is a fact about attention rather than a fact about the slide, which is why the replacement rule has a mechanism and the counting rules do not.

## The mechanism, and what follows from it

Reading prose and listening to speech compete for the same limited channel. Cognitive load theory, developed by John Sweller and colleagues from the late 1980s onward, names two effects that decide slide design.

**Split attention.** When understanding requires integrating two sources that are separated, the integration itself consumes working memory that was meant for the content. On a slide this is the diagram whose explanation is elsewhere, or the chart whose key is on the far side of the slide.

**Redundancy.** When the same information is presented twice in two forms, processing both and reconciling them costs more than processing one. This is the counter-intuitive one and it is the one that matters most in decks.

Two consequences, in order of severity:

- **Slide text that duplicates what you are saying is worse than no text at all.** Not neutral. Worse. The audience reads ahead, finishes before you do, and then waits, having stopped listening. The information arrived once and cost twice.
- **Slide text that differs from what you are saying is worse still.** The audience now has two different messages arriving at once and will drop one of them. They drop the speaker, because the text is still there and you are not.

The practical rule that falls out: **any slide the audience has to read is a slide you have to stop talking for.** So your density budget is however long you are willing to stand in silence. If a slide takes twenty seconds to read and you are not going to give it twenty silent seconds, it is too dense. That is the test, and it is enforceable in rehearsal.

## Legibility, in numbers that mean something

There is no universal minimum point size, because point size means nothing without viewing distance and canvas size. What has to hold is a visual angle at the back of the room, and that depends on the screen and the room, neither of which the slide knows about.

Two facts make this tractable.

First, projection scales the entire slide, so **text size as a fraction of slide height is the invariant**, not point size. Work in percentages, then convert.

- Smallest text on a presented slide: at least about **4% of slide height**.
- Body text: about **4% to 5%**.
- Headline: about **6% to 8%**.

Second, canvas heights differ between tools and this is where decks quietly break. The common 4:3 and 16:9 defaults in desktop presentation software are both **7.5 inches tall**, which is why point sizes transfer between those two aspect ratios without anything looking different. Several web-based tools default to a 16:9 canvas that is **5.625 inches tall**. Same slide on screen, different numbers.

Converting the 4% floor:

- On a 7.5 inch canvas, which is 540 points tall: the 4% floor is 0.3 inches, or **21.6 points**, so round it up to **22 point**. Body text at 4% to 5% of height is **22 to 27 point**, which is where the familiar 24 point advice comes from. A headline at 6% to 8% is **32 to 43 point**.
- On a 5.625 inch canvas, which is 405 points tall: the same 4% floor is 0.225 inches, or **16.2 points**, so **17 point** at the floor, **17 to 20 point** for body, and **24 to 32 point** for a headline. The same visual result comes from smaller numbers, and a deck moved between the two tools without rescaling will be wrong in one direction or the other.

**Check the canvas height before you judge any point size.** A deck that looks reckless at 18 point may be fine, and a deck at 24 point may be too small, and the point number alone does not tell you which.

The check that works without measuring the room: full screen the slide, put the laptop or tablet on the floor, and stand upright over it. For a typical laptop that puts your eyes at roughly eight to nine times the screen height, which approximates a back row. If you can read the smallest text from there, it survives.

## Projection and screen reality

The deck was reviewed under the best conditions it will ever encounter. The author was close to the screen, in a controlled light, on a bright colour-accurate display, and already knew what every slide said. Every legibility judgement made in that state is optimistic, and that is the single most useful thing to remember in this section.

- **Contrast collapses under ambient light.** A projector in a lit room loses contrast because ambient light adds to the black level, so the ratio at the screen can fall to a fraction of what the specification promises. Light grey on white, mid grey on dark, and brand tints that look refined on a monitor go to nothing. Treat the accessibility floors as floors: 4.5:1 for normal text and 3:1 for large text are minimums for a screen you are holding, and for projection you want considerably more.
- **Thin weights disappear.** Hairline, thin, and light weights have less stroke area to survive projector bloom and distance. Use regular or medium as the lightest weight for anything below headline size, and reserve light weights for very large display text if at all.
- **Colour shifts.** Projectors are not colour accurate. A palette that distinguishes two series by a subtle hue difference will merge, so anything carrying meaning must also differ in lightness.
- **Compressed video is worse than projection.** On a lossy video link, fine text smears, thin lines break, and animation frames drop. If the deck is going to a remote audience, the density budget is tighter, not looser.

## The squint test and the arm's length test

Both are one minute long and both are more reliable than any inference from exported text.

**The squint test.** Display the slide full screen. Defocus your eyes or squint until no word is readable. What remains is the visual hierarchy. You should still be able to tell which block is the headline, which is the main exhibit, and in what order to read them. If the slide is one uniform grey field with no dominant element, it has no hierarchy, and the audience will spend their first two seconds deciding where to look rather than reading.

**The arm's length test.** Full screen the slide, place the device flat on the floor, stand upright, and read the smallest text on it. That geometry approximates the back of a mid-sized room. Do it on the machine the deck was built on, and do it with the room lights on.

Neither test needs a projector, a room, or anybody else, which is why the excuse for skipping them is always about time and never about access.

## Bullets

A list is genuinely right when three things hold: the items are members of one set, they are parallel in form, and the reader will scan rather than read. Options being compared, steps in an order, criteria being applied, a set of things a system supports. In those cases the list structure is information.

The far more common case is bullets that are sentences with the connective tissue removed. The author wrote a paragraph in their head where each idea connected to the next with "because", "so", "but", or "which means", then stripped the connectors to make the text fit. What is left looks tidy and has lost the argument, and the audience has to reconstruct the logic that was deleted.

**The test:** try inserting "and therefore", "but", or "because" between consecutive bullets. If the meaning improves, it was prose. Then choose: restore the prose in the speaker notes and put only the assertion on the slide, or keep the one bullet that carries the point and cut the rest.

## Builds and animation

There is one case where a build genuinely helps comprehension: **when the intermediate states carry meaning and seeing the whole at once would let the audience read the ending before you explain the middle.** A process diagram assembled in the order you narrate it. A chart where the second series appears after the first has been established. A map where regions light up as you name them. In each case the build is doing work no static slide could do.

Everything else delays the audience without informing them:

- **Bullets revealed one at a time.** The audience learns nothing from being made to wait, they now know a list exists and cannot see it, and the deck becomes unusable as a read artefact because a reader sees only the final state or, worse, only the first.
- **Transitions with motion.** Flying, cube, spin. They cost attention and add nothing.
- **Builds used to hide density.** A slide too dense to show at once is still too dense. Revealing it in four parts converts one bad slide into four slides that happen to share a background.

Two mechanical costs worth knowing. A build usually exports to PDF either as one flattened slide, losing the sequence, or as N nearly identical pages, which makes the read version painful. And on a lossy remote link, animation frames drop, so the reveal that landed in rehearsal arrives as a jump.

## Tables

The ceiling on a presented slide is roughly **five to seven rows and four to five columns**. Past that the audience stops listening and starts reading, because finding a cell and comparing it to another cell is a search task and search tasks are not compatible with listening.

When the data genuinely needs more, do not shrink it. Shrinking a table to fit is the visible symptom of a slide that has lost its argument, and it produces text below the legibility floor by definition.

Instead, show the shape and move the detail:

- Replace the table with the chart that shows the pattern, and keep the table in the appendix.
- Keep the table and reduce it to the two or three rows that differ, with a note that the full version is in the appendix.
- Keep the full table but highlight the cells the finding depends on, so the reader's search is done for them. This works for a read deck and is marginal for a presented one.

## Whitespace and alignment

Both are legibility, not decoration.

- **Margins.** Keep at least about 5% of slide width clear on every edge. Projectors overscan, screens crop, and video conferencing tools sometimes letterbox, so text near an edge is text at risk.
- **Alignment.** A single consistent left edge gives the eye one scan path. Elements that start at three slightly different x positions read as noise, and the eye resolves the noise before it reads the words. This is a real cost measured in the audience's first second on the slide.
- **Proximity.** Whitespace defines grouping. A label closer to the wrong element belongs to the wrong element, whatever the author intended, and a legend set adrift from its chart is a split attention problem rather than a styling one.

## The density scoring procedure

Score every slide, then report only the worst. Reporting on all forty guarantees the review gets skimmed.

Start each slide at 0 and add:

- **On-slide words.** 0 to 25 words: add 0. 26 to 50: add 1. 51 to 75: add 2. Over 75: add 3.
- **Smallest text below the height floor.** Below 4% of slide height: add 2. Below 3%: add 3 instead.
- **Extra ideas.** Add 1 for each distinct idea beyond the first.
- **Redundancy.** Slide text that repeats the speaker notes almost word for word: add 2.
- **Contradiction.** Slide text that says something materially different from the notes: add 3.
- **Oversized table.** Over 7 rows or over 5 columns: add 2.
- **Encoding clutter.** More than two colours carrying meaning, or more than two text sizes beyond headline and body: add 1.
- **List build.** A build revealing list items one at a time: add 1.

Thresholds: **0 to 2 is fine. 3 to 4 is watch. 5 or more is rework.**

Report the slides scoring 5 or more, ranked, plus the top five overall if fewer than five cross the threshold. For each, give the score, the components that produced it, and the specific fix.

**The decision rule for what "fix" means:**

The read deck against the presented deck is set out in full in the deck narrative audit skill, and only its density consequence belongs here: a presented slide is stripped to the assertion plus one exhibit with the words moved to the notes, while a read slide keeps the words, so the density rules relax for it and the legibility floors never do. Where you cannot tell which one you have, which is the usual case, assume the deck will be forwarded and report the ambiguity as a finding in its own right rather than quietly scoring it as one or the other.

## Worked example

Three slides from a deck due to be shown on a projector in a lit meeting room. Canvas height 7.5 inches. All slide figures below are invented for this example.

**Slide 7, "Q3 performance".** 94 words of text in six bullets. Smallest text is 16 point, which is 2.96% of slide height, below the 3% line. The speaker notes repeat five of the six bullets almost exactly. Two distinct ideas: volume grew, and margin fell. Score: 3 for words, 3 for text below 3%, 1 for the second idea, 2 for redundancy. **Total 9. Rework.** Fix: split into two slides, each with an assertion headline and one chart, and move all six bullets to the notes.

**Slide 12, vendor comparison table.** 9 rows, 6 columns, shrunk to 11 point to fit, which is 2.04% of slide height. 140 words counting cells. Notes are a single line. Score: 3 for words, 3 for text below 3%, 2 for the oversized table. **Total 8. Rework.** Fix: on the slide, show the three rows where the vendors actually differ, at 24 point, with the winning cells marked. Full matrix to the appendix.

**Slide 18, architecture diagram.** 22 words total, all labels at 20 point, which is 3.7% of slide height, so below the 4% floor but above 3%. One idea. No redundancy. Score: 0 for words, 2 for text below the floor. **Total 2. Fine, with one fix.** Raise the labels to 24 point, which will require dropping two labels that were only there for completeness. That is the correct trade.

**Verdict on the three: two rework, one minor.** The two rework slides account for one third of the deck's total text. Fixing them and moving the cut material to the appendix reduces the main line by one slide and removes both of the deck's legibility failures.

## Failure modes

**Counting compliance.** A slide that satisfies six-by-six, carries seventy-two words, and cannot be read while anyone is speaking. The rule was followed and the audience still lost.

**The read-along slide.** Slide text and speaker script are the same words. The audience finishes reading before the speaker finishes talking, then disengages. It looks like a well-prepared slide and it is the redundancy effect in its purest form.

**The contradiction slide.** The slide says one thing, the speaker says another, and the audience keeps the slide. Usually caused by editing the notes after the slide, or the reverse.

**The shrink-to-fit table.** Nine rows squeezed to 11 point. Nobody in the second row can read it, and the presenter says "you probably cannot read this", which is an admission that the slide should not exist.

**The laptop review.** The whole deck approved at close range, on a bright screen, by someone who already knows what it says. Every subsequent legibility complaint is a surprise.

**The build used as a lid.** A dense slide revealed in four steps so it never looks dense. It is still four times too much material and it now exports badly.

**The uniform edit.** Every slide reduced to three bullets, including the one slide that genuinely needed a table and the one that needed a paragraph. Consistency applied where the content varies.

**The thin-weight rebrand.** A new template rolled out in a light weight, signed off on a design monitor, illegible from row four in a lit room. This one is expensive because it affects every deck in the organisation at once.

**The comment-on-everything review.** Forty slides, forty notes, no ranking. The author fixes the first six and stops.

## What this skill does not do

- It does not open, render, or edit the file. It works from exported text plus the font sizes and canvas height you supply, and it will name the checks it could not run rather than guessing.
- It cannot measure contrast without real colour values. With hex codes it can compute a ratio, without them it has nothing.
- It cannot run the squint test or the arm's length test. Those need a human and a screen, and they are the two most reliable checks on this page.
- It does not judge design quality, template choice, or brand fit. A designer does that better and this offers them nothing.
- It says nothing about whether the deck argues anything, whether the order is right, or whether the ask is stated. A completely legible deck can still be forty slides of nothing.
