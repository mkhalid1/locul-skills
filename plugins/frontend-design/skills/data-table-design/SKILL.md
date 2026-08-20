---
name: data-table-design
description: Reviews a data table for column priority at narrow widths and the three patterns available there, sorting that is stable and deterministic including null handling and numbers held as strings, the choice between pagination, infinite scroll and virtualisation with keyset versus offset as a separate decision, the select-all-across-pages trap and bulk action confirmation, numeric alignment with tabular figures, the four distinct empty and loading states, table accessibility semantics, and density, sticky headers and scroll affordances. This skill should be used when building or reviewing any table of records that can be sorted, filtered, paged, or acted on in bulk.
---

# Data table design review

## The claim this skill is built on

A table exists so that a value can be compared with the value above it and the value beside it.
Everything a table does well follows from vertical alignment down a column, and almost every defect
in a real table is something that broke alignment, broke the ordering that makes alignment
meaningful, or broke the user's belief that the rows they are looking at are all the rows there are.

The obvious approach fails at both ends of the size range. At the small end, a table is
over-engineered: twelve rows do not need virtualisation, keyset pagination or a column priority
scheme. At the large end it is under-engineered in ways that only appear in production, because a
table built and tested against fifty tidy rows meets no nulls, no forty-character strings, no
inserted rows between page loads, and no user who selects everything and presses delete.

So this is a review skill rather than a construction skill. It asks what happens at the sizes and
in the states the development data never contained.

## Pass 1. Column priority

Rank columns before deciding anything about layout, because every responsive pattern is a way of
spending that ranking.

The ranking question is not "which column is most important" in the abstract. It is: **what does the
user do next with this row, and which values does that decision need?** Three tiers fall out:

- **Tier 1, the identity column.** The value the user is scanning for. Always visible, always first,
  and it is the column that gets pinned if anything does.
- **Tier 2, the decision columns.** The one, two or three values the next action depends on. Status
  is usually here. So is whatever the default sort is on, because sorting by an invisible column is
  disorienting.
- **Tier 3, everything else.** Reference data, secondary timestamps, identifiers, anything only
  looked at once the row has already been chosen.

Tier 3 is what disappears first. If tier 2 has five columns in it, the ranking has not been done.

### The three narrow-width patterns

**Horizontal scroll with a pinned first column.** The identity column is fixed with
`position: sticky; left: 0` and the rest scroll under it. Keeps every column, keeps alignment, keeps
comparison. Costs: the existence of the other columns is invisible without an explicit affordance, a
horizontal scroll region is awkward with a keyboard unless the container is focusable, and borders on
sticky cells render unreliably, so use a `box-shadow` for the dividing line instead of `border`.

**Progressive disclosure into a row detail.** Show tiers 1 and 2, and move tier 3 into an expandable
row or a detail view. Keeps the list scannable at any width and reads well on a phone. Costs:
comparison across rows for a hidden value becomes impossible, so if users regularly compare a tier 3
value this pattern is wrong; and every inspection costs an interaction.

**Switch to cards.** Each row becomes a stacked block with labels beside values. Costs: this destroys
column alignment entirely, which means it destroys the comparison the table was for; vertical length
explodes, so twenty rows becomes a very long scroll; and sorting becomes hard to express, because the
sort control no longer lives in a header.

**Cards are the default answer and are usually the wrong one.** They are chosen because they are the
easiest to make look tidy in a narrow viewport, and tidiness is not the requirement. Cards are right
when each row is genuinely consumed on its own, as with a notification list or a feed. They are wrong
whenever the user's job is to find the outlier in a column.

## Pass 2. Sorting

**Stability.** A stable sort preserves the relative order of rows that compare equal. An unstable one
does not, so re-sorting by a column with many ties reshuffles rows that did not change, and the table
appears to be rewriting itself. In JavaScript this is settled: `Array.prototype.sort` has been
required to be stable since ES2019 and every major engine implements it. The problem is almost never
the client sort.

