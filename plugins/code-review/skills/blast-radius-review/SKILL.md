---
name: blast-radius-review
description: Reviews a change by first finding every other place in the codebase that has the same defect. Turns each finding into a falsifiable rule, searches the whole repository for violations of that rule, and reports the full population rather than the one instance that happened to be in the diff. This skill should be used when reviewing a pull request, auditing a bug fix, or after fixing any defect that could plausibly exist more than once.
---

# Blast radius review

## The claim this skill is built on

A diff is a sample, not a population.

When a bug reaches a code review, it is almost never the only instance. It got there because
somebody held a wrong belief about how the system works, or copied a pattern from a neighbouring
file, or forgot a step that no type checker enforces. Those causes do not produce one bug. They
produce a family of bugs, scattered across every place the same belief was held or the same
pattern was copied.

A reviewer who reads only the diff finds one member of the family and declares victory. Two weeks
later the same bug is reported against a different endpoint, and everybody is surprised.

This skill exists to stop that. The review is not finished when the diff is understood. It is
finished when, for every defect class the diff touches, the whole population has been enumerated
and each member has been either confirmed, cleared, or explicitly handed to an owner.

## What this skill is for, and what it is not

Use it when:

- reviewing a pull request that fixes a bug,
- auditing a fix somebody else already merged,
- a defect was found in production and you want to know where else it lives,
- a change introduces a new invariant that older code does not yet satisfy.

Do not use it for greenfield code with no siblings, for pure dependency bumps, for formatting-only
changes, or as a general style review. It is a search procedure, and search needs something to
search for.

## The procedure

### Phase 0. Classify the change, in one line, before reading closely

Write down which of these the diff is, because the rest of the procedure branches on it:

| Class | What the diff does | Does blast radius apply? |
|---|---|---|
| **Defect fix** | corrects behaviour that was wrong | Yes. This is the primary case. |
| **Invariant introduction** | adds a rule the codebase did not previously have | Yes. Older code almost certainly violates it. |
| **Contract change** | changes a signature, a field name, a return shape, an error code | Yes, but the search is for callers, not for clones. |
| **Behaviour extension** | adds a genuinely new capability | Usually no. Check only for a duplicated helper. |
| **Refactor** | moves code without changing behaviour | Only if it changes a shared helper. |

If the class is Behaviour extension or Refactor and no shared code moved, say so in one sentence
and do a normal review. Do not manufacture a sweep that has nothing to find.

### Phase 1. Review the diff on its own merits, briefly

Read the change. Confirm it actually fixes what it claims to fix. Note anything wrong with the fix
itself. Keep this short. It is the part every other reviewer already does, and it is not where the
value of this skill is.

One thing to check here that is specific to this skill: **is the fix local or systemic?** A local
fix patches the one call site. A systemic fix makes the bug unrepresentable, by moving the logic
into a helper, a middleware, a type, or a query builder that every call site must pass through. If
a local fix was chosen and a systemic one was available, that is a finding in its own right,
because it guarantees the next instance.

### Phase 2. Derive the invariant

This is the move that makes the rest of the work possible, and it is the step people skip.

For every defect in the diff, write the rule it violated as a **falsifiable predicate over code**.
Not a description of the bug. A rule that any given piece of code either satisfies or does not.

Bad, because you cannot search for it:

> The invoice endpoint had an authorisation bug.

Good, because you can:

> Every query against a user-owned table inside a request handler must constrain on the
> authenticated user's id, or must be preceded in the same handler by an explicit ownership or
> membership check.

Test the predicate before you use it. Take three pieces of code you already know are correct and
confirm the predicate says they are correct. Take the bug and confirm the predicate says it is
wrong. A predicate that flags known-good code will bury you in false positives at scale, and a
predicate that does not flag the original bug is not the predicate you meant to write.

Write down the **exceptions** at the same time, because you will meet them during triage and you
want the rule decided in advance rather than negotiated case by case. For the example above:
admin-scoped handlers, public read endpoints where the resource is deliberately unlisted, and
handlers whose scoping is done by an upstream middleware.

If the diff fixed three unrelated things, you have three predicates. Run the rest of the procedure
once per predicate. Do not merge them.

### Phase 3. Search for the population

You now need every piece of code that the predicate could apply to. There are four search
strategies and they find different things. Use more than one, because each has a characteristic
blind spot.

**3a. Call-site search.** For a contract change, find every caller of the changed symbol. This is
the only one of the four that tooling does well. Use the language server or `grep` for the symbol
name, then widen: also search for the symbol as a string (dynamic dispatch, dependency injection
containers, route tables, config files, feature flags, serialised job names, database-stored
handler names). A rename that passes the compiler and breaks in production almost always broke
through a string.

