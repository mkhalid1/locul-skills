---
name: feed-stopper-card-system
description: Designs a reusable visual system for the single square image that accompanies a social post, rather than designing each card from scratch. Covers the canvas and margin specification with the crop-safe bands, six palette roles given as values with their computed contrast ratios, a hard cap of three reading type sizes with the floor derived from feed render scale, five card archetypes mapped one to one onto post types, a decision rule for picking one, the legibility and screen-reader requirements that social graphic guides omit, and an anti-pattern catalogue written as what each default signals to the reader. This skill should be used when a card, graphic or image is being made for a text-first social feed, when a run of posts needs one consistent look rather than forty separate ones, or when an existing card needs checking for contrast, type size and readability at feed scale.
---

# Feed stopper card system

## The claim this skill is built on

The card attached to a social post is not illustration and it is not decoration. Its only job is to buy the half second in which the first line of the copy gets read. If the first line does not get read, the post did not happen.

The obvious approach is to design each card for its post. It fails in a way that is invisible from inside, because each individual card comes out fine. What fails is the aggregate.

**A reader never decides to remember you.** Recognition is a by-product of repetition, and repetition only accumulates when the thing repeated is the same thing. Forty cards designed one at a time are forty separate objects, and the counter resets at every one of them. Forty cards from one system are one object seen forty times. That is the entire mechanism, and it is why a mediocre system beats a run of individually better cards: the system is what gets recognised, not any card in it.

Two smaller reasons follow. **A system is arithmetic on your own time:** the first card costs an hour because every decision is open, and card two onwards costs the minutes it takes to pick an archetype and type the words. **And a system makes a card checkable.** A one-off can only be judged by whether somebody likes it. A card built to a spec can be failed on a number: this colour sits at 2.59:1 and the floor is 4.5:1, this label renders at nine pixels and the floor is twelve. You cannot argue with either, and you cannot have either without a spec.

Everything below is the spec.

## Part one. The canvas, and why square

**1200 by 1200 pixels, exported as PNG.** Square, not landscape, and the reason is arithmetic rather than taste.

A feed renders an image to the column width and derives the height from the aspect ratio. At a column width `w`, a 1:1 card occupies `w` of vertical space and a 1.91:1 card occupies `w / 1.91`, which is about `0.52w`. **Square therefore takes 1.91 times the vertical space of a landscape card at the same width.** A scrolling thumb moves a roughly fixed number of pixels per second, so vertical space is time on screen, and time on screen is the whole product.

A 4:5 portrait card is taller still, at `1.25w`, and is a legitimate choice where it is supported. It is not the default because the same file usually has to survive being a link preview, a repost, a search tile and a profile grid cell, and square survives the most crops with the least damage.

**Why 1200 and not 2400.** A 1200 pixel card shown at 356 to 555 pixels is being oversampled by 2.2 to 3.4 times, which already clears the 2x that a high-density display needs. Doubling the canvas multiplies the file size by roughly four for no visible gain, and on a slow connection a heavier image is a card that arrives after the thumb has passed it.

### What the platform actually documents

Checked against LinkedIn's own help pages in August 2026, because this is the layer where vendor guides are reliably wrong.

- **The supported aspect ratio for a shared photo runs from 3:1 to 4:5, width to height.** Anything outside that range is **centred and cropped** to the nearest supported ratio. It is not letterboxed and it is not scaled down to fit. A 1:1 card sits comfortably inside the range and is never cropped. A 9:16 card is cropped hard, which is why the taller-is-better instinct backfires here.
- **Minimum 552 by 276 pixels, with at least 1080 pixels of width recommended, and a 5 MB upload cap.** A flat 1200 square PNG carrying type lands far under that cap, which is the second reason to use PNG rather than JPEG: no compression artefacts around the letterforms, and file size never becomes the constraint.
- **1200 by 627 is not the feed image size,** despite being quoted as one almost everywhere. That specification governs the open-graph image used for a *link preview*, minimum 1200 by 627 at 1.91:1. Using it for an uploaded card throws away almost half the vertical space you were entitled to.