**Determinism, which is the one that causes real damage.** A server sort on a non-unique column has
no defined order among ties, so the database is free to return them differently on each query. Add
a limit and an offset and the consequence is that the same row can appear on page one and page two
while another row appears on neither. Users report this as data loss and it is extremely hard to
reproduce on a small dataset. The fix is one line: always append a unique tiebreaker, normally the
primary key, to every sort, on both the client and the server.

**Nulls.** Decide and document one of two policies: nulls always last regardless of direction, which
most people find least surprising, or nulls following the sort direction. Then hold to it everywhere.
Also keep null, empty string and zero distinct, in the sort and in the rendering. An empty cell that
might mean "none", "not yet known" or "zero" is three different facts wearing one appearance.

**Mixed case.** A naive JavaScript string comparison sorts by code unit, so every capitalised word
sorts before every lowercase one and a name list comes out visibly wrong. Use `Intl.Collator` with a
`sensitivity` setting, which also handles accented characters correctly for the locale.

**Numbers held as strings.** Lexicographic ordering puts "100" before "99" and "v1.10" before "v1.9".
Two fixes: coerce to a number before comparing where the value is genuinely numeric, or set
`numeric: true` on `Intl.Collator` for mixed alphanumeric labels, which sorts "Item 2" before
"Item 10". Currency stored as a formatted string is the common trap, because it sorts by the symbol
first.

**Multi-column sort.** Usually not worth it. The affordance is undiscoverable, since shift-clicking a
header is not something users try; the state is hard to display honestly; and it is hard to encode in
a URL. Add it only when users have asked for a specific repeated comparison. What is always worth it
is the invisible version: the deterministic tiebreaker described above.

**Sort state must be visible and linkable.** The sorted column carries a direction indicator and
`aria-sort`, and the sort belongs in the URL so a sorted view can be shared and restored.

## Pass 3. Pagination, infinite scroll and virtualisation

These are three answers to two different questions and they get conflated. Virtualisation is about
how many DOM nodes exist. Pagination and infinite scroll are about how the user moves through the
set. You can virtualise a paginated table.

**The decision inputs:**

- **Row count.** Tens of rows: none of this. Hundreds: pagination or a plain scroll. Thousands:
  virtualisation becomes worth its cost. Tens of thousands and up: virtualisation plus keyset
  pagination.
- **Does the user need to reach the end?** If yes, infinite scroll is out. Its defining property is
  that the end is unreachable, which also takes the footer with it.
- **Does deep linking matter?** If a user has to send a colleague the exact view they are looking at,
  pagination with the page in the URL wins outright.
- **Accessibility cost.** Pagination is the cheapest: it is a set of links, focus can be managed on
  page change, and the row count is finite and announceable. Infinite scroll is the most expensive:
  it strands keyboard users, hides the footer permanently, and loses position on back navigation.
  Virtualisation is expensive in a different way, described below.

**Offset versus keyset is a separate decision from the interface pattern.** Offset pagination asks
for a limit and an offset. It breaks in two ways. Correctness: if rows are inserted or deleted
between two page requests, the window shifts, so the user sees duplicates or misses rows entirely.
Performance: a large offset forces the database to walk and discard everything before it, so page
five hundred is dramatically slower than page one.

Keyset pagination, also called seek or cursor pagination, asks for rows after a given sort key and
unique id. It stays fast at any depth and never duplicates or skips. Its costs are real: no
jump-to-page-N, no easy total count, and the cursor has to encode every column in the sort. **Past a
few thousand rows, or on any table with frequent inserts, keyset is the correct answer.**

**Virtualisation** renders only the visible window, perhaps thirty rows out of fifty thousand, and
its cost is that everything which assumes the DOM contains the data stops working: in-page find
(Ctrl+F, or Cmd+F) only searches what is rendered, anchor links to a row fail, printing produces one
screen of content, and a screen reader is told the table has thirty rows. Mitigations, in order of
importance: declare the true total with `aria-rowcount` on the grid and the real index of each
rendered row with `aria-rowindex`, so assistive technology reports "row 4,318 of 50,000" correctly;
provide a search that queries the server rather than relying on browser find; and offer an export or
print path that is not virtualised.

