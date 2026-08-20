---
name: deck-accessibility-audit
description: Audits a finished presentation for the accessibility defects that sighted review cannot see. Covers per-slide object reading order and why it is not the visual order, unique slide titles including visually hidden ones, what alt text on a chart should say and when a chart needs its data as text instead, contrast thresholds applied to projection rather than a laptop, colour as the only carrier of meaning, real tables against grids of text boxes, captions and transcripts for embedded media, link text, and whether tags survive the PDF export on each platform. This skill should be used before a deck is shared, uploaded or exported, and after the built-in accessibility checker reports no issues.
---

# Deck accessibility audit

## The claim this skill is built on

The accessibility defects in a deck are almost all invisible to the person who made it, and the one that matters most is invisible to everybody.

A slide is a canvas of floating objects. There is no document flow, no source order that corresponds to what you see, and no structure beyond the one the author happened to create. Assistive technology has to read those objects in *some* order, so it reads them in the order they appear in the file. That order is the order the objects were created, modified by every subsequent send-to-back and bring-to-front, and it has nothing to do with where they sit on the slide. A deck can be visually immaculate and be announced as gibberish.

Everything else in this audit follows from the same asymmetry. Contrast is judged on a laptop and experienced on a projector. Alt text is written by someone who already knows what the chart says. A title is deleted because the slide looked cleaner without it, taking with it the only navigation a screen reader user has.

**Scope: this does not generate or redesign a deck.** It reviews one that exists.

Run the checks in this order. Reading order first, because a deck with scrambled order is not improved by better alt text.

## 1. Reading order, per slide, every slide

**Where the order lives.** The slide stores its shapes in a list. That list is document order, it is also the back-to-front stacking order, and it is what a screen reader follows. Group a set of shapes and the group is read as one item at the group's position in the list.

**How to inspect it.** In PowerPoint, the Selection Pane lists every object on the slide. The critical detail, and the one that produces wrong fixes: **the Selection Pane shows the front-most object at the top, and reading order runs from the bottom of that list upwards.** Newer builds also offer a dedicated reading order pane that lists items in reading order, top to bottom, which is the reverse. Two panes, two directions, same slide. Check which pane you have open before you reorder anything, and confirm against the version in front of you, because these panes have changed across releases on both Windows and Mac.

**The rule that makes this expensive and unavoidable: order is stored per slide.** There is no deck-level setting, nothing inherited from the master, and no way to fix it once for all slides. A deck assembled by pasting slides out of three other decks has three unrelated orders in it. Every slide has to be opened.

**What correct looks like.** Title first. Then the content in the order a reader would take it: the main body before the supporting note, the chart before its caption, the left column before the right in a two-column layout. Footers, page numbers, logos and decorative shapes last, or marked decorative so they are skipped entirely.

**The check.** For each slide, read the object list in announcement order and write it down as a sequence. If that sequence would not make sense read aloud to somebody who cannot see the slide, it is wrong. That is the whole test, and it takes about twenty seconds per slide.

## 2. Every slide needs a unique title

Slide titles are how a screen reader user navigates a deck. They populate the outline, they are what a reader hears when moving between slides, and without them the deck is an unlabelled sequence.

Three failures, in order of frequency:

- **No title at all**, because the title placeholder was deleted for a cleaner look, or because the title was drawn as a free text box, which is not a title as far as any tool is concerned.
- **Duplicate titles.** Three consecutive slides called Results give a navigation list reading Results, Results, Results, which is no better than no titles. Continuation slides need distinguishing text: Results, revenue. Results, retention. Results, what we are changing.
- **A title that is a topic rather than a statement**, which is a structural problem more than an accessibility one, but it is fixed in the same pass.

**The mechanism for a title that exists without being shown.** Two options. The application documents hiding the title shape through the Selection Pane, which leaves the text available to a screen reader while removing it from view. Moving the title placeholder off the slide canvas achieves the same thing and travels better between tools, because it does not depend on how a given reader treats a hidden shape. Both are legitimate; pick one and verify with the screen reader you actually support, because this is precisely the kind of case where readers differ.

