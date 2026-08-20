---
name: accessible-name-audit
description: Audits what every control in an interface is actually called and what it claims to be, using the accessible name computation rather than a reading of the markup. Covers the precedence chain of aria-labelledby over aria-label over native labelling over title, the visible label rule and the voice control failure that breaks it, implicit roles and what a div plus a role does not restore, the states that must be kept in sync including aria-expanded, aria-selected, aria-checked, aria-current and aria-disabled, labelling patterns that silently fail such as placeholders, orphaned label targets and duplicate ids, group naming with fieldset and legend, landmark and region naming, and how to read the accessibility tree in browser developer tools. This skill should be used when building or reviewing any custom control, when an aria-label is about to be added, or when a control behaves correctly with a mouse and does nothing under voice control or a screen reader.
---

# Accessible name and role audit

## The claim this skill is built on

The accessible name is computed, not written. Everyone treats it as a thing you set, and it is a thing the browser derives from up to five competing sources in a fixed order, where the first one that produces text wins and every source below it is discarded silently.

That single fact explains most of the defects in this area. An `aria-label` added with good intentions overrides the visible text of a button, and the person who added it has no way to notice, because the page looks identical, the scan passes, and the only symptom appears for someone driving the interface with their voice.

The second claim is that a role is a promise about behaviour, and adding one to a `div` makes the promise without keeping it. `role="button"` changes exactly one thing: what the accessibility tree reports. It adds no keyboard activation, no focusability, no form participation, no disabled handling. Everything the native element did for free is now yours to write, and the review question is always which of those obligations were actually met.

This skill is the computation order, the obligations a role creates, and the specific things to look for in the accessibility tree.

## The name computation, as a precedence chain

For any element, the browser works down this list and stops at the first step that yields non-empty text.

1. **`aria-labelledby`**, resolved by following the id references and gathering their text. Wins over everything, including visible content.
2. **`aria-label`**, if present and not empty after trimming.
3. **The native host language mechanism.** A `<label>` associated with a form control, a `<legend>` for a `<fieldset>`, a `<caption>` for a `<table>`, `alt` for an `<img>`, `<figcaption>` for a `<figure>`.
4. **The element's own content**, but only for roles that take their name from content: `button`, `link`, `heading`, `cell`, `columnheader`, `rowheader`, `checkbox`, `radio`, `switch`, `option`, `menuitem`, `tab`, `treeitem` and a handful more. Roles that do not include `textbox`, `combobox`, `listbox`, `region`, `group`, `form`, `navigation`, `table`, `img`, `slider` and every landmark. This is why `<div role="button">Save</div>` gets the name "Save" and `<div role="region">Filters</div>` gets no name at all.
5. **The `title` attribute**, as a last resort. For a text input with nothing else, browsers fall back further still, to the placeholder. Both are fallbacks, neither is a labelling strategy, and `title` in particular does not appear on touch devices and does not appear on keyboard focus.

**The consequence people do not expect.** Steps 1 and 2 beat step 4. Put an `aria-label` on a button that already reads "Add to basket" and the visible words stop being the name. Speech input tools match what the user says against the accessible name. "Click Add to basket" now matches nothing, and the control has become unusable for a group of people while looking perfectly fine to everyone else and passing every automated check that only tests whether a name exists.

**One more surprise, in the other direction.** Text pulled in by `aria-labelledby` is used even when the referenced element is hidden with `display: none`, because a node referenced directly by `aria-labelledby` contributes regardless of its hidden state. Hiding a label to remove it from the name does not remove it from the name.

## The visible label rule

Where a control has a visible text label, the accessible name **must contain that visible text**, and for practical purposes it must **start with it**.

The containment part is a level A requirement of WCAG 2.2, criterion 2.5.3. The starting-with part is not required and is what makes voice control reliable, because several speech engines match on a prefix or on an exact string rather than on a substring.