**3b. Sibling search.** Find the files that live in the same architectural slot as the changed
file. If the bug was in `routes/invoices.js`, the siblings are every other file in `routes/`. This
finds copy-paste families, which are the single most common source of repeat defects. Look at the
directory listing, not just at grep results, because the sibling you need may not contain any of
the tokens you would think to search for.

**3c. Shape search.** Search for the syntactic shape of the bug rather than its identifiers. If the
bug was `where id = $1` with no owner constraint, search for `where id =` across the repository and
read each hit. Shape searches are noisy by design. That is acceptable, because the cost of reading
forty hits is far below the cost of missing one. Write the regex to over-match, then triage.

**3d. Absence search, the hard one.** The other three find code that contains something. The most
dangerous defects are code that is *missing* something, and you cannot grep for absence.

Three techniques that work:

- **Enumerate then subtract.** Build the full list of things the predicate applies to from a source
  that is guaranteed complete: the route table, the schema, the exported members of a module, the
  list of files in a directory. Then check each one. Completeness comes from the enumeration, not
  from the search.
- **Grep the anchor, then check the neighbourhood.** You cannot search for a missing ownership
  check, but you can search for the thing it must accompany. Every handler that reads a user-owned
  table must contain a query against that table. Search for the table name, then inspect each hit
  for the missing check. The anchor is present even when the fix is absent.
- **Lean on the type system or a lint rule.** If the invariant can be encoded, encoding it is
  strictly better than finding today's violations, because it also finds tomorrow's. A branded type
  that only a scoped query builder can produce, or a custom lint rule, converts an ongoing search
  problem into a one-time migration. Propose this whenever the predicate is mechanical.

**Search hygiene that matters at scale.** Search generated code, vendored code, and tests too, but
triage them separately. A violation in generated code is a violation in the generator. A violation
in a test is often the test asserting the buggy behaviour, which means fixing the bug will break the
test, which is information the author needs before they merge.

Note explicitly which parts of the system your search could not reach: other repositories, database
functions and triggers, infrastructure as code, scheduled jobs defined outside the codebase, client
applications you do not have. Unreachable is a finding. Silence about it is not.

### Phase 4. Triage every hit into exactly one of three buckets

Do not report raw search results. Raw results are noise and they destroy trust in the review. Every
hit gets read and assigned.

- **CONFIRMED.** The predicate is violated and the consequence is real. Give the file, the line,
  and one sentence on what an attacker or an unlucky user actually gets. If you cannot state the
  consequence, you have not confirmed it.
- **CLEARED.** The predicate appears violated but is not, and you must say **why**, naming the
  mechanism. "Scoped by the `requireWorkspace` middleware at line 12." "Reached only from an admin
  route." A cleared hit with no stated mechanism is an unread hit.
- **UNCERTAIN.** You cannot resolve it from the code available. Say precisely what would resolve it:
  a specific question for the author, a runtime check, a look at a repository you do not have. Never
  silently drop these, and never promote them to CONFIRMED to look thorough.

The CLEARED bucket is what separates this from a grep dump. It is also the bucket that most often
reveals the real answer, because a hit you clear by naming a middleware tells you the systemic fix
already exists and the bug was a handler that bypassed it.

### Phase 5. Report

Lead with the population count, because that is the decision-relevant number:

> The diff fixes 1 of 6 instances of this defect. 5 remain: 4 confirmed, 1 uncertain.

Then, in order:

1. **The predicate**, stated once, so a reader can disagree with the rule rather than with each finding.
2. **The confirmed list**, ordered by consequence, not by file path.
3. **The cleared list**, compressed to one line each with the mechanism named.
4. **The uncertain list**, with the specific question attached to each.
5. **The systemic fix**, if one exists, and an honest note on its migration cost.
6. **What the search could not reach.**

Then a recommendation with a shape, not a vibe: ship the diff and file the rest, hold the diff until
the family is fixed, or escalate because the consequence is severe enough that partial disclosure in
a public repository is itself a risk.

## Decision rules

**Ship now or hold?** Hold when fixing one instance makes the others more dangerous, which happens
when the fix is publicly visible and the defect is a security defect, because the diff is a map. Ship
now when the instances are independent, the fix is a strict improvement, and the remaining work is
tracked. Default to shipping, because holding a correct fix hostage to a sweep is how sweeps get
resented.

