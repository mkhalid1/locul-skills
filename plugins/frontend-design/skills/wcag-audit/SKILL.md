---
name: wcag-audit
description: Audits a page or component set against WCAG 2.2 level A and AA, organised by the test that finds each defect rather than by criterion number. Covers text alternatives and their four distinct cases, information and relationships, contrast ratios and the pixel thresholds that define large text, reflow at 320 CSS pixels, the text spacing override, content on hover or focus, form errors and error prevention, status messages, and the nine criteria added in WCAG 2.2 including target size and dragging alternatives. It states what an automated scan genuinely settles, the ordered manual pass that catches the rest, conformance mechanics for full pages and complete processes, and a severity model for turning findings into a fixable list. This skill should be used when auditing a page for accessibility, when preparing or checking a conformance claim or accessibility statement, or when an automated scan has come back clean and someone needs to know what it did not look at.
---

# WCAG 2.2 audit

## The claim this skill is built on

An accessibility audit fails in one of two ways, and neither is a lack of care.

The first is testing by list. Someone works down the criteria in numerical order and produces a document proving each was considered, without ever putting the page under the conditions that break it. Criterion 1.4.10 is not evaluated by reading it. It is evaluated by making the viewport 320 CSS pixels wide and looking for a horizontal scrollbar.

The second is trusting the scan. A rule engine can tell you an image has no alt attribute and can never tell you the alt text says "image123.png". Published estimates of automated coverage vary by method and by page and cluster somewhere around a third of what a real audit finds. A page with zero violations is a page whose mechanical defects are fixed, which is not the same thing as an accessible page.

This skill is the criteria arranged by the test that finds each one, with the per-criterion procedure and the actual numbers.

## What you are held to

WCAG 2.2 became a W3C Recommendation on 5 October 2023, and is backwards compatible: content conforming to 2.2 also conforms to 2.1 and 2.0.

**Level AA is the level almost every procurement requirement, contract clause and legal reference names.** Level A alone is treated as sufficient nowhere that matters, and the W3C does not recommend AAA as a whole-site policy because some AAA criteria cannot be met for all content.

AA conformance under 2.2 means 55 success criteria: 31 at A and 24 at AA. WCAG 2.1 required 50.

The version binding you is often older than the current one, because regulations reference a fixed version and lag it. Auditing against 2.2 satisfies an obligation stated in 2.0 or 2.1, with one exception: 4.1.1 parsing was removed in 2.2 and still exists in an older obligation. Check the version named in your contract before writing a conformance claim.

## The four principles as a defect taxonomy

Useful as a classification of what kind of defect you are hunting, useless as a mnemonic.

**Perceivable.** The information exists, the user cannot receive it. Missing alt text, an unlabelled chart, grey on light grey, uncaptioned video, a layout needing horizontal scrolling at zoom. Found by looking, measuring and overriding, and the largest group by count.

**Operable.** The information arrives, the user cannot act on it. Keyboard traps, targets too small to hit, drag-only reordering, a timeout that expires mid-read. Found by putting the mouse away and measuring targets.

**Understandable.** The user receives and can act but cannot tell what to do or what went wrong. Unlabelled inputs, "invalid input", a control that navigates on focus, a wrong `lang` that makes a screen reader read French with English phonemes. Found by using the failure paths.

**Robust.** It works in a browser and breaks in assistive technology, because the markup does not expose what the visuals imply. Custom controls with no name, role or state; a status message never announced. Found in the accessibility tree.

If every finding sits under one principle, the method is one-dimensional.

## The criteria, by what you check and how

Keyboard operability, tab order, focus visibility and focus obscuring are covered by a separate focus and keyboard audit. Named here for completeness, not developed.

### Perceivable

**1.1.1, text alternatives (A).** Four distinct cases, one answer usually applied to all of them.

