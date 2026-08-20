---
name: deck-generation-constraints
description: The specification and layout constraints that decide whether a programmatically produced presentation file opens correctly on somebody else's machine. Covers English Metric Units and the four different unit systems inside one file, the two slide sizes both called 16:9, why content must be placed after the slide size is fixed, the difference between layout placeholders and free text boxes, why shrink-to-fit text does not survive generation, estimating text width without a rendering engine, font substitution across Mac and Windows, image cropping and resolution, native charts against chart pictures, and the metadata a generator can set. This skill should be used when writing or debugging code that produces .pptx files, and whenever a generated deck looks correct on one machine and wrong on another.
---

# Deck generation constraints

## The claim this skill is built on

A generated deck rarely fails because the content was wrong. It fails because a number was expressed in the wrong unit, because the canvas was resized after the content was placed, or because the machine that opened it did not have the font.

This is not a design problem and no amount of care about the writing prevents it. It is specification mechanics, and the specification is unforgiving in a specific way: nearly every value in the file is an integer in a unit you would not guess, and passing a plausible-looking number in the wrong one produces a file that is perfectly valid and completely wrong. Nothing errors. The deck opens.

The second claim is that the defects cluster. When a generated deck is bad, it is usually bad for three or four independent reasons at once, and fixing the visible one leaves the deck still broken. So the useful thing is not a tip, it is the complete constraint list, checked in an order that fixes causes before symptoms.

**Scope, stated plainly: this does not generate a deck.** There are good generators, including a published skill from Anthropic that produces presentation files. Use one. This is the layer underneath, for the code that calls it and the file that comes out.

## The unit system, which is where most of the bugs live

Office Open XML measures geometry in **English Metric Units**. The definition is exact:

- **1 inch = 914,400 EMU**
- **1 centimetre = 360,000 EMU**
- 1 millimetre = 36,000 EMU
- 1 point = 12,700 EMU
- 1 pixel at 96 dpi = 9,525 EMU

The unit exists so that inches, centimetres, points and screen pixels all divide into it without remainder, which is why the numbers look arbitrary and are not. Positions (`a:off x, y`), sizes (`a:ext cx, cy`) and line widths are all EMU.

But the file does not use one unit system. It uses at least four, and mixing them is the most common generation bug there is:

| Value | Unit | Example |
| --- | --- | --- |
| Position, size, line width | EMU | 914400 is one inch |
| Font size (`sz`) | hundredths of a point | 1800 is 18 point |
| Rotation (`rot`) | sixty-thousandths of a degree | 5400000 is 90 degrees |
| Scale, line spacing percent, crop offsets | thousandths of a percent | 62500 is 62.5 percent |
| Space before and after (`spcPts`) | hundredths of a point | 600 is 6 point |

Passing 18 where hundredths of a point are expected gives 0.18 point text. Passing 100 where EMU are expected puts the shape roughly one ten-thousandth of an inch from the corner. Neither raises an error.

**Where the libraries hide it.** Each one makes a different choice and none of them protects you if you bypass it.

- **python-pptx** wraps everything in a `Length` type that is an integer number of EMU. `Inches(1)` is literally 914400, and `Cm`, `Pt` and `Emu` all return the same type, so you can mix constructors freely but a bare integer is interpreted as EMU. Font size is set with `Pt()` and stored as centipoints internally.
- **PptxGenJS** takes plain numbers in **inches**, and also accepts percentage strings for position and size, so a bare number there means something entirely different from a bare number in the XML.
- **The Open XML SDK** in C# takes raw EMU as long integers with no conversion helpers at all. Nothing is hidden and nothing is checked.
- **Apache POI** positions shapes in points through a rectangle, and exposes the conversion constants directly in its `Units` class, including 12,700 EMU per point and 914,400 per inch.

**Decision rule for a library you have not used before.** If its API takes a typed constructor, use it everywhere and never mix in a bare integer. If it takes bare numbers, find the documented unit before the first call rather than after the first render. **If you cannot tell from the documentation**, write one shape at a known size, then unzip the .pptx (it is a ZIP archive), open `ppt/slides/slide1.xml`, and read the `a:ext` values. One inch will show as 914400 or it will not. That takes two minutes and settles it permanently.

