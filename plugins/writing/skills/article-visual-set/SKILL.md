---
name: article-visual-set
description: Produces the visual set for a long-form article: exactly two visuals, each placed inside a defined band of the article measured in rendered words, with distinct jobs. Decides which visual each slot gets by walking a source priority ladder, binds the infographic word for word to the article's comparison table, enforces readability caps and a byte budget, forbids readable copy inside a raster image, verifies that every uploaded asset actually loaded rather than trusting an HTTP 200, and reuses the same asset URLs across every translated locale. This skill should be used when adding images to a long-form article, when an infographic and the table above it have started to disagree, when images are about to be regenerated per language, or when a design tool has failed mid-run and somebody is about to improvise a featured image.
---

# Article visual set

## The claim this skill is built on

The obvious approach to illustrating an article is to write it, notice it looks like a wall, and add images until it does not. That produces four to six pictures, mostly stock, placed where the writer felt a gap, each carrying alt text that is a category name rather than a sentence, and collectively adding several hundred kilobytes for no informational gain.

The claim here is narrower and it is checkable: **a long-form explanatory article needs exactly two visuals, they do different jobs, their positions are determined by the article's structure rather than by taste, and the second one is bound word for word to something already in the text.**

The binding is the part that matters. An unbound picture is decoration and drifts freely. A bound picture is a second rendering of a fact set that already exists, which means it can be checked by counting rows, and which means that when the facts change there is a defined place to look.

Scope: explanatory long-form of roughly 1,200 to 3,500 words with a body of prose and usually one comparison table. A step-by-step tutorial is a different genre that needs one screenshot per step, and none of this applies to it.

## Phase 1: two visuals, two bands, two jobs

**Fewer than two and the article is a wall.** The first scroll-depth cliff on a long article is the reader deciding the page is homogeneous, and that decision is made visually, before any of the words are read.

**More than two is decoration.** Each additional image is transfer weight, a layout shift risk, and one more thing for the reader to decide to skip. On a 2,000-word article, a third visual is almost always a restatement of the introduction.

**Band A, 30 to 50 per cent of the way through: the atmospheric visual.** Its job is to break the wall and to double as the featured image and social card. Placed before 30 per cent it reads as a header decoration and does not break anything, because the reader has not yet committed. Placed after 50 per cent the wall has already done its damage.

**Band B, 50 to 75 per cent through: the infographic.** Its job is to carry the article's comparison, so that a reader who only looks at pictures gets the same facts as a reader of the table. It must sit **after** the table it restates in document order, never before, because a picture that pre-empts the table makes the table redundant instead of reinforcing it. And it must sit before the last quarter, where the reader has either converted or gone.

Measure both bands **in rendered words**, not in headings and not in screens. Heading count varies by writer, and screen height is a property of the reader's device rather than of the article.

If the comparison table falls after 75 per cent, the article is structured wrongly. Move the table, not the image.

## Phase 2: slot division, and the source priority ladder

The two slots are not interchangeable and they do not draw from the same pool.

**The featured or thumbnail slot comes only from the brand template.** This is absolute and it has no fallback. If the template tool is unavailable on a given run, the correct degradation is to **publish with the featured image empty and log a backfill**. Never substitute a generated raster into the featured slot.

The reason is blast radius. The featured image is the one asset that appears on the article, on every index and category page, on every social card, and in every syndicated copy. It is also the asset nobody re-checks, because it is not in the body and a reader scrolling the article never sees it at full size. An off-brand substitute therefore propagates everywhere and is corrected nowhere. An empty slot is visible, ugly, and gets fixed within a day.

This rule regresses more than any other in the file. In a multi-property operation it was restated eight times per property and still regressed, because a missing image at publish time feels like a failure while an improvised one feels like a save. It is the other way round.

**For any in-body slot, walk this ladder in order and stop at the first fit:**

1. **Owned video.** If you already have footage of the thing, a video figure beats every static option and cost nothing to acquire.
2. **Reuse from the asset ledger.** An asset already produced, already hosted, already verified and already carrying alt text.
3. **Brand template.** Featured slot only. It never fills an in-body slot.
4. **Generate.** Last, and only when the three above return nothing.

**Reuse and template use are features, not compromises.** Tag them as such in whatever record the run produces. A run that reports "reused 2 assets, generated 0" has done better work than one that reports "generated 2", and if the record does not distinguish them, the operation will drift towards generating everything because generating feels like output.

## Phase 3: the verbatim restatement rule

**The infographic restates every row of the article's comparison table, verbatim.**

Not a summary. Not the top three. Not the highlights. Every row, in the same order, with the cell text copied rather than paraphrased.