- Visible "Send", name "Send". Correct.
- Visible "Send", name "Send message to support". Conformant and usable: it contains the visible text and starts with it.
- Visible "Send", name "Submit the contact form". A failure. The user says "click Send" and nothing happens.
- Visible "Next", name "Next: delivery options". Fine.
- Visible "Next", name "Step 3 of 5, next". Contains the word and does not start with it. Conformant, and the least reliable of the workable options.

For a control with **no** visible text, an icon-only button, any accurate name is legal. Match it to whatever the tooltip says, because that is the word the user will speak.

## Roles, and what a role does not restore

Every native element has an implicit role. `<button>` is `button`, `<a href>` is `link`, `<a>` without `href` is `generic` and is not focusable, `<input type="checkbox">` is `checkbox`, `<select>` is `combobox`, `<nav>` is `navigation`, `<main>` is `main`, `<ul>` is `list`, `<h2>` is a `heading` at level 2. Adding a matching role to a native element is redundant. Adding a conflicting one is a defect.

Replacing a native element with a `div` and a role loses everything the element did that the role does not describe:

- **Focusability.** A `div` is not in the tab order. It needs `tabindex="0"`, and remembering that is on you.
- **Keyboard activation.** A native button fires on both Enter and Space. `role="button"` fires on neither. A native link fires on Enter only, and a `role="button"` on a link creates a control announced as a button that does not respond to Space, which is a mismatch between what is announced and what happens.
- **Form participation.** A `div` submits no value, is ignored by form validation, and cannot be `type="submit"`.
- **Default states.** The `disabled` attribute does nothing on a `div`. Neither does `checked`, `required` or `readonly`.
- **Label association.** A `<label>` only labels a labelable element: `button`, `input`, `meter`, `output`, `progress`, `select`, `textarea`. Wrapping a `div role="checkbox"` in a `<label>` does nothing at all, silently.
- **Descendant semantics, in some cases.** Several roles including `button`, `checkbox`, `img`, `switch`, `tab` and `option` have presentational children, which means the semantics of anything inside them are dropped. A heading inside a button is not a heading.

**The first rule of using ARIA is not to use ARIA when a native element will do**, and the reason is exactly the list above. ARIA changes what is reported to assistive technology and changes no behaviour whatsoever. Every role you add is a promise you then have to keep in JavaScript, and the review question is which promises were kept.

**Decision rule.** If a native element exists with the behaviour you need, use it. If a native element exists and the styling looks impossible, use it anyway and style it, because the genuinely unstylable set is small and shrinking. If no native equivalent exists, which is true for tabs, trees, comboboxes with rich listboxes and most menus, build from the published pattern and accept that you now own the keyboard behaviour, the states and the focus management. **If you cannot tell whether the native element covers your case**, build the native version first and write down what is missing: either that list is short, in which case use it, or it is long, in which case you have your justification recorded rather than assumed.

## The states that must be kept in sync

These are attributes with a value that has to change when the interface changes. Nothing throws when they do not, which is why they are the most common defect in a shipped custom control.

- **`aria-expanded`** goes on the control that operates a disclosure, never on the region being disclosed. It must flip on every path that changes the state, including Escape, a click outside, a route change and a programmatic close. The failure looks like a panel that is visibly open while the button still announces "collapsed", so the announcement directly contradicts the screen.
- **`aria-selected`** belongs on `role="tab"` inside a `role="tablist"`, and on `option`, `row`, `gridcell`, `columnheader`, `rowheader` and `treeitem`. It is **not valid on a button or a list item**, where it is ignored entirely. The failure is a tab strip built from buttons where the selected tab is announced identically to the other three.
- **`aria-checked`** is required on `role="checkbox"`, `radio`, `switch`, `menuitemcheckbox` and `menuitemradio`. A custom checkbox without it announces "checkbox" and no state, which carries the same information as no announcement. The value `mixed` is valid on `checkbox` and `menuitemcheckbox` only.
- **`aria-current`** marks the one item in a set that is current, with the token values `page`, `step`, `location`, `date`, `time`, or `true`. Use `page` for the current item in navigation. The failure is applying it to every item, or using it where `aria-selected` belongs.
- **`aria-disabled` against the `disabled` attribute** is a real choice and not a synonym. `disabled` removes the control from the tab order, blocks activation, and excludes it from form submission, which means a keyboard user cannot reach it and therefore cannot read the explanation of why it is unavailable. `aria-disabled="true"` announces the state and changes nothing else: the control stays focusable, still fires its handlers, and you must block the action yourself. Use `disabled` when the control is inert and the reason is visible next to it. Use `aria-disabled` plus a blocked handler when the user needs to reach the control to learn why they cannot use it, which is usually the better experience.
- **`aria-hidden="true"` on anything focusable** produces an element that is reachable by Tab and absent from the accessibility tree. The user lands there and hears nothing, or hears "blank". This breaks a stated rule of ARIA and in practice fails the name, role and value criterion. Two related traps: `aria-hidden="false"` does not undo an `aria-hidden` ancestor, and the background-hiding technique used for modals must never end up covering the element that currently has focus.

