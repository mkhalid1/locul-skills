---
name: spec-to-tickets
description: Turns a feature idea into a concise specification and a set of implementation-ordered tickets a developer can start on immediately. Verifies every field, endpoint and table name against the real codebase before it enters the document, forces an explicit out-of-scope list, writes acceptance criteria with pass or fail boundaries, and orders tickets as a dependency graph rather than by priority. This skill should be used when a feature has been agreed and needs writing up, when a vague brief needs turning into work someone can start, or when an existing specification is being revised.
---

# Spec to tickets

## The claim this skill is built on

A specification is a naming document before it is a description document.

The prose in a specification is read once and forgotten. The names in it are copied: into the ticket, the branch name, the migration, the test fixture, the API response. By then four people have agreed on a word invented in a hurry by somebody looking at a chat thread rather than a schema.

The obvious approach makes this worse in three specific ways. It writes a comprehensive document first, so the document is long and nobody reads it. It sorts the resulting tickets by priority, so the top ticket is the most valuable one and cannot be started because the column it writes to does not exist yet. And it describes only what is in scope, so everything adjacent is left ambiguous, and ambiguity resolves in favour of whoever asks last.

This skill produces two artefacts and nothing else: one specification and one ordered ticket list. The value is concentrated in three steps that are easy to skip: verifying the names, forcing the boundary, and ordering by dependency.

## Output shape

Write into one dated folder per feature, at your repository's conventional location for planning documents, for example `docs/specs/2026-08-20-task-snooze/`. Use an ISO date prefix, which sorts correctly everywhere and is unambiguous on any operating system, and forward slashes, which both Windows and Unix-like systems accept in tooling paths.

The folder contains exactly two files: `specification.md` and `tickets.md`. Two, because the specification is read by people deciding and the tickets by people building, and merging them means each audience skims past the other's half.

## Step 0. At most three questions, and usually fewer

Ask clarifying questions only when the missing information would change the artefact. The ceiling is three, and they go in one message rather than a conversation.

The three worth asking, when genuinely absent: who the target user is, what the core problem is in their words, and what hard constraint is not negotiable, such as a deadline, a compliance requirement, or a contract you may not break.

The test for whether a question earns a slot: **write down the two most plausible answers, and check whether they produce different documents.** If both answers produce the same specification, you already have enough. Timezone handling, for instance, usually does not change the section list; it changes one acceptance criterion, so it belongs in open questions with an owner rather than in an interrogation.

When information is missing and nobody is available, do not stall. Write the assumption into the specification, marked as an assumption, and carry it into open questions with the name of the person who can settle it. A marked assumption is auditable. A blocked thread is not.

## Step 1. Verify every name before it enters the document

This is the step that changes the value of everything downstream, and it happens before the first section is written.

**Why an invented field name is the most expensive error in a specification.** Every other error stays in the document. A vague problem statement is annoying in exactly one place. A wrong identifier propagates. The specification says `user.last_active_at`. The ticket copies it verbatim, because copying is what tickets do. The developer writes the query. The test mocks the same name, so the test passes. The reviewer reads the ticket and the diff, not the schema. It surfaces at integration, or later, as a column of nulls in a dashboard nobody trusts again.

Count the cost by the stage it is caught at. At specification time it is one lookup. At ticket time, two edits. At implementation time, a rewrite plus an argument about whether to add the column or use the existing one. In production, an incident and a backfill.

**The procedure:**

1. **Extract the identifier list from your own draft.** Read what you have written and pull out every proper noun the system will have to resolve: table names, column names, endpoint paths, query parameters, response fields, event names, job names, config keys, permission strings, feature flags, enum values.
2. **Resolve each one to a location.** A file and a line, a migration, or a schema definition. "I believe there is a users table" is not a resolution. If you cannot point at it, it is not verified.
3. **Assign one of three verdicts.**
   - **EXISTS**, with the location recorded. Use the exact spelling found, not the one you drafted.
   - **NEW**, meaning it does not exist and this feature creates it. This is fine, and it must be labelled, because a new column is a migration and a migration is a ticket. An unlabelled new name reads as existing and gets no migration ticket, which is the second-order cost of this error and the one that produces a ticket list whose first item cannot run.
   - **UNVERIFIED**, meaning you have no access to the thing that would settle it: another service, a partner API, a database behind credentials you do not hold. Render it in the document with an inline marker, never as a bare name, and give the ticket that touches it a verification step as its first acceptance criterion.