## Pass 4. Selection and bulk actions

**The select-all trap.** A checkbox in the header selects the rows on the current page. A user who
has filtered to 4,312 results, seen "20 of 4,312", and ticked that box believes they have selected
4,312 rows. The interface believes they have selected 20. The next action is where that disagreement
becomes expensive.

The correct pattern has three parts. The header checkbox selects the visible page and shows an
indeterminate state when the page is partially selected, which is a DOM property rather than an
attribute and must be set in script. When the page is fully selected, reveal an explicit second
control: "All 20 rows on this page are selected. Select all 4,312 rows matching this filter." And
represent that second selection as a query rather than as a list of ids, because the list will be
stale by the time the action runs and because sending fifty thousand identifiers is its own problem.

**The bulk action confirmation rule:**

- Reversible action, selection is visible on screen: **act immediately and offer undo.** Undo beats a
  confirmation dialogue, because confirmations are dismissed reflexively.
- Reversible action, selection extends beyond what is visible: **state the count in the action itself**
  rather than in a dialogue. A button reading "Archive 4,312 rows" is its own confirmation.
- Irreversible action: **confirm, and put the count and the scope in the confirmation text**, not just
  in the button. Where the count is large or the target is destructive, require a deliberate act such
  as typing the count or the resource name.
- Any bulk action: **report per-row outcomes.** "3,908 archived, 404 skipped because they were already
  archived" is the honest result. A single success toast for a partially failed operation is worse
  than an error.

## Pass 5. Alignment, figures and units

**Right-align numbers.** This is not a style preference. Right alignment puts the units digit of
every value in the same column position, which is what makes magnitude comparable at a glance: 1,000
and 100 differ visibly in width only when their right edges line up. Right-align the column header
too, so it sits over its own data. Left-align text. Do not centre anything in a data table, because
centring destroys both the left edge that text scanning uses and the right edge that number scanning
uses.

**Use tabular figures.** Most interface typefaces default to proportional digits, where a 1 is
narrower than an 8, so a column of numbers wobbles and a live-updating value visibly jitters. Set
`font-variant-numeric: tabular-nums` on numeric cells, which gives every digit the same advance
width. The same applies to timestamps, identifiers and version strings.

**Decimals.** Render a fixed number of decimal places within a column so the decimal points align.
Mixed precision down a column breaks the alignment right alignment was for.

**Units.** Put the unit in the column header rather than repeating it in every cell: a header of
"Amount (USD)" beats a currency symbol on 4,312 rows. The exception is a genuinely mixed column,
where the symbol goes in the cell and the numbers must still align on their right edge. Very large
numbers get thousands separators appropriate to the locale, and abbreviations such as 1.2M only
where precision is genuinely not needed, because an abbreviation cannot be compared precisely with
its neighbour.

## Pass 6. The four states

Empty, loading, error and filtered-to-nothing are four different screens and they are routinely one.

- **Empty, never had rows.** This is onboarding. Explain what will appear here and offer the action
  that creates the first row.
- **Loading.** Skeleton rows that match the real column widths and row height, so nothing moves when
  data arrives. Keep the header rendered, because it is already known.
- **Error.** Keep the header and the filter controls visible so the user can see what they asked for,
  say what failed in terms of the object, and offer a retry.
- **Filtered to nothing.** Nothing is wrong with the account. Echo the active filters back, and offer
  a single control that clears them.

A fifth worth handling: **partial failure**, where the page loaded but one column's data did not. A
per-cell fallback beats failing the whole table.

The common defect is one component rendering "No data" in the middle of an empty table body for all
four cases, which tells the user nothing and offers no way out of any of them.

## Pass 7. Accessibility

**Use a real `<table>`** with `<thead>`, `<tbody>` and `<th>` wherever the content is genuinely
tabular. It gives you the semantics for free and every assistive technology has supported it for
decades.