Note that neither hidden nor off-canvas titles appear in an exported PDF, so if the PDF is the deliverable, the title has to be somewhere the export will find it.

## 3. Alt text, and the chart rule

**A good description states the finding, not the shape.** The bad version describes what the image is: *bar chart showing five bars in blue*. That tells a reader nothing they could not guess and consumes their time saying it. The good version says what the chart is there to say: *bar chart of support ticket volume by month; volume roughly doubles between March and June and then holds flat*.

Alt text is a sentence with the same job as the slide headline, and if the slide headline already states the finding, the alt text can be short and point at it.

**The complex chart rule.** A chart with several series, a secondary axis, or values a reader needs individually cannot be described in a sentence. Do not try. **The data has to be available as text somewhere**: an appendix slide with the table, the figures written into the speaker notes, a linked source file, or a caption carrying the specific numbers. Alt text then gives the finding and points at where the data is. A heroic 400 word alt text is a failure mode, not a solution, because it is read as an uninterruptible block.

**Decorative images are a decision, not an omission.** A background texture, a divider rule, a stock photograph of people at a desk: mark it decorative so it is skipped. Marking decorative is not the same as leaving the field empty by accident, and a reader that encounters a shape with no description will often announce its filename instead, which is worse than either.

**Decision rule.**

- The image carries information the reader needs, and that information is not stated in the visible text. → **Alt text that states the information.**
- The image is presentational and removing it would not change what the slide says. → **Mark it decorative.**
- The image is a chart or complex diagram with values that matter individually. → **Alt text stating the finding, plus the data as text elsewhere in the deck.**
- **You cannot tell.** → Read the slide aloud with the image removed. If it still makes its point, the image is decorative. If it does not, the image is carrying the point and needs describing.

Check groups as well as individual shapes. Alt text on a group is announced for the group; alt text on members inside a group may not be reached at all, and the combination of both produces duplication.

## 4. Colour

**Contrast thresholds.** WCAG requires 4.5:1 for normal text, 3:1 for large text, where large means 18 point or 14 point bold, and 3:1 for meaningful non-text elements such as chart lines and icons. Most deck body text is 18 point or larger, so the 3:1 threshold technically applies more often than people expect. Ignore that and aim for 4.5:1 anyway, for the reason below.

**Passing on a laptop is not passing.** Projection reduces effective contrast, sometimes severely: ambient light, a washed-out projector, a screen in a bright room and a wall that is not white all subtract from the ratio you measured. So does a bad video call connection re-compressing the slide. A mid-grey caption that passes at 4.6:1 on a calibrated laptop can be invisible from the fourth row. Treat the measured number as the best case and give yourself headroom on anything that will be projected.

**Colour must never be the only carrier of meaning.** The canonical case is the status column: green fill for on track, amber for at risk, red for slipping, with nothing else distinguishing them. To a reader with a common form of colour vision deficiency, and roughly one in twelve men of northern European descent has one, the red and green cells are the same cell. In a black and white print, all three are the same grey. The fix is to add a second channel: the word, an icon, a shape, a pattern, or a position. Same for a line chart distinguished only by line colour, where the answer is direct labels on the lines or distinct dash patterns.

**The cheapest test available.** Switch the deck to greyscale view. Every case where colour is doing work alone becomes obvious immediately, and it doubles as a rough contrast check. It takes one click and finds more than any amount of squinting at a colour picker.

## 5. Tables

**A table must be a real table object**, not an arrangement of text boxes that looks like one. A grid of text boxes has no rows, no columns and no header. It is announced as a sequence of unrelated fragments, it cannot be navigated cell by cell, and it falls out of alignment the moment any cell's text changes length.

**Mark the header row.** A real table has a header row setting, and it is what allows a reader to announce the column name with each value rather than reading a column of bare numbers.

**Merged cells break navigation.** Cell-by-cell movement assumes a rectangular grid. Merges make position ambiguous, so the reader loses track of which column a value belongs to, and support for merged cells varies between readers. Avoid merges in a table that has to be accessible. If the data genuinely needs them, it needs to be a picture with a proper text alternative, or a link to the real spreadsheet.