**The mismatches that actually occur:** singular against plural table names; camel case in the API response against snake case in the database, where both exist and only one is correct in context; a field that exists on the model and is deliberately not serialised, so the frontend ticket is impossible as written; a query parameter whose name is right and whose accepted values are wrong; an endpoint living under a different version prefix; a field that exists, is deprecated, and is written by nothing.

Two further checks that cost nothing and prevent untestable criteria: **nullability**, because an acceptance criterion that assumes a value is present is unfalsifiable against a nullable column, and **relation direction**, because "the project's owner" and "the owner's projects" imply different queries and only one of them is cheap.

## Step 2. The optional customer pass, and the omit-entirely rule

You may run a pass through an ideal-customer lens, putting targeted questions to it rather than open ones. Would this user value the automatic behaviour or find it presumptuous. What would they expect after the item wakes. What friction would they hit the first time. Fold the answers into a **Customer insights** section, each insight tied to a decision the specification then makes, so it reads as input rather than as colour.

**If you did not run the pass, omit the section entirely. Do not leave the heading.**

The reason is not tidiness. A heading is a claim that the thing under it was considered. A reader scanning the document sees "Customer insights" and takes it as evidence somebody thought about the customer, and a heading with "N/A" under it converts an absence into a false positive that survives every later conversation about the feature. The same rule applies to technical notes and open questions. **No section exists unless it contains something that changes a decision.**

## Step 3. The specification, to a fixed section list

**Problem statement.** Two to three sentences maximum: who has the problem, what they do instead today, what that costs. Longer than three sentences and it is a solution statement in disguise. The tell is an interface noun: if "button" or "page" appears, you have written a solution.

**Customer insights.** Omitted entirely if the pass was not run.

**Goals and success metrics.** Each one measurable, which means it names a number, a time window, and the source the number will be read from. "Improve onboarding" is not a goal. "Median time from signup to first saved record under four minutes, read from the existing signup and record-created events, within thirty days of release" is one, and the phrase that makes it real is "existing events", because a metric with no existing source implies an instrumentation ticket that has to appear in the list.

**Scope, with an explicit out-of-scope list.** See the next step, which is the one that does the work.

**Epics, each with a one-line description and its user stories.** An epic exists to group. If every story has its own epic, delete the layer, since it carries no information.

**Technical notes**, only where a real constraint exists: a lock, a rate limit, an index that must be added, a third-party quota.

**Open questions**, each with an owner and the decision it blocks. A question with no owner is a note.

Keep the whole specification under about 900 words for a two-week feature, and treat 1,500 as a ceiling for anything. The reason is competitive rather than aesthetic: the document competes for attention against the ticket, and the ticket always wins, because the ticket is what got assigned.

## Step 4. The out-of-scope list is where scope creep is actually stopped

Scope creep does not enter through the in-scope list. Everything in that list was argued over. It enters through the silence around the list, because every adjacent capability that goes unnamed is ambiguous by default.

**Rules for a list that actually holds:**

- **Minimum three entries.** If you cannot name three things this feature deliberately does not do, you have not found the boundary yet.
- **Each entry names the capability a reasonable person would assume was included**, not a strawman. "We are not building a whole reporting suite" stops nobody. "Bulk import of existing records, which carries its own file-format decisions and belongs to a separate feature" stops the conversation in one line.
- **Each entry carries a reason and a destination.** Later, never, or an existing ticket reference. The destination is what turns a refusal into a plan.
- **Cover the four doorways.** Creep arrives through the *adjacent entity*, because whatever you built this for has a sibling and somebody wants it there too. Through the *admin surface*, because someone always wants a back-office view of the new thing. Through the *bulk case*, since one is in scope and one thousand is a different feature with different performance characteristics. And through *existing data*, because new behaviour applies to new records only unless you say otherwise, and nobody assumes that.

