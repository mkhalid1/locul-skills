---
name: design-system-conformant-ui
description: Builds a new screen that is indistinguishable from the application it lands in, by inventorying the token set, the existing components and the installed dependencies before writing anything, then composing from what exists and escalating only with a written reason. Covers the three-rung escalation ladder, a shared-component blast radius threshold, the tie-break rule for a codebase with conflicting conventions, the house constraints and the exact searches that check them, and the interaction minimums treated as construction constraints rather than as judging criteria. This skill should be used when adding a screen or a feature to an existing codebase, when a design hand-off arrives as a picture, or before installing a package for something the codebase may already have.
---

# Design system conformant UI

## The claim this skill is built on

Telling anyone to be consistent does not work, and it does not work for a reason that has nothing to do with discipline. Consistency is an information problem. The person writing the screen does not know that a segmented control already exists, that the caption step is 12 and not 14, or that the icon package was chosen two years ago after an argument. They are not being careless. They are working from what is in front of them, and what is in front of them is a picture.

The second claim is about which drift actually survives review. A stray colour value gets caught, because it is visible and because everyone has an opinion about colour. What survives is structural: a second icon package, a date picker duplicating one already installed, a component that reimplements something three folders away, a new breakpoint that makes this screen reflow at a width no other screen reflows at. Those pass review because each one arrives with a plausible local reason, and nobody is comparing against the whole codebase at eleven at night.

So the method is not an exhortation. It is an ordering, an escalation ladder where every rung costs you a written sentence, and a closing gate made entirely of searches that return counts. If a constraint in this file cannot be checked by a search, it is a taste question and it belongs to a person, not to this.

## Phase 1. Inventory, before writing anything at all

Read in this order. The order matters because the manifest changes what you design, not just how you build it: knowing there is already a dialog primitive with focus management changes the shape of the screen you propose, and finding that out afterwards means the screen gets rebuilt or, more often, does not.

1. **The dependency manifest and its lockfile.** What user interface library, icon package, form library, date library, table library and animation library are installed, and at what versions. This is first because rung three of the ladder is the most expensive decision on the list and it is made in the first thirty seconds by anyone who does not check.
2. **The token or theme source.** A token file, a theme module, or a stylesheet declaring custom properties on the root. Record the semantic colour names, the type scale steps, the spacing scale, the radii, the shadow steps and the named breakpoints. Record the names, not the values, because the names are what you will write.
3. **The component directory.** Every component, and for each one its variants and sizes. A button with four variants and three sizes is four by three answers you do not have to invent. Note the empty states, the skeletons and the table, which are the three most commonly rebuilt from scratch by someone who did not look.
4. **One sibling screen, read end to end.** The tokens give you the vocabulary and the sibling gives you the idiom: where the page heading sits, whether content is in cards or plain sections, whether there is a toolbar, how the primary action is positioned, what the loading treatment looks like. Copying the idiom is most of what makes a screen feel native.
5. **The copy of one existing error and one existing empty state**, verbatim, to match register later.

**Write the inventory down.** A short table is enough: category, what exists, where it lives, notes. Nine rows: colours, type scale, spacing, radii, shadows, breakpoints, components with their variants, icon package, other installed libraries.

That table is the working palette for this screen. From here on, anything not in it requires a justification rather than a preference, and the ladder is what a justification looks like.

## Phase 2. Compose, with the escalation ladder

Every element in the design resolves to exactly one rung. You may not skip a rung, and each rung above zero costs a sentence recorded in the change description where a reviewer will read it.

**Rung 0. Use what exists, unchanged.** An existing component with an existing variant. No reason required, because there is nothing to justify. Most of a screen should land here, and if very little does, either the inventory was done badly or the design is fighting the system on purpose, and both are worth saying out loud before writing code.

**Rung 1. Extend an existing component.** Add a variant, a size or a prop. The written reason names the component, the new variant, and whether the change is additive or not. This distinction is the whole risk: **adding a new variant is additive and safe, changing what an existing variant resolves to is not**, because every current caller inherits it. If the change is not additive, count the callers before you touch it.

**Rung 2. Create a new component.** Built only from primitives and tokens already in the inventory. The written reason names the closest existing component and the specific behaviour it cannot be made to do. Put it in the feature folder, not the shared library, until a second screen needs it: a component with one caller is a local component, and promoting it early is how shared libraries fill with things nobody else uses.

**Rung 3. Add a dependency.** The written reason names the installed library, the specific capability it lacks, the size of the new package, and what happens to the two of them coexisting. Reaching rung three for a dropdown, a modal, a tooltip or an icon almost always means the inventory was skipped.

### Decision rule: extend the shared component, or wrap it locally

- **If the change is additive**, a new variant nobody else references, extend the shared component. Nothing existing can break.
- **If the change alters an existing variant** and the component has fewer than about ten call sites, extend it and open every one of them. Ten is a threshold you can afford to check by hand in a few minutes.
- **If it alters an existing variant and there are more than about ten call sites**, do not touch it. Create a local wrapper in your feature folder, note it as a candidate for promotion, and let the person who owns the shared component decide. This is not timidity: a shared component with forty callers is an interface, and changing an interface to serve one screen is a change to thirty-nine screens nobody asked for.
- **If you cannot tell how many callers there are**, because the component is re-exported through an index file or reached by a path alias, search for both the direct import and the alias, and if the count is still unclear, treat it as the large case. The wrapper is cheap and reversible. The silent change to forty screens is neither.