## Slide dimensions, which have to be decided first

Three sizes account for nearly everything:

- **Widescreen 16:9, the modern default: 13.333 by 7.5 inches**, which is exactly **12,192,000 by 6,858,000 EMU**.
- **The older 4:3: 10 by 7.5 inches**, which is **9,144,000 by 6,858,000 EMU**.
- **The other 16:9: 10 by 5.625 inches**, which is **9,144,000 by 5,143,500 EMU**. This is the widescreen size used by older versions of PowerPoint and by several web-based editors, and it is a genuine trap because it has the same aspect ratio and a third less absolute size. Font sizes are absolute, so 24 point body text occupies a third more of the slide on this canvas than on the 13.333 inch one. A deck that reads well in one place looks oversized in the other, and nobody can see why because the shape of the slide is identical.

Use the exact EMU value rather than the decimal inch. `Inches(13.333)` evaluates to 12,191,695 EMU, 305 short of the real width. That is about a third of a thousandth of an inch, far too small to see, but it makes any equality check against 12,192,000 fail and it accumulates quietly in grid arithmetic where you divide the width into columns.

**Changing the slide size after content is placed reflows nothing.** When you resize interactively the application offers to maximise or to ensure fit, and both of those rescale existing content. A programmatic change to the presentation's size element does neither: every shape keeps the offsets it already had, so content sits where it was and anything beyond the new edge falls off the canvas. **Set the size on the presentation before you add the first slide.** There is no cheap fix later.

Two library defaults worth checking against the version you have installed, because defaults change between releases: python-pptx creates a 4:3 presentation when you call it with no template, and PptxGenJS defaults to the 10 by 5.625 widescreen layout with the 13.333 inch one available as a named alternative.

## Placeholders against free text boxes

This is the mechanism behind the single most common complaint about generated decks: *it used our template and it looks nothing like our deck*.

Content placed in a **layout placeholder** inherits through a chain. The placeholder resolves against the slide layout, the layout against the slide master, and the master against the theme, which is where the heading typeface, the body typeface and the twelve theme colour slots actually live. Text in a placeholder therefore comes out in the template's fonts and colours without your generator specifying anything, and it re-styles automatically when the theme changes.

Content placed in a **free text box** inherits almost none of that. It picks up the master's default text style and comes out at the presentation default size in the default typeface with no link to the theme colours. The background is still the template's, because the background comes from the master. Everything on top of it is not.

So a generator that loads a corporate template and then writes every slide with an add-a-textbox call produces exactly the reported symptom: correct background, correct dimensions, none of the typography.

**The rule.** Fill layout placeholders. Enumerate the layouts in the supplied template by name and inspect each one's placeholders and their index values, because the ordering is template-specific: the familiar built-in order where index 1 is Title and Content and index 6 is Blank holds only for the stock template, and a customer's template will differ. Reach for a free text box only when the layout genuinely has no slot for the thing, and when you do, set the font by theme reference (the major and minor latin font references) rather than by literal typeface name, so a theme change still reaches it.

## Autofit does not survive generation

The shrink-text-on-overflow behaviour is not a rendering rule. It is a **stored calculation result**.

The body properties of a text frame carry one of three settings: no autofit, shrink text on overflow, or resize shape to fit text. The shrink setting carries two attributes, a font scale and a line-space reduction, both in thousandths of a percent. Those attributes hold the answer the application computed when somebody edited the text. They are not instructions to a renderer.

A file written programmatically almost always carries the shrink element with **no attributes at all**, which means shrink is enabled and the computed factor is one hundred percent. Nothing has been shrunk. The text overflows its box, and it will keep overflowing until an application recalculates, which happens when the text is edited rather than when the file is opened.

The consequences are precise and they are why this defect is so persistent:

