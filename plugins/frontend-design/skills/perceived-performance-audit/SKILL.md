---
name: perceived-performance-audit
description: Decides what a wait should look like based on which human timing band it falls in, covering the roughly 100ms, 1 second and 10 second limits, the delay before a spinner should appear, when a skeleton helps and when it causes a second perceived load, which actions are safe to apply optimistically and how a rollback must be communicated, the current Core Web Vitals thresholds for LCP, CLS and INP with the specific front-end causes of each, and font loading strategy. This skill should be used when adding a loading state, an optimistic update, a hero image or a web font, or when a page measures well in a lab tool and still feels slow.
---

# Perceived performance audit

## The claim this skill is built on

Perceived performance is treated as a synonym for actual performance, and it is not. It is a
separate discipline with its own rules, and the rules are mostly about which of several correct
treatments applies to a wait of a given length.

The obvious approach fails in a specific way. Someone notices a wait, adds a spinner, and moves on.
That is right for one band of durations and wrong for the two either side of it. Below about a third
of a second, the spinner appears and disappears fast enough to register as a flicker, and users
report a page with a flashing spinner as buggier than the same page with no indicator at all. Above
a few seconds, an indeterminate spinning animation stops carrying information: it looked identical
one second in and it will look identical thirty seconds in, so it cannot distinguish "working" from
"hung", and the user's only available inference is that something has broken.

So the first question is never "what indicator should this have". It is "how long is this wait",
and the answer selects the treatment.

## The three limits

The response-time limits have been stable since a 1968 paper on response time in man-computer
conversational transactions, restated for interface work in 1993 and unchallenged since:

- **About 0.1 seconds.** The limit for feeling that the system reacted instantly. Below this,
  cause and effect are fused, and the interface feels like direct manipulation rather than like a
  request. Anything triggered by a hover, a keystroke, a drag or a toggle belongs here.
- **About 1 second.** The limit for uninterrupted flow of thought. The user notices the delay but
  does not lose their place, and does not need reassurance that anything is happening. Navigation
  and most fetches should land here.
- **About 10 seconds.** The limit for keeping attention on the task. Past this, people switch to
  something else, and the interface has to be designed for a person coming back rather than for a
  person waiting.

Those three numbers are the whole decision input. Everything below follows from them.

## The spinner rules

**Under about 300 milliseconds, show nothing.** No spinner, no skeleton, no dimming. The response
arrives before anyone forms the expectation of a wait, and an indicator that renders and unrenders
inside that window is perceived as a defect. This is the rule people find counter-intuitive and it
is the one with the most immediate effect.

**Implement it as a delay, not as a guess.** Do not try to predict which requests will be fast.
Start the request, start a timer, and render the indicator only if the timer fires first. The
indicator becomes self-correcting: fast responses never show one, slow ones always do, and no
prediction is required.

**Once shown, keep it for a minimum.** A spinner that appears at 300 milliseconds and vanishes at
340 is the same flicker in a different place. A floor of around half a second after it appears
removes that.

**Between about 1 and 10 seconds, show something with content in it.** A skeleton where the layout
is known, a spinner with a label where it is not. The label is doing real work: "Loading invoices"
tells a user what is happening, and a bare rotating shape does not.

**Past about 10 seconds, show progress rather than an indeterminate animation.** Determinate progress
where a real fraction is available, and a step count where it is not: "Processing 3 of 12 files" is
determinate, honest, and possible without any percentage at all. Add a cancel where cancelling is
safe. An indeterminate animation past this point is the single strongest signal an interface can send
that it has hung.

## Skeletons versus spinners

A skeleton genuinely helps when three conditions hold together: the layout is known before the data
arrives, the layout is stable so the skeleton's shape matches what replaces it, and the wait is long
enough to see but not long enough to become a wall, which in practice is roughly half a second to a
few seconds.

A skeleton is actively worse than a spinner in three cases:

- **When the shape changes on arrival.** Three skeleton rows replaced by eleven real rows, or a
  fixed-height card replaced by one with a paragraph of text, produce a visible jump. The user has
  now perceived two loads: the skeleton settling, then the content shifting. That is worse than one
  honest wait, and it also shows up as measurable layout shift.
