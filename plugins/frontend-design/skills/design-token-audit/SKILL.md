---
name: design-token-audit
description: Audits a design token system for structural defects rather than aesthetic ones: raw values leaking past the tokens, tokens named after their value instead of their role, components reaching past the semantic layer, and semantic tokens that resolve in only one of the three theme states a page can be in. Carries the WCAG 2.2 contrast numbers, the reason a dark theme is not an inversion, and the rules for building a perceptually even colour ramp. This skill should be used when reviewing or building a token file, adding a dark or high-contrast theme, or investigating a colour or spacing defect that only appears for some users.
---

# Design token and theming audit

## The claim this skill is built on

A token system does not fail because somebody picked the wrong blue. It fails in three structural ways, and none of them are visible in a design review.

It fails because raw values leak past the tokens, so the token file describes a design the product does not actually render. It fails because tokens are named after their values rather than their roles, so a token called `blue-500` ends up as the border on a destructive button and there is now no safe way to change either the blue or the button. And it fails because the palette is only complete in one theme state, so the moment a user is in a different state, part of the surface falls back to whatever the browser or the parent element decided.

The third one is the expensive one. It does not throw, it does not fail a build, and it does not appear in a review, because a review happens on one machine with one operating system preference and one theme selected. It appears later as a screenshot from a user, showing text nobody on the team can make invisible.

This skill is a structural audit. It has no opinion about your palette.

## The three layers, and the one rule

**Layer 1, primitive.** Names describe the value: a numbered colour ramp, a spacing sequence, a type scale. No opinions and no roles. This layer is allowed to be large, because it is a vocabulary rather than a design.

**Layer 2, semantic.** Names describe the role: a raised surface, muted text, a critical border, the inline gap between a label and its control. Each semantic token points at a primitive, and each one is redefined per theme. This is the only layer that changes between themes.

**Layer 3, component.** Names describe the place: the background of a primary button, the border between table rows. Optional, and only worth creating when one component genuinely has to diverge from the semantic value. A component layer created by default triples the token count and communicates nothing.

**The rule that makes the model work: a component may reference the semantic layer and nothing else.** Not a primitive, not a raw value.

The reason is mechanical rather than stylistic. The semantic layer is the layer that gets redefined per theme. A component that reads a primitive reads the identical value in every theme, so it will not follow the theme, and it will not follow a rebrand either. Every dark-theme bug of the form "this one component stayed light" is this rule being broken.

That rule is checkable in one search. Search the component source for the primitive prefix. Every hit is a defect, and the count is the health of the system.

The naming test is equally short. **If you can change the value without changing the token name, the name describes a role. If changing the value forces you to rename the token, the name describes a value.** A token called `blue-500` fails. A token called `surface-raised` passes. A token called `blue-primary` fails in a way that is easy to miss, because it is half a role and half a value, and it will be wrong the first time the brand stops being blue.

One quick diagnostic: count the two layers. A set with 200 primitives and 12 semantics does not have a semantic layer. It has a colour list and a handful of aliases, and every theme after the first will be a rewrite.

## The three theme states

A page can be in exactly three states, and almost everyone designs for two.

1. **Explicit light.** The user chose light, and the root element carries a marker: an attribute or a class.
2. **Explicit dark.** The user chose dark, and the root carries the corresponding marker.
3. **Default.** The user has chosen nothing. No marker exists anywhere. `prefers-color-scheme` is the only signal, and it can say light, dark, or nothing at all.

**The most common theming defect in existence is a palette whose only definition lives inside `@media (prefers-color-scheme: dark)`, or only inside a `[data-theme="dark"]` selector.** In the state it was written for it looks perfect. In the other two it resolves to nothing, and the affected properties fall back to inherited or initial values. Text inherits a colour that may happen to be legible on the page background and illegible on a card. Nothing errors.

It survives review because review happens in one state. The developer's machine is set to dark, the toggle defaults to system, and state 3 with a dark system preference is the only state anyone ever renders.

The rule, in four parts:

- Define the **complete** palette as tokens on bare `:root`. Every token gets a value here, with no media query and no attribute selector involved.
- Under `@media (prefers-color-scheme: dark)`, redefine **only the tokens that change**, and scope the block so an explicit light choice still wins: `:root:not([data-theme="light"])`.
- Redefine the same tokens again under `:root[data-theme="dark"]`, so an explicit dark choice wins on a light system.
- Never let any token's only definition sit inside a media query or an attribute selector. If a token is defined in exactly one place and that place is conditional, it is a defect regardless of how the page currently looks.

Two adjacent items that belong in the same pass:

- **Set `color-scheme`.** Native form controls, scrollbars, and the default canvas background follow the `color-scheme` property, not your tokens. A page with a fully dark palette and no `color-scheme: dark` renders light scrollbars, a light date picker, and a white flash before the first paint.
- **Give the body an explicit background token.** A transparent body shows whatever is behind it, which in an embedded or previewed context is not your page.

## Dark mode is not an inversion

Four mechanisms, each with a consequence for the token set.

**Elevation cannot be a shadow.** On a light surface, elevation reads because a shadow is darker than the surface it falls on and there is a large range below the surface lightness to work in. On a near-black surface there is almost no range below, so the shadow is nearly invisible and the card looks flat. On dark, elevation must be expressed as lightness: each level of elevation is a lighter surface than the one behind it. This is a token structure decision, not a per-component fix. Model elevation as a semantic surface scale, `surface-0` through `surface-3`, where the light theme resolves those to a shadow plus a flat background and the dark theme resolves them to genuinely different lightnesses.

**Saturated brand colours vibrate.** A fully saturated hue against a near-black background produces a shimmering edge, an effect of the eye focusing the wavelengths at slightly different depths, made worse by a large lightness gap. The fix is to reduce chroma and raise lightness for the dark-theme variant of every saturated brand colour. As a starting point, reduce chroma by roughly a quarter to a third and then look at it on a real screen. That range is approximate and depends entirely on the hue, so treat it as where to begin rather than a value to ship.

**Pure black is a decision, not a default.** It maximises contrast, which is not automatically good: maximum contrast maximises halation, the bleeding of light text into a dark ground, which is worse for readers with astigmatism and makes long-form text harder. It also removes the room needed to express elevation as lightness. A near-black in roughly the 6 to 12 percent lightness band gives you both, and that band is approximate. Against that, on an OLED panel pure black switches pixels off, which saves power and produces genuinely deeper blacks, at the cost of visible smearing on scroll. Pick deliberately and write down why.

**Pure white text is usually too much** for the same halation reason. Step the primary text token down from white in the dark theme.

## Contrast, with the actual numbers

**WCAG 2.2 became a W3C Recommendation on 5 October 2023 and is the normative standard as of August 2026. WCAG 3 is a working draft and is not normative.** That distinction decides what you may claim in a conformance statement.

Level AA, the level most obligations reference:

- **1.4.3 Contrast (Minimum):** 4.5:1 for normal text. 3:1 for large text, where large means at least 18pt, which is 24px, at normal weight, or at least 14pt, which is 18.66px, at bold.
- **1.4.11 Non-text Contrast**, added in WCAG 2.1 in June 2018: 3:1 for the parts of a user interface component needed to identify it and its state, and for graphical objects required to understand the content. This is the criterion that catches an input whose only boundary is a very light border, and a chart whose series are distinguished by colours that differ only slightly.

Level AAA, **1.4.6 Contrast (Enhanced):** 7:1 normal, 4.5:1 large.

Two exemptions worth knowing precisely, because they are widely half-remembered. Disabled controls are exempt from both 1.4.3 and 1.4.11. Placeholder text is not exempt, and a placeholder used as the only label fails a different criterion anyway. The disabled exemption is regularly treated as permission to make disabled states illegible, which is a usability failure even where it is not a conformance failure.

**The known weakness.** The WCAG 2 ratio is computed from relative luminance with a flare constant of 0.05 added to both terms. At the dark end that constant dominates, so the ratio moves very little across large perceptual differences. The practical consequences: pairs on dark backgrounds can pass at 4.5:1 while being visibly harder to read than a light-theme pair with the same number, and some genuinely readable dark pairs fail. The formula also ignores font weight entirely and treats size as two bands.