**And one thing the card is not: a reach lever.** LinkedIn's published explanation of how the feed ranks content, checked August 2026, does not name images or media format as a ranking signal at all. Its own marketing pages separately claim that posts with images see roughly twice the comment rate, with no study, sample or date attached, so treat that as a vendor claim made by the vendor rather than as research. Meanwhile the largest published third-party benchmark, Socialinsider's across 1.3 million posts from 16,645 business pages between January 2024 and December 2025, places single-image posts *below* native documents and multi-image posts on engagement rate, which is close to the opposite of the story these numbers are usually used to tell. The honest position, and the one this file is built on: **a card buys attention from people the post already reached. Nothing here claims it buys reach.**

### Margins and the crop-safe bands

- **Hard margin: 80 pixels on all four sides.** Live area 1040 by 1040. That is 6.67 per cent per side, and it is a floor rather than a target. Crowding the edge is the single cheapest way to make a card look like template output.
- **The 1.91:1 band, which is a cross-surface precaution rather than a feed one.** The feed will not crop a square, but the same file usually gets reused as an open-graph image, an email header, or a post on a network with different rules. A centred 1.91:1 crop of a 1200 square keeps only a band 1200 by 628 and discards 286 pixels off the top and 286 off the bottom. If the card will be reused that way, keep the headline between y = 286 and y = 914.
- **The 4:5 band.** A centred portrait crop keeps 960 pixels of width and cuts 120 off each side, which eats 40 pixels into your 80 pixel margin. If the card may ever appear on a portrait-cropping surface, raise the side margin to 120.
- **Never place type in a corner.** Corners are where every crop, every rounded container and every overlaid play button lands.

## Part two. Six palette roles

Six roles, not six favourite colours. Substitute your own hues freely, but keep the count at six and keep the luminance order intact, because the order is what carries the hierarchy once the card is scaled down to the size of a stamp.

The values below are one working set. The ratios are computed against the field colour under the WCAG 2.x relative luminance formula, which has been unchanged since WCAG 2.0 and is current in WCAG 2.2, verified as of August 2026.

| Role | What it is for | Example value | Ratio vs field |
| --- | --- | --- | --- |
| **Field** | The background. Dark against a light feed, and that contrast is the interruption. | RGB(22, 48, 48) | 14.0:1 vs white |
| **Primary** | The one line that must be read. Warm white, never pure white. | RGB(245, 240, 232) | 12.34:1 |
| **Secondary** | Setup lines, the words that make the primary land. | RGB(160, 175, 170) | 6.14:1 |
| **Accent** | Exactly one thing per card: the punchline, the figure, the right answer. | RGB(200, 165, 100) | 6.02:1 |
| **Signal** | The wrong option, the crossed-out, the stop. | RGB(180, 70, 60) | **2.59:1, fails** |
| **Recede** | The brand line, rules, footnotes. Deliberately quiet. | RGB(80, 100, 100) | 2.23:1, by design |

**Three things fall straight out of that table.**

The field is dark but not black. Pure black on a light feed reads as an error state or a video with no poster frame, and it kills the warmth of the type. A deep desaturated dark holds 14:1 against a white feed and about 12.5:1 against the warm off-white some clients render, which is all the interruption the card needs.

**The signal role as normally chosen does not work.** A muted red at RGB(180, 70, 60) sits at 2.59:1 against this field, which fails the 4.5:1 minimum and fails the 3:1 large-text minimum too. Lifting it to about RGB(224, 128, 116) reaches 5.01:1 and still reads as red. This is the most common real defect in a dark card system and it is invisible on a bright monitor at full size.

**The recede role failing is correct, and it constrains what may go in it.** At 2.23:1 the brand line is meant to be found only by somebody who has already stopped, so nothing in that role may carry information the reader needs.