- **When the wait is very short.** The skeleton flashes, same as a spinner.
- **When the wait is very long.** A shimmering placeholder animating for fifteen seconds reads as
  broken more strongly than a spinner does, because the shimmer implies imminence.

Two implementation notes. The shimmer animation must respect `prefers-reduced-motion`. And when
content is being replaced rather than loaded for the first time, do not swap it for a skeleton at
all: keep the stale content visible and mark it as refreshing. Replacing loaded content with a
skeleton on every refetch is a common and self-inflicted regression, and modern rendering
approaches, including transitions in React 18 and later, exist specifically to let you keep the old
view on screen while the new one prepares.

## Optimistic updates

An action is safe to apply optimistically when all four hold:

1. **Success is overwhelmingly likely**, and failure is a network problem rather than a policy
   decision.
2. **The result is fully predictable from client state.** If the server assigns an id, a rank, a
   timestamp or a computed total that other parts of the interface display, the client cannot
   render the true post-state and is guessing.
3. **The action is reversible or cheap to reconcile.** A toggle is. A destructive operation that
   another user can see is not.
4. **Failure is visible and recoverable** in the place the user is looking.

The rollback is the part that gets skipped, and a rollback with no communication is worse than no
optimism at all, because the interface silently disagrees with what the user remembers doing.
Rollback correctly means three things: restore the previous state, tell the user in a message that
persists rather than in a toast that has already faded, and preserve whatever they typed so the retry
is one click rather than a redo.

**Never optimistic:** payments and anything else that moves money; irreversible deletion with no undo
window; anything gated by a server-side check the client cannot replicate, such as a uniqueness
constraint or a permission; anything where the user immediately navigates away, because the failure
then has nowhere to appear; and anything whose optimistic version would be visible to other people
before it is real.

## Core Web Vitals, and what causes each

Thresholds as of August 2026, assessed at the 75th percentile of page views, segmented by mobile and
desktop:

| Metric | Good | Poor above |
|---|---|---|
| Largest Contentful Paint | 2.5 seconds or under | 4.0 seconds |
| Cumulative Layout Shift | 0.1 or under | 0.25 |
| Interaction to Next Paint | 200 milliseconds or under | 500 milliseconds |

**Interaction to Next Paint replaced First Input Delay as a Core Web Vital in March 2024**, after
being announced as a successor the previous year, and FID was subsequently retired from the reporting
tools. The difference is not cosmetic. FID measured only the delay before the first interaction began
processing, which meant a page could score perfectly while every interaction produced a frozen second
of nothing, because the freeze was in the processing and rendering rather than in the delay. INP
measures the full interaction, from input to the next frame painted, across the page's whole
lifetime, and reports close to the worst one. Any advice that still names FID predates this and
should be treated as stale on everything nearby, too.

**LCP is hurt by**, in rough order of how often it is the cause: a hero image marked
`loading="lazy"`, which defers the exact element the metric is measuring; a render-blocking
stylesheet or a synchronous script in the head; a web font that blocks text rendering while it
downloads; and fetching the above-the-fold content on the client after hydration, which serialises
document, script, request and render into four sequential round trips. The useful diagnostic is to
break the LCP time into its parts: time to first byte, the delay before the resource starts loading,
the resource's own load time, and the delay between load and render. Each part has a different fix,
and treating LCP as one number sends people to optimise the wrong one.

**CLS is caused by**: images and video without `width` and `height` attributes or a CSS
`aspect-ratio`, so the space is not reserved; ads, embeds and iframes of unknown size; web fonts
swapping from a fallback with different metrics; and content injected above existing content, which
is why a cookie banner, a promotional bar or a late-arriving error message at the top of a page are
such reliable offenders. Animating `top` or `height` counts as layout shift, animating `transform`
does not. Note that CLS is scored over the page's lifetime using a session window, so a shift caused
by a late-loading element several seconds in still counts.

**INP is caused by** long tasks blocking the main thread, where a long task is anything over 50
milliseconds, and by event handlers that do too much synchronously. The three parts are input delay,
processing time and presentation delay, and the fixes differ: break long work into chunks that yield
to the main thread, move the expensive part off the critical path so the visual response paints
first, and avoid re-rendering an entire list in response to a single keystroke.

