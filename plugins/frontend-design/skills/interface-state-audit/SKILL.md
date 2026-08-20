---
name: interface-state-audit
description: Enumerates every state a screen can actually be in, decides which ones are reachable, and specifies what each must show. Covers the three different empty states, loading and slow and stale, partial and permanent failure, permission and quota limits, hostile content shapes, optimistic and conflicting mutations, and identity edge cases like the last admin. This skill should be used when building or reviewing any screen that loads data, submits data, or renders a list.
---

# Interface state audit

## The claim this skill is built on

Almost every interface is built for one state: a moderate amount of well-formed data, loaded
successfully, by a user with full permissions, on a wide screen.

Users spend a startling proportion of their time in the other states. The first five minutes of every
account are the first-run empty state. Every slow network is the loading state. Every typo in a search
box is the no-results state. Every expired session is the permission state. The states that feel like
edge cases are, in aggregate, most of the experience, and they are disproportionately concentrated in
the moments that decide whether someone keeps using the product.

They are also where implementations quietly break rather than loudly fail. A list that renders nothing
when the array is empty does not throw. A name that overflows its container does not error. A second
click that fires a duplicate request returns 200. None of this shows up in a demo, because a demo has
three well-named rows loaded instantly by an admin.

This skill is a systematic enumeration so that the states are chosen rather than discovered.

## How to use it

There are three moves, in order:

1. **Enumerate** every state on the eight axes below.
2. **Decide** which are reachable for this screen. Most are not, and pretending otherwise produces
   bloated components nobody maintains.
3. **Specify** what each reachable state shows, and how the user gets out of it.

The third move is the one people skip. A state that has been identified but not designed becomes a
spinner or a blank area, which is the same as not handling it.

## The eight axes

### Axis 1. Data cardinality

The most common source of missed states, and the one where the distinctions matter most.

| State | Why it is different |
|---|---|
| **Empty, never had data** | First run. This is onboarding, not an error. It should teach and offer the primary action. |
| **Empty, had data and now does not** | The user deleted everything, or completed everything. Often a success, sometimes a mistake. Offer undo if a deletion caused it. |
| **Empty, because of a filter or search** | Nothing is wrong with the account. The user needs the query echoed back and a way to clear it. |
| **Exactly one item** | Breaks layouts that assume a grid. Also breaks copy that says "items". |
| **A handful** | The state everyone builds. |
| **Many** | Needs pagination, virtualisation, or a cap, and a decision about which. |
| **Far too many** | Ten thousand rows. Does the page still render? Does the filter still respond? |
| **One item that is enormous** | A single record with a 40,000 character field. Different failure than many small ones. |

**These three empty states must have different copy, and this is the most valuable single rule in this
skill.** Showing "No results found" to a brand new user who has never added anything is a small
disaster: it reads as a failure, gives no next action, and makes the product look broken at the exact
moment the user is deciding whether it works. Showing "Invite your first teammate" to a user who just
searched for a name that does not exist is equally wrong in the other direction. If a codebase has one
empty state component with one string, that is a finding on its own.

### Axis 2. Time

| State | What it needs |
|---|---|
| **Not started** | Deliberate: is this lazy, or does it fetch on mount? |
| **Loading, first time** | Skeleton matching the real layout, or a spinner. Skeletons only if they match; a skeleton whose shape differs from the loaded content causes a visible jump. |
| **Loading more** | The existing content must stay visible and stable. Never replace a loaded list with a spinner. |
| **Refreshing already-visible data** | Subtle indicator, no layout shift, no scroll jump. |
| **Slow** | Past roughly ten seconds, a spinner stops reassuring and starts looking broken. Say what is happening, or offer a cancel. |
| **Timed out** | Distinct from failure. Retry is usually the right primary action. |

### Axis 3. Outcome

- **Success.**
- **Partial success.** Three of five items saved. The single most under-built state in this axis, and
  the most damaging, because reporting it as a flat failure makes the user redo work that succeeded.
  It needs per-item status, not a global banner.
- **Retryable failure.** Network, timeout, 502, rate limit. Show a retry. If it is a rate limit, say
  when to try again.
