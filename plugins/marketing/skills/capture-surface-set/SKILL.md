---
name: capture-surface-set
description: Designs the email capture layer of a website as a coordinated set of surfaces rather than a single popup, mapping each page type to exactly one surface and one trigger, with the numeric trigger values, the desktop-only constraint on exit intent, the mobile field and button ordering, global suppression and frequency rules, the consent and accessibility contract for interruption surfaces, and per-surface measurement with named denominators. This skill should be used when adding, replacing or rationalising email capture on a site that has more than one page type, or when a site-wide signup rate is being reported as a single blended number.
---

# Capture surface set

## The claim this skill is built on

The usual approach is to pick a widget. Somebody installs a popup, gives it a timer, points it at every page, and the site now has a capture strategy. It converts at something under one per cent, it asks people who already subscribed to subscribe, it fires on the pricing page while a visitor is reading the pricing, and the only number anyone reports is a site-wide percentage that cannot be improved because nothing inside it can be seen.

The claim is that capture is a set, not a widget, and the unit of design is the page type. A visitor on an expired listing has a specific, nameable, unmet want. A visitor halfway through a long guide has a different one. A visitor on the pricing page is already being asked for something larger. One surface shown to all three is wrong twice out of three times by construction, and no amount of copy testing on that surface recovers the two you got wrong.

The second claim is the part people skip. Suppression, frequency and eligibility are properties of the set rather than of any surface in it, so if each widget carries its own cookie and its own rules, the rules will disagree within a month.

## The surfaces, which is the part worth enumerating

Nine types. Everything else is a variant of one of these.

1. **Inline block.** Server-rendered markup in the flow of the page. No interruption, lowest conversion per display, highest reach, and the only surface that still works when a consent-gated tag manager never loads. The backbone of any set.
2. **Sticky bar.** A short persistent strip at the top or bottom. Low interruption, permanently present, and the usual way a focused element gets obscured for a keyboard user.
3. **Slide-in box.** A corner panel on a scroll or time trigger. The compromise between the inline block and the modal.
4. **Timed modal.** A centre-screen overlay on a timer. Highest interruption cost in the set, and the surface most often over-used.
5. **Exit-intent modal.** The same overlay on a pointer-leaving trigger. Desktop only, for reasons given below.
6. **Welcome mat.** A full-viewport interstitial before the content, and the most aggressive surface available. Google's guidance on intrusive interstitials, announced in August 2016 and effective for mobile search results from 10 January 2017, describes this pattern directly, while carving out consent notices, age gates and login walls. Verify the current wording before relying on an exemption.
7. **Contextual notify-me.** A form whose ask matches a specific unmet intent on the page: tell me when this is back, when a role like this appears, when this date opens. Different surface, different copy, different list.
8. **Content upgrade.** A page-specific asset offered in the flow of that page. Technically an inline block, listed separately because the offer rather than the surface does the work.
9. **Post-action capture.** The ask placed straight after the visitor completed something: a calculator returned a result, a download finished, an order was placed. Usually the highest-converting surface in a set and the least used, because it needs product work rather than a script tag.

## The trigger vocabulary

Time on page. Scroll depth. Element in view. Idle, meaning no scroll, pointer or key for an interval. Exit intent. Click on a named element, which turns the ask into a two-step the visitor opens themselves. Session count. Referral source. Page state, meaning expired, out of stock, zero results, waitlisted. Completion of an action.

Every surface gets exactly one primary trigger and a number. A surface in a plan without a number is not finished.

## The order of the build, and why it is this order

1. Inventory the page types by intent.
2. Decide the offer for each page type.
3. Assign one primary surface per page type.
4. Set the trigger and its number.
5. Apply the device rules.
6. Lay out the surface, mobile first.
7. Write suppression and frequency once, for the whole set.
8. Write the consent and accessibility contract.
9. Define the measurement, with denominators.

The offer comes before the surface because the offer decides what the surface can be. A page-specific asset justifies an inline block people seek out. A generic newsletter needs interruption to get noticed, which is another way of saying the offer is too weak to be found, so when you catch yourself reaching for a more aggressive surface, check whether you are compensating for one. Suppression comes last but before launch: retrofitted it becomes one cookie per widget and the rules contradict each other, while written once against the set it is a single eligibility function every surface calls.

## Step 1. Inventory page types by intent, not by template

Group by what the visitor wants, not by which component renders it. A typical content and commerce site has six to nine: home and index, long-form content, product or listing detail, unavailable states, search results including the zero-result case, conversion pages, tools, campaign landing pages, and legal or account pages. Write the share of sessions each one receives next to it, which is the step that stops you designing an elaborate surface for a template with 400 sessions a month.