The list is also the negotiation record. When the request arrives mid-build, the answer is a pointer to a line that was agreed, rather than an argument held under deadline pressure.

## Step 5. Stories and criteria with pass or fail boundaries

Write each story as "As a [user], I want to [action] so that [outcome]". The **so that** must not restate the action. Test it by covering the outcome clause and trying to predict it from the action: if you can, the outcome is empty and the story has no rationale attached to it.

Acceptance criteria are checkboxes, and a criterion is only a criterion if a person can mark it pass or fail without a discussion. The operational test is: **name the observation that would falsify it.** If you cannot, rewrite.

- Not a criterion: the list loads quickly. A criterion: the list renders within two seconds on an account holding 200 records.
- Not a criterion: errors are handled gracefully. A criterion: submitting with an empty required field keeps the values already entered, shows the message next to that field, and does not navigate away.

Two criteria that are almost always missing and almost always cause rework: **one negative criterion**, stating what must not happen, and **one covering the empty state**, which is the screen every user sees first and the one that gets built last.

Three to seven criteria per story is the working range. Below three the story is under-specified or too small to exist. Above eight it is two stories.

## Step 6. Tickets ordered by implementation sequence

Priority order answers "what matters most". Build order answers "what can be started". They are different questions, and only one of them is executable.

**The rule: the ticket list is a topological sort of the dependency graph, grouped by epic, and the first ticket must be startable today by one person with nothing else merged.**

Procedure:

1. List the tickets unordered.
2. For each, write preconditions as ticket references rather than prose. "Blocked by: 2" is checkable. "Needs the schema work" is not.
3. Sort topologically. Where two tickets are both unblocked, **priority breaks the tie**. Priority is never the sort key.
4. Validate: no cycles, every predecessor above its dependant, and at least one ticket with an empty precondition list.
5. **A cycle is information, not an obstacle.** It means either two tickets are one ticket, or the split was made at the wrong seam. Merge or re-cut. Do not break the cycle by deleting the inconvenient edge, which just moves the problem into the build.

Shapes that recur: schema before write path before read path before interface; the migration before anything reading the column; the feature flag before the first user-visible ticket; and instrumentation before the ticket whose success metric depends on it, which otherwise ends up last and leaves the metric with no baseline.

Every ticket carries exactly: parent epic, type (feature, bug, chore, spike), priority, a description of one to three sentences, checkbox acceptance criteria, and notes covering edge cases and dependencies.

**Spikes get a time box and a question they must answer**, and their output is a written decision, not code. A spike with no question is a licence to wander, and it is the ticket most likely to still be open in a month.

## Decision rule: how much verification is enough

- **If the repository or schema is in context:** every identifier resolves to a location before it enters the document. No exceptions and no "probably".
- **If access is partial**, such as a public API reference but no database, or one service of three: verify what you can, mark the rest inline as unverified, and give the affected ticket a verification step as its first acceptance criterion.
- **If you cannot tell, because there is no access at all:** do not invent a plausible name. Describe the field in prose by its meaning, for example "the timestamp of the user's most recent session", and put the naming decision into open questions with an owner. A described field cannot be miscopied into code. A plausible invented name can, and will, because it looks like a decision that has already been made.

## On revision, update in place

When the specification changes, edit the existing files. Do not emit a new document.

A second document means two sources of truth, and the team reads the wrong one. It is reliably the older one, because that is what the ticket, the channel topic and the calendar invitation already link to. The tell is the phrase "the updated spec" appearing in chat, which only ever exists because there is a stale one.

Keep a short dated changelog at the bottom. When scope is removed, **move it into the out-of-scope list with the date** rather than deleting it, because deletion loses the negotiation record and a deleted item returns as a fresh request. When a ticket has already been started, do not silently rewrite it: note the change on the ticket and flag every ticket whose preconditions moved, since the order may no longer be valid.

