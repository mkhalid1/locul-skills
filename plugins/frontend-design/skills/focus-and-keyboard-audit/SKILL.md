---
name: focus-and-keyboard-audit
description: Audits a screen for keyboard operability and focus management. Covers what must be reachable and operable by keyboard, the components that are almost always broken (custom selects, comboboxes, date pickers, drag handles, carousels, tooltips, and anything built from a div with a click handler), the mismatch a CSS reorder creates between visual order and tab order, focus trapping and focus return for modals, where focus goes after a destructive action removes the focused element, roving tabindex against aria-activedescendant, skip links, focus indicator requirements under WCAG 2.2, and live region announcement. This skill should be used when reviewing or building any interactive interface, and before any modal, menu, custom control or destructive action ships.
---

# Focus and keyboard audit

## The claim this skill is built on

Keyboard defects are not caught by clicking, and clicking is how every interface is tested.

A mouse user cannot experience a broken tab order, cannot notice that focus fell to the document body after a delete, and cannot tell that the fourth card in a grid is visually first. Every one of those is invisible to the person who built the screen and immediately disabling to the person who cannot use a pointer. That asymmetry, rather than any lack of care, is why these defects survive review and reach production.

The second claim is that the list of what to check is longer than anyone reproduces from memory. Most reviews cover three things: is there a focus ring, does Tab reach the buttons, does the modal close. Those are the easy third. The parts that strand people are focus placement after the DOM changes, the mismatch between visual and DOM order, and announcement of anything that happened without a page load.

This skill is that longer list, in the order the defects actually bite.

## Phase 1: reachability

Every interactive thing must be reachable by keyboard and operable by keyboard. WCAG 2.1.1 Keyboard, level A, and it is the criterion with the least room for negotiation in the whole standard.

Reachable means Tab and Shift+Tab get there, or an arrow key does inside a composite widget. Operable means the action can be completed, not merely triggered.

**The components that are almost always broken**, in rough order of how reliably they are broken:

- **Anything built from a `div` with a click handler.** A native button receives focus, fires on both Enter and Space, and is announced as a button. A div does none of that. Retrofitting it takes `tabindex="0"`, `role="button"`, an accessible name, a keydown handler for Enter, and a separate handler for Space that also prevents the default page scroll on keydown and fires on keyup. Five things to remember instead of one element, which is why the correct finding is almost always "use a button".
- **Custom selects and comboboxes.** The list opens on click and not on Enter, Space, Alt+Down or typing. Arrow keys scroll the page instead of moving the highlighted option. Escape does not close it. Focus is left inside a list that has been removed from the DOM. Type-ahead does not exist.
- **Date pickers.** The grid needs arrow keys for days, Page Up and Page Down for months, Home and End for the ends of a week, and a way to reach the month and year controls without leaving the grid. Most implementations offer a text input and a mouse-only calendar and call it a choice.
- **Drag handles.** Reordering that exists only as a drag is unusable by keyboard and now also fails WCAG 2.2 SC 2.5.7 Dragging Movements at level AA, which requires a single-pointer alternative that is not a drag. There must be a keyboard path, usually Space to pick up, arrows to move, Space to drop, Escape to cancel, with each step announced.
- **Carousels.** Controls that are visible only on hover are unreachable. Auto-advance that cannot be paused fails SC 2.2.2. Slides moved out of view are commonly left focusable, so Tab disappears into content nobody can see.
- **Tooltips.** SC 1.4.13 Content on Hover or Focus requires that the content be dismissible without moving the pointer or focus, usually with Escape, that it can be hovered without vanishing, and that it persists until dismissed or invalidated. A tooltip that appears on mouseover only fails reachability outright.
- **Anything that opens on hover.** Menus, popovers, previews. If there is no focus or click equivalent, it does not exist for a keyboard user.
- **Custom checkboxes and switches** where the real input is hidden with `display: none`, which removes it from the focus order entirely. Hide it with a clipping technique that keeps it focusable, or use `appearance: none` on the real control.

**Also check that nothing is reachable that should not be.** Elements behind an open modal, offscreen slides, and hidden menu items all remain in the tab order unless they are removed from it. Tab disappearing into invisible content is the same failure from the other direction.

## Phase 2: focus order