## Step 2. The offer, per page type

**Offer asymmetry.** The thing you give must be larger than the thing you ask for. An email address is a real cost, so a newsletter about your company is not an offer, it is a request with a friendly name. A checklist, a template, a dataset, a calculator result by email, or a notification about something they cannot otherwise know: those are offers.

**The offer must be usable without the product.** If the asset only makes sense to somebody who has already signed up, you have asked the visitor to commit twice and hidden the second commitment behind the first. A pricing spreadsheet that opens in any spreadsheet application, on Windows or macOS, is an offer. A template that only opens inside your product is a trial in disguise and should be labelled as one.

## Step 3. The mapping

| Page type | Primary surface | Trigger |
|---|---|---|
| Home and index | Timed modal or slide-in | 15 seconds, first session only |
| Long-form content | Inline block plus content upgrade | After the third subheading, or 50 to 60 per cent depth |
| Product or listing detail | None, or a low sticky bar | Page load, suppressed inside a purchase flow |
| Expired, sold out, unavailable | Contextual notify-me | Page state, nothing else fires here |
| Zero-result search | Contextual notify-me, seeded with the query | Page state |
| Pricing, checkout, signup | None | Not applicable |
| Campaign landing page | The page itself is the surface | Not applicable |
| Tool or calculator | Post-action capture | On completion |

Two rows carry the whole argument. Unavailable states and zero-result searches get their own surface, because the visitor has already told you what they want. Conversion pages get nothing, because you already have a larger ask on the page and adding a smaller one moves people down.

## Step 4. Trigger numbers

**Timed modal: 15 seconds**, band 10 to 20, and derive it rather than copying it. Fire at roughly half the template's median engaged time, and if that median is under 20 seconds put no timed surface on the template at all, because the timer will mostly fire at people who have already gone and interrupt the few who have not. Earlier than about 10 seconds interrupts before the page has delivered anything, which is when a dismissal becomes reflexive rather than considered.

**Inline block: anchor to structure, not to a percentage.** After the third subheading beats 55 per cent, because 55 per cent of a 600 word page and 55 per cent of a 4,000 word page are not the same moment.

**Idle: 30 seconds** with no scroll, pointer or key on a page with content still below the fold. That combination means finished or gone, and it reads intent better than a raw timer.

**Session count: second or third visit** for anything aggressive. A returning visitor has demonstrated something a first-time visitor has not, and this is the least-used strong trigger in the vocabulary.

**Referral source** personalises the welcome: name the community somebody arrived from back to them and the ask stops being generic. The caveat is mechanical. Chromium-based browsers moved to a default referrer policy of strict-origin-when-cross-origin in 2020 and other engines followed, so you generally receive the origin and not the path, and nothing at all for many app-to-browser transitions. You can usually tell which site, rarely which page, so design the personalisation to degrade to the generic version.

## Step 5. The two device constraints

**Exit intent runs on desktop only, and here is the reason.** The trigger is a pointer leaving through the top edge of the viewport: a mouseout or mouseleave on the document with a clientY at or below zero. A touch device has no persistent pointer and no viewport edge to cross, so on a phone that code path is never entered. Shipping it there gives you dead code plus the bytes.

What usually happens instead is worse. Somebody substitutes a mobile heuristic: a fast upward scroll, or a pushed history entry so the back gesture is intercepted. Fast upward scrolling is what an engaged reader does when they want to re-read the paragraph above, so the substitute fires on your best readers at the moment they are concentrating. The back-button trap hijacks a system control, fires on people navigating inside your own site, and has been actively discouraged by browser vendors. Neither is exit intent. They are guesses with a confident name.

So on mobile, bind capture to intent that actually exists: end of article, action completed, second session, idle, or a tap on a persistent low bar. Or accept that mobile gets fewer surfaces than desktop and design for that.

Desktop exit intent needs guarding too. Require at least 10 seconds on the page and at least one prior pointer movement, ignore exits through the left and right edges, which are the bookmark bar and the second monitor, and add a 50 millisecond confirmation before showing. Test on Windows and on macOS: browser chrome heights differ, the scrollbar takes layout width on Windows by default and not on macOS, and a modal sized against the viewport can have its close control clipped on one and not the other.

**On mobile, the submit control goes after the email field.** Not beside it, not above it. Three reasons, each sufficient on its own.