## Worked example, compressed

**Brief:** "Let people snooze a task until a date, so it drops off the list and comes back." Target system: a project management tool for small teams.

**Questions asked, two:** can any member snooze a task or only the assignee, and does a snoozed task still count towards the project's overdue total. A third candidate, timezone handling, was not asked, because both plausible answers produce the same section list; it became an open question with an owner.

**Verification pass, eleven identifiers:**

| Identifier from the brief | Verdict |
|---|---|
| `tasks` table | EXISTS |
| `tasks.snoozed_until` | NEW. Requires a migration, nullable, indexed. |
| `GET /api/tasks?status=active` | DOES NOT EXIST as written. The real parameter is `state`, and its values are `open` and `closed`. |
| the project overdue count | UNVERIFIED. Computed in a reporting service not present in this repository. |
| remaining seven | EXISTS |

The wrong query parameter is the one that matters. Copied unchecked, it becomes a ticket instructing a developer to filter on a parameter the API ignores silently, which returns the full list and looks like a caching bug for two days.

**Out-of-scope, five entries:** snoozing a whole project (adjacent entity); a bulk snooze across a filtered view (bulk case); an administrator view of everything currently snoozed (admin surface); applying the new exclusion rule retrospectively to archived reports (existing data); recurring snoozes, which need a schedule model that does not exist.

**Tickets, in build order:** 1, migration adding the nullable indexed column. 2, expose the field on the task serialiser. 3, the snooze write endpoint with validation that the date is in the future and within twelve months. 4, exclude snoozed tasks from the default list, which is the parameter that was wrong in the brief. 5, the wake job, written to be idempotent because it will be retried. 6, the interface control. 7, instrumentation for the success metric. Priority says ticket 6 is the most valuable item in the list. It is sixth, because it cannot start.

**Verdict:** the specification runs to 640 words with five out-of-scope entries. Of eleven identifiers, nine resolved to a location, one is labelled new work and carries the migration that became ticket 1, and one is marked unverified with its own criterion. Seven tickets form an acyclic graph whose first ticket is startable immediately. The invented filter was caught in step 1 and never reached a ticket.

## Failure modes

**The unread specification.** Symptom from the outside: the document is linked from the ticket, and the ticket comments re-ask two questions the document answers on page one. Cause is almost always length. The fix is subtraction, not clearer prose.

**The invented identifier.** Symptom: a message on day one saying there is no such field, do you mean this other one. The worse symptom is no message at all, and an unplanned migration appearing in review.

**The unfalsifiable criterion.** Symptom: the ticket sits in review while two people negotiate what "handled gracefully" was supposed to mean. Every criterion quoted in that thread is one nobody could have marked.

**The missing boundary.** Symptom: the feature ships a fortnight late carrying a bulk action nobody specified. From outside it reads as a slow team. It was an unbounded one.

**Priority order wearing build order's clothes.** Symptom: the top ticket is picked up and put back within the hour, or a developer quietly starts the third one instead, after which the board no longer describes the work in progress.

**Interrogation stall.** Symptom: eight clarifying questions arrive before any artefact exists, four get answered, the thread dies, and the feature is eventually specified by somebody else in a chat message.

**The orphaned second document.** Symptom: two files with similar names, the better one unlinked, and a developer building from the other.

**Section theatre.** Symptom: a full set of headings with one line under each, several reading "TBD". It scans as completeness and is the opposite.

## What this skill does not do

- It does not estimate, schedule, or assign. Ordering is not sizing.
- It does not decide whether the feature should exist. Demand, positioning and willingness to pay are all upstream of it, and it will happily specify something nobody wants.
- It cannot verify anything it cannot read. External services, partner APIs and databases behind credentials it does not hold come back as unverified, which is a marker rather than an answer.
- It is not a design specification. No wireframes, no interface copy, no interaction detail beyond what an acceptance criterion can express.
- It does not manage the tracker. States, transitions, assignment and sprint mechanics belong to the tool you already pay for.