## Labelling patterns that fail

- **A placeholder used as a label.** It disappears the moment the user types, it is the lowest fallback in the name computation, and its default styling almost never reaches the required contrast. It also removes the label from the screen at exactly the moment the user is reviewing what they entered.
- **An icon-only button with no name.** The computed name is empty and the control announces as "button". Fix it with visually hidden text inside the button, which is preferable because a revealed tooltip then matches it, or with `aria-label` matching the tooltip word for word.
- **A `label` whose `for` points at an id that does not exist.** Nothing happens, no console warning, no visual difference. Common after a refactor renames an input.
- **Several elements sharing one id.** `for` and `aria-labelledby` resolve to the first element with that id in document order, so every later instance is unlabelled. This appears almost exclusively in repeated components: a card, a row, a list item whose ids were written once and rendered forty times.
- **A `label` wrapping two controls.** A label's labelled control is its first labelable descendant. The second control inside it gets no label and no warning.
- **A link whose only text is "Read more", repeated forty times.** The links list a screen reader user pulls up shows forty identical entries with no way to distinguish them. Extend the link text with visually hidden context, or rewrite it.
- **`aria-label` on a plain `div` or `span`.** With no role, the element is `generic`, which does not support naming, so the attribute is ignored. The name you thought you set does not exist.
- **`aria-describedby` used to carry the label.** A description is announced after the name, after a pause, and some configurations suppress it. Nothing essential belongs there.

## Groups

A control that only makes sense as part of a set needs the set to have a name.

Use `<fieldset>` with `<legend>`, where the legend becomes the group's accessible name, or `role="group"` and `role="radiogroup"` with `aria-labelledby` pointing at an existing heading.

**The failure almost nothing gets right is the radio group.** The pattern is a question in a paragraph or heading, then three radio inputs each with its own label, and no fieldset. Each radio announces only its own label. Someone hears "Standard, radio button, one of three" with no indication of what the question was, and the question was "Delivery speed" or "Refund method" or "Which account do you want to close", which matters rather a lot. The same applies to a set of checkboxes under a shared question.

Keep nesting to one level. Nested fieldsets are legal and every legend accumulates into the announcement.

## Landmarks and regions

Landmarks are how a screen reader user skips your layout. Getting them roughly right is high value and cheap; getting them elaborately right is not.

- **One `banner`, one `main`, one `contentinfo`** per page. `<header>` and `<footer>` only map to `banner` and `contentinfo` when they are not inside `<article>`, `<aside>`, `<main>`, `<nav>` or `<section>`.
- **Multiple `<nav>` elements need distinct names.** Primary, breadcrumb, footer, in-page contents. Do not put the word "navigation" in the name: the role is already announced, and "Primary navigation" becomes "Primary navigation navigation".
- **`<section>` is only a `region` when it has an accessible name.** An unnamed section is generic and contributes nothing, so either name it or use a `<div>`. The same applies to `<form>`, which is only exposed as a form landmark when it is named.
- **Everything on the page should sit inside some landmark**, because a user navigating by landmark will otherwise never reach the orphaned content.
- **Do not inflate the count.** Four to eight landmarks on a page is a usable map. Twenty named regions is a second navigation problem.
- **Nesting is where it goes wrong.** A `<header>` inside `<main>` is not a banner, which is correct and rarely expected. `<main>` inside a `<section>` is a structural error. Use the search role, or the `<search>` element, for the site search form rather than a named region.