## Font loading

`font-display` has five values and each trades a different thing:

- `auto` leaves it to the browser, which usually behaves like `block`.
- `block` gives a block period of around three seconds where text is invisible, then swaps whenever
  the font arrives. Worst for perceived speed, because the user sees nothing.
- `swap` uses a very short block period then shows the fallback and swaps whenever the font arrives.
  Best for getting text on screen, worst for layout shift, because the swap can happen late.
- `fallback` blocks briefly, allows a short swap window of a few seconds, and then keeps the fallback
  permanently. A reasonable compromise.
- `optional` blocks briefly, has no swap period, and lets the browser decline to use the font at all
  on this page load. It is the only value that guarantees no font-driven layout shift after first
  paint.

Two things make `swap` safe. Preload the font with `<link rel="preload" as="font" type="font/woff2"
crossorigin>`, remembering that the `crossorigin` attribute is required even for a same-origin font
or the preload is discarded and fetched twice. And metric-match the fallback to the web font, which
is what removes the reflow at the swap rather than merely hiding it, and can reduce the visible
shift to nothing. The `@font-face` descriptors that do it, and how to tune them, are the Design
token and theming audit skill's.

## Progress that lies

Fake progress is not automatically dishonest and the distinction is worth stating precisely.

A bar that animates smoothly toward an asymptote while a genuinely indeterminate operation runs is
communicating something true, which is that the system is still working. Research going back to a
2007 paper on progress bar design found that pacing changes perceived duration: bars that accelerate
toward the end are remembered as faster than linear ones of identical real duration. Using that is
a legitimate design choice.

What is not legitimate: a bar that reaches 100 percent before the work is done, a bar that goes
backwards, a bar that stalls at 90 percent for the majority of the wait, and a percentage presented
as precise when it is invented. All four destroy trust in every future indicator you show.

The best answer usually avoids the question. A step list is honest and also feels faster, because it
gives the user something to read and a sense of structure: "Uploading, Converting, Generating
preview" with a tick against each is fully truthful, needs no fraction, and is more informative than
any percentage.

## The ordering rule: field before lab

Optimise against field data, not lab data, and when they disagree believe the field.

A lab tool runs on one machine, on a simulated network, with a cold cache, at one viewport, with no
user. It cannot observe interaction latency the way a real session does, so it reports a proxy for
it. Field data is a distribution of real devices, real networks and real behaviour, and it is the
version that is used for assessment.

They come apart in predictable ways. Deferring scripts improves a lab blocking-time score and can
make a real user's first click slower, because the real user clicks earlier than the synthetic run
ever does. Lazy-loading everything improves a synthetic largest-paint element and delays the actual
largest element on a real viewport, which may not be the one the lab picked. A cached second visit
can be fast in the field while the lab, always cold, says otherwise. And a single lab run has a
variance of its own that is easy to mistake for an improvement.

## Decision procedure

Start with the duration. If you do not have one, that is the first branch.

1. **Can you state the duration at the 75th percentile?**
   - **Yes, and it is under about 300 milliseconds.** Show no indicator. Ensure the control gives
     immediate feedback of its own: a pressed state, a focus change, a disabled toggle.
   - **Yes, roughly 300 milliseconds to 1 second.** Delayed indicator, appearing at around 300
     milliseconds, with a minimum visible duration.
   - **Yes, roughly 1 to 10 seconds.** Skeleton if the layout is known and stable, otherwise a
     labelled spinner. Keep existing content on screen if this is a refresh rather than a first load.
   - **Yes, over about 10 seconds.** Determinate progress or a step count, a cancel if cancelling is
     safe, and a design for the user who leaves and comes back. Consider making it a background job
     with a notification rather than a wait at all.
   - **No, you cannot tell.** Do not guess and do not choose the pattern that would be right if it
     were fast. Instrument it, ship the delayed indicator in the meantime, because the delayed
     indicator is the only pattern that is correct in both the fast and the slow case, and revisit
     with real numbers. Write the assumption down where the next person will see it.
