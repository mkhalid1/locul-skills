---
name: authorisation-model-build
description: Builds an authorisation model for an application or API: the resource and actor inventory including service accounts and support staff, a rule table of permitted verbs, one named enforcement point chosen for whether it fails closed, and a negative test matrix covering wrong tenant, wrong user, unauthenticated caller and lower-privileged role. Covers object level, object property level and function level authorisation, tenant isolation, ownership versus role versus relationship, and the not-found versus forbidden decision. This skill should be used when designing permissions for a new feature, when making a single-tenant service multi-tenant, when a report says one user saw another user's data, or when deciding where an access check should live.
---

# Authorisation model build

## What this is built on

Authorisation defects are the one serious class a scanner structurally cannot find. Everything about the request is correct. The session is valid, the token verifies, the SQL is parameterised, the input passes schema validation, and the query is exactly the query the developer meant to write. The only thing wrong is that the identifier in the path belongs to somebody else, and nothing in the code says so. There is no dangerous function to grep for and no tainted input to trace, because the input is not tainted. It is well formed and it is not yours.

OWASP's own definition is worth keeping in front of you while you build, because it names the consequence rather than the mechanism: "Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification or destruction of all data, or performing a business function outside the user's limits."

That is why this class does not get engineered out the way injection largely has. In the OWASP Top 10 2025, the eighth installment, Broken Access Control is A01, the same position it held in the 2021 edition. The 2025 entry states that "100% of the applications tested were found to have some form of broken access control", with an average incidence rate of 3.74 per cent, a maximum incidence rate of 20.15 per cent, average coverage of 42.93 per cent, 1,839,701 total occurrences and 32,654 mapped CVEs across 40 CWEs. It carries the highest occurrence count in the contributed data.

**Do not compare that figure to the 2021 one.** The 2021 edition recorded that "94% of applications were tested for some form of broken access control", which counts test coverage, not defects, and it is routinely misquoted as 94 per cent of applications having the defect. The two sentences measure different things. If you want a like-for-like comparison across the two editions, use occurrences: 318,487 in 2021 against 1,839,701 in 2025.

One structural change in 2025 matters for what you put in the model. **Server-Side Request Forgery is no longer its own category.** CWE-918 now sits inside A01:2025 alongside CWE-200, CWE-201 and CWE-352. That is the right home for it, and it means an endpoint that fetches a URL the caller supplies belongs in your rule table: the privilege being borrowed is your server's network position, and it is an authorisation decision even though no object identifier is involved.

Separately, MITRE's 2025 CWE Top 25, drawn from 39,080 CVE records and published in December 2025, ranks CWE-862 Missing Authorization at number 4, up from number 9 the previous year, with CWE-863 Incorrect Authorization at 17, CWE-284 Improper Access Control at 19 and CWE-639 Authorization Bypass Through User-Controlled Key at 24. That is a different dataset from OWASP's and a different method, which is the point of citing both.

So the goal of this build is not a fixed endpoint. It is an application where the defect is difficult to reintroduce, which is a different and more valuable output. That difference is entirely decided by where the check lives.

## The four defects, named properly

Use the published names. They are from the OWASP API Security Top 10, whose current edition is 2023 as of August 2026. Do not harmonise the years out of tidiness: the main list is on its 2025 edition and the API list is still on 2023, and citing an API entry as 2025 is the fastest way to look like you have not read either.

**Object level, API1:2023 Broken Object Level Authorization.** A valid user reaches another user's object by changing an identifier that arrives in a path parameter, a query string, a header or the body. OWASP describes it as extremely common in API-based applications because servers routinely take the client's object ID as the answer to which data to return. The tell: a handler that fetches by primary key and never mentions the caller after the session lookup.

**Object property level, API3:2023 Broken Object Property Level Authorization.** The object is yours, but a field inside it is not. This entry merges two from the 2019 edition, Excessive Data Exposure on the read side and Mass Assignment on the write side. On reads, the tell is a generic serialiser that returns the whole record and relies on the client to display only some of it. On writes, the tell is any framework helper that binds the request body straight onto a model. OWASP's guidance is explicit: avoid generic serialisation, cherry-pick the properties you return, and allow changes only to properties the client is meant to update.