## Tables and lists, briefly

Tabular content is a name and role problem like any other.

A data table needs a name, from `<caption>` or `aria-label`, and header cells marked with `<th>` and `scope="col"` or `scope="row"`. A table used for layout needs `role="presentation"`. A grid built from `<div>` elements needs `role="table"`, `row`, `cell` and `columnheader` explicitly, and if it is virtualised it also needs `aria-rowcount` and `aria-rowindex`, because otherwise the announced count describes what is rendered rather than what exists. A sortable column carries `aria-sort` on the header cell, on one column at a time.

For lists, the specific trap worth knowing: applying `list-style: none` causes Safari to drop list semantics for VoiceOver unless `role="list"` is added back explicitly. Since almost every design system removes bullets, almost every design system needs the role. And a `<ul>` may only contain `<li>`, `<script>` and `<template>` as children, so a wrapper `<div>` between the list and its items breaks the relationship.

## How to actually check: the accessibility tree

This is the practical half, and it takes ten minutes.

Open the browser's element inspector and find the accessibility panel. In Chrome and Edge it is a pane in the sidebar of the Elements panel, alongside Styles and Computed, with a toggle for viewing the full page tree. In Firefox it is a top-level Accessibility panel. In Safari it is a section of the Node tab in Web Inspector.

What the pane gives you for the selected element: the computed **role**, the computed **name**, the **source** the name came from, and the ARIA states and properties in effect. The name source is the highest-value field on the screen, because it is labelled with the mechanism that won, and the losing candidates are shown too. Seeing "from aria-label" on a control with visible text is the whole finding, immediately.

**Pin the pane and Tab through the page**, reading role and name at every stop. Then look for these seven things:

1. **An interactive node with an empty name.** Announces as bare "button" or "link".
2. **A name that differs from the visible text.** The voice control failure, and the fastest of all of these to spot.
3. **`generic` as the role on something that is clicked.** A `div` with a handler and no role.
4. **A node marked ignored, with its reason.** `aria-hidden`, presentational, or not rendered. An ignored node that is also focusable is always a defect.
5. **A heading sequence that jumps or repeats.** Levels going 1, 2, 4, or four headings all reading the same words.
6. **Identical names repeated in a list.** Forty "Read more" entries, or three tabs whose selected state is invisible in the tree.
7. **A state property that is missing or stale** on a control that visibly has states. Open the panel, then toggle the control, and watch whether the property changes. If it does not change in the tree, it did not change for the user.

That last one is the only reliable way to catch stale state, and it is why this has to be done in a live page rather than by reading source.

## Worked example, compressed

A product listing page: a header with a logo link and an icon-only search toggle, a primary nav and a breadcrumb nav, a filter sidebar with three disclosure sections, a delivery speed radio set, a grid of cards each with a "Read more" link and a save button, and a sort dropdown built from divs.