- *Decorative.* A flourish, a divider, an icon repeating adjacent text. Correct alt is `alt=""`: present and empty, never absent, because a missing attribute makes some screen readers read the filename.
- *Functional.* The image is the whole content of a link or button. Describe the destination or action, not the picture. A printer icon that prints is "Print", not "Printer".
- *Informative.* Describe the information, not the medium. Drop "image of", which the role already announces.
- *Complex.* Charts, maps, infographics. Short alt plus a longer description elsewhere, or the data as a table. Test: could someone reconstruct the conclusion from the alternative alone?
- *Text in an image.* Alt contains the text verbatim. Separately 1.4.5 (AA) says do not use images of text where real text would do, except logos and where a presentation is essential.

**1.2.1 to 1.2.5, time-based media (A and AA).** Prerecorded video with audio needs captions (1.2.2, A) and audio description (1.2.5, AA). Audio-only needs a transcript and video-only a description (1.2.1, A). Live video needs live captions (1.2.4, AA). Auto-generated captions are a starting point, not a pass: sample thirty seconds containing a proper noun and the quality question answers itself.

**1.3.1, info and relationships (A).** The highest-value criterion and the one automation is worst at. Anything conveyed visually as structure must exist in the markup as structure. A heading that is only a bold paragraph fails; a list that is only lines starting with a dash fails; a label sitting beside its input with no `for` and no wrapping fails; a data table using `<td>` where the header row needs `<th scope="col">` fails. Test in the accessibility tree, or disable the stylesheet and ask whether the document still reads as a structure.

**1.3.2, meaningful sequence (A).** DOM reading order matches intended reading order. Separate from tab order. Usual cause is a CSS grid or flex `order` that moves content visually only. Test with CSS off or the source order overlay in developer tools.

**1.3.3, sensory characteristics (A).** No instruction relies solely on shape, size, position, orientation or sound. "Click the round button on the right", "see the box below", "press the green icon" each fail. Adding the name fixes it: "select Continue, the round button on the right" passes.

**1.3.4 orientation (AA), 1.3.5 identify input purpose (AA).** No lock to portrait or landscape unless essential, because a phone mounted on a wheelchair does not rotate. Inputs collecting information about the user carry the right `autocomplete` token: `name`, `email`, `tel`, `street-address`, `postal-code` and the rest of the defined list.

**1.4.1, colour as the only means (A).** Required fields marked only in red. A colour-only chart legend. A link in body text with no underline, which passes only if it differs from surrounding text by at least 3:1 *and* something non-colour appears on hover and focus. Test with a greyscale screenshot: does the distinction survive?

**1.4.3, contrast minimum (AA).** The number everyone half-remembers.

- **4.5:1** for normal text.
- **3:1** for large text, meaning at least 18 point or 14 point bold, which in CSS is **24 pixels regular or 18.66 pixels bold**.
- Exempt: logotypes, purely decorative text, text in an inactive control, incidental text in a picture.
- Fails most at: placeholder text, helper text, disabled-looking active text, and text over photographs, where you sample the worst point of the image rather than the average.
- Sample the computed colours, not the design file. Alpha over an unexpected background computes to something else.

**1.4.2, audio control (A).** Anything auto-playing beyond three seconds needs pause, stop or independent volume.

**1.4.4, resize text (AA).** Text scales to 200 percent with no loss of content or function. Browser zoom usually satisfies it; text-only enlargement is the harder case, and fixed-height containers with `overflow: hidden` clip.

**1.4.10, reflow (AA).** Works at **320 CSS pixels wide** with no two-dimensional scrolling, and at 256 CSS pixels of height for horizontally scrolling content. 320 is 1280 at 400 percent zoom. Exceptions for content genuinely needing two-dimensional layout: data tables, maps, diagrams, toolbars, whitespace-significant code. Test at 320 by 256 and look for a document-level horizontal scrollbar. Usual causes: fixed widths, wide tables outside a scroll container, a sticky header eating the viewport height.