**Function level, API5:2023 Broken Function Level Authorization.** The endpoint itself should be out of reach for this role. OWASP's advice includes one line worth quoting into your own review notes: do not assume an endpoint is administrative solely because of its URL path. The tell is a route that is protected by not appearing in the navigation menu, or an administrative action living inside an ordinary controller that inherits no role check.

**Tenant level.** Not a separate OWASP entry, and in practice the most expensive. Isolation is implemented as a filter in a `WHERE` clause, which means it is implemented as something a person has to remember. The tell is any query in the codebase that touches a tenant-scoped table without mentioning the tenant.

The three API entries are three different questions, and this is the part worth writing on the whiteboard rather than the list itself. Object level asks whether this user may reach this **object**. Function level asks whether this user may reach this **operation**. Object property level asks whether this user may read or write this **field** of this object. Passing one implies nothing about the other two, which is why a rule table with a row per resource is not enough on its own: an endpoint that correctly refuses another tenant's invoice can still return a cost field to a viewer, and an admin operation can still be reachable by a role that owns nothing at all.

## Step 1. The inventory, resources crossed with actors

List every resource type. Not every table: every thing a person would name in a sentence about permission. Report, workspace, invoice, membership, export job, API key, audit entry.

Then list every actor type, and this is the step that pays for itself. The ones people write down are anonymous visitor, member, admin and owner. The ones they miss are the ones that already have production access:

- **Service accounts.** The nightly scheduler, the webhook receiver, the data pipeline. These usually connect with a credential that has no tenant at all, because they legitimately act across tenants, which makes them the highest-privilege actor in the system and the least modelled.
- **Support and internal staff.** Someone built a tool so support can see a customer's account. It typically works by impersonating or by a header that skips the tenant filter. It is an actor. Model it as one, with its own row, its own scope, a time bound and a written audit record, or it becomes the account takeover route that has no login attached to it.
- **Former members.** A membership row that was deleted, or an invitation that was never accepted.
- **Delegates.** An accountant with access to one workspace's invoices only.

Now build the grid: for each resource and actor pair, the permitted verbs, written as create, read, list, update, delete, share, export. Empty cells are the point of the exercise. A cell you cannot fill without asking somebody is a requirement nobody has decided, and it will otherwise be decided by whichever developer writes the endpoint first.

## Step 2. Ownership, role, relationship

Three ways to express a rule, and they are not interchangeable.

**Role.** The rule depends only on what the actor is. Admins may invite members. Expressible as a static table, cheap to check, and it is the only one a route decorator can evaluate, because a decorator runs before anything has been loaded.

**Ownership.** The rule depends on a field of the object. The author of a document may edit it. This needs the object, which is why it cannot live in the decorator, and it is exactly the rule a role-based system cannot express: there is no role called "author of this particular document".

**Relationship.** The rule depends on a path between actor and object, possibly several hops. A user may read a comment if they are a member of the workspace that owns the project that contains the thread. This is where systems get expensive, because the check becomes a graph traversal and the traversal itself has to be authorised.

The rule for choosing: use role where the answer does not depend on which object it is; use ownership where the answer is one field on the object you already loaded; use relationship only where the path is genuinely more than one hop, and when you reach that point, store the derived relationship as a queryable row rather than recomputing it per request. A relationship check written as three nested lookups inside a handler is a performance problem that will be optimised away by somebody who does not know it was a security control.

## Step 3. Choose one enforcement point, and choose it for how it fails

Six places a check can live. What matters for each is not what it catches when written, but what happens when a developer six months from now forgets to write it.

| Layer | Catches | On omission |
| --- | --- | --- |
| Interface, hiding a control | Nothing | Route still answers, fails open |
| Route decorator or middleware | Function level only | Fails open unless the router refuses unannotated routes |
| Top of the handler | Object and property level for that handler | Fails open, once per handler |
| Service layer | Everything routed through it | Fails open for any caller that skips the service |
| Data access layer | Everything, if there is exactly one accessor | Fails open only if an unscoped accessor is reachable |
| Database row policies | Everything, including background jobs and a console session | Fails closed, subject to the owner caveat below |