The reason is drift, and it has a direction. The table and the picture are two public statements of the same fact set. When a price, a limit or a version changes, the table is corrected because it is a one-line edit. The picture is not, because correcting it means a re-render, a re-upload, a cache purge, and every locale again. So they do not drift randomly. They drift towards a correct table sitting above a stale picture, and nobody notices, because nobody has diffed an image against a table since the day both shipped.

Rules that follow directly:

- **Row count equality is a check.** Rows in the picture equals rows in the table. One number, checkable in a second.
- **Cell text is copied, not paraphrased.** A paraphrase is a second wording of the same claim, and a second wording is a second thing that can be quoted, screenshotted and disputed independently.
- **Column headers are copied too.** A table column headed "Monthly cap" and a picture column headed "Limit" are two different claims about scope.
- **If the article has no comparison table**, band B does not get an infographic by force. It gets a process diagram of the article's own ordered steps, under the same rule: every step, in order, verbatim, not a selection. See the decision rule below for the branches.

## Phase 4: readability caps that hard-fail

Each of these blocks the visual rather than raising a concern:

- **At most 60 words of rendered text in the entire visual**, counting labels, headers and cells. Beyond 60 words nobody reads the picture, they read the table, and the picture is a heavier duplicate of something the page already has.
- **At most 6 matrix rows.** A seven-row comparison rendered at body width produces rows too shallow to scan and type too small to read.
- **Minimum type size equivalent to 18pt at output.** Not 18pt in the authoring canvas. At output, at the width the page actually renders the image, which on a body-width slot is commonly 680 to 760 CSS pixels and on a phone under 400. A 10-point label inside a 1,600-pixel-wide export is illegible at both.

**When the table has more than 6 rows**, do not shrink and do not summarise. Render the **6 most decisive rows** and link to the full table in body text near the image.

"Most decisive" needs a stated criterion or it becomes cherry-picking, so: select the six rows on which the options differ most, meaning the widest spread of distinct values across the compared options. Never select by which rows flatter the recommendation. And the picture must carry a visible marker of what it is, in the form "6 of 9 rows shown", plus the body-text link. A subset that does not announce itself is a summary wearing the clothes of a complete restatement, which is the exact failure phase 3 exists to prevent.

## Phase 5: never bake readable copy into a raster

**No line of prose, no label, no number, no heading goes into a PNG, JPEG, WebP or AVIF as pixels.**

Four consequences, each concrete:

1. **Untranslatable.** The article ships in five locales. The picture ships in one. Every non-source reader gets an image in a language they may not read, sitting in the middle of an article that is otherwise theirs. Translating it means five renders, five uploads and five files that will drift.
2. **Uncorrectable.** A price, a version or a limit changes. The paragraph is a one-line edit. The image is a re-render, a re-upload, a cache purge and every locale again. In practice it does not happen, and the wrong figure stays live underneath a correct paragraph.
3. **Invisible to search.** Text in a raster is not text. It is not indexed, it contributes nothing to relevance, and it cannot be quoted by anything that reads the page as text, including the answer engines that are an increasing share of the page's audience.
4. **Unreadable to a screen reader.** A raster's entire text content is its alt attribute and nothing more. Sixty words of infographic behind a twelve-word alt attribute has silently discarded forty-eight words for every reader using assistive technology.

**What to do instead, in order of preference:**

- **Put the words in real markup adjacent to the image.** A caption element, a definition list, or the table itself. The picture carries the shape, the markup carries the words.
- **Author the visual as SVG with real text elements**, and inline it rather than referencing it from an image element, because an SVG inside an image element is opaque to the page and inherits every problem in the list above.
- **Only if neither is possible**, ship a raster and repeat every word that matters in the surrounding markup, so the picture is redundant rather than load-bearing.

**The test.** Delete the image and reread the passage. If the reader has lost a fact, the fact existed only in the picture and the rule has been broken.

This is how the 60-word cap and the no-raster-text rule fit together, and they are not in conflict: the cap is the ceiling on how much text a visual may carry **in any format**, because beyond that nobody reads it. The raster rule governs the **form** those words take. Sixty words as real text, never sixty words as pixels.

## Phase 6: alt text that carries the information