**1.4.11, non-text contrast (AA).** **3:1** for two forgotten things: the visual boundary of components needed to identify them, and the parts of a graphic needed to understand the content. In practice, input borders, an unchecked checkbox, focus indicators, toggle tracks, icon-only buttons and chart series. A 1 pixel light grey input border on white is the commonest instance.

**1.4.12, text spacing (AA).** No loss when all four are applied at once and nothing else changes: line height at least **1.5** times font size, paragraph spacing at least **2** times, letter spacing at least **0.12** times, word spacing at least **0.16** times. Apply them together with a bookmarklet or injected stylesheet. The failure is clipping and overlap in fixed-height containers, invisible until all four are on.

**1.4.13, content on hover or focus (AA).** Every tooltip, popover and hover menu must satisfy three separate requirements, and most satisfy one. *Dismissible*: closes without moving pointer or focus, normally by Escape. *Hoverable*: the pointer can move onto the content without it vanishing, which rules out a gap between trigger and tooltip. *Persistent*: stays until hover or focus leaves, the user dismisses it, or the information becomes invalid, so a tooltip on a timer fails.

### Operable

**2.1.1 keyboard, 2.1.2 no keyboard trap, 2.4.3 focus order, 2.4.7 focus visible, 2.4.11 focus not obscured (minimum), 2.5.3 label in name.** Covered by the separate focus and keyboard audit.

**2.1.4, character key shortcuts (A).** A single-character shortcut with no modifier must be switchable off, remappable, or active only on focus, because single letters collide with screen reader navigation keys.

**2.2.1, timing adjustable (A).** A time limit must be turnable off, adjustable to ten times the default, or extendable with a warning giving at least twenty seconds and twenty attempts.

**2.2.2, pause, stop, hide (A).** Anything moving, blinking, scrolling or auto-updating past five seconds needs a pause mechanism: carousels, tickers, animated backgrounds. **2.3.1, three flashes (A):** nothing flashes more than three times a second unless below the general and red flash thresholds. **2.4.1, bypass blocks (A):** a skip link, or landmarks and headings that let repeated content be skipped.

**2.4.2, page titled (A).** A `<title>` describing topic or purpose. In a single-page application the title must change on route change, which is where it fails and where a crawl never sees it.

**2.4.4, link purpose in context (A).** Determinable from the link text, or from the text plus its programmatically determined context: same sentence, paragraph, list item, table cell, or the cell's headers. "Read more" repeated down a page of cards may survive a strict reading and still fails the users it protects, because a links list shows forty identical entries. Treat it as a defect.

**2.4.5 multiple ways (AA), 2.4.6 headings and labels (AA).** More than one route to a page in a set, navigation plus search, a sitemap or an index, excepting a page that is a step in a process. And headings and labels that describe topic or purpose: headings all reading "Overview" fail, and so does an input labelled "Field 3".

**2.5.1 pointer gestures, 2.5.2 pointer cancellation, 2.5.4 motion actuation (all A).** Path-based or multipoint gestures need a single-pointer alternative unless essential, so pinch-zoom on a map is exempt and swipe-to-delete is not. Actions fire on the up event, or are abortable or reversible, so someone who pressed the wrong control can slide off before releasing. Shake-to-undo and tilt-to-scroll need a control alternative and a switch to turn the motion trigger off.

**2.5.7, dragging movements (AA), new in 2.2.** Anything operated by dragging must also work with a single pointer without dragging. A board needs a move menu, a slider needs clickable increments or an input, a drag-to-reorder list needs up and down buttons. Exceptions: dragging is essential, or the behaviour is the user agent's and unmodified.

**2.5.8, target size minimum (AA), new in 2.2.** Targets at least **24 by 24 CSS pixels**, with five exceptions. *Spacing*: a smaller target passes if a 24 pixel diameter circle centred on it does not intersect another target's circle, which is what allows a padded toolbar of small icons. *Equivalent*: another control on the page does the same job at full size. *Inline*: the target sits in a sentence or is constrained by surrounding line height, so a link in a paragraph is exempt. *User agent control*: the size is the browser's and unmodified. *Essential*: a specific presentation is essential or legally required, such as a map pin at its real location. Measure the hit area, not the glyph. Usual failures: modal close buttons, table row action icons, pagination numbers, compact switches.