**What APCA changes.** The Accessible Perceptual Contrast Algorithm, developed for WCAG 3, replaces the ratio with a polarity-aware lightness contrast value, Lc, running to roughly plus or minus 106, computed differently for dark-on-light than for light-on-dark, and tied to font size and weight rather than two bands. Draft guidance has sat somewhere around Lc 75 for body text and Lc 60 for larger or bolder text, but those thresholds have moved between drafts and should be treated as approximate. **It is not normative.** Use it as a second opinion on dark themes, which is exactly where the WCAG 2 number carries least information, and report the WCAG 2 ratio when anyone is asking about conformance.

Check the **resolved semantic pairs** in every theme state, not the primitives. A primitive has no partner, so it has no contrast.

## Colour space, and why ramps go grey in the middle

sRGB is not perceptually uniform and its channels are gamma encoded. Two consequences follow directly.

**Even numeric steps are uneven perceptual steps.** A ramp built by stepping the sRGB channels evenly bunches at one end and the middle steps look nearly identical.

**Interpolating between two saturated hues passes through mud.** Between blue and yellow, every channel is middling somewhere in the middle of the gradient, and middling channels are grey. This is why a two-stop gradient that looks vivid at both ends is dull across the centre.

OKLCH separates perceptual lightness from chroma and hue, so changing lightness does not shift apparent hue, and a fixed lightness across several hues actually looks like one lightness. The practical rules for generating a ramp:

- **Step lightness evenly and let chroma vary.** Chroma cannot be constant across a ramp, because the sRGB gamut is much narrower at very high and very low lightness. A ramp with constant chroma clips at both ends and the extreme steps come out wrong. Clamp chroma to what the gamut allows at each lightness.
- **Shift hue slightly as lightness falls.** Real materials do this, and a ramp with a few degrees of hue shift towards the dark end reads as more natural than one held rigid.
- **Interpolate gradients in a perceptual space.** CSS supports specifying the interpolation space in a gradient, so a gradient declared to interpolate in Oklab avoids the grey middle without adding stops.
- `oklch()` shipped in Safari during 2022 and in Chrome and Firefox during 2023, so it is broadly available now. Check your own support floor before making it a token's only definition, and provide an sRGB fallback if you support older engines.

## Typography tokens

**Use one modular scale.** Pick a ratio and generate from it: roughly 1.125 for a dense interface, 1.2 or 1.25 for general product UI, 1.333 or 1.5 for editorial. A ratio of 1.5 in a product UI gives you about three usable sizes before the next step is too large for a table row.

**Count the distinct font sizes actually rendered.** More than about seven in a product interface means the scale is decorative and the real scale is whatever people typed.

**Line height must be unitless, and this is the most consequential mechanical rule in typography tokens.** A line-height declared with a unit computes to a length on that element and is inherited by descendants as that fixed length, so a child with a larger font size inherits the parent's smaller absolute line height and the lines overlap. A unitless line-height is inherited as a factor and recomputed against each element's own font size. The bug is invisible until somebody nests a heading inside a container that set line-height in pixels.

**Line height is not one number.** Roughly 1.4 to 1.6 for body copy, roughly 1.1 to 1.25 for large headings, because a heading's lines are shorter and its type is larger. Tokenise line height against size, not globally.

**Optical sizing.** Variable fonts may carry an `opsz` axis that adjusts stroke contrast and spacing for the rendered size. Where the font has it, leave `font-optical-sizing` at automatic. Setting `opsz` by hand overrides a decision the type designer made deliberately.

**Fallback metrics, or the swap causes layout shift.** When a web font loads with `font-display: swap`, the fallback is replaced by a font with different metrics, the text reflows, and that reflow is a layout shift that counts against Cumulative Layout Shift. The fix is to declare an `@font-face` rule for the fallback family that overrides its metrics with `size-adjust`, `ascent-override`, `descent-override` and `line-gap-override`, tuned so the fallback occupies the same space, then name that adjusted family in the stack. These descriptors reached the major engines somewhere between 2021 and 2023, which is approximate, so check support against your own floor. A stack that simply ends at a generic family has no metric-matched fallback, only optimism.

## Spacing