Fail-closed means something specific at each layer, and it is worth writing the definition into your own document: when the check is absent, the request is refused rather than served. Only the last two can be made to behave that way, and neither does so automatically.

A data access layer fails closed only if the unscoped accessor is impossible to call by accident. That means the scoped accessor is the default export, the unscoped one lives in a separately named module that has to be imported deliberately, and a lint rule or code owner rule flags the import. If `findById` exists next to `findByIdForActor` and both are equally reachable, you have documentation, not enforcement.

## The decision rule

**If your data store supports row level policies and tenancy is a column on the tenant-scoped tables**, enforce there and treat the application check as defence in depth. You get the background job, the migration script and the manual console session for free, and those are the three callers that never had the filter.

**If it does not, or tenancy is not a column**, enforce in a single data access layer that every query passes through, and spend the effort on making the unscoped accessor impossible to reach by accident rather than on writing more checks.

**If you cannot tell whether every query already passes through one place**, do not add another decorator. Stop and find out. Count the distinct call sites that construct a query against your three largest tenant-scoped tables. If that number is above about ten, or if you find a reporting service, an export worker or an admin script among them, you do not have a chokepoint, you have a convention, and scattering more checks over a convention is precisely how this defect class survives for a decade. Build the chokepoint first. It is the only work on this page that changes the shape of the problem rather than the count of instances.

## Step 4. Row level security, and the caveat that decides it

If you take the database branch, these facts decide whether it works. All are from the PostgreSQL project's own documentation, checked on 20 August 2026.

**The owner bypass is the one that quietly voids the whole thing.** Superusers and roles with the `BYPASSRLS` attribute always bypass row security. Table owners normally bypass it as well, unless the owner opts in with `ALTER TABLE ... FORCE ROW LEVEL SECURITY`. The default deployment shape, one database role that owns the tables and is also the role the application connects as, produces policies that are enabled, visible in the schema, and enforcing nothing. Connect the application as a role that does not own the tables, or set `FORCE ROW LEVEL SECURITY`, and prove it with a query before you write the test matrix.

**Default deny is on your side.** With row level security enabled and no policy present, no rows are visible or modifiable. A new table therefore fails closed, which is the opposite of the application-layer behaviour where a new table starts wide open.

**Permissive policies widen, restrictive policies narrow.** Permissive policies applicable to a query are combined with `OR`, restrictive ones with `AND`, and a record is accessible only if at least one permissive policy passes plus all restrictive ones. Restrictive policies alone grant nothing. The practical consequence: adding a second permissive policy can only ever add access. Somebody who adds one to fix a support ticket has widened the model.

**`USING` and `WITH CHECK` are not the same control.** `USING` decides which existing rows are visible, and rows that fail it are silently suppressed with no error. `WITH CHECK` decides which new or updated rows may be written, and a failure raises an error. `SELECT` policies cannot have `WITH CHECK`, `INSERT` policies cannot have `USING`, and for `ALL` or `UPDATE` policies with no `WITH CHECK`, the `USING` expression is used for both. Write the `WITH CHECK` expression deliberately, because that is the clause that stops a row being moved out of its tenant by an update.

**Two leaks the policies do not close.** Referential integrity checks, including unique constraints and foreign keys, always bypass row security in order to preserve data integrity, and the documentation names this as a covert channel to design around: a unique constraint on an email column will tell an attacker whether that address exists in a tenant they cannot read. Separately, `leakproof` functions may be applied by the optimiser ahead of the row security check, so a policy is not a guarantee about evaluation order for every expression in the query.

**One operational note.** Setting `row_security = off` does not bypass policies. It raises an error if any query's results would be filtered by one, which makes it a useful assertion for backups and for a test that proves an admin path is genuinely unfiltered.