**Focus order follows DOM order.** It does not follow visual order, and the two come apart the moment CSS reorders anything.

CSS grid placement, flex `order`, `flex-direction: row-reverse`, `grid-auto-flow: dense` and absolute positioning all move things visually while leaving the DOM untouched. A sighted keyboard user then watches the focus ring jump from the top left card to somewhere in the middle of the grid and back, which is disorienting and, when the reordering is meaningful, a failure of SC 2.4.3 Focus Order at level A.

**This is the defect nobody catches by clicking**, because a mouse user never observes order at all. It is also the one most likely to be introduced by a responsive layout change months after the component was reviewed, since the reorder often only applies at one breakpoint.

The fix is to reorder the DOM and let CSS follow, not the other way round. Where that is genuinely impossible, the reordering must at least be visually insignificant, meaning nothing about the meaning of the page depends on the order.

**Positive `tabindex` values are almost always a bug.** Any element with `tabindex="1"` or higher is placed in a separate sequence that comes before every element with `tabindex="0"`, ordered by number. Add one and the whole page's order changes. The correct values are `0`, meaning include in the natural order, and `-1`, meaning focusable by script but not by Tab. A single positive value anywhere in a codebase is worth a finding on its own.

## Phase 3: focus placement when the DOM changes

This is where the real damage is done, and it is the phase reviews skip.

**Focus after a destructive action.** When the element holding focus is removed from the document, most browsers move focus to `<body>`. That is not a neutral outcome. A screen reader loses its place and returns to the top of the document, the next Tab starts from the beginning of the page rather than where the user was, and the user has no indication that anything happened or where they now are. **Losing focus to the body is the single most common focus failure in production interfaces.**

The rule, in order of preference: move focus to the equivalent element that took the removed one's place, usually the next row in the list; if the removed item was last, move to the previous one; if the list is now empty, move to the container holding the empty state, made programmatically focusable with `tabindex="-1"`; and if the removal came from a menu or a dialog, return focus to the control that triggered it. Whichever you choose, the outcome must also be announced, because moving focus silently tells a screen reader user where they are but not what happened.

The same rule applies to any mutation that removes the focused element: filtering a list, collapsing a section, completing a step in a flow, or a live update from the server.

**Focus on new content.** When an action reveals content, decide deliberately whether focus moves to it. Move it for anything the user must now act on, such as a dialog, an error summary after a failed submit, or a newly revealed form section. Do not move it for an incidental update, because unexpected focus movement is its own failure. When focus is not moved, the change needs a live region instead. Where the new content is a form's error summary, this skill settles only where focus lands: the `aria-describedby` and `aria-invalid` wiring, the summary's construction and the validation timing that produced it are the Form design and validation audit skill's.

## Phase 4: modals and the background

A modal has four obligations and most implementations meet two.

**On open:** move focus into the dialog. Either to the first meaningful control, or to the dialog container itself when the first control is destructive, which avoids the pattern where a fast Enter confirms something the user has not read. Remember the element that opened it.

**While open:** the background must be inert, not merely covered. `inert` on the background containers is now the correct mechanism, and it does the whole job at once: it removes descendants from the focus order, from hit testing, and from the accessibility tree, so a screen reader cannot reach the background by virtual cursor either. That last part is what a hand-rolled JavaScript focus trap never achieves, because it only intercepts Tab. The `inert` attribute reached the major browser engines between 2022 and 2023, which is approximate, so confirm against your support floor.

The native `<dialog>` element opened with `showModal()` gives you inert background, the top layer, and Escape handling without writing any of it. It became available across the major engines during 2022. If you are building a modal now and do not have a specific reason to avoid it, that is the answer, and half of this section stops being your problem.

**Escape:** must close the dialog, from anywhere inside it, without saving. Two things break this in practice. A nested component, most often a custom select, swallows the keydown to close itself and does not let it propagate, so Escape closes the select on the first press and the dialog on the second, which is correct, but many implementations stop the event unconditionally, so Escape never reaches the dialog at all. And a dialog that has unsaved changes should confirm rather than discard silently, which means Escape triggers a decision rather than a close.

**On close:** **return focus to the element that opened the dialog.** This is the obligation that is almost never implemented. Without it, focus falls to the body and the user is returned to the top of the page having just completed an action somewhere in the middle of it. If the triggering element no longer exists, because the dialog deleted it, fall back to the nearest sensible ancestor by the rule in phase 3.