2. **Is this action a candidate for optimism?** Apply the four conditions. If any fails, it is not,
   and the answer is the loading treatment from step 1.
3. **Does this change add an image, an embed, a font or injected content above the fold?** If so,
   reserve its space explicitly before shipping.
4. **Does this change add a handler that runs on every keystroke, scroll or pointer move?** If so,
   check that its synchronous work is bounded.

## Worked example

A filterable list of records in a general analytics tool. Applying a filter takes about 700
milliseconds at the 75th percentile. Exporting takes about 4 seconds for a typical selection and up
to a minute for a large one. There is a chart above the list and the interface uses one web font.

**Filter, at 700 milliseconds.** Band two. Not fast enough to show nothing, not slow enough to need
progress. The current implementation clears the list and renders a spinner, which is wrong twice: it
destroys the content the user was reading, and the destruction itself is the perceived cost. The fix
is to keep the existing rows on screen at reduced opacity, mark the region busy, and show a small
inline indicator that appears only after 300 milliseconds, so the responses that come back in 200
never flash anything. The filter control gets its own immediate visual response so the sub-100
millisecond feedback requirement is met by the control rather than by the data.

**Export, at 4 seconds to a minute.** Straddles the 10 second limit, so it is designed for the slow
case. It becomes a background job with a step list, a cancel, and a notification when it completes,
so the user is not held on the screen. No indeterminate spinner appears anywhere in it.

**Chart and font.** The chart container gets an explicit `aspect-ratio` so its arrival does not push
the list down. The font moves to `font-display: swap`, is preloaded with `crossorigin`, and gets a
`size-adjust` value tuned to the fallback so the swap does not reflow the headings.

**Optimism.** The "star this record" toggle qualifies on all four conditions and becomes optimistic,
with an explicit rollback that restores the previous state and shows a persistent inline message.
"Delete record" does not qualify and stays pessimistic with an undo window after the fact.

**Verdict: the page's lab score was already good and its felt speed was poor, and the two facts are
not in tension.** The lab score never sees a filter interaction. Every real complaint traced to the
same defect, which is that a 700 millisecond wait was being presented in the vocabulary of a 10
second one.

## Failure modes

**The universal spinner.** One loading component, one duration assumption, applied to a 150
millisecond toggle and a 30 second export alike. Wrong at both ends and right only in the middle.

**Content replaced by a skeleton on every refetch.** The user reads a row, changes a filter, and the
row they were reading is destroyed and rebuilt. The data was already on screen and the interface
threw it away.

**The skeleton that does not match.** Placeholder shapes settle, then real content arrives at a
different height and everything moves. Two perceived loads instead of one, plus a layout shift score.

**Optimistic update with silent rollback.** The item appears, the request fails, the item quietly
vanishes, and the user believes they imagined it or that the product loses data.

**A hero image marked lazy.** Someone applied `loading="lazy"` across every image as a blanket
optimisation and deferred the one element the largest-paint metric is timing.

**Guidance written against a retired metric.** Advice naming First Input Delay is describing
something that stopped being a Core Web Vital in March 2024, and usually carries an implementation
approach that was tuned for it.

**Progress that stalls at 90 percent.** The fraction was invented, the last step is the long one, and
every subsequent progress bar in the product is now disbelieved.

**Optimising the lab score.** A run improves synthetically, ships, and the field distribution does
not move or gets worse, because the change traded real interaction latency for a synthetic blocking
number.

## What this skill does not do

- It measures nothing. It reasons about latencies you supply, and if those are guesses, so is
  everything it concludes.
- It has no view on back-end performance. It decides what an interface does with a wait, not how to
  make the wait shorter, and a slow query stays slow.
- It cannot find your long task. A browser profiler does that in minutes and nothing here replaces
  it.
- It does not cover mobile application performance, native rendering, or anything outside a browser.
  The metrics named here are web metrics.
- Metric definitions and thresholds change. The numbers here carry a verification date and must be
  rechecked against current documentation before anyone is held to them.
- It does not weigh perceived performance against other goals. Sometimes the honest answer to a slow
  operation is to remove the feature, and that decision is not one this makes.