**Size.** A table that needs more than about five columns to be legible on a slide is not a slide, it is an appendix. Split it or link it. That ceiling is the same one the slide density audit skill scores against, so the two audits will not disagree with each other in front of you.

## 6. Text size, and the overlap with legibility

An accessible deck and a legible deck share most of their requirements, which is convenient: run the two audits together and neither costs much more than the other alone.

- **Judge a point size against the canvas height, never on its own.** The same number is a different size on screen depending on how tall the canvas is, so the invariant is text height as a fraction of slide height. The slide density audit skill carries those percentages and the point conversions for both common canvas heights, and this audit defers to it on sizing rather than repeating a figure that would drift out of step. What belongs here is the consequence: anything below the floor that skill sets is a footnote, and a footnote nobody can read is decoration.
- A long-standing rule of thumb for a room is to take the age of the oldest expected audience member and halve it to get a minimum point size. It is folklore rather than research, but it lands in roughly the right place and it is easy to apply.
- **A deck built to be read is a different artefact from a deck built to be presented.** The read version can carry smaller text and more of it, because the reader controls the distance and the pace. Decide which one you are auditing before you judge the sizes, and if the answer is both, that is the actual finding.
- Avoid text set over a busy photograph. If it must happen, put a solid or heavily blurred panel behind the text, because contrast against a photograph varies pixel by pixel and no single measurement covers it.

## 7. Video and audio

- **Video needs captions.** PowerPoint accepts caption files in the WebVTT format attached to an inserted video, on both Windows and Mac in current versions. Auto-generated captions from a hosting platform are a starting point and not a deliverable; they routinely mangle names, numbers and product terms, which are exactly the words that matter.
- **Audio needs a transcript.** There is nowhere else for the content to go.
- **Autoplaying animation is a specific problem with a specific threshold.** Content that moves automatically, lasts more than five seconds, and is presented alongside other content must be pausable, stoppable or hideable. A looping background animation on a slide the presenter talks over for two minutes fails this, and it also makes the slide harder to read for everybody. Separately, nothing may flash more than three times in any one second, which is a seizure risk rather than an inconvenience.
- **Anything communicated only visually in the video** needs to be said aloud or captioned as a description, not just transcribed as dialogue.

## 8. Links

**Link text must say where the link goes.** This matters more in a deck than on a web page, because a URL read aloud is unusable: a reader announces it character by character, including the tracking parameters. *Click here*, *this link*, and a bare address are all failures.

The complication specific to decks is that a printed or projected slide has no click. If the audience needs to type the address, it has to be visible and short. The resolution is a readable label as the link text, plus a short human-typeable address shown on the slide or collected on a final references slide, and never a long tracking URL pasted into the body.

## 9. The exported PDF

The PDF is a different artefact and it is frequently worse than the deck it came from.

**Whether tags survive depends on the command you use, and the commands differ by platform.** On Windows, saving as PDF exposes an options dialogue containing a setting for document structure tags for accessibility; with it off, the tags do not exist. On Mac, saving as PDF from the application's own save dialogue with the option intended for electronic distribution and accessibility produces a tagged file, while printing to PDF through the operating system print dialogue produces an untagged picture of the deck. **Same deck, same machine, same person, two entirely different results.** This is the single most commonly wasted piece of accessibility work in a deck.

**What to check in the exported file.** Tags are present at all. The document title is set in the file properties and is a real title rather than a filename, because that is what a reader announces and what a browser tab shows. The document language is set. The tag tree order matches the reading order you fixed in the deck, which it may not, since the export makes its own decisions. Alt text carried through. Links are live and carry their text. Headings exist as headings rather than as styled text.

Expect to remediate. A deck export commonly produces a flat tag tree with everything as body text, and fixing that is a job for a PDF tool rather than for the deck.

## 10. The built-in accessibility checker

Run it. It is free, it is in the application, and skipping it is indefensible.

**What it catches:** missing alt text, missing slide titles, duplicated slide titles, tables without a header row, some low-contrast text, and it will prompt you to check reading order on slides with several objects.

**What it misses:** whether your alt text says anything useful, whether the reading order is actually correct as opposed to worth checking, colour used as the only carrier of meaning, contrast of text over an image or a gradient, text that is too small, caption quality, link text quality in many cases, and everything about the exported PDF.