- **Logo link.** Contains an `<img>` with `alt=""` and an `aria-label` of "Home". Name resolves to "Home" from `aria-label`. Works, and the cleaner fix is `alt="Home"` on the image so the name comes from content. **Pass, with a note.**
- **Search toggle.** A `<button>` containing only an inline SVG with no `<title>` and no label. Computed name empty. **Failure**, and a blocker: the control cannot be named, described or spoken to. Fix with visually hidden text reading "Search".
- **Two `<nav>` elements, neither named.** The tree shows two navigation landmarks with identical empty names, so a user cannot tell which is which. **Failure.** Add `aria-label="Primary"` and `aria-label="Breadcrumb"`, without the word navigation.
- **Filter disclosures.** Built from buttons with `aria-expanded`, correctly on the button rather than the panel. Opening one flips the attribute. Pressing Escape closes the panel visually and leaves `aria-expanded="true"`. **Failure**, stale state, and invisible to any static check.
- **Delivery speed radios.** Three native `<input type="radio">` with proper labels, under an `<h3>` reading "Delivery speed", with no fieldset and no `role="radiogroup"`. Each option announces alone. **Failure**, fixed by wrapping in a fieldset with a legend, or a group named by the existing heading id.
- **Card "Read more" links.** Forty identical names in the links list. **Failure** of link purpose, fixed with visually hidden text naming the product.
- **Card save buttons.** A `<button>` reading "Save" with `aria-label="Save product to your wishlist"`. The name contains the visible word and does not start with it. **Conformant, and a voice control risk**, because an exact-match engine will not resolve "click Save". Reorder to "Save to wishlist" so the visible word leads.
- **Sort dropdown.** A `div` with `role="combobox"`, `tabindex="0"`, and a list of `div` children with no roles. `aria-expanded` present, `aria-controls` present, `aria-activedescendant` absent, options not exposed as `option`. The tree shows a combobox containing generic nodes. **Failure**, and the honest fix is a native `<select>`, which the design can absorb.
- **Card price.** `<div aria-label="Price">` around the amount. The element is generic, so the label is ignored. **CANNOT TELL** from the tree alone whether this matters, because the price text is present as content and reads fine in sequence. Record it as a redundant attribute to delete rather than a defect, and note that confirming it needs a screen reader pass.

**Verdict: six failures, one conformant risk, one attribute to remove.** Two are blockers, the unnamed search toggle and the combobox, and the combobox is the only one that needs more than fifteen minutes because the correct fix replaces the component. The stale `aria-expanded` is the one that would have survived every automated check and every code review, because the markup is right and only the behaviour is wrong.

## Failure modes

**`aria-label` applied as a habit.** Added to everything interactive on the theory that more labelling is safer. It overrides visible text, it is invisible in the rendered page, it does not translate with the page content, and it is the single most common cause of a control that voice users cannot operate.

**The tree never opened.** The entire review conducted by reading JSX or HTML. Everything in this file about stale state, ignored nodes and computed names is undetectable that way, because those are properties of the running page.

**State set on mount and never again.** `aria-expanded="false"` written into the markup, never touched by the toggle handler. The interface is correct on first paint and wrong from the first interaction onwards.

**A `div` with a role and nothing else.** The role is announced, so a check for a role passes, and the control cannot be focused, cannot be activated by keyboard, and reports no state. Announcing a promise is not keeping it.

**Label present, association absent.** A visible label sitting next to its input with no `for`, or a `for` pointing at a renamed id. Looks labelled to every reviewer looking at the screen and is unlabelled in the tree.

**Naming the region instead of the control.** An `aria-label` on the wrapping `div` rather than on the button inside it, so the wrapper carries a name nothing reads and the control has none.

**`aria-hidden` used to tidy up the tree.** Applied to a decorative wrapper that happens to contain a focusable element, producing a tab stop that announces nothing.

**Duplicate ids in a repeated component.** Written once, correct once, rendered forty times, and thirty-nine of the labels are silently orphaned. This one scales with the success of the component.

**Landmark inflation.** Every section given a role and a name, so the landmark list becomes twenty entries and stops being a shortcut. Structure that helps nobody is still structure that has to be read.

## What this skill does not do

- It does not test with assistive technology, and several things here can only be confirmed that way, including whether a name is announced at a useful moment. NVDA on Windows and VoiceOver on Mac differ on several patterns.
- It does not cover keyboard interaction, tab order, focus trapping or focus placement, which are half of what a custom control needs and belong to a separate focus and keyboard audit.
- It covers one criterion of the standard in depth and ignores the rest. It says nothing about contrast, reflow, captions, timing, target size or error recovery.
- It cannot judge whether a name is the right wording for your users. Save against Apply against Update is a content decision, and only user testing settles it.
- It will produce almost nothing on a page of prose, links and a native form, because native HTML computes all of this correctly on its own.
- It does not implement anything. It produces a list of controls, their computed names and roles, and what each one should be instead.