- It looks correct on the machine where somebody clicked into the box, which is usually the author's.
- It overflows in the PDF export, in the slide thumbnails, in image renders, and in every viewer that does not implement the calculation.
- Different applications behave differently. Some recompute on open, some render the stored factor as written, and you cannot control which one your reader has.

**The working response is to not use it.** Measure the text yourself and set an explicit font size that fits. Optionally also write a plausible font-scale value so a non-recalculating renderer at least shrinks something, but treat that as insurance rather than a solution. The same applies in the other direction to resize-shape-to-fit: the shape's height in the file is whatever you wrote, so the box does not grow.

Some libraries offer a fit-text helper. It needs access to the actual font file to measure, it is documented as approximate, and it only runs when you call it during generation, never on the reader's machine.

## Measuring text without a rendering engine

You cannot know exactly how wide a string will be without shaping it in the real font. Kerning pairs, ligatures, hinting and the renderer's own rounding all move the number by a percent or two, and complex scripts move it much more.

Three tiers, in order of preference:

1. **You have the font file.** Sum the advance widths of the glyphs at the target size with a font metrics library. For Latin text without complex shaping this lands within about a percent of the truth, which is close enough to make a fitting decision.
2. **You do not have the font file.** Estimate. For a typical sans face at size *S* points, mixed-case English prose averages roughly **0.5 S per character**; all-capitals and strings of digits run wider, roughly **0.6 to 0.7 S**; condensed faces run lower. These are approximations offered as approximations, not measurements.
3. **Vertical space.** Single line spacing is about **1.2 times the point size** for most Latin faces, so an 18 point line occupies about 21.6 points, which is 0.3 inch, which is 274,320 EMU. Multiply by the line count and compare against the box height.

**The safety margin is what makes it survive.** Whatever the estimate, allow **15 to 20 percent headroom in width and one full spare line in height.** The failure is asymmetric: text set slightly smaller than it could be is invisible to everyone, and text 5 percent too wide wraps and pushes a line out of the box, which is visible from the back of the room.

If the deck will be translated, budget more. Short strings expanding from English commonly grow by around 30 percent into German and by 20 to 25 percent into French or Spanish. Those are rules of thumb from localisation practice, not guarantees, and a title that fits exactly in English is a title that will break in translation.

## Fonts, and the machine you are not sitting at

The file stores a typeface **name**. It does not store the font. When the opening machine does not have that font, the application substitutes another one, silently, with different metrics. Line breaks move. A two-line title becomes three. Text that fitted overflows.

**Mac and Windows differ, and the difference is not cosmetic.** Office ships a shared core on both platforms, including the current default face and its predecessor, which had been the default since 2007 and was replaced in a change announced in 2023 and rolled out through 2024. But the *system* font sets diverge: the Windows interface faces are not on a stock Mac, and the Mac interface faces are not on a stock Windows machine. Verified as of August 2026, the faces you can rely on across both without an Office installation are the old core web set: Arial, Times New Roman, Courier New, Georgia, Verdana and Trebuchet MS. If you are choosing a typeface with no information about the destination, choose from that list.

**Embedding is the partial fix.** PowerPoint can embed fonts in the file. It has been available on Windows for many versions and arrived later on Mac, so check the version in front of you rather than assuming. There are two modes: embed only the characters in use, which is smaller and blocks later editing of that text, and embed all characters, which is larger and stays editable.

The constraint is licensing, and it is enforced mechanically. An OpenType font carries an embedding permission value in its metadata, with settings that amount to installable, editable, preview and print only, or restricted. A font marked restricted will not embed, and the application will refuse rather than warn. Many commercial desktop licences are exactly that. Embedded fonts are also commonly dropped when the file passes through a different editor or a web viewer, so embedding protects the direct path and not the forwarded one.

The fallback that always works, used sparingly: render the one piece of text that must be exact as an image, and put the words in its alt text. It costs editability and buys certainty.

## Images

**Aspect ratio.** The display size is the shape's extent; the source pixels are whatever you put in the package. Set one dimension and compute the other from the source ratio. Setting both independently produces a stretch that everybody notices and nobody can name, and it is most obvious on faces and logos.