- **`scope="col"` on column headers and `scope="row"` on row headers.** This is what lets a screen
  reader announce "Status, Overdue" while moving across a row instead of just "Overdue".
- **A `<caption>`** naming the table. Visually hide it if the design does not want it, but do not omit
  it, because it is the table's accessible name.
- **`aria-sort`** on the `<th>` of the currently sorted column, with a value of `ascending`,
  `descending` or `none`, and on exactly one column at a time. The control inside the header should
  be a real `<button>` so it is focusable and announced as pressable.
- **Interactive tables need `role="grid"`, and that role is a promise.** It commits you to the
  composite widget keyboard model, one tab stop with arrow keys inside it, which the Focus and
  keyboard audit skill specifies key by key. Declaring the role without implementing the model is
  worse than leaving it as a plain table, where Tab simply visits the interactive controls in
  order.
- **CSS can destroy table semantics.** Applying `display: flex` or `display: grid` to a `<table>`,
  `<tr>` or `<td>` removes the implicit table roles in several engines, so a CSS grid layout over
  table markup needs the roles restored explicitly. This is a common and silent regression.
- **Virtualisation needs `aria-rowcount` and `aria-rowindex`**, as above, or the table lies about its
  size.

## Pass 8. Density, striping and sticky chrome

**Row height** is a real trade-off between how many rows fit and how easily the eye tracks across
one. A comfortable row is around 44 to 48 pixels, a default around 36 to 40, and a compact around 32.
Offer density as a user setting on any table people spend hours in. Whatever you choose, interactive
controls inside a row must still meet the minimum target size in WCAG 2.2, which is 24 by 24 CSS
pixels for success criterion 2.5.8 at level AA.

**Zebra striping versus a border.** Striping helps when rows are tall or there are many columns, so
the eye has a long distance to track. A horizontal border beats a stripe when rows are short and
dense, when the table has few columns, or when rows already carry their own background colour for
status, because the stripe then fights the semantics. Remember that striping multiplies your
background states: default, striped, hover, selected, and the combinations, and if selected-on-striped
is not distinguishable from selected-on-plain the pattern has cost more than it gave.

**Sticky headers** need three things that are easy to miss: an explicit background colour, or rows
scroll through them; a `z-index`, or later content paints on top; and a `box-shadow` for the bottom
edge, because a `border` on a sticky element does not render reliably.

**The horizontal scroll shadow** is not decoration. When a table scrolls sideways, nothing in the
default rendering tells the user that more columns exist, so they conclude the data is missing. Add a
shadow at the left edge once scrolled and at the right edge whenever more content remains. It can be
done in CSS alone with layered background gradients that use `background-attachment: local` for the
covering layers and `scroll` for the shadows, which needs no scroll listener at all.

## Decision procedure: the narrow-width pattern

1. **Do users compare a value across rows in this table?**
   - **Yes, and the compared value is in tier 1 or 2.** Horizontal scroll with a pinned first column,
     plus a scroll shadow. Keep the compared columns in the first screenful.
   - **Yes, but the compared value is tier 3.** Promote it to tier 2 and then apply the branch above,
     or accept that this table needs a different default column set on small screens.
   - **No, each row is read on its own.** Progressive disclosure, or cards if rows are few and short.
2. **Is the table primarily read on phones?** If so, and only if the answer to 1 was no, cards are
   legitimate. Otherwise treat cards as a last resort.
3. **You cannot tell.** This is common and the honest branch. Do not guess, and do not pick cards
   because they are easiest to build. Ship horizontal scroll with a pinned identity column and a
   scroll shadow, because it is the only one of the three that loses no information and no capability,
   then instrument which columns are actually scrolled to and revisit. Write the assumption down next
   to the component.

## Worked example

An invoices table in a general billing tool. Columns: invoice number, customer, status, issue date,
due date, amount, currency, last reminder sent. About forty thousand rows. Users chase overdue
invoices and reconcile totals.