## Step 5. Identifiers, stated honestly

Unguessable identifiers, UUIDv4 or a random string, raise the cost of discovery. They are not access control, and treating them as such is a design error with its own name below. An object whose only protection is that its identifier is hard to guess is protected until the identifier appears in a referrer header, a shared link, a support ticket, a browser history, a server access log, an analytics payload or a third-party script.

What they do buy is real and narrow: they remove bulk enumeration, so an attacker cannot walk from record 1 to record 50,000. Sequential integers make a single defect into a full database export. So use opaque identifiers, and separately enforce the rule. The enumeration surface that survives regardless: list endpoints that return counts or totals for filtered sets, search endpoints that report the number of matches, error messages that differ by existence, and export or report jobs that accept a filter and report how many rows matched.

## Step 6. The negative test matrix

This is the part nobody writes, and it is the deliverable that keeps the model true after you stop looking at it. For each resource and each mutating verb, four negative callers at minimum:

1. Unauthenticated.
2. Authenticated, different tenant.
3. Authenticated, same tenant, not the owner, where the rule is ownership-based.
4. Authenticated, same tenant, lower-privileged role, where the rule is role-based.

Then the two that get skipped. Fifth, a property level write: the same legitimate caller sending a payload that includes a field they may not set, such as `role`, `tenant_id`, `price` or `verified`. Sixth, a list endpoint asserted on its count, not only on its contents, because a list that leaks a total is a leak even when the rows are filtered.

Two assertion rules make the difference between a matrix that works and one that passes while broken:

- **Assert the body, not only the status code.** A handler that returns `200` with a null object passes a status-only assertion and has still told the caller the object exists. Assert the status and the absence of the resource-specific field.
- **Re-read after every refused write.** A refusal test that checks only the response can pass while the row was updated before the check ran. The write test must read the row back as its legitimate owner and assert it is unchanged.

## The refusal, and what it must not leak

Choose between 404 and 403 on one question, and it is not whether the row was found. It is whether this caller is permitted to learn that the resource exists.

If the caller has no legitimate way to know the resource exists, return the same response for "no such object" and "exists but not yours". That is 404 in both cases, with an identical body. If the caller is entitled to know it exists but not to perform this action, for example a viewer in a workspace attempting a delete, 403 is correct and more useful, because a 404 there sends a legitimate user hunting for a bug that is not there.

Then close the two side channels. Keep the message identical across both paths, since a different `error` string undoes the identical status code. And keep the timing comparable: the classic tell is a handler that fetches the object, then checks ownership, so the "not yours" path takes a database round trip that the "does not exist" path also takes but the "no such route" path does not. Deciding authorisation before or during the fetch, rather than after it, removes that difference and removes the check-after-fetch failure at the same time.

## Worked example

A logistics SaaS with workspace-scoped reporting. The endpoint under review is `GET /reports/{report_id}/export`.

**Inventory.** Resources: report, report schedule, export job, workspace, membership, API key. Actors: viewer, editor, workspace admin, the nightly schedule runner, and a support engineer using an internal console.

**Grid gaps found immediately.** Nobody had decided whether a viewer may export. The schedule runner had no row at all, because it was a cron job with the same database credential as the web application. The support engineer had no row either, because the console was built by a different team and reached workspaces through an `X-Workspace-Override` header.

**Defect.** The export handler loads the report by primary key and renders it. The tenancy filter exists, but only in the list endpoint, which is where the developer was thinking about tenancy. That is object level, API1:2023, and the property level version is one field away: the export accepts a `columns` array that will happily include a cost column viewers are not meant to see.

**Placement.** Reports, schedules and export jobs all carry `workspace_id`, so the database branch applies. Policies on the three tables, the application role changed so it is not the table owner, and `FORCE ROW LEVEL SECURITY` set anyway. The handler check stays as defence in depth. The schedule runner gets its own role with an explicit policy rather than an implicit bypass. The support path becomes a fifth row in the rule table: a distinct actor, a scoped grant, a duration and an audit entry, not a header.

**Two negative tests.**