### Understandable

**3.1.1 language of page (A), 3.1.2 language of parts (AA).** `<html lang>` present and correct, and a phrase in another language carrying its own `lang`, excepting proper names and naturalised technical terms. A wrong value is worse than a missing one, because the screen reader applies the wrong phoneme set and the text becomes noise.

**3.2.1 on focus (A), 3.2.2 on input (A).** Receiving focus never changes context: no navigation, submit, new window or focus move. Changing a setting never changes context automatically unless the user was told first, and the classic failure is a country `<select>` that submits on change, stranding a keyboard user on the second option.

**3.2.3 consistent navigation (AA), 3.2.4 consistent identification (AA).** Repeated navigation in the same relative order on every page, and same-function components named the same way throughout. "Delete" here, "Remove" there and "Bin" elsewhere is a real 3.2.4 failure for anyone navigating by name.

**3.2.6, consistent help (A), new in 2.2.** Where a set of pages offers help, contact details, a contact form, a chat widget, a self-help link, it appears in the same relative order on each page that has it. Not on every page: in the same place on the pages where it appears.

**3.3.1, error identification (A).** A detected error is identified and described **in text**. A red border alone fails, and so does an icon alone.

**3.3.2, labels or instructions (A).** Every input labelled. Format requirements stated before the user gets them wrong. Required fields indicated in text, not by colour alone.

**3.3.3, error suggestion (AA).** Where the correction is known, suggest it: "Enter a date as DD/MM/YYYY", not "Invalid date". Exception where suggesting would compromise security, which is why a login failure may stay vague.

**3.3.4, error prevention for legal, financial and data commitments (AA).** For a page creating a legal commitment, moving money, modifying or deleting user data, or submitting test responses, one of three must hold: **reversible**, **checked** for input errors with a chance to correct, or **confirmed** through a review-and-correct step before finalising. Auditing this means walking the whole process.

**3.3.7, redundant entry (A), new in 2.2.** Information already entered in the same process is auto-populated or selectable rather than retyped. The instance everyone has hit is a checkout asking for a billing address with no "same as delivery" option. Exceptions for essential re-entry, security, and values no longer valid.

**3.3.8, accessible authentication minimum (AA), new in 2.2.** No authentication step may require a cognitive function test, remembering a password, transcribing a code, solving a puzzle, unless there is an alternative, an assisting mechanism, object recognition or personal content. In practice: do not block paste into a password field, support password managers, and treat a text-transcription CAPTCHA as a failure.

### Robust

**4.1.1 parsing was removed in WCAG 2.2 and marked obsolete.** This matters twice. Older checklists and tool configurations still report duplicate ids and unclosed tags as a 4.1.1 failure, which is wrong under 2.2. And the underlying problem did not vanish: a duplicate id that breaks a `for` reference or an `aria-labelledby` is still a failure, now of 1.3.1 or 4.1.2, so report it under the criterion it actually breaks. If your obligation names 2.0 or 2.1, 4.1.1 still applies to you.

**4.1.2, name, role, value (A).** Every component exposes a name, a role, its state and value, and changes are notified. This is the criterion behind every custom control defect and it needs its own procedure: a separate accessible name and role audit carries the name computation order, the states that must stay in sync, and how to read the accessibility tree.

**4.1.3, status messages (AA).** Anything reporting status without moving focus must be programmatically determinable. "3 results found", "Saved", "2 items remaining", an error summary appearing without focus moving. The mechanism is a live region, `role="status"`, `role="alert"` or `aria-live`, present in the DOM *before* the message is written into it. The commonest failure is a container created and populated in one operation, which announces nothing.