**A base of 4 or 8 works for two reasons.** They land on whole device pixels at common densities, which keeps hairlines and dividers crisp, and a small set of multiples covers nearly every real gap, which ends the argument about whether this one should be 13 or 14. The usual arrangement is a base of 8 with 4 as a half step, giving 4, 8, 12, 16, 24, 32, 48, 64. A base of 5 or 6 produces fractional pixels at common zoom levels.

**When to break it, which is four cases and no more:**

- **Optical alignment.** Glyphs and icons carry their own bearings, so a one or two pixel nudge that makes an icon look centred beats a token that makes it measurably centred.
- **Hairlines.** A one pixel border is not a spacing value and does not belong on the scale.
- **Type-derived spacing.** The space above a heading is more usefully derived from that heading's line box than from the spacing scale, because it has to relate to the type rather than to the grid.
- **Positioning against a fixed asset.** Aligning to a feature inside a fixed-size image is arithmetic, not design.

Every one of those is an exception that gets written down in a comment. **An exception that is not written down is indistinguishable from a leak**, which is the entire reason the audit can work at all.

## Motion tokens

Tokenise duration, easing and distance. As starting points, roughly 100 to 150 ms for a hover or state change, roughly 200 to 300 ms for something entering or leaving, and longer only for a deliberate transition of the whole view. Those are approximate and depend on distance travelled.

**Treat `prefers-reduced-motion: reduce` as a first-class branch of the token set, not as a suppression applied afterwards.** Two rules follow.

The reduced branch must still communicate the change. Replacing a slide with an instant opacity change is correct. Replacing it with nothing at all is a defect, because the user loses the only signal that something happened.

Do not implement it as one global rule that sets every duration to a hundredth of a second. That pattern breaks any animation whose completion event drives logic, and it removes cross-fades that reduced-motion users benefit from. Define reduced values for the motion tokens instead, so components keep their structure and only the numbers change.

The motions that actually trigger vestibular discomfort are autoplaying carousels, parallax, large-scale zooms and continuous background movement. Those should stop under a reduced-motion preference. A 150 ms button transition was never the problem.

## The audit procedure, in order

The order matters for one reason: an unresolved token makes every contrast number computed after it meaningless, so resolution is settled before anything is measured.

1. **Inventory the layers.** Count primitives, semantics and component tokens. Report the ratio.
2. **Find every hard-coded value.** Search the component source for hex colours, `rgb(`, `rgba(`, `hsl(`, raw pixel values in properties the token set covers, and raw font families. Produce the count before producing an opinion about it.
3. **Classify each hit** as a leak or an exception, using the decision rule below.
4. **Resolve every semantic token in all three theme states.** Any token that resolves to nothing in one state, or resolves to the same value in light and dark where it must differ, is a defect. This is the highest-severity finding in the audit.
5. **Check contrast on the resolved pairs**, per state, for body text, secondary text, placeholder text, and every interface component boundary.
6. **Check for components referencing the primitive layer** directly.
7. **Report as a ranked list**, in this order of severity: a token that fails to resolve in a theme state, a contrast failure on body text, a component reading a primitive, a contrast failure on a component boundary, a leak, a value-named semantic token, a scale violation.

## Decision rule: leak, exception, or gap

For each hard-coded value found in step 2:

- **Does a token already hold this exact value, or one within rounding?** Then it is a **leak**. Replace it.
- **Is the property one the token set does not cover at all**, such as a z-index, a clip path, or a keyframe percentage? Then it is a **gap**, not a leak. Do not invent a token for a single use. Note it if the same property keeps appearing.
- **Is it one of the four named exceptions**, an optical nudge, a hairline, type-derived spacing, or alignment to a fixed asset? Then it is a **legitimate exception** and it must carry a one-line comment naming which. Add the comment as part of the audit.
- **If you cannot tell**, which happens when the value is near a token but not equal to it, record it as a **question rather than a fix**. A 15 pixel gap sitting next to a 16 pixel token is usually drift, but occasionally it is a deliberate optical correction that only the person who wrote it can confirm. Write it as "15px here, the spacing token is 16px, was this deliberate?" and let the owner answer. Silently normalising intentional values is the fastest way for an audit to lose the room, because the one thing you changed that mattered will be the one thing anyone remembers.