**One accent per card, no exceptions.** If the figure and the punchline are both gold, neither of them is the answer, and the eye lands on whichever is larger, which was not a decision anybody made.

## Part three. Type: a cap, not a scale

**Maximum three reading sizes per card.** Headline, body, label. That is a cap and it is doing different work from a type scale.

A modular scale with a ratio of 1.25 across six steps hands you six legal sizes, and a card will happily use five of them. Hierarchy then rests on 25 per cent differences. Multiply by the render scale below and a step from 56 to 44 canvas pixels becomes a step from 16.6 to 13.1 rendered pixels, which a moving thumb does not read as hierarchy at all. **A cap forces the ratios to be large enough to survive the downscale**, because with only three sizes available you have to make each gap count: roughly 2x from headline to body, roughly 1.4x from body to label.

### The floor, derived rather than asserted

A 1200 pixel canvas is not shown at 1200 pixels. It is shown at whatever the feed column is, and the phone is the binding case because that is where most feed reading happens.

```
render scale = displayed width / 1200

phone, image about 356 CSS px wide     scale 0.297
tablet or narrow window, about 400     scale 0.333
desktop web, about 555                 scale 0.463
```

**The three widths are observational, not documented.** No major feed publishes its column
width, and it changes with viewport, client version and window size. Measure yours by
inspecting a rendered post, then redo the arithmetic below with your own number. The method
survives a different width. The specific pixel values do not.

Run the canvas sizes through the phone scale and the floor picks itself:

| Canvas size | Renders at (phone) | Verdict |
| --- | --- | --- |
| 16 to 20 px | 5 to 6 px | Sub-legible. May not carry information. |
| 40 px | 11.9 px | The floor. Legible at a glance, nothing lower. |
| 48 px | 14.2 px | Label. Comfortable. |
| 56 to 64 px | 16.6 to 19.0 px | Body. Actually readable. |
| 110 to 140 px | 32.6 to 41.5 px | Headline. Lands without being read. |

So: **label 40 to 48, body 56 to 64, headline 110 to 140, and a brand line at 16 to 20 that says nothing load-bearing.** If the brand line has to be understood, it is not a brand line, and it goes above 40 pixels or into the post body.

### The rest of the type rules

- **Hierarchy from size, weight and colour only.** No underlines, no boxes, no badges, no pills, no icons, no drop shadows. Each of those is a device for producing separation without deciding what is subordinate, so their presence is evidence that the decision was skipped.
- **Left aligned, never centred.** A centred block has a ragged left edge, and the eye returns to the left edge on every line. At scroll speed you have one or two fixations in total and a ragged edge spends them. Centred also reads as a greeting card or a certificate. Left aligned reads as a document, and a document reads as authority.
- **One typeface, two weights.** Optionally one monospace, used only for labels and the brand line.
- **Rules 1 to 2 pixels, in the accent colour**, including the vertical rule that divides a two-column layout. Nothing thicker: a 4 pixel rule at 0.3 scale is a 1.2 pixel line that anti-aliases into a smudge.
- **No post numbers, no category chips, no date stamps.** They are metadata for you, not information for the reader.

## Part four. Five archetypes, mapped to post types

Five, and five is a cap for the same reason three sizes is. A sixth archetype is almost always a variant of one of these, and every variant costs a little of the recognition the system exists to build.