**So passing it is a floor.** The most dangerous moment in a deck's accessibility is the green tick, because it converts an unfinished job into a finished-looking one. Treat a clean result as evidence that the mechanical layer is done and the judgement layer has not started.

## Worked example

An eight-slide quarterly review, checker clean, going to an intranet. The deck and its defects are invented for this example.

**Reading order.** Slide 3 was built by pasting a chart in last, so the chart sits at the end of the object list and is announced after the commentary that refers to it. Slide 6 was assembled from another deck and its order is title, footer, logo, body, body, chart, meaning a reader hears the page number and the logo before any content. Six slides are correct by accident; two are not, and there was no way to know which without opening each.

**Titles.** Slides 4, 5 and 6 are all titled Pipeline. Slide 7 has no title placeholder, its heading having been drawn as a text box for a tighter layout.

**Alt text.** Present on all five images, because the checker demanded it. Four of the five read as descriptions of shapes: *line chart*, *bar chart*, *photo*, *diagram*. The fifth is 300 words describing a matrix cell by cell, which is unreadable as a block. The matrix needs the finding in a sentence and the data on an appendix slide.

**Colour.** The status column on slide 5 uses red, amber and green fill and no other signal. In greyscale view all three are the same mid-grey, which is also what it looks like in the printed handout the operations team will bring.

**Tables.** Slide 6's table is fourteen text boxes. It looks like a table, is announced as fourteen fragments, and two cells have already drifted out of alignment.

**Media.** The 40 second product clip on slide 8 has no captions, and a background animation on the title slide loops indefinitely with no way to stop it.

**Links.** Slide 8 carries a full tracking URL of about ninety characters as visible link text.

**Export.** The file was produced by printing to PDF, so it has no tags, no document title and no link structure. Every fix above is invisible in the artefact people will actually download.

**Verdict: hold.** The export defect makes the entire audit moot until it is corrected, so fix that first even though it was found last. Then reading order on slides 3 and 6, then the titles, then the status column, then the table, then the captions and the alt text. The checker's clean result was accurate and told you almost nothing.

## Failure modes

**Auditing with your eyes.** The slide reads correctly to you and the file order is unrelated to what you see. This is the whole reason the audit exists and it is still the first thing skipped.

**Reordering in the wrong pane.** Fixing the order in a pane that lists objects front-to-back while thinking it lists them in reading order produces a deck that is exactly backwards, and it now looks deliberate.

**Fixing reading order once.** The order is per slide. A fix on the master or on one slide changes nothing anywhere else.

**Alt text that describes the shape.** *Bar chart with five bars* passes the checker, satisfies the audit, and communicates nothing. It is the most common way a deck is technically compliant and practically unusable.

**Deleting the title instead of hiding it.** The slide looks cleaner and the deck loses its navigation. The person who did it will not see any effect at all.

**Checking contrast on a laptop.** The numbers pass, the room does not, and nobody in the fourth row says anything.

**A table made of text boxes.** Looks right, announces as fragments, and drifts out of alignment as soon as anyone edits a cell.

**Trusting the green tick.** The built-in checker measures a subset. A clean result mostly means the alt text fields are non-empty.

**Exporting with the wrong command.** Hours of work discarded by a print dialogue, silently, with a file that looks identical.

## What this skill does not do

- It does not generate or redesign a deck. It reviews one that already exists, which is deliberate: deck generation is well covered elsewhere.
- It cannot tell you what a screen reader actually announces. It reasons about structure, and readers differ from each other on grouped shapes, hidden shapes and table headers, so the only settlement is running the deck with the reader you support.
- It cannot judge whether your alt text is any good, only whether it exists and whether it is describing a shape instead of a finding.
- It does not remediate the exported PDF. It tells you which export to use and what to check; fixing a flat tag tree is a job for a PDF tool.
- It says nothing about live delivery: microphone use, describing the screen aloud, pace, or whether remote attendees can see anything. Those matter as much and no file audits them.
- It is not a legal compliance opinion. Obligations differ by jurisdiction, sector and procurement contract, and meeting a technical standard is not the same as meeting a duty.