## What WCAG 2.2 changed

Most published guidance predates October 2023, so state this on its own.

**Added at A:** consistent help (3.2.6), redundant entry (3.3.7). **Added at AA:** focus not obscured minimum (2.4.11), dragging movements (2.5.7), target size minimum (2.5.8), accessible authentication minimum (3.3.8). **Added at AAA:** focus not obscured enhanced (2.4.12), focus appearance (2.4.13), accessible authentication enhanced (3.3.9). **Removed:** 4.1.1 parsing.

The two that break the most existing interfaces are target size and dragging movements. A dense table with 16 pixel icon buttons packed together fails the first; every drag-and-drop reorder built without a menu alternative fails the second.

## What automation actually settles

Run the rule engine first. Then read what it cannot decide, because that list is the audit plan.

It settles: missing alt attributes, empty links and buttons, missing form labels, missing document language, duplicate ids, positive tabindex, contrast where both colours are computable from CSS, missing table headers, invalid ARIA role and attribute combinations, and landmark structure.

It cannot settle:

- **Whether an alt text is correct**, only that one exists. "photo.jpg" passes a presence check.
- **Whether the heading structure reflects the content.** A tool sees h1, h2, h3. It cannot see that the h3 is a sibling topic wrongly demoted, or that a prominent section has no heading at all.
- **Whether an error message is useful.** Text is present; whether it says how to fix the problem is a reading task.
- **Whether a name matches its visible label meaningfully.** A tool compares strings; it cannot tell that "Submit" and "Place order" refer to the same button in a way a voice user will find.
- **Whether contrast passes over an image, a gradient, or a runtime colour.** Anything not computable from static CSS is skipped or guessed.
- **Whether a link's purpose is clear in context.**
- **Whether a live region fires at the right moment**, or says anything useful when it does.
- **Whether a process conforms**, since automation runs page by page.
- **Whether an exception applies**, such as a target being inline or a table genuinely needing two dimensions.

## The manual pass, in order

Order matters, because each pass changes the page state for the next and the cheap passes tell you where to spend the expensive one.

1. **Keyboard only.** Unplug the mouse and tab the page. See the separate focus and keyboard audit. First, because a page that cannot be operated by keyboard fails at a level that makes the rest academic.
2. **Zoom to 200 percent.** Full page browser zoom. Look for clipped text, overlap, and controls that have left the viewport. Covers 1.4.4.
3. **Reflow at 320 CSS pixels.** Viewport 320 by 256, or 400 percent zoom at 1280 wide. Look for a document-level horizontal scrollbar and for content that vanished rather than reflowed. Covers 1.4.10. After the zoom pass, because it is the stricter version of the same failure and you want to know which broke first.
4. **Text spacing override.** All four values at once, then repeat at the reflow width where it is far more likely to break. Look for clipping and overlap in fixed-height containers. Covers 1.4.12.
5. **Colour and contrast.** Sample computed foreground and background for every distinct text style, control border, focus indicator and chart series, then take a greyscale screenshot and look for information that vanished. Covers 1.4.1, 1.4.3, 1.4.11.
6. **Screen reader pass.** Read top to bottom, then navigate by heading, by landmark, by form control, then pull the links list. Then trigger every state change and listen. Covers 1.1.1 correctness, 1.3.1, 2.4.4, 2.4.6, 4.1.2, 4.1.3. Last, because it is the most expensive and the earlier passes say where to concentrate.
7. **Reduced motion.** Set the operating system preference and reload. Anything animating past a fade should stop. This is not itself an AA criterion under 2.2, so record it as a best practice finding and say so.

## Decision rule: failure, best practice, or cannot tell

Take one of three branches for every finding, and never blur them.

**FAILURE.** You can name the criterion, the element and the observed value, and the value is on the wrong side of a stated threshold. "1.4.3, helper text under the email field, computed #9A9A9A on #FFFFFF, ratio 2.81:1, needs 4.5:1." Goes in the conformance list, not negotiable.