## Phase 5: composite widgets

**The rule: a group of peer items takes one tab stop, and arrow keys move within it.** Tabs, radio groups, toolbars, menus, menu bars, trees, grids, and listboxes all follow it. Twenty tabs should not be twenty tab stops, because a user trying to reach the content after them should not have to press Tab twenty-one times.

There are two implementations and the choice between them is not arbitrary.

**Roving `tabindex`.** Exactly one item in the group has `tabindex="0"` and every other has `tabindex="-1"`. Arrow keys move the `0` to the newly current item and call `focus()` on it. DOM focus is genuinely on the item. Use this when the items are real focusable controls and when focus being physically on them is correct: toolbars, tab lists, tree items, radio groups.

**`aria-activedescendant`.** DOM focus stays on the container, and the container's `aria-activedescendant` attribute points at the id of the currently active item. Nothing inside ever receives real focus. Use this when focus must remain in a text input while a separate list is being navigated, which is exactly the combobox case: the user is typing and arrowing at the same time, and moving real focus into the list would take it out of the input.

**The tell that a composite widget is wrong**: Tab moves through every item one at a time, or arrow keys scroll the page instead of moving between items. Both are immediately visible in the test procedure below.

**Home and End** should go to the first and last item, and for a long list that is not optional, it is the only fast path. Type-ahead, jumping to the item beginning with a typed character, is expected in menus and listboxes.

## Phase 6: the focus indicator

**Removing outlines without replacing them is a failure of SC 2.4.7 Focus Visible at level AA.** A blanket rule that sets `outline: none` anywhere in the codebase is a finding regardless of what else is present, because it will apply to elements the author never considered.

**`:focus` against `:focus-visible`.** `:focus` matches whenever an element has focus, including after a mouse click, which is why people removed outlines in the first place: they did not want a ring around a button somebody just clicked. `:focus-visible` matches only when the browser judges that a visible indicator is appropriate, which in practice means keyboard interaction, and always for text inputs. Style `:focus-visible` and leave `:focus` alone. Provide a fallback for older engines if your support floor needs one; `:focus-visible` reached the major engines between 2020 and 2022, which is approximate.

**What the indicator must satisfy.** Under WCAG 2.1, SC 1.4.11 Non-text Contrast requires 3:1 against adjacent colours for the indicator, and this is the criterion that catches a default browser ring left on a surface it barely shows against.

WCAG 2.2, a W3C Recommendation since 5 October 2023, added three things worth knowing exactly:

- **2.4.11 Focus Not Obscured (Minimum), level AA.** When an element receives focus, it must not be entirely hidden by author-created content. In practice this is about sticky headers, sticky footers and cookie bars: an element scrolled to just under a sticky header gets focus, and the user sees nothing. This is the most common new failure in 2.2 and it is introduced by a layout decision rather than by anything in the component.
- **2.4.12 Focus Not Obscured (Enhanced), level AAA.** The same, but no part may be obscured.
- **2.4.13 Focus Appearance, level AAA.** The indicator must cover an area at least as large as a 2 CSS pixel thick perimeter of the component, and the changed pixels must have at least 3:1 contrast between the focused and unfocused states.

The AAA criteria are not usually contractual, but they are the only published numbers for what a good indicator looks like, so use them as the design target and report against AA.

## Phase 7: announcement

Anything that changes without a page load must be announced, or it did not happen for a screen reader user.

**`aria-live="polite"`** waits for a natural pause. It is correct for almost everything: a saved confirmation, a result count, a status change.

**`aria-live="assertive"`** interrupts whatever is being spoken. Reserve it for things the user must know immediately, typically an error that blocks what they are doing. Assertive used routinely makes an interface unusable, because every minor update cuts off the sentence the user was listening to.

`role="status"` carries an implicit polite live region and `role="alert"` an implicit assertive one, which is usually cleaner than the attributes.

**The mechanical rule that catches most broken announcements: the live region must already exist in the DOM, empty, before the message is written into it.** Screen readers register a live region when it appears and then watch it for changes. A region that is created and populated in the same update is frequently announced late or not at all, and the behaviour differs between screen readers, which is why it looks intermittent. Render the empty container on first paint and write text into it afterwards. A short delay before writing, on the order of a tenth of a second, makes it more reliable in practice.