**Cropping is not trimming.** A crop is stored as four offsets into the source rectangle, in thousandths of a percent, and it is a display instruction. **The full original image stays in the package.** Cropping a twelve megapixel photograph down to a thumbnail does not remove a single byte. Only an explicit compression pass removes pixels, which the application offers as a command and which a generator has to do for itself before packaging.

**Resolution, wasted against needed.** For projection or a video call, a full-bleed image on a 13.333 inch slide needs about **1,920 pixels** across to be pixel-perfect on a 1080p display and about **2,560** on a 1440p one. Anything beyond that is storage nobody sees. For print at 300 pixels per inch across the same slide you would need about **4,000 pixels**, which is a real requirement when the deck goes to a printer and pure waste when it does not. Decide which case you are in before you choose the assets.

**File size follows directly.** A .pptx is a ZIP, and JPEG and PNG data are already compressed, so the package is roughly the sum of the media plus a little XML. Twenty screenshots at 3 MB each is a 60 MB file, and common mail attachment limits sit around 20 to 25 MB depending on the service. Photographs as JPEG, flat-colour screenshots and diagrams as PNG, logos as SVG where the toolchain supports it, noting that an SVG is stored alongside a raster fallback so the file carries both.

## Tables and charts

**A chart written as a picture** is a rectangle of pixels. It cannot be edited, its numbers cannot be corrected without regenerating it, it does not restyle when the theme changes, and it is invisible to assistive technology unless you set alt text. It is also completely predictable: it looks identical on every machine, which is not nothing.

**A native chart** is a separate part in the package with its **own embedded workbook**. The data travels with the deck, which is the entire point, and it is also the risk. Anyone who receives the file can open that workbook, and it can contain more than the plotted series, up to and including the full source sheet with the rows you filtered out. Check what is inside it before the deck leaves the organisation.

**The trade-off rule.** Native chart when the deck is a working document that people will edit and interrogate. Picture when the deck is a final artefact going to an audience, when the styling must be exact, or when the underlying numbers must not travel. **If you cannot tell**, use a native chart backed by a workbook trimmed to only the plotted series, because the confidentiality problem has a fix and un-editability does not.

**Tables** must be real table objects, not a grid of text boxes. A grid of text boxes cannot be navigated, misaligns the moment any cell's text changes length, and is announced to a screen reader as a pile of unrelated fragments.

## Notes, alt text and the metadata almost no generator sets

- **Speaker notes** live in a separate notes part, one per slide. A generator can write them in a single call and almost none do, which leaves the deck's argument nowhere at all.
- **Alt text** is one attribute on the shape's non-visual properties. It is the cheapest accessibility improvement available to a generator and it is usually skipped.
- **Marking an image decorative** is not a core attribute. It is a vendor extension inside the shape's extension list, which is why some libraries cannot write it and some readers ignore it. Where you cannot write it, empty alt text is the pragmatic fallback and is better than leaving a filename in the field.
- **Slide titles** should be real title placeholders with unique text. The outline view, the navigation and assistive technology all read the title placeholder, so a title drawn as a free text box means the deck has no titles at all as far as those tools are concerned.
- **Document properties and language.** Set the document title and the language attribute on text runs. The language setting decides which dictionary checks it and which voice reads it aloud.

## Verification, and what verification cannot reach

After generating and before shipping:

1. Confirm the package is a valid archive with the parts you expect.
2. Check every shape's offset plus extent against the slide size, and report anything extending past the canvas. This catches the whole off-canvas class mechanically.
3. Estimate text width against box width for every text frame and flag anything over about 85 percent.
4. Confirm every slide has a title placeholder with non-empty, unique text.
5. Confirm every picture has alt text or is explicitly decorative.
6. Confirm no typeface name appears outside your approved list.
7. Render to PDF or images with a headless converter and look at the output.