**BEST PRACTICE.** Real, worth fixing, no criterion. Reduced motion. A focus ring that meets 3:1 and is still hard to see on a busy background. A form that meets 3.3.2 and would be far clearer with format hints. Separate list. Mixing these into the conformance list is how a findings document loses its authority, because one arguable item lets a team dismiss the rest.

**CANNOT TELL.** You genuinely cannot decide from what you have. Say so, say what would decide it, and keep a third list. Real instances: contrast over a user-uploaded image, which needs the worst point sampled or a scrim added; whether a caption track is accurate, which needs a transcript review; whether the target size inline exception applies to a control whose layout context you cannot see; whether a status message announces, which needs a real screen reader on a real platform. The failure here is not uncertainty, it is resolving uncertainty by guessing. A false pass hides a barrier and a false fail burns credibility.

## Conformance mechanics people get wrong

**Conformance is per page and per complete process.** A page mid-checkout can meet every criterion and still not conform if any other page in that process does not. An accessible product page in an inaccessible basket buys nothing. Audit signup, checkout, booking, application and account deletion end to end.

**There is no partial conformance for a page.** You cannot claim conformance for the parts you control and carve out a widget. The one recognised statement of partial conformance covers third-party content injected after publication that you cannot control or monitor, such as user-generated content or externally served advertising. A component you chose, installed and could replace is your content, and a vendor's conformance report is evidence rather than an exemption, describing the component in the vendor's demo rather than in your page.

**Non-interference applies to everything on the page.** Content you do not rely on for conformance must still not block access: no keyboard trap, no uncontrollable audio, no unstoppable motion, no flashing. A third-party chat widget that traps focus breaks the page whatever else you did.

**The accessibility statement is a real artefact with expected contents**, and for European public sector bodies a legal requirement rather than a courtesy. A useful one names the standard, version and level claimed; the status, fully, partially or not conformant; known non-conformant content with reasons and alternatives; preparation and review dates; the assessment method; a feedback address with a stated response time; and the escalation route. A statement claiming full conformance with no known issues is nearly always false and reads that way.

## A severity model that survives a backlog

A raw list of two hundred issues does not get fixed. Rank on two axes.

**Impact.** *Blocker*: the task cannot be completed at all, such as no keyboard access to submit, an unlabelled required field, or a CAPTCHA with no alternative. *Severe*: completable only with real difficulty or guessing, such as error text that does not say what is wrong. *Moderate*: friction and uncertainty, task still achievable, such as a heading level jump. *Minor*: noticeable, not obstructive, such as a redundant "image of".

**Reach.** *Systemic*: in a shared component or template, so it appears everywhere that component does. *Page level*: one page or flow. *Instance*: a single element.

Fix order: systemic blockers first, because one change removes many failures and it is the cheapest accessibility work that exists. Then page-level blockers on the highest-traffic flows. Then systemic severe. Expect never to reach the bottom, which is fine as long as the top is honest.

Report **distinct defects**, not instances. "One design token has insufficient contrast, affecting 340 elements across 22 pages" is a fixable sentence. "340 contrast errors" is a reason to give up.

## Worked example, compressed

A subscription pricing page: three plan cards, a monthly and annual toggle, a comparison table, an FAQ accordion, and a sign-up form. Audited against 1.1.1, 1.3.1, 1.4.1, 1.4.3, 1.4.10, 1.4.11, 2.5.8, 3.3.1, 3.3.7 and 4.1.3.

The automated scan returns two violations: a missing document language and one empty link. Both real, both four-minute fixes, both the least important things here.

*Contrast.* The "Most popular" badge is white on brand orange at 2.1:1, a **1.4.3 failure**. The toggle's inactive track is #E8E8E8 on white at 1.2:1, a **1.4.11 failure**, because the track is the boundary identifying the control.