Tapping the field raises the software keyboard, which takes roughly 40 to 50 per cent of a portrait viewport. The browser scrolls the focused field into view and the layout around it moves. When the keyboard closes, the thumb is resting in the lower half of the screen, and the control needed next should be directly under it rather than above the field just left.

DOM order is also reading order and tab order. Field then button is the correct sequence for a keyboard and for a screen reader, so the visual rule and the semantic rule agree, which is rare enough to be worth taking.

And a two-element row collapses at narrow widths in source order. A button authored before the input ends up on top of it at exactly the breakpoint where it matters most. That is how the defect reaches production without anyone deciding it.

Two supporting details. Size a full-screen surface with dynamic viewport units rather than 100vh, because on mobile browsers with collapsing toolbars 100vh is taller than the visible area and the button lands under the fold. And when you need the real available height with the keyboard open, read it from the visual viewport rather than the window, because the layout viewport does not shrink.

Field-level mechanics beyond this, the autocomplete token, the input mode, the validation timing, belong to a form audit rather than here. This layer decides which surface the field sits in, when it appears, and where the button sits relative to it.

## Step 6. Suppression and frequency, written once

One eligibility function, called by every surface, in this order:

- **Already subscribed: suppress everything.** The rule whose absence is most visible to the people you least want to annoy. Identify them with a first-party flag set server-side at subscribe time, and again when somebody arrives from a link in one of your emails, since that link can carry an identifier you own.
- **Inside a conversion flow: suppress everything.** Cart, checkout, signup, account creation.
- **One ask per session**, across the whole set.
- **After a dismissal: 30 days.** After a submission: 180 days, or permanently for that surface.
- **A consent notice counts as an interruption.** If a regional cookie banner has just been shown, delay to the next page view rather than stacking a capture surface behind it.

The frequency numbers have a storage problem worth knowing. Safari's tracking prevention caps script-writeable storage, including cookies written by JavaScript and local storage, at seven days of no interaction, a change WebKit announced in March 2020. A 30 day dismissal cap set in a client-side cookie therefore becomes a seven day cap for a meaningful share of visitors, who see the popup again in week two. If the cap matters, set it server-side or against an identifier you hold, and verify the current behaviour, because this area moves.

## Step 7. The consent and accessibility contract

**Consent.** Under the European position consent must be a specific, informed and unambiguous affirmative act, and a pre-ticked box is not one, which the Court of Justice of the European Union settled in the Planet49 judgment in October 2019. So the marketing tick box is separate from the asset and starts unticked: asking for an address in order to send a file is not consent to a newsletter unless the visitor was told and agreed to that too. In the United States, CAN-SPAM requires no prior consent but does require a working unsubscribe honoured within ten business days and a postal address in every message. Confirmed opt-in is the practical evidentiary standard in several European markets and the cleanest per-surface quality signal you will get.

**Accessibility, for interruption surfaces.** Use the native dialog element with showModal, or role="dialog" with aria-modal="true". Move focus in on open, keep it there, return it to the trigger on close, and let Escape close. Give the close control an accessible name and at least 24 by 24 CSS pixels, the WCAG 2.2 minimum target size criterion published in October 2023, and 44 pixels in practice on touch. Stop the page behind from scrolling and restore its position afterwards. For sticky bars watch the WCAG 2.2 criterion on focus not being obscured, since a sticky footer covering the focused element as somebody tabs down the page is the usual way to fail it. Reserve space for inline blocks at render, because content injected after paint contributes directly to layout shift, whose good threshold is 0.1.

## Step 8. Measurement, per surface

Record six numbers per surface, weekly. Eligible sessions, meaning sessions where the surface could have fired under the rules. Displays. Interactions, meaning anything other than a dismissal. Submissions. Confirmations. And engaged addresses at 90 days, meaning how many of the addresses that surface produced are still opening or clicking three months later.

Two derived rates, each with its denominator stated. Submission rate is submissions over displays, never over sessions. Coverage is displays over eligible sessions.

A blended number is unimprovable, and the reason is arithmetic rather than taste: a surface that fires rarely and converts well is indistinguishable from one that fires constantly and converts badly once you have added them together. The worst surface hides behind the best one and both hide behind the total.

## Decision rule: which surface does this page type get?