**Priority.** Tier 1 is invoice number. Tier 2 is status, due date and amount, because chasing is
the task and those three determine it. Tier 3 is issue date, currency, last reminder. Narrow width
gets horizontal scroll with the invoice number pinned, not cards, because the whole job is scanning
the amount column for the large outstanding ones.

**Sorting.** Default sort is due date ascending, which puts the most overdue first, and every sort
appends invoice id as a tiebreaker so paging is deterministic. Nulls in "last reminder sent" are
always last. Amount is stored as a decimal string in the API, so it is parsed before comparison
rather than sorted lexicographically. Customer name is sorted with a collator rather than by code
unit, so lowercase names do not all sink to the bottom.

**Paging.** Forty thousand rows and frequent inserts, so keyset pagination on the compound key of
due date and invoice id. The total count is shown as an approximate figure from a separate cached
query rather than being computed on every page. Rows are virtualised within the page, with
`aria-rowcount` set to the true filtered total.

**Selection.** The header checkbox selects the visible page and shows an indeterminate state when
partially selected. Selecting the full page reveals "Select all 4,312 invoices matching this filter",
which is held as a query. "Send reminder" states the count in the button. "Void" is irreversible, so
it confirms with the count in the sentence and reports per-row outcomes afterwards.

**Alignment.** Amount is right-aligned with `tabular-nums` and two decimal places, its header is
right-aligned too, and the currency lives in the header because this view is filtered to one
currency at a time.

**States.** Four distinct screens, plus a per-cell fallback for the reminder column, which comes from
a separate service that can fail on its own.

**Verdict: the table is well built for its first thousand rows and defective past that.** The sort
has no tiebreaker, so pagination duplicates rows under load, which explains an old unreproducible bug
report. Pagination is offset-based, which is both the cause of that and the reason the last pages
are slow. Select-all silently means the visible page. Amounts are left-aligned with proportional
digits. None of that is visible in a screenshot of the first twenty rows.

## Failure modes

**The non-deterministic sort.** No tiebreaker on a non-unique sort column, so paging duplicates and
drops rows. Reported as data loss, impossible to reproduce on a development dataset, and fixed by one
extra term in the order clause.

**Cards by default on mobile.** Chosen because it looks tidy, it removes the ability to compare
values down a column, and the scroll length quadruples.

**Horizontal scroll with no shadow.** The columns are all there and the user has no way to know it.
They conclude the table is missing fields and file a bug.

**Select-all meaning the visible page.** The user filters, ticks the header box, presses a
destructive action, and either far less happens than expected or, in the worse implementation, far
more.

**Proportional digits in a numeric column.** Amounts wobble, live-updating figures jitter, and
comparing two values requires reading rather than glancing.

**One "No data" message for four different states.** The new user, the failed request, the empty
filter and the loading table all show the same grey sentence, and none of them offers a way forward.

**Virtualisation with no row count declared.** In-page find stops working, the screen reader reports
thirty rows out of fifty thousand, and printing produces one screenful.

**`role="grid"` with no keyboard model.** The role promises arrow-key navigation, the implementation
provides a tab stop per cell, and the table is now less usable than the plain one it replaced.

**Sticky header with a border and no background.** The border does not render, the rows scroll
visibly through the header text, and the fix is a `box-shadow` and an opaque background.

## What this skill does not do

- It does not know your users' tasks, and column priority is entirely a function of the task. It
  gives you the ranking method, not the ranking.
- It cannot test with assistive technology. It specifies the markup and roles that should work, and
  only a real screen reader pass confirms that they do.
- It says nothing about indexes, query plans or database performance beyond the shape of the
  pagination request.
- It does not evaluate whether the data should be a table at all. A dataset better served by a
  summary, a chart, or a search result list will pass every check here.
- It does not cover editable grids, where cell editing, validation, paste from a spreadsheet and
  undo make the interaction model substantially harder than anything described here.
- Library defaults change. If you adopt a table component, verify its sorting, selection scope and
  virtualisation semantics against these checks rather than assuming they match.