- **Permanent failure.** Validation, 404, 403, a malformed record. Retry is not the answer and
  offering it is cruel. Say what is wrong and what would fix it.
- **Offline.** Different from failed: it will resolve on its own. Say so, and say what happens to
  anything unsaved.
- **Stale, showing cached data.** The screen is not empty and is not current. Label it and say how old.

The failure copy rule: name the object, the action, and the next step. "Could not load members. Check
your connection and retry." Not "Something went wrong." A user who cannot tell whether the failure is
theirs or yours will assume it is yours and leave.

### Axis 4. Permission and quota

- Can view and act. The state you built.
- **Can view but not act.** The controls should be visibly disabled with a reason, not hidden. Hiding
  them makes the interface look different for different users and generates support tickets that are
  impossible to reproduce.
- **Cannot view at all.** Distinguish "you are not allowed" from "it does not exist", and make that
  distinction deliberately, because for some resources leaking existence is itself a disclosure.
- **Session expired mid-session.** The user was allowed a second ago. Preserve their unsaved input
  across the re-authentication, or you have just deleted their work.
- **At a limit.** Seats used, storage full, plan cap reached. Show it before the action fails, not
  after. A disabled button with "9 of 10 seats used" prevents the failure entirely.
- **Would exceed a limit.** Inviting five people with three seats left. Say so at input time, not on
  submit.

### Axis 5. Content shape

The states that hostile or merely realistic data produces.

- Optional fields absent. Missing avatar, no display name, null timestamp. What renders?
- Very long unbroken strings. A 200 character name with no spaces, an email at the length limit.
  Truncate with the full value available on hover or focus, and never let it break the layout.
- The opposite: a one character name.
- Non-Latin scripts, right-to-left text, combining characters, emoji in names. If the product is
  available in those locales, they are not edge cases.
- Content that looks like markup or a formula. Confirm it is escaped.
- Broken image URLs. Every avatar list needs a fallback, because avatars are user-supplied and will
  404 eventually.
- Extreme numbers. Zero, negative, very large, and whatever the currency or unit formatting does with
  each.
- Dates far in the past or future, and whatever the relative formatter says about them.

### Axis 6. Mutation lifecycle

- Idle.
- **In flight.** The control must be disabled or the action must be idempotent. Double submission is
  the most common bug in this entire axis, and it is invisible in testing because testers click once.
- **Optimistically applied but unconfirmed.** If the change is shown before the server agrees, there
  must be a rollback path and the user must be told when it rolls back.
- **Rolled back.** What the user sees when the optimistic update fails. Silent reversion is
  disorienting and is worse than never having applied it.
- **Conflicting concurrent edit.** Somebody else changed the record. Last-write-wins is a decision, and
  it should be a stated one rather than an accident.
- **Succeeded but the list is now stale.** Refetch, patch locally, or accept staleness. Pick.

### Axis 7. Identity and singularity

The states that come from *who* is looking and from *last-of-a-kind* records. Usually forgotten
entirely, and usually the source of the worst bugs.

- **The row that is you.** Can you remove yourself? Should the button say "Leave" instead of "Remove"?
- **The last administrator.** Removing them, or demoting them, orphans the resource. Block it at the
  interface, with an explanation, and do not rely on the server to be the only guard.
- **The owner.** Usually not removable by an admin. Does the interface reflect that or does it offer
  an action that will fail?
- **A pending or invited member.** Different affordances: resend, revoke, not remove.
- **A suspended or deactivated record.** Visually distinct, and most actions disabled.
- **The item just created.** Highlighted, scrolled into view, or lost at the bottom of an unsorted list.

### Axis 8. Environment

- Narrow viewport. A table is the usual casualty; decide between horizontal scroll, column priority,
  and a card layout, and make it a decision rather than an overflow.
- Browser zoom at 200 percent, which is a legal accessibility requirement in many contexts and is not
  the same as a narrow viewport.
- Keyboard only. Every action reachable, focus visible, and focus placed sensibly after a row is
  removed. Focus that falls to the document body after a deletion strands keyboard users completely.
- Screen reader. Loading, success, and error must be announced, which means a live region, not just a
  visual change.
- Reduced motion.
- Dark mode, if the product has one.

## Deciding which states are reachable