- **A sentence, not a label.** "Infographic" and "chart" carry nothing and exist to satisfy a counter.
- **The working band is 100 to 250 characters.** Below 100 it is almost certainly a label. Above roughly 250 a screen reader user is hearing a paragraph with no way to skim it, and the excess belongs in adjacent markup instead.
- **For the infographic, the alt text carries the comparison**, not the format. Name the options and the attribute on which they differ most. A reader who hears only the alt text should be able to state the article's conclusion.
- **For the atmospheric image, describe the subject, not the mood.** "A rack of network switches with one port lit amber" rather than "technology background".
- **Never begin with "image of" or "picture of".** The technology already announced that.
- A genuinely decorative image takes an **empty** alt attribute rather than a missing one, so assistive technology skips it instead of reading the filename. Note that under the two-visual rule nothing in this set is decorative, so an empty alt attribute here is a symptom rather than a compliance win.

## Phase 7: one aspect ratio per role, and the byte budget

**Aspect ratio is a site-level decision made once and held**, not an article-level choice. A body-width slot whose ratio follows the content produces a page whose rhythm changes for no reason, and a varying intrinsic ratio is the classic cause of layout shift.

- **Atmospheric and featured: 1.91:1.** Author at 1200 by 630. Every widely used card renderer centre-crops to something near this, which means **whatever matters must sit inside the central 60 per cent of the frame**. This is the single most common reason a share card shows a corner of something.
- **Infographic: pick 4:3 or 1:1 and hold it across the site.**
- **Always emit intrinsic width and height attributes**, or an equivalent aspect-ratio style, on every image element, so the browser reserves the box before the bytes arrive.

**Byte budget:**

- Atmospheric and featured: 150 KB target, 250 KB hard ceiling.
- Infographic: 120 KB target, 200 KB hard ceiling.
- Both together under 400 KB. On a long article the images are typically the largest category of transfer after fonts, and the featured image is very often the largest contentful paint element, so its transfer time is the metric that shows up in field data.

**Quantise before you downscale, not after.** The instinct is to shrink the dimensions first and then compress. That order costs you twice:

- Downscaling first destroys detail irreversibly, before the encoder has had any chance to decide what was cheap to keep.
- The resampled image is frequently **harder** to compress, not easier. Resampling introduces new intermediate colour values across regions that used to be flat, so an image with a few dozen distinct colours, which is exactly what an infographic is, arrives at the encoder with several thousand. A quantising or palette-based encoder then has more work to do, and the result is often no smaller than a quantised full-size version while being visibly softer.

So: quantise at the authored resolution first, inspect, and downscale only if the budget is still missed. For a visual authored as SVG the question never arises, which is one more argument for authoring it that way.

**Choose the codec by content, not by habit.** Flat regions and text-like edges go down a palette or lossless path: PNG-8 after quantisation, or lossless WebP. Continuous tone goes down a lossy path: WebP or AVIF, with JPEG where the pipeline demands it. A photograph as PNG is routinely four to eight times larger for no visible gain. An infographic as JPEG produces ringing around every edge, which is the visible symptom of the wrong path.

## Phase 8: host it, then prove it actually loaded

**HTTP 200 is not proof the asset is good.** A corrupt or truncated upload still returns 200, with an image content type, and renders as a broken box on a live page.

Verify one of these two things before the URL is used anywhere:

- The loaded image reports a **non-zero natural width**.
- The served bytes begin with a **real image magic number**: the PNG signature, the JPEG start-of-image marker FF D8, or the RIFF container marker for WebP. And specifically **not** a zlib header such as 78 DA, which means what got stored was a raw compressed stream rather than an image and is the signature failure of a mishandled upload.

If verification fails, **re-host rather than shipping the URL.** A URL that returns 200 and renders nothing is worse than a missing image, because every automated check reports it as present.

**Hosting mechanics worth stating explicitly:**

- **Ban inline base64 upload.** It inflates the payload by roughly a third, and long base64 strings get mangled in transit often enough that the failure mode is a file that uploads successfully and is corrupt.
- **Use a presigned upload flow**, and treat the presigned URL as short-lived. Expiries as short as 300 seconds are real and observed, so generate the URL and upload immediately rather than generating a batch of URLs and working through them.
- **The content type on the upload must match the content type in the presign request exactly.** A mismatch is commonly rejected, and where it is not rejected it is stored with the wrong type and served as a download.

## Phase 9: place it in markup the theme renders correctly

**Never put an image inside a blockquote.** Themes decorate blockquotes with a large opening glyph and a left rail. Around a paragraph that is a design. Around an image the glyph floats next to nothing, the rail runs down the side of a picture, and the result looks like a rendering bug rather than a choice.

More generally, the image goes in the markup the destination actually renders. Many content fields escape raw markup, which means a figure element written as HTML into a markdown field ships as literal angle brackets on the live page. Check the field's rendering mode once per system and write it down, because it is a property of the platform rather than of the article.