### Decision rule: the codebase contradicts itself

Real codebases carry two conventions because they were half migrated. Resolve it in this order and stop at the first that answers.

1. **The most recently modified sibling screen wins.** Recency is the best available signal of the direction the codebase is moving.
2. **If two conventions are equally recent, the one used in more files wins.**
3. **If it is still tied, copy the nearest sibling screen wholesale**, including the convention you like less, and record the ambiguity in the change description so somebody with the history can settle it.
4. **Never invent a third.** A third convention is the only outcome here that is definitely wrong, and it is the default outcome when nobody writes down a tie-break rule.

## Phase 3. The house constraints

Each of these was chosen because a search settles it. Taste is not involved and none of them require a meeting.

- **No colour literals.** Semantic tokens only. A literal looks correct in the theme you are viewing and breaks in the others, and there are three theme states rather than two: light, dark, and the state where the user has expressed no preference at all.
- **One icon package, and no hand-drawn vectors beside it.** A second package is visible to users as two different stroke weights and two different corner treatments, even to people who could not name what they are seeing.
- **A shadow ceiling.** Pick the step used by the existing cards and do not go above it. Elevation that exceeds the system reads as a component from a different application.
- **No hover states on things that are not interactive.** A hover response is a promise that something will happen when clicked.
- **No absolutely positioned decoration.** It escapes the layout, it overlaps at widths nobody tested, and it is the first thing to break in a language with longer words.
- **Framework primitives for links and images**, where the framework provides them. Raw anchor and image tags skip client routing, skip image optimisation and skip the layout reservation that stops the page shifting.
- **No one-off animation.** Use the durations and easings in the token set, or nothing.
- **Spacing from the scale.** Every margin, padding and gap is a scale step. An arbitrary value is a finding even when it looks better, because the next person will pick a different arbitrary value.

## Phase 4. The interaction minimums, as construction constraints

These are floors you build to, not a conformance claim. Judging the finished screen belongs to the audits named at the end of this file.

**Touch targets.** The number to know is that WCAG 2.2, a W3C Recommendation since October 2023, requires **24 by 24 CSS pixels** at level AA in success criterion 2.5.8, with real exceptions including one for targets that are smaller but adequately spaced, one for inline targets in a sentence, and one for controls whose presentation the user agent decides. The **44 by 44** figure that gets quoted everywhere is level AAA in 2.5.5, and it also matches long-standing platform guidance, where one major mobile platform specifies 44 points and the other 48 density-independent pixels. Build to the platform figure and know that the standard's floor is lower, so that you can tell the difference between a defect and a preference during review.

**Text size.** Body text at 16 pixels or the scale step that resolves to it, which is also the default in every desktop browser. The one place this is close to a hard rule is form inputs: a mobile browser will zoom the viewport when a user focuses an input whose font size is below 16 pixels, which yanks the layout sideways mid-form. That is not an accessibility criterion, it is a specific behaviour with a specific trigger.

**Contrast.** 4.5:1 for normal text and 3:1 for large text, where large means 24 pixels regular or 18.66 pixels bold. 3:1 for interface component boundaries and meaningful graphics, which is the one people miss: an input border, a toggle in its off state and a chart series all have to clear it.

**Focus.** Everything focusable has a visible focus indicator. Never remove an outline without replacing it, and prefer the focus-visible pseudo-class so a mouse click does not draw a ring while a keyboard tab does.

**Labels.** Every control gets a real label. A placeholder is not a label: it disappears at the exact moment the user starts typing, which is the moment they most need to know what the field is, and it is frequently too low in contrast to read before that.

## Phase 5. The gate, which is a search rather than a judgement

Run these over the diff, not over the whole repository, so the numbers stay small enough to read. Expected value is zero for every one of them.

| Check | What to search for | Expected |
|---|---|---|
| Colour literals | `#[0-9a-fA-F]{3,8}`, `rgb(`, `rgba(`, `hsl(` | 0 new |
| Arbitrary spacing | a raw `px` value in a style prop, or the arbitrary-value bracket syntax if your utility framework has one | 0 new |
| Icon sources | distinct import paths matching any icon package | exactly 1 |
| Inline vectors | `<svg` written by hand | 0 |
| Dependencies | any change to the manifest or the lockfile | 0, unless rung 3 was recorded |
| Raw tags | `<a href`, `<img ` where the framework provides primitives | 0 |
| Breakpoints | any raw media query, or a breakpoint token not in the inventory | 0 |
| Animation | `@keyframes` added, or a duration not from the token set | 0 |
| Shadows | a raw `box-shadow`, or a shadow step above the ceiling | 0 |

The searches themselves, on both platforms. With ripgrep, which is the same command everywhere:

```
rg -n "#[0-9a-fA-F]{3,8}\b" src
```

Without it, on macOS or Linux:

```
grep -rEn "#[0-9a-fA-F]{3,8}" src
```

And in PowerShell on Windows:

```
Get-ChildItem -Recurse src | Select-String -Pattern "#[0-9a-fA-F]{3,8}"
```

Any hit is either a finding or an entry in the inventory table you failed to record. Both are worth knowing, and neither requires anybody's opinion.

## Worked example, compressed

A usage and limits screen is being added to a project management tool that already has ten screens.

**Inventory.** The token file carries semantic colours resolving in all three theme states, a six-step type scale of 12, 14, 16, 20, 24 and 32, an eight-point spacing scale, three shadow steps and four named breakpoints. The component directory has a button with four variants and three sizes, a card, a dialog, a sortable table, a skeleton and an empty state. One icon package and one form library are installed. There is no chart library and no progress indicator of any kind. The nearest sibling is the billing screen, which uses a page heading, a two-column card grid and a toolbar with the primary action on the right.

**The design hand-off asks for five things that are not in the inventory.** A progress meter for quota consumption. A red button for reset usage. A warning triangle icon. A 14 pixel caption under each meter, which is on the scale. And a stacked area chart of usage over 30 days.

**Resolution, rung by rung.** The warning triangle is in the installed icon package under a different name, so it is rung 0 and no second package is added. The red button is rung 1 and additive: a destructive variant on the existing button, so no existing caller changes, and the reason is one sentence. The progress meter is rung 2: the closest existing component is the card and there is no bar primitive, so a local component is composed from a track and a fill using the existing radius and semantic colours, and it stays in the feature folder because one screen uses it. The chart is the only rung 3: nothing installed can draw one, so the reason names the package, its size, and the fact that it becomes the codebase's charting answer rather than this screen's.

**The gate.** Colour literals, zero. Icon import sources, one. Raw anchor and image tags, zero. Breakpoint tokens, all four already in the inventory. Manifest change, one package with a recorded reason. Two arbitrary spacing values turn up in the meter component, an 18 pixel padding and a 6 pixel gap, both snapped to the nearest scale steps.

**Verdict: the screen ships, with one rung 3 escalation recorded in the change description and two spacing values corrected by the gate.** The thing worth noticing is what never reached the code. The design's 14 pixel caption was fine, but had it been 13, the type scale being a closed set was decided during inventory rather than argued about in review, and the closest step would have been used without a conversation.

## Failure modes, named, and what each looks like from the outside

**The beautiful stranger.** Everyone agrees the screen looks good and something feels off. The reliable way to see it is to open the new screen and its nearest sibling in two tabs and switch between them: the heading sits at a different height, the card padding is a step out, and the primary action is on the other side.

**The second icon package.** Users cannot name it and can see it. One icon is a hair heavier than every other icon on the page, or its corners are rounded where the rest are square. It reads as two designers, which is exactly what it is.

**The colour that only works in one theme.** A bug report says text is invisible, one person can reproduce it and nobody else can, and it turns out to depend on the operating system theme of the reporter rather than on anything in the product.

**The duplicate dependency.** The bundle grows, two date pickers are now installed, and the newer one's dialog does not trap focus, so the screen that uses it has a keyboard defect that the older component solved two years ago.

**The invented breakpoint.** The screen reflows at 900 pixels while every sibling reflows at 1024. Dragging a window across that width shows one panel collapsing on its own while the rest of the application waits, which looks like a rendering bug and is a configuration decision.

**The placeholder standing in for a label.** It passes review because the reviewer sees a field with helpful grey text in it. It fails in use, because the text disappears on the first keystroke and never comes back, and on a long form the user has to clear the field to remember what it wanted.

**The silent change to a shared component.** The new screen is correct, and three older screens shift slightly on the same day. Nobody connects the two, because the change description talked about the new screen.

**Copy register drift.** The new screen says something went wrong, in an application whose other ten screens say they could not load your projects and offer a retry. It is the cheapest tell that a screen was written by somebody who did not read the neighbours.

**Conformance without hierarchy.** Every value comes from the token set and the page is still flat and hard to scan, because tokens constrain vocabulary and say nothing about composition. This is the failure this method cannot catch, and it is why a person still looks at the result.

## What this skill does not do

- It does not evaluate the design. A screen can be perfectly conformant and badly composed, and nothing here will tell you the hierarchy is wrong or the whitespace is arrhythmic.
- It does not verify accessibility. The minimums here are floors to build to, and whether the finished screen meets them needs a browser, a contrast sampler and the audits for WCAG conformance, focus and keyboard behaviour, and accessible names and roles.
- It does not enumerate states. It will build the loading, empty and error states you specify and will not notice the ones you forgot, which is a separate pass.
- It cannot fix a system that is already inconsistent. The tie-break rule picks a side, which is not the same as the codebase being right, and following the dominant convention faithfully propagates the dominant mistake.
- It does not replace a linter. Everything in the gate can be enforced continuously by import restriction rules and a declaration-value rule, and a rule that runs on every commit beats a procedure that runs when somebody remembers.
- It has no view on whether the design system itself is any good. If the shared components are wrong, this makes the new screen wrong in exactly the same way, which is the point and also the cost.