**Toasts are announced wrongly more often than not**, for three separate reasons. They are frequently given `role="alert"` for routine confirmations, which interrupts. They are frequently created and filled at once, so they are not announced at all. And they auto-dismiss on a timer, so any control inside them, an undo button most often, is unreachable by a keyboard user who has to tab to it. If a toast contains an action, it must not auto-dismiss, and SC 2.2.1 has something to say about the timer in any case.

## Phase 8: keyboard shortcuts

Screen readers in browse mode consume single letter keys for navigation: a key to jump to the next heading, another for the next button, another for the next form field. **An application that binds a bare letter to an action collides with that, and the collision is completely invisible to sighted testing.**

SC 2.1.4 Character Key Shortcuts, level A, added in WCAG 2.1, requires one of three things for any shortcut using only letters, punctuation, numbers or symbols: it can be turned off, it can be remapped to include a modifier, or it is active only while the relevant component has focus.

The cheap compliant answer is to require a modifier. The next cheapest is to scope the shortcut to a focused component. Offering remapping is the most work and the best experience.

Two more rules that are not in the standard but should be. Do not override the browser's own shortcuts, particularly the find command, without a very good reason. And document the shortcuts somewhere reachable by keyboard, which usually means a dialog opened by a question mark that itself follows every rule in phase 4.

## The test procedure

Unplug the mouse. Not "avoid using it": physically remove it, because the habit of nudging it is unbreakable otherwise. On a laptop, sit on your hands.

Then Tab through the entire page from the address bar, and watch for exactly this list:

1. **Focus disappears.** You press Tab and no indicator is anywhere on screen. Either focus is on an invisible element, or the indicator is hidden behind a sticky header, or it has fallen to the body.
2. **Focus goes somewhere unexpected.** The order does not match what you are looking at. Note the element before and after the jump.
3. **You cannot reach something you can see.** A control, a link, a menu item.
4. **You reach something you cannot see.** Offscreen slides, hidden menus, content behind an overlay.
5. **You cannot get out.** Tab cycles inside a component forever, or Escape does nothing. This is SC 2.1.2 and it is the most serious single defect in this list, because it ends the session.
6. **Enter or Space does not activate a control** that looks like a button.
7. **Arrow keys scroll the page** while you are inside something that should consume them.
8. **Something changes and nothing is announced.** Check with a screen reader, since this is the one item on the list you cannot verify visually.
9. **Focus is lost after an action.** Delete a row, close a dialog, submit a form, and press Tab immediately: if the next focus is the first link on the page, focus was lost.
10. **The indicator is invisible on one surface but not another**, which usually means one fixed indicator colour against two theme backgrounds.

Repeat items 5 and 9 with a screen reader running, because a virtual cursor can reach content that Tab cannot, and a focus trap that appears fine to Tab can still leave the virtual cursor loose in the background.

## Decision rule: one tab stop, or many

For any collection of interactive items:

- **Are the items peers where exactly one is current at a time**, such as tabs, radio buttons, toolbar controls, tree nodes, menu items, or grid cells? Then it is a **composite widget**: one tab stop, arrow keys inside, Home and End to the ends.
- **Are the items independent controls a user might want in any order**, such as the fields of a form, a list of article links, or a set of cards each linking somewhere different? Then **each is its own tab stop** and no arrow key handling is needed.
- **Does keeping real focus in a text input matter** while a list is navigated? Then the composite widget uses `aria-activedescendant`. Otherwise use a roving `tabindex`.
- **If you cannot tell**, apply this test: would a user ever want to reach the fortieth item without passing the first thirty-nine? If yes, it needs to be a composite widget or the tab stops need collapsing some other way. If the collection is small, under roughly seven items, and you still cannot tell, **leave them as separate tab stops**. That is verbose but it never traps anyone and it never makes an item unreachable, whereas a half-implemented composite widget does both. The failure modes are not symmetrical, so the default should not be either.

## Worked example, compressed

A document library screen: a filter combobox, a grid of document cards, a per-card actions menu, a delete confirmation dialog, and a toast with an undo action.