- **If the page has a specific unmet intent you can name**, expired, out of stock, waitlisted, zero results, the surface is a contextual notify-me and nothing else fires there.
- **If the page is long-form content a meaningful share read to the end**, it gets an inline block plus a page-specific asset, and no interruption surface.
- **If the page is a conversion page**, it gets nothing.
- **If the page is a paid landing page**, the page is the surface. An overlay on a page you paid to send somebody to is a tax on a click you already bought.
- **If the traffic is first-touch, undifferentiated and short-session**, a timed modal is defensible, at 15 seconds, on home and index pages only.
- **If you cannot tell which of these the page type is**, do not guess upward. Ship the inline block, which is the only surface whose worst case is being ignored, instrument it for a fortnight, and read the median engaged time and the scroll distribution before reclassifying. A modal you cannot justify costs you every session where you were wrong. An inline block you cannot justify costs nothing.

## Worked example

A niche job board for laboratory technicians. Page types: home, guides, live listings, expired listings, search results, an employer pricing page.

The set as built: inline block plus a salary-benchmark asset on guides, after the third subheading. Timed modal at 15 seconds on home and index pages only, first session. Contextual notify-me on expired listings, offering an alert when a similar role appears, seeded with the role title and region from the page. The same notify-me on zero-result searches, seeded with the query. Nothing on live listings, nothing on pricing. No exit-intent surface at any mobile breakpoint, and on desktop only on guides. Suppression: one ask per session, subscribers excluded by a server-set flag.

One month:

| Surface | Displays | Submissions | Rate | Confirmed |
|---|---|---|---|---|
| Timed modal | 62,000 | 380 | 0.61% | 205 |
| Inline block on guides | 21,000 | 420 | 2.00% | 340 |
| Notify-me on expired listings | 3,900 | 445 | 11.41% | 392 |

Blended: 1,245 submissions on 86,900 displays, which is 1.43 per cent, and 937 confirmed.

Read it properly. The timed modal takes 71 per cent of displays, produces 31 per cent of raw submissions and 22 per cent of confirmed ones, at the highest interruption cost in the set. The notify-me surface takes 4.5 per cent of displays and produces 42 per cent of the confirmed addresses.

**Verdict: the set is inverted.** The surface shown least is the one working, and the surface shown most buys its volume with interruption and returns the worst confirmation rate of the three. The actions follow directly: extend the notify-me pattern to expiring-soon listings and saved-search pages, restrict the timed modal to the home page and re-measure, and stop reporting the blended 1.4 per cent, which described none of this.

## Failure modes

**One surface, every page.** The site has a capture strategy and it is a single timer. Intent is ignored, so the ask is slightly wrong nearly everywhere, and most wrong on the pages where the visitor came closest to telling you what they wanted.

**Exit intent on a phone.** Either it never runs, and the team believes a surface is live that has never been seen, or it has been replaced by a scroll-velocity heuristic that fires on somebody scrolling back to re-read a paragraph. Both look identical in a display count, and only the second shows up in the dismissal rate.

**The button above or beside the field on mobile.** Authored as a neat row on a desktop mock, collapsed in source order at the narrow breakpoint, and the tap target now sits above the field the keyboard has just pushed around. Nobody notices, because nobody tests the surface with the keyboard open.

**No suppression, so subscribers are asked to subscribe.** The most visible defect in the set and invisible in the metrics, because the people it insults are already on the list and never appear in a conversion denominator.

**Blended reporting.** One percentage for the whole site, often because displays were never counted at all and submissions were. It cannot go up, since nothing inside it can be seen, and every change made against it is a guess evaluated against noise.

**The offer that needs the product.** The asset only makes sense to a customer, so the visitor commits twice and hears about the second commitment after the first. Submissions look normal while confirmation and 90-day engagement are both poor.

**The stacked ask.** A consent banner, a modal and a sticky bar on one page view, each added by somebody who checked only their own surface.

**The undismissable modal.** No visible close control, or one at 16 pixels in pale grey, or Escape does nothing while focus sits on the page behind. A keyboard user is stuck, a screen reader user does not know a dialog opened, and everybody else uses the back button, which leaves your site.

## What this skill does not do

- It does not choose or write your offer. It will reject an asset that only works for existing customers, and it cannot judge whether the one you picked is worth an email address to your reader.
- It does not build anything, and a bought rules engine will do targeting, capping and split-testing better than hand-written code.
- It has no view on what happens after the address arrives. Welcome sequences, list hygiene and sending reputation are elsewhere, and a capture set feeding a broken sending domain achieves nothing.
- It cannot see your analytics. Every threshold here is a starting position, to be replaced by your own median engaged time, scroll distribution and confirmation rates.
- The consent material is orientation rather than legal advice, and a snapshot of the European and United States positions as of August 2026.
- The platform behaviour underneath it moves. Referrer policies, storage caps and search guidance on interstitials have all changed within the last decade, so every such claim here is dated and should be checked.