*Colour alone.* The recommended plan is marked by an orange border, which disappears in greyscale, but the badge text says "Most popular", so the card passes. The same border does the same job in the comparison table where no badge exists, so the table instance is the **1.4.1 failure** and the card is a pass.

*Structure.* The comparison table is `<div>` elements in a grid: relationships exist visually, not programmatically. **1.3.1 failure**, systemic, since the component renders on four pages. The FAQ headings are bold `<div>` elements: **1.3.1 failure** again, different component.

*Reflow.* At 320 pixels the table forces a horizontal scrollbar on the whole document. The table may scroll; the page may not. **1.4.10 failure**, fixed by a scroll container rather than a redesign.

*Target size.* The monthly and annual toggle is 20 pixels tall with adjacent halves, so neither the size nor the spacing exception is met: **2.5.8 failure**. The FAQ chevrons are 18 pixels and sit 40 pixels apart, so the circles do not intersect and the spacing exception applies. **Pass**, written down as a pass so nobody re-raises it.

*Forms.* Required fields use a red asterisk explained in a legend, which passes 3.3.2. A bad email produces a red border and no text: **3.3.1 failure**. The success state replaces the button with "Check your inbox" in a container created at that moment: **4.1.3 failure**, because the live region must pre-exist.

*Redundant entry.* The form asks for the email, then asks again to confirm it. Whether a confirmation field counts as essential re-entry is arguable. **CANNOT TELL**, with the note that dropping the confirm field and offering a reveal control also removes the paste problem under 3.3.8.

**Verdict: not conformant at AA.** Seven failures across five criteria plus one unresolved. Two are systemic and sit in shared components, so fixing those removes failures on four other pages. Ranked: the table structure first, since screen reader users cannot read the comparison at all; then the missing error text, which blocks form completion; then the live region; then the contrast and target items, which are severe rather than blocking. The document language goes first only because it is free.

## Failure modes

**Auditing the criteria instead of the page.** Fifty-five rows all marked "pass", produced without a browser ever being resized. The tell is that no finding contains a measured value.

**Reporting the scan as the audit.** Zero violations, page unusable with a screen reader. Presenting a rule engine's output as a conformance result misrepresents the tool, usually without meaning to.

**Instance counting.** Two hundred findings, one root cause. Technically accurate, practically useless, and the team's first response is despair rather than a fix.

**Mixing best practice into the conformance list.** One arguable item at the top gives a reader permission to treat the whole document as opinion.

**Measuring contrast from the design file.** The design says #757575 on white; the render has 90 percent opacity over a light grey section and computes to something else. Sample rendered pixels.

**Auditing pages and missing the process.** Every page passes and the checkout does not conform, because step three loses the keyboard user. Page-by-page auditing structurally cannot see it.

**Testing 4.1.1 under 2.2.** Reporting unclosed tags against a criterion the standard no longer contains, instead of reporting the duplicate id under the criterion it actually breaks.

**Treating a vendor conformance report as an exemption.** The component is in your page, so its defects are your page's defects.

**Auditing at one viewport.** Everything passes at 1440 pixels. Reflow, target size and text spacing all fail at 320, which is where the criteria are actually defined.

## What this skill does not do

- It cannot measure anything. Contrast, target size, reflow and text spacing all need a rendered page, a browser and a colour picker, and this only says where to point them.
- It does not run assistive technology, and several findings can only be settled that way. Announcement behaviour differs between NVDA on Windows and VoiceOver on Mac, and neither is the definition of correct.
- It covers levels A and AA. AAA is out of scope, including the separate question of which AAA criteria are worth adopting for a given audience.
- It defers keyboard operability, focus order, focus indicators and focus obscuring to a separate audit, and treats name, role and value at one level of depth rather than the several it deserves.
- It is not a legal opinion and provides no legal protection. Which standard, version and deadline binds you is a question for a lawyer or an accessibility consultancy.
- It does not replace testing with disabled users, which remains the only method that finds barriers nobody thought to look for.