| Archetype | Post type it serves | Layout skeleton |
| --- | --- | --- |
| **Bold statement** | Opinion, hot take, contrarian claim | Two or three setup lines top-left at body size in secondary. Punchline in primary at headline size across the vertical middle, with one phrase in accent. Optional single insight line at label size beneath a 1px accent rule. The hook is the visual. |
| **Two-column comparison** | Framework, before and after, wrong versus right | 1px vertical accent rule at x = 600, with 40px clearance each side, giving two 480px columns at x = 80 to 560 and x = 640 to 1120. Column headers at label size in recede. Left column items in signal, lifted to clear 4.5:1. Right column in primary with the operative word in accent. The tension between the columns does the stopping. |
| **Numbers** | Data, arithmetic, cost, benchmark | One to three figures at headline size or larger in accent, each with a label directly beneath at label size in secondary. One supporting sentence at body size along the bottom. The arithmetic lands before any prose is read. |
| **Timeline** | Trend, evolution, then and now, prediction | Three or four stations along a horizontal 1px accent rule. Era labels in monospace at label size. Past in recede, present in primary, future in accent or signal. Direction of travel is the message. |
| **Framework list** | Actionable how-to, checklist, ordered steps | Zero-padded numerals 01, 02, 03 in accent at body size, hanging in a 100px left indent. Item titles in primary at body size, heavier weight. One-line descriptions in secondary at label size. Should read like an internal systems document, not a listicle. |

## Part five. The decision rule

Run it on the finished copy, in this order, and stop at the first match.

- **The argument rests on one sentence you would say out loud.** Bold statement.
- **The post contrasts two approaches, two eras or a mistake and its correction, and reaches a verdict.** Two-column comparison.
- **The strongest single element in the post is a figure, a ratio or an arithmetic result.** Numbers.
- **The structure is chronological and the point is the direction of travel.** Timeline.
- **The post is three to six parallel, actionable items.** Framework list.
- **You cannot tell, because the post has several of these and no obvious centre.** Default to **Bold statement**, with the post's own first line set verbatim as the punchline. It is the safe default for three reasons: its content already exists, so nothing has to be invented; it has the fewest elements, so it fails least visibly; and if the first line is not strong enough to carry a card on its own, that is a finding about the post rather than about the card, and the correct response is to rewrite the hook and not to add a graphic device.
- **You cannot tell, because two archetypes fit equally well.** Pick the one you have used least recently. The value of the set is that all five appear. Running one for a month collapses a system into a single template, and a reader stops distinguishing your posts from each other, which is the opposite of what the system is for.
- **None of them fit, because there is no single line, no comparison, no figure, no chronology and no list.** The post does not get a card. A card with nothing to carry is decoration, and decoration costs production time and buys a shape the reader learns to ignore.

## Part six. Legibility and accessibility, which every social graphic guide omits

This section is not a compliance appendix. Three of the four items below are common ways a card silently fails, and the third is a checkable fact about your post that most people have never been told.

**One. Type size is judged at rendered size, not at canvas size.** Use the table in part three. The acceptance test is physical: export the card, display it at about 356 pixels wide, and look at it on a phone at arm's length, not zoomed. Anything you have to lean in for has already failed.

**Two. Contrast is judged against your own field colour, and large canvas type is not large text.** WCAG 2.2, current as of August 2026, sets 4.5:1 for normal text and relaxes to 3:1 only for large text, defined as 18pt or 14pt bold, which is about 24 CSS pixels or 18.66 bold. The catch is that the relevant size is the rendered one. A 56 pixel body line on the canvas renders at 16.6 pixels on a phone, so it is normal text and needs 4.5:1 no matter how large it looked while you were making it. **The honest default is to hold every reading role to 4.5:1 against the field and treat the 3:1 relaxation as unavailable.** Check each role once, when the palette is chosen, not after forty cards exist.

**Three. Text baked into an image is invisible to a screen reader unless you repeat it.** This is not a nuance, it is how the technology works: assistive software reads the alternative text and has no access to the pixels behind it. WCAG 2.2 SC 1.1.1 requires a text alternative that serves an equivalent purpose, and SC 1.4.5 asks that text be presented as real text rather than as an image of text wherever it can be, which is precisely the case a card like this is. So:

- **The card's punchline must exist as real text in the post body.** Verbatim, not paraphrased. The body is the better home for it than the alt text, because body copy is selectable, quotable and indexable, and alt text is none of those.
- **The alt text carries the card's content in reading order**, not a description of the card. For a two-column comparison that means both columns as a list, not "a comparison of two approaches". For a numbers card it means the figures and their labels, not "a chart".
- **A card that is genuinely decorative gets empty alt text** rather than a description nobody needs. That case is rare here, because a card in this system is never decorative by definition.
- **The alt text length limit is widely misquoted, so write to the documented recommendation.** LinkedIn's developer documentation for the images API, checked August 2026, gives the `altText` field a maximum of 4,086 characters and recommends **under 120**. LinkedIn's consumer help page for alt text states no limit at all. The 300 character figure repeated across scheduling tools is community-observed from the web composer, and the tools do not even agree with each other, quoting 120, 250, 256 and 300. Write to under 120 and never design a card whose meaning needs 300 characters to describe.

**Four, which follows from the palette.** Never let colour be the only thing carrying meaning. In the two-column archetype the signal role marks the wrong option, and a reader with a red-green deficiency sees a warm gold and a muted red as two similar mid-luminance warm colours. WCAG 2.2 SC 1.4.1 is the rule and the fix is cheap: the wrong column also gets a word, a strike, or a position that is itself the marker.

## Part seven. Production order

1. **Write the post.** All of it.
2. **Choose the archetype from the finished copy**, using part five.
3. **Extract the one line, one comparison or one figure that carries the pause.** If you cannot find it, go back to step one.
4. **Build to the spec.**
5. **Paste the punchline into the post body as text, and write the alt text.**
6. **Run the acceptance tests.**

**Never build the card first.** A card that exists before the copy becomes a constraint on the copy, and it will tempt you to write to fill a layout. The layout has no opinion about what is true.

### The acceptance tests

- **The scroll-speed test.** View at rendered size, scroll past once at your normal speed, then look away and say the punchline. A squint or a second look is a failure, not a near miss: what you have is a speed bump, not a stop.
- **The 7am test.** Picture the target reader one-handed with coffee, thumb moving. Would they stop? If not, redesign rather than tweak: bigger type and a brighter accent are a rendering fix applied to a content problem.
- **The system test.** Lay the last six cards out in a row at thumbnail size. If you cannot tell they came from one author, the system has drifted. If you cannot tell them apart, the archetype rotation has collapsed. Both are failures and they have opposite fixes.

## Worked example, compressed

A project management tool's founder writes a post arguing that the daily stand-up is a symptom of bad tooling rather than a practice worth defending. The opening line, already written: *Your stand-up is a status report your tools should have written.*

**Archetype.** No comparison, no figure, no chronology, no list. One sentence you would say out loud. **Bold statement**, and the decision took ten seconds because the rule is ordered.

**Build.** Setup lines top-left at 56px in secondary: "Fifteen minutes." / "Nine people." / "Every morning." Punchline at 120px in primary across three lines, with *should have written* in accent. A 1px accent rule at y = 940, and one insight line beneath it at 44px in secondary. Brand line at 18px monospace, bottom-left, in recede.

**Checks, in order.**

- *Margins.* All type inside the 1040 live area. The punchline block runs y = 460 to 820, which sits inside the 286 to 914 landscape crop band, so it survives being shown as a link-preview thumbnail.
- *Palette.* Primary 12.34:1, accent 6.02:1, secondary 6.14:1, all above 4.5:1. Signal is unused here, which is the only reason its 2.59:1 problem does not bite. One accent, on one phrase.
- *Type.* Three reading sizes: 120, 56, 44. At the cap, not over it. The 44px insight line renders at 13.1 pixels on a phone, above the 40 pixel floor but not by much. The brand line at 18px renders at 5.3 and carries nothing, which is correct, because the author's name is already on the post.
- *Accessibility.* The punchline goes into the post body verbatim as the first line. Alt text reads the three setup lines then the punchline, in that order.
- *Scroll test.* At 356 pixels the punchline renders at 35.6, the setup at 16.6, the insight at 13.1. The punchline lands. The insight does not, and that is by design: it is a reward for stopping, not a reason to stop.