## Phase 10: the never-list, checked before publishing

Any single entry blocks the image. These are not preferences and they are not weighted.

**Content of the image:**

- An em dash anywhere in the image text.
- Any emoji.
- Placeholder text of any kind: lorem ipsum, "Tool A" and "Tool B", a bracketed insert left in, or a domain you do not own.
- A misspelling, or the wrong wordmark.
- Third-party platform interface or logos, because reproducing them implies an affiliation you do not have.
- Any price, limit or numeric cap that does not match the article's table **exactly**. This is the phase 3 rule again, arriving at the last gate.

**Provenance and craft:**

- Stock photography of people pointing at a screen, shaking hands, or standing around a laptop. It says nothing, it is recognisable as stock, and readers have learned to skip it.
- Generic abstract gradients, particle meshes, glowing circuit boards, or a human head made of glowing nodes. Same failure at a different budget.
- Any visible watermark or comp overlay, which nearly always means the licence was never bought.
- A screenshot containing a real account name, an email address, a token, an internal hostname or a customer's data. This is the only entry on the list that becomes an incident rather than an aesthetic complaint.
- A generated image with visible artefacts in text-like regions: mangled letterforms, invented axis labels, impossible hands.
- Any chart whose axis was truncated without saying so, or whose marks were scaled by radius rather than by area.
- Anything whose only relationship to the article is the topic keyword. If the caption would sit equally well under any of the last six articles, it carries nothing.

## Phase 11: the asset ledger, reuse and translations

**Append the real hosted URL to the ledger after every publish, never a placeholder.** A ledger row carrying a placeholder is worse than no row, because the next run will find it, treat it as a reusable asset, and reference a URL that was never real.

**Promote a one-off asset into the reusable set only after it has genuinely been reused in a second article.** Never pre-promote on the expectation that it will be useful. A reusable set full of hopeful entries makes step 2 of the source ladder return matches that nobody actually wants, and the run either uses a poor fit or learns to skip the step.

**Stamp any data-carrying asset with a freshness caveat naming the month it reflects.** An image showing prices, limits or market positions, reused a year later, is not a stale image. It is a published falsehood with a date nobody can determine.

**Across translations, reuse the same asset URLs.** Because the visuals carry no baked copy under phase 5, they are locale-independent by construction. Translate exactly two things: the alt text, and the sentence in the body that introduces the image. Never re-upload per locale. Re-uploading multiplies storage by the locale count, leaves the cache cold in every locale, and guarantees that the first correction applied to whichever file somebody happened to open silently diverges from the rest.

**The one exception**, and it must not be allowed to leak: an image that genuinely must contain words, such as a screenshot of an interface that is itself localised. That goes in a per-locale path with the source locale recorded, so a later corrections pass can find every variant.

## Decision rule: what goes in band B?

1. **The article has a comparison table with 3 to 6 rows?** Infographic restating it verbatim.
2. **A comparison table with 7 or more rows?** Infographic of the 6 most decisive rows, marked "6 of N rows shown", with a body-text link to the full table.
3. **An ordered process of 3 to 7 steps and no table?** A process diagram naming every step verbatim, in order.
4. **One quantitative series genuinely worth showing?** A chart, which then inherits the chart rules: an unbroken baseline on bars, area-scaled marks, a stated source and a stated date.
5. **None of those, because the piece is narrative or opinion?** Band B gets nothing and the article ships with one visual. This is the correct outcome and not a failure. Forcing an infographic onto a narrative piece produces a picture of the introduction, which is the most common decorative infographic there is.
6. **You cannot tell.** The article has a loose list that is neither a real table nor a real process, or a table whose rows are not parallel because they carry different units or different subjects, or a series with two or three points. Then: **do not build the visual from it, and do not improvise a shape.** Fix the article first. A non-parallel table cannot be restated verbatim in a picture, because the picture has to impose a regularity the table does not have, and imposing that regularity is precisely where invented facts come from. If the article cannot be fixed in this pass, ship one visual and record `band-b: deferred, table not parallel`. Shipping one visual is a small, visible loss. A picture that quietly regularises an irregular table is a false statement in a format nobody proof-reads.

## Worked example, compressed

An explanatory article of about 2,400 rendered words for a documentation site, comparing four backup strategies for a small self-hosted database, carrying one comparison table of 5 rows by 4 columns.

**Band A.** 30 to 50 per cent is words 720 to 1,200. The natural section break falls at word 940. The featured slot is filled from the brand template, authored at 1200 by 630, with the subject occupying the central 55 per cent of the frame so a centre crop to 1.91:1 keeps it. Alt text, 96 characters, describes the subject rather than the mood.