**The honest statement: none of that is complete.** The only complete check is opening the file in the application your readers will use, on the platform they will use, at the size it will be shown. A converter's renderer is not the application's renderer, and the gap between them falls exactly on autofit and font substitution, which are the two defects this whole file is about. Budget five minutes. There is no substitute for it.

## Worked example

A generated 22-slide deck. Two symptoms reported: titles overflow on six slides, and the deck looks nothing like the supplied template. The deck and its symptoms are invented for this example; the unit constants are not.

**Trace.** The generator created the presentation with no template argument, inheriting a 4:3 canvas at 9,144,000 by 6,858,000 EMU, then placed shapes using coordinates computed for a 13.333 inch slide. Everything past 10 inches is off the canvas, which accounts for content missing on the right of almost every slide. The template was applied afterwards by setting the slide size to 12,192,000 by 6,858,000, which moved nothing, because a size change never reflows.

The branding symptom is separate. Every text element was created as a free text box rather than filled into a layout placeholder, so none of them resolve the theme's heading and body fonts or the theme colours. The background came from the master; everything on top of it did not.

The overflowing titles are a third cause. Those text frames carry shrink-on-overflow with no computed scale factor, so shrink is enabled and has never run. On the author's machine the boxes that were clicked into look right. In the PDF export, all six overflow.

A fourth problem is not a code defect at all: the template's heading font is a licensed corporate face installed on the design team's machines and absent from the sales team's, so the sales team sees a substituted face with wider metrics, pushing two further titles onto a second line.

**Verdict: four independent causes, three of them code and one of them distribution.** Fix order matters. Set the slide size before creating any slide, fill layout placeholders instead of creating text boxes, measure and set the title font size explicitly rather than relying on autofit, and for anything leaving the organisation either embed the corporate face, if its licence permits, or fall back to a cross-platform one. Fixing only the overflow, which was the loudest symptom, would have left three of the four in place.

## Failure modes

**Points where EMU are expected.** A shape placed at 100 meaning points lands at 100 EMU, about a ten-thousandth of an inch. Looks like a deck where every slide is empty and there is a small smudge in the top-left corner.

**Font size in the position unit, or the other way round.** Writing 18 for an 18 point font gives 0.18 point text, invisible. Writing an EMU value gives a single letter that fills the slide.

**Slide size set after content.** Every slide has content drifting off the same edge by the same amount, which is the tell that distinguishes it from a per-slide layout bug.

**Everything in free text boxes.** The template is applied, the theme is present, and no text uses either. Reported by the person who supplied the template as the tool ignoring their branding.

**Trusting autofit.** Correct on the machine that edited the file, overflowing everywhere else and in every export. The likeliest defect to survive review, because the reviewer is usually the person who clicked into the box.

**Aspect ratio set from two independent numbers.** Images subtly stretched throughout. Nobody articulates it; the deck just looks cheap.

**Cropping to save space.** The crop is a display instruction and the bytes remain, so the "compressed" deck is still ninety megabytes and nobody can work out why.

**Hardcoded layout indices.** Index 1 is Title and Content in the stock template and something else entirely in a customer's, so the title lands in a subtitle slot and the body lands nowhere.

**Assuming decimal inches are exact.** 13.333 is not thirteen and a third. Visually harmless, breaks equality checks, and accumulates when you divide the width into columns.

## What this skill does not do

- It does not generate a presentation. That is well covered elsewhere, including by a published skill from Anthropic, and duplicating it would be a waste.
- It does not know your template. Layout names, placeholder index values and theme colour slots are specific to the file you supply, so it will tell you to enumerate them rather than hand you numbers that are probably wrong.
- It does not measure text accurately without the actual font file. Its estimates are labelled as estimates because that is what they are.
- It does not verify rendering. It reasons about the file, and the last two questions are only settled by opening it on the target platform.
- It does not cover narrative, slide density, chart honesty or visual design. Those are separate problems with separate files.
- It does not cover Keynote or Google Slides native formats, and it cannot promise anything about a round trip through either, which routinely drops embedded fonts and can rewrite chart parts.