1. A member of workspace B requests a report belonging to workspace A. Expect `404`, an identical body to a non-existent report ID, and no `rows` key present.
2. A viewer in workspace A posts the export with `columns` including `unit_cost`. Expect `403`, and a re-read of the generated export as a workspace admin showing the column absent from the stored artifact.

**Verdict.** Enforce in the database, keep the handler check as the second layer, and treat the support console as an actor with a row rather than as infrastructure. The defect that was reported was one endpoint. The defect that was fixed was the absence of a chokepoint.

## Failure modes

**Interface-Only Enforcement.** The control is hidden from the menu and the route is untouched. It looks fixed in a browser and fails on the first person who reads the JavaScript bundle.

**Forgotten Filter.** Tenancy is a `WHERE` clause, so isolation depends on memory. Symptom: a reporting or export path, written later and by someone else, that is missing the clause every other query has.

**Owner Bypass.** Row level policies are enabled, reviewed and enforcing nothing, because the application connects as the role that owns the tables. The schema looks correct, and no error is ever raised.

**Mass Assignment Drift.** The model gains a field, the binding is generic, and the field is writable from the day it is added. Nothing changed in the handler, so nothing appears in the diff a reviewer reads.

**Support Actor Unmodelled.** The internal tool grew from a debugging script and never entered the permission table. Access is unbounded in time, unlogged, and attached to a shared credential rather than to a person.

**Identifier Faith.** A UUID is treated as the control. It survives until the identifier appears in a log, a referrer, a screenshot or a support thread, and then it fails for every copy of that identifier at once.

**Existence Leak.** The status codes are correct but the refusals are not identical: a different message, a different response time, or a `404` on one path and a `403` on another for the same underlying condition. Enumeration is restored without a single successful request.

**Check After Fetch.** The object is loaded, then the check runs. The write may already have happened, the timing difference is measurable, and a stack trace from the fetch can escape before the check is reached.

**New Endpoint Amnesia.** The model was right on the day it was written. Six months later a new route ships without the pattern, because the pattern lived in reviewers' heads rather than in a layer that refuses by default. This is the failure the enforcement placement step exists to prevent, and the reason a model that passes today is not evidence about next quarter.

## What this skill does not do

- **It does not cover authentication, session management or token issuance.** Who the caller is, is a separate and larger subject. Everything here assumes the identity is already trustworthy, and none of it survives if that assumption is false.
- **It cannot see your data model.** The grid, the chokepoint count and the policy expressions all depend on facts about your schema and your call sites that have to be supplied. Given a vague description it will produce a confident model of a system that does not exist.
- **It does not audit an existing codebase for instances.** Finding the forty endpoints that already have the defect is a review pass over the route table, it scales with routes rather than resources, and an automated security review is faster at the parts of it that are visible in a diff.
- **It has no view on policy engines beyond naming that they exist.** Externalised policy decision points are a real option and a real operational commitment. Which one, and how to keep its bundle in step with a schema that changes, is out of scope here.
- **It cannot fix a tenancy model that is wrong.** Where one customer's records legitimately live inside another customer's account, or where a shared record is owned by two tenants at once, no enforcement point can express the rule and the answer is a schema change.
- **It is dated.** The published figures above are the OWASP Top 10 2025, the OWASP API Security Top 10 2023 edition and the 2025 CWE Top 25 published in December 2025, all read on 20 August 2026. Check the edition before you quote a position.

## The output

One document, and it is not a findings list:

1. The resource and actor inventory, with service accounts and support staff as named actors.
2. The rule table, every resource and actor pair, with permitted verbs and each rule labelled role, ownership or relationship.
3. The enforcement point, stated as a single place, with the reason it fails closed and the specific configuration that makes it true.
4. The identifier decision and the enumeration surfaces that remain.
5. The negative test matrix, four callers per resource and verb plus the property write and the list count, each with its expected status, its body assertion and, for writes, its re-read.
6. The refusal convention, 404 or 403 per resource, with the message and timing rules written down so the next endpoint inherits them.