Do not build all of them. Go axis by axis and mark each state **REACHABLE**, **NOT REACHABLE**, or
**OUT OF SCOPE**, with a one-line reason. The reasons are the deliverable, because they are what a
reviewer checks.

- **NOT REACHABLE** requires a mechanism, not a hope. "Cannot be empty because the workspace creator is
  always a member" is a mechanism. "Users will not do that" is not.
- **OUT OF SCOPE** is a legitimate decision when the state is real but deliberately unhandled for now.
  Write it down so it is a known gap rather than an oversight.

A good audit of a moderately complex screen typically marks fifteen to twenty-five states reachable
out of roughly fifty enumerated. If everything is reachable you have not thought. If three are, you
have not looked.

## Build order

When time is limited, this is the priority, ordered by how many users hit the state times how bad it
is when unhandled:

1. **First-run empty**, because every single user passes through it and it is the moment they judge
   the product.
2. **Loading**, because everyone sees it and an unstyled flash of empty content reads as a bug.
3. **Retryable failure**, because it is common and the recovery is cheap to build.
4. **In-flight double submission**, because it silently corrupts data.
5. **Permission and quota**, because it produces support load out of proportion to its frequency.
6. **No-results-from-filter**, because it is constantly confused with first-run empty.
7. Everything else.

## Specifying a state properly

For each reachable state, four things. A state with fewer than four is not specified.

1. **What is on screen**, structurally.
2. **The exact copy.** Not a placeholder. Copy is most of the value of an empty or error state, and
   "TODO: error message" always ships.
3. **The way out.** Every non-terminal state needs an action. An error with no retry and no
   explanation is a dead end.
4. **What is announced**, for assistive technology, and where focus goes.

## Failure modes

**One empty state for all three cases.** The single most common finding, and the most damaging to
first impressions.

**Spinner as the universal answer.** A spinner is correct for exactly one state and is used for six.

**States that exist in code but were never seen.** If nobody has rendered the error state, it is
broken. Make each state reachable in development, with a query parameter, a story, or a mock toggle.

**Layout shift between states.** The empty, loading, and loaded states should occupy comparable space,
or the page jumps and the user loses their place.

**Error copy that blames the user for a server problem, or blames the server for a validation
problem.** Both destroy trust in the message.

**Optimistic updates with no rollback design.** Fast until it is wrong, and then silently wrong.

**Treating identity states as server concerns.** The last-admin guard belongs in both places. An
interface that offers an action the server will refuse has already failed.

## Worked example, compressed

A team members screen with a list, invite, role change, and remove.

Reachable and needing specification: first-run empty (a one person workspace: the owner is alone, so
"empty" means "just you", and the copy is an invite prompt, not a null state); no-results-from-search;
loading skeleton; loading more if paginated; retryable load failure; per-invite partial failure when
inviting several addresses at once; viewer cannot manage, so role and remove controls are disabled with
a reason rather than hidden; seats exhausted, which disables invite before it fails; the row that is
you, where "Remove" becomes "Leave workspace"; the last admin, where demote and remove are blocked with
an explanation; invited members, which get resend and revoke rather than remove; suspended members;
long display names and missing avatars; role change in flight, which disables that row's control only,
not the whole table; role change rolled back; keyboard focus after removing a row, which must move to
the next row rather than to the body; and the narrow viewport, where the table becomes stacked cards.

Not reachable, with mechanisms: the list is never truly empty, because the owner is always a member;
there is no offline mode, because the app requires a connection to render at all.

Out of scope, stated: concurrent edit conflicts, since last-write-wins is accepted for role changes,
and the window is small.

## What this skill does not do

- It does not implement anything. It produces the enumeration and the specification, and the code is a
  separate step.
- It does not tell you which states matter most for your specific product, only a general priority
  order. A read-heavy dashboard and a high-volume data entry tool have different answers.
- It is not an accessibility audit. It covers announcement and focus for the states it enumerates and
  nothing beyond that.
- It does not verify that implemented states are correct. It says what should exist; checking what does
  exist is a separate review.
- Applied to a trivial screen it will over-generate. Use the reachability pass honestly, and be willing
  to mark most of the taxonomy not applicable.