**Verdict: ship it, with one change.** It passes every threshold, but it spends the whole three-size cap on a card whose entire job is one sentence, and the insight line sits closest to the floor of anything on it. Cut the setup from three lines to two, freeing the vertical space to take the punchline from 120 to 132 pixels, rendering at 39 rather than 35.6 on a phone. That is a 10 per cent gain on the only element that has to work at speed, bought by deleting a line nobody was going to read. **The general lesson: when a card is at its size cap and one element is near the floor, the fix is subtraction, and it is almost never a new size.**

## Failure modes, written as what each one signals

Each of these is a default that some tool will hand you. What matters is not that they are ugly. It is what a sceptical professional reader concludes about you within the half second the card gets.

**The meme or reaction image.** Signals: performing for engagement rather than saying something. Borrowed humour carries borrowed authority, and the people who reshare it are reliably not the people who buy.

**Stock photography.** Signals: generic. A handshake, a lightbulb, a team laughing at a laptop. The reader has seen the exact frame elsewhere, so the card announces that nothing in the post was specific enough to need a specific image.

**Your own face on the card.** Signals: influencer, and it is scrolled past by exactly the senior readers you want, who are scanning for a claim rather than a person. The avatar already carries the face. Repeating it spends the whole canvas on information the reader has.

**The busy multicolour infographic.** Signals: a junior social media manager made this. Six colours, four fonts, an icon per item, a legend. It fails legibility before it fails taste, because almost nothing on it clears the 40 pixel floor. It also signals that the argument needed nine boxes, which usually means there was no argument.

**Gradient and icon template output.** Signals: amateur, or worse, that this came out of the same tool as ten thousand cards that week. Gradient background, rounded card inside a card, a line icon in a circle, a soft shadow. Every element is a default, and defaults are the precise thing a recognisable system exists to escape.

**The wall of text.** Signals: this could have been the post, and reading it costs more than the post is worth. Eight lines on a card is a screenshot of a document. The mechanical tell is that it clears none of the three reading sizes.

**Everything centred.** Signals: greeting card, or certificate. The eye pays a fixation per line finding a moving left edge, and at scroll speed there are one or two fixations available in total.

**The watermark.** A large logo, a URL and a handle in a corner at full contrast. Signals: advertisement. The brand line is meant to be found by somebody who already stopped, and a watermark makes a claim on attention that has not been earned yet.

**Two accents.** Signals: nothing, which is the whole problem. When the figure and the punchline are both in the accent colour, neither is the answer, and the eye picks the larger one by default.

**Quotation marks and an attribution block.** Signals: aggregator. The convention was established by accounts reposting other people's sentences, so a card dressed that way reads as somebody else's line even when it is yours.

## What this skill does not do

- **It does not render the image.** It produces a specification: canvas, a value per role, type sizes, positions, an archetype and the alt text. A design tool or a rendering library makes the file, and both are better at that than any description of one.
- **It cannot see the result.** Every check in it is arithmetic on the spec. It can prove a colour fails a threshold and it cannot tell you a composition is unbalanced, which is visible only with the file open at the size the reader will see it.
- **It does not choose the typeface,** and the typeface decides more of the outcome than any rule here. Whether a face holds together at roughly 30 per cent depends on x-height, apertures and stroke contrast, and that has to be tested with the real font file rather than reasoned about.
- **It does not write the post,** and the production order says so twice. A card built for a weak hook is a well-specified card for a post that will not be read, and no amount of contrast fixes that.
- **It does not claim a card wins reach,** and it names the reason: the published ranking explanation does not list media format as a signal, the platform's own engagement claim carries no study, and the largest third-party benchmark disagrees with both. Any number of that kind should be re-derived from your own analytics before it changes a decision.
- **It does not cover carousels, video or multi-image posts.** Those are separate surfaces with separate crops, a first-frame problem this file does not address, and for document carousels a page-order problem it does not address either.