## Worked example, compressed

A settings page in a product with a token file, a dark theme, and a billing section added last month.

**Inventory:** 180 primitives, 40 semantics, no component layer. The ratio is workable, so proceed.

**Hard-coded values:** 34 found. Classified as 21 leaks, all hex colours matching existing primitives exactly or pixel gaps matching the spacing scale; 6 gaps, being z-index values and keyframe percentages the set does not cover; 4 exceptions, two one-pixel icon nudges, one hairline and one heading margin derived from its line box, all now commented; and 3 that cannot be told, a 15 pixel gap, a 13 pixel font size and a shadow at an alpha not in the set, all recorded as questions.

**Theme resolution:** the billing section's five semantic tokens are defined only inside the dark media query. Under an explicit light choice, and in the default state on a light system, they resolve to nothing. The affected text falls back to an inherited colour that happens to be legible on the page background and is very nearly invisible on the raised card. Nobody on the team had reproduced it because everybody's machine is set to dark.

**Contrast:** muted text on the raised surface resolves to 3.9:1 in dark, under the 4.5:1 that 1.4.3 requires for normal text. The brand accent used for inline links resolves to 2.8:1 on the dark surface, which fails both as text and as a component boundary under 1.4.11.

**Naming:** three components reference `blue-500` directly. One semantic token is used both for a destructive action and for an informational badge, which means its value can no longer be changed for either.

**Verdict: the token system is structurally sound and the dark theme is not finished.** Five tokens exist in one theme state only, two resolved text pairs fail 1.4.3 at AA, and three components bypass the semantic layer. Fix order: theme resolution first, because it is the only defect that makes text disappear; then the two contrast pairs; then the primitive references; then the 21 leaks as one mechanical sweep.

## Failure modes

**The one-theme palette.** A token group whose only definition sits inside a media query or a theme attribute selector. Looks perfect in the state it was authored in, resolves to nothing in the other two, and produces bug reports nobody can reproduce.

**The value-named semantic.** A colour named for what it is rather than what it does, referenced from a dozen places that each mean something different by it. The tell is that nobody can change it, because every change breaks something unrelated.

**Shadow-only elevation carried into dark.** Every card looks flat, everybody agrees the dark theme feels wrong, and nobody can say why, because the missing thing is the absence of a signal rather than the presence of a defect.

**The saturated brand colour used unchanged on near-black.** Edges shimmer, text on it is tiring, and it usually gets blamed on the screen.

**The constant-chroma ramp.** Generated in a perceptual space but with chroma held fixed, so the lightest and darkest steps clip against the gamut boundary and come out duller or shifted in hue than every step between them.

**The unit-ful line height.** Inherited as a fixed length by a descendant with larger type, so headings inside that container overlap their own lines. It appears only in the specific nesting that triggers it.

**Reduced motion as a global kill switch.** One rule setting every duration to near zero. Transition-end handlers stop firing reliably, and any logic waiting on them stalls, which turns an accessibility preference into a functional bug for the users who set it.

**Contrast checked on primitives rather than resolved pairs.** Produces a page of green ticks that says nothing about what any user sees, because the pair that matters is the one the theme actually resolves to.

**`color-scheme` never declared.** The palette is dark and the scrollbars, form controls and the paint before first render are all light.

## What this skill does not do

- It has no taste. It checks structure, resolution and contrast, and a token set can pass all three while being drab, over-scaled, or wrong for the product.
- It cannot see a rendered page, so every contrast number comes from resolved token values. Text over a photograph, a gradient or a video needs a screenshot and a pixel sampler, and this cannot supply either.
- It does not read component APIs. A component that accepts a raw colour prop lets any caller bypass every rule above, and that will not appear in this audit.
- It is not an accessibility audit. Contrast is one criterion. Keyboard operability, focus order and screen reader announcement are all outside it.
- It will over-report on a young codebase that has deliberately not built a semantic layer yet, and the reachability of that judgement is yours rather than the audit's.
- It does not touch your build pipeline, which is where most of these findings should eventually be enforced rather than reviewed.