**How wide to search.** Widen until two consecutive widenings find nothing new. If the sibling search
found instances, do the shape search. If the shape search found instances in a directory you did not
expect, search that directory's siblings too. Stop when the marginal search is empty, not when you
are tired.

**When the population is large.** Above roughly fifteen confirmed instances, stop enumerating and
change the deliverable. A list of forty is not actionable. Report the count, the pattern, the two or
three worst instances as evidence, and make the systemic fix the recommendation. The value has moved
from the list to the diagnosis.

**When you find nothing.** Say so explicitly, and say what you searched. "Searched all 14 files in
`routes/`, all 31 call sites of `query()`, and every occurrence of `where id =`. No other instances."
A negative result with its method stated is a real result. A negative result with no method is an
absence of work.

## Failure modes, and what to do about each

**Predicate too narrow.** You searched for the exact identifier from the diff and found only the diff.
Symptom: a clean sweep on a bug class you know is common. Fix: re-derive the predicate one level more
abstract, then search again.

**Predicate too broad.** Two hundred hits, most of them fine. Symptom: triage becomes unbearable and
you start skimming. Fix: add the qualifying condition that distinguishes the dangerous case, usually
"reachable from untrusted input" or "inside a request handler", and re-run.

**Clone blindness.** The same bug exists in a file that uses different variable names and a different
helper, so no textual search finds it. This is why sibling search by directory exists. Read the
neighbours, do not only grep them.

**The fix that creates the next bug.** A local fix at one call site strongly implies the next
developer will copy the unfixed neighbour. Always ask whether the fix belongs one layer down.

**Triage inflation.** Reporting hits you did not actually read, on the theory that the author will
sort it out. This is the fastest way to make a reviewer's findings ignored. If you did not read it,
it goes in UNCERTAIN with the reason.

**Scope revolt.** The author asked for a review of three lines and received a report on twelve files.
Manage this in the framing: the diff is approved or not on its own merits, in the first paragraph, and
the sweep is presented as separate follow-up work with its own priority. Never hold a good fix hostage
to a finding it did not cause.

## Worked example

The diff adds an ownership constraint to a single handler:

```diff
- const rows = await query('select * from invoices where id = $1', [req.params.id]);
+ const rows = await query(
+   'select * from invoices where id = $1 and owner_user_id = $2',
+   [req.params.id, req.user.id]
+ );
```

**Class:** defect fix, authorisation.

**Predicate:** every query against a user-owned table inside an authenticated request handler must
either constrain on `req.user.id`, or be preceded in the same handler by an explicit ownership or
membership check.

**Exceptions decided in advance:** admin-only routers, deliberately public resources, and handlers
whose scoping is performed by upstream middleware.

**Searches:**
- sibling: every file in `routes/`,
- shape: `where id = \$1` and `where id =`,
- absence, anchor technique: every occurrence of each user-owned table name, then inspect for the check,
- absence, enumerate and subtract: every route in the route table, checked one by one.

**Triage:**

| File | Verdict | Note |
|---|---|---|
| `routes/invoices.js` PATCH | CONFIRMED | update by id with no owner constraint. Any user can change any invoice's status. |
| `routes/attachments.js` GET | CONFIRMED | returns a signed download URL for any attachment id. |
| `routes/attachments.js` DELETE | CONFIRMED | any user can delete any attachment. |
| `routes/exports.js` GET | CONFIRMED | export jobs contain a download URL for another tenant's data. |
| `routes/webhooks.js` POST rotate-secret | CONFIRMED | rotating another tenant's signing secret is a denial of service, and the response returns the new secret. |
| `routes/teams.js` GET members | CLEARED | membership check on lines 9 to 13 precedes the read. |

**Report:** the diff fixes 1 of 6. Five confirmed remain. The systemic fix is a scoped query helper
that takes the user id as a required argument, which makes the unscoped form unrepresentable across
all 14 route files. Migration is mechanical. The GET handlers should ship first because they are
readable without authentication to any logged-in account.

**Could not reach:** the mobile client's local cache, and any database views that bypass these
handlers.

## What this skill does not do

- It does not run the code, execute tests, or verify at runtime. Every verdict is a static reading
  and can be wrong about reachability.
- It does not find bugs that are not represented in the diff. It expands from what the diff reveals,
  so a diff that reveals nothing produces nothing.
- It is not a security audit. It finds repeats of one defect class, not the classes nobody has hit yet.
- It is weak on cross-repository and cross-language blast radius unless those repositories are
  present. It will say so rather than guess.
- It does not replace reviewing the diff for correctness, design, or readability. It is what you do
  after that, not instead of it.