**Band B.** 50 to 75 per cent is words 1,200 to 1,800. The comparison table sits at word 1,380, so the infographic goes at word 1,520, after it. Source ladder: no owned video, the ledger returns no match, the template is featured-only, so it is generated. Five rows restated verbatim, five equals five, 47 words of text, well inside the 60-word cap. Authored as SVG with real text elements and inlined, so nothing is baked.

**Compression.** The template-supplied photograph is continuous tone, so it goes down the lossy path: encoded as WebP at the authored size and quantised to 138 KB, no downscale needed. A first attempt had been downscaled to 1,000 pixels wide before encoding and landed at 151 KB and visibly softer, which is the ordering failure showing up as a single measurement.

**Rejections.** A stock photograph of two people looking at a laptop is blocked on the cliche entry. A generated illustration is blocked because it carries three mangled words along the bottom edge. A draft of the infographic is blocked because its "Monthly cap" column said 500 where the table said 512.

**Upload.** Presigned flow, content type matching the presign request, uploaded within 40 seconds of the URL being issued. Verification: the served bytes begin with the RIFF marker and the loaded image reports a non-zero natural width. Both pass. An earlier attempt returned 200 with bytes beginning 78 DA, so it was re-hosted rather than shipped.

**Placement.** The figure sits in the body markup, not inside a blockquote, with intrinsic width and height attributes present.

**Translations.** Three locales. The same two asset URLs in all three. Alt text and the introducing sentence translated. Nothing re-uploaded. The ledger gets the two real hosted URLs appended, and neither is promoted to the reusable set, because neither has yet been used in a second article.

**Verdict: two visuals, one per band, all five table rows carried verbatim, zero rendered words inside any raster, 138 KB total image transfer, one aspect ratio per role, three locales sharing two files, and one featured slot filled from the template rather than improvised.**

## Failure modes

**Copy baked into a raster.** From the outside: an article live in four languages where one image is in the source language in all four, carrying a version number two releases out of date underneath a paragraph that is correct.

**The summarising infographic.** The picture shows three of five rows, and the two it dropped are the two where the article's own recommendation loses. It goes unnoticed for a year, and the first person to notice is a reader who screenshots the picture rather than the table.

**The improvised featured image.** A generated raster in the featured slot because the template tool was down for twenty minutes. It appears on every index page, every card and every syndicated copy, and it is the one asset nobody scrolls past, so nobody ever reports it.

**Decorative alt text.** Every image on the site carries alt text reading "infographic" or a filename. An accessibility report shows full alt coverage, because coverage is a count, and every image is still empty.

**Downscale then compress.** A file that looks worse than the original at roughly the same weight, with mushy edges around every text-like region. The measurement and the appearance disagree with the intent, which is why the ordering keeps getting reversed.

**The cropped subject.** The share card shows half a face, the edge of a monitor, or nothing but background. Correct in the article, wrong everywhere it is shared, and invisible until after it has been posted.

**The 200 that renders nothing.** A broken image box on a live page whose URL returns 200 with an image content type. Every automated link check reports it as present, so it survives every audit that counts rather than looks.

**Per-locale re-uploads.** Storage multiplied by the locale count, and six months later one language's diagram says five where the rest say four, because a correction was applied to whichever file the person editing happened to have open.

**The blockquoted image.** A large decorative quotation glyph floating beside a picture with a rail down its left edge, which readers reliably report as a broken page rather than as a design choice.

**The stale reused asset.** A price comparison generated in one year, promoted into the reusable set on hope, and dropped into an article the following year. It is not out of date, it is wrong, and there is no date on it for anyone to check against.

## What this skill does not do

- It does not draw anything. It decides how many visuals exist, where they sit, what they may contain, how heavy they may be, how they are hosted and how they are verified. Something else makes the picture.
- It does not judge whether a chart's encoding is honest. A chart can satisfy every rule here, sit in the right band at the right weight with excellent alt text, and still mislead through a truncated axis.
- It cannot verify that alt text is accurate, only that it is a sentence of about the right length carrying about the right content.
- It cannot licence an image, and the blocking list only catches the visible signals: a watermark, a third-party logo, a comp overlay. An unlicensed image with none of those passes.
- It says nothing about video, animation or interactive embeds beyond preferring owned video at the top of the source ladder. Those carry different weight, accessibility and placement rules entirely.
- It has no access to your image pipeline. Byte budgets, codec choice, intrinsic dimensions and the quantise-before-downscale ordering are instructions for whatever tool you use, and a build-time pipeline enforces all four better than a per-article procedure can.