**Reachability.** The combobox is a div with a click handler and a rendered list. It opens on click only, not on Enter, Space or Alt+Down. Arrow keys scroll the page. Escape does nothing. Finding: rebuild against the combobox pattern, or use a native select if the filtering does not need one.

**Focus order.** The grid uses `order` in CSS to promote pinned documents to the top row at the widest breakpoint only. Visual order and DOM order disagree at that breakpoint and agree at every other, which is why nobody noticed. Finding: sort in the data layer and remove the CSS reorder.

**Composite widgets.** The actions menu gives every item its own tab stop and does not handle arrow keys. Finding: one tab stop, roving `tabindex`, arrows, Home and End, Escape returns focus to the trigger.

**Modal.** The delete dialog moves focus to the confirm button on open, which is destructive and one keypress away from a user who is still reading. The background is covered by an overlay but not made inert, so Tab walks behind it and a screen reader can read the whole page underneath. Focus is not returned on close. Findings: focus the dialog container, apply `inert` to the background or use the native dialog element, store and restore the trigger.

**Focus after delete.** The confirmed delete removes the card, and focus falls to the body. Finding: move focus to the next card, or to the previous one if the deleted card was last, or to the empty-state container if none remain.

**Announcement.** The toast is created and filled in the same update and carries `role="alert"`. It auto-dismisses after five seconds and contains the undo action. Findings: render an empty polite live region on first paint and write into it, and either drop the auto-dismiss or move undo out of the toast.

**Indicator.** A global rule removes outlines and a replacement ring is defined on `:focus` for buttons only, so links and inputs have nothing. Findings: remove the blanket rule, style `:focus-visible` globally.

**Verdict: the screen is not operable by keyboard.** Two defects end the session on their own, the combobox that cannot be opened and the delete that discards focus, and either is enough to stop a keyboard user completing the primary task. Fix order: combobox, focus after delete, modal inertness and focus return, indicator, menu arrow keys, live region, grid DOM order.

## Failure modes

**Focus lost to the body.** After a delete, a close, or any removal of the focused element. Tab restarts at the top of the page and a screen reader loses its place. The most common focus defect in production.

**The CSS reorder.** Visual order and DOM order disagree because of grid placement or flex `order`, frequently at one breakpoint only, so it is invisible in the layout everybody develops in.

**The overlay that covers but does not inert.** Tab walks into the background, and a screen reader's virtual cursor reads the page under the dialog as though it were still there. A Tab-only focus trap does not fix this.

**The swallowed Escape.** A nested component stops the keydown unconditionally, so Escape never reaches the dialog and the only way out is the close button, which the user may not be able to reach.

**The div that acts like a button.** No focus, no Enter, no Space, no role, no name. Recognisable in review because it has an onClick and no `tabindex`.

**The blanket outline removal.** One rule, applied everywhere, replaced for buttons only, leaving links and inputs with no indicator at all in the theme where the default ring does not show.

**The unannounced live region.** Created and filled in the same update, so it registers too late to be spoken. Intermittent by nature and different between screen readers, which is why it gets dismissed as flaky.

**The assertive toast.** Routine confirmations that interrupt the sentence in progress, sometimes several in a row, which makes the whole interface hostile to listen to.

**The positive tabindex.** One `tabindex="3"` left in a component years ago, pulling that element in front of every other focusable thing on any page that includes it.

**Focus obscured by the sticky header.** The element is focused and scrolled into view, and the browser's scroll position places it exactly under the fixed bar. Introduced by a layout change, never by the component, and now an explicit AA failure under WCAG 2.2.

## What this skill does not do

- It reads source rather than behaviour. Whether an indicator is genuinely visible against the surface behind it, and whether a live region actually speaks, need the page running and a screen reader listening.
- It does not resolve disagreements between assistive technologies. Screen readers differ on live region timing and on several ARIA patterns, and the only way to know is to run more than one.
- It is keyboard and focus only. Contrast, captions, alternative text, cognitive load and form error recovery beyond focus placement are all outside it.
- It cannot judge whether an interaction is a good idea, only whether it is operable. A keyboard-accessible drag-and-drop reorder is still a poor way to reorder fifty items.
- It will over-specify a simple page, and the composite widget analysis in particular is wasted on a screen with no groups of peer controls.
- It does not replace an automated rule engine, which is better at the mechanical checks, cheaper, and runs on every commit rather than once.
