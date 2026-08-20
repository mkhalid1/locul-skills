---
name: concurrency-review
description: Reviews a change for race conditions by walking a fixed taxonomy of seven race shapes over the diff: check-then-act, lost update, double submit, read-modify-write across a network, at-least-once delivery, inconsistent lock ordering, and write skew. For each one found it selects the fix from the shape, choosing between a unique constraint, an optimistic version column, a row lock, and an idempotency key, and states the conditions under which each of those is the wrong choice. Covers transaction isolation, side effects inside transactions, and cache stampede. This skill should be used when reviewing any handler that reads then writes, any queue or webhook consumer, or any endpoint where running it twice would be visibly wrong.
---

# Concurrency and race condition review

## The claim this skill is built on

A review that says "watch out for race conditions here" is worth nothing, because the author already knows that and does not know which one. Races are not a general hazard, they are a small set of specific shapes, and each shape has a fix that works and two or three that look like they work.

The most common failed fix is the tell. When a duplicate record appears in production, the reflex is to make the existence check more careful: check again after the write, check inside a helper, add a second condition. None of it can work, because the defect is not that the check was sloppy. It is that there is a gap between the check and the write, and no amount of care makes a gap zero. Only something that makes the write itself fail can close it.

So this skill does two things in order. It names the shape, from a fixed list. Then it selects the fix from the shape, from a fixed list of four. Naming without fixing produces anxiety, and fixing without naming produces a `SELECT ... FOR UPDATE` on everything.

## The four fixes, so the taxonomy has somewhere to land

1. **A constraint the database enforces.** Unique index, partial unique index, exclusion constraint, or a check. The write fails and the failure is the answer.
2. **Optimistic concurrency.** A version or updated-at column, compared in the `WHERE` clause of the update, with the affected row count checked. Cheap when conflict is rare.
3. **Pessimistic locking.** `SELECT ... FOR UPDATE` inside a transaction, or an advisory lock. Correct when conflict is common and the critical section is short.
4. **Idempotency.** A durable key that makes the second execution a no-op returning the first result. The only fix that works across a network, where you cannot roll anything back.

## The seven shapes

### 1. Time-of-check to time-of-use

The canonical form:

```
row = SELECT * FROM accounts WHERE email = ?
if row is None:
    INSERT INTO accounts (email, ...) VALUES (?, ...)
```

Two requests both read nothing and both insert. The window is as small as you like and it is never zero.

**Fix: the constraint.** A unique index on `email`, and an insert that expects to fail. `INSERT ... ON CONFLICT DO NOTHING` or `ON CONFLICT DO UPDATE` in PostgreSQL, available since 9.5 (January 2016), or `INSERT ... ON DUPLICATE KEY UPDATE` in MySQL. Where the framework insists on the check-first shape, catch the unique violation and treat it as the ordinary outcome it is, rather than letting it become a 500.

The same shape wears other clothes and the fix is the same. "Only one active subscription per account" is a partial unique index on `(account_id) WHERE status = 'active'`. "No two bookings overlapping in time for one room" is an exclusion constraint on a range type. "The seat limit is ten" is a check the database performs, or a counter updated conditionally, never a count followed by an insert.

**The wrong fix to reject on sight:** wrapping the check and the insert in a transaction. Under READ COMMITTED and under snapshot isolation, both transactions read the state before either wrote, so a transaction changes nothing about this shape. The database has to be the one saying no.

### 2. Lost update

Two requests read a value, each modifies its own copy, each writes back. One update vanishes with no error anywhere.

**Fix A, optimistic.** Add a version column and make the update conditional:

```sql
UPDATE settings SET payload = ?, version = version + 1
 WHERE id = ? AND version = ?
```

Then check the affected row count. Zero means somebody else got there first, and the handler either retries against fresh state or returns a conflict to the caller. **The check on the row count is the entire mechanism**, and it is the line most often missing: an update that ignores its result has added a version column and no safety.

*Wrong when* the row is hot. Under heavy contention on one row, every writer retries, most retries fail, and throughput collapses while CPU rises. Optimistic control assumes conflict is the exception.

**Fix B, pessimistic.** `SELECT ... FOR UPDATE` inside a transaction, then modify, then commit. Serialises writers on that row.

*Wrong when* the transaction is long, and catastrophically wrong when it contains a network call. A lock held for the length of a third-party HTTP request with a thirty second timeout is a thirty second lock, and thirty seconds of queued writers exhausts the connection pool. Also wrong across processes that do not share a transaction.

**Fix C, and prefer it whenever it applies: do not read at all.** Most lost updates are arithmetic, and arithmetic belongs in the statement:

```sql
UPDATE accounts SET balance = balance - ? WHERE id = ? AND balance >= ?
```

One statement, no read, no version, no lock held across application code, and the affected row count tells you whether it succeeded. If a race can be removed by moving the decision into the `WHERE` clause, that beats both of the other fixes.

### 3. Double submit and the non-idempotent handler

The user clicks twice, the mobile client retries on a timeout, the load balancer replays after a 502. The handler charges twice.

**Fix: an idempotency key.**

What makes a good key. **Supplied by the caller**, generated when the intent is formed rather than when the request is sent, so every retry of that one intent carries the same key. Unique per intent. Opaque, high entropy, single use.

**The specific failure of hashing the request body**, which is the design people reach for because it needs no client change. It is wrong in both directions at once. Two genuinely distinct intents that happen to be byte-identical, the same customer buying the same item twice in one minute, collapse into one and the second purchase silently disappears. And any body carrying a timestamp, a nonce, a client-generated id or a field ordered non-deterministically hashes differently on retry, so the retry is treated as new and the double charge you were preventing happens anyway.

Mechanics that matter as much as the key. Insert the key with a unique constraint **before** performing the effect, in a state such as `in_progress`, so a crash mid-flight is distinguishable from a fresh request. Store the response body and the status against the key and return the stored response on replay, because a caller that retries needs the original answer, not a bare 409. Write the key and the effect in the same transaction where the effect is a database write, so you can never end with one and not the other. **Retention** is a real decision: keep keys at least as long as the longest retry window any client uses, and 24 hours is the common published figure across large payment APIs. Expiring keys in an hour while a client retries a stuck job for six is the same bug with extra steps.

### 4. Read-modify-write across a network boundary

`GET /resource`, edit, `PUT /resource`. Two clients edit from the same version and the second silently overwrites the first. This is the lost update again, with no shared transaction available to fix it.

**Fix: conditional requests.** Return an `ETag` on the `GET`, require `If-Match` on the `PUT`, and answer `412 Precondition Failed` when the tag does not match. `428 Precondition Required` (RFC 6585, 2012) is the correct answer when the client omits the header entirely, so an unconditional overwrite cannot happen by accident. The API contract has to say the header is required, because a client that can omit it will.

### 5. At-least-once delivery, which is every queue and every webhook

This is a knowledge item rather than a code smell. Message brokers, webhook senders, cron runners and retrying HTTP clients all deliver more than once under normal operation, not only under failure. A consumer that acknowledges after processing will reprocess anything it crashed part way through, and a consumer that acknowledges before processing loses messages instead. There is no third option in the delivery layer.

Even the exactly-once features are narrower than they read. FIFO queues on the major cloud brokers deduplicate within a fixed window, commonly five minutes, which is shorter than most retry schedules. Stream processors offer exactly-once semantics for state inside the stream system, which says nothing about the email your consumer sent.

**Fix: idempotency by construction.** Derive a deterministic key from the message or event id, insert it into a processed-events table with a unique constraint in the same transaction as the effect, and let the violation short-circuit the handler. Handlers must also tolerate **out of order** delivery, which is the second half people forget: apply updates conditionally on a sequence number or a timestamp, so a stale redelivery cannot overwrite newer state.

The review question is simply: if this handler runs twice with the same input, what is different afterwards? If the answer is anything, it is a defect regardless of how reliable the sender claims to be.

### 6. Deadlock from inconsistent lock ordering

Transaction A locks row 1 then row 2. Transaction B locks row 2 then row 1. Both wait. The database eventually kills one: PostgreSQL after `deadlock_timeout`, which defaults to one second, with SQLSTATE 40P01, and MySQL InnoDB immediately with error 1213, while a plain lock wait ends at `innodb_lock_wait_timeout`, which defaults to 50 seconds.

**Fix: a canonical order, written down.** Always acquire in ascending primary key order, or always in a fixed order of table names. The transfer that locks the source account then the destination account deadlocks with its own mirror image, and locking the lower id first fixes it completely.

The version that hides. A multi-row statement locks rows in whatever order the plan produces, so two concurrent `UPDATE ... WHERE id IN (...)` statements with the same set in different orders deadlock. If the code selects rows to lock, it must add `ORDER BY id` to the `SELECT ... FOR UPDATE`. Foreign keys also take locks the author did not write, and cascading updates take them in an order nobody chose.

Deadlocks are recoverable, so the second half of the fix is a retry with jitter on the deadlock error class specifically, and a metric, because a rising deadlock rate is a design signal.

### 7. Write skew, and what the isolation level does not cover

Two transactions read overlapping data, each checks an invariant that holds, each writes a different row, and the invariant is false afterwards. The classic form: two people are on call, each transaction checks that at least one other person is on call, both see one, both go off call, nobody is on call. No row was written twice. No lost update occurred. Snapshot isolation permits it.

**What each level actually buys.**

- **READ COMMITTED**, the PostgreSQL default, gives one thing: you never read uncommitted data. Each statement takes a fresh snapshot, so two reads in one transaction can differ. It protects nothing you read and then act on.
- **REPEATABLE READ**, the MySQL InnoDB default, gives one snapshot for the transaction. PostgreSQL's implementation also prevents phantom reads, which is stronger than the standard requires, and raises `could not serialize access due to concurrent update`, SQLSTATE 40001, when two transactions update the same row. It does not prevent write skew in either engine.
- **SERIALIZABLE** in PostgreSQL, implemented as serialisable snapshot isolation since 9.1 (2011), detects the dangerous read-write cycles that produce write skew and aborts one transaction with 40001. This is the only level that fixes shape 7 without changing the code.

**The cost of SERIALIZABLE is a code cost, not a performance cost, and it is the part that gets skipped.** Any transaction can be aborted at commit through no fault of its own, so every transaction needs a retry loop that **re-runs the application logic** from the start, not one that re-issues the last statement. Anything not safe to run twice, including any side effect, must be outside the transaction. And the guarantee only holds when every transaction touching that data runs at SERIALIZABLE.

**The alternative to raising the level is materialising the conflict:** give the invariant a row, and lock that row. A `shifts` row locked `FOR UPDATE` by both transactions turns write skew into ordinary contention, which is easier to reason about and cheaper than promoting every transaction in the system.

## The transaction boundary rule

**No external side effect inside an open transaction.** No card charge, no email, no webhook, no publish to a broker, no third-party call of any kind.

It fails in two directions. Forwards: the transaction rolls back after the charge succeeded, so the customer is charged for a thing that does not exist in your database, and there is no rollback available for a completed charge. Backwards: the call takes as long as its timeout, and every lock the transaction holds is held for that long, so one slow vendor turns into a connection pool exhaustion and a site-wide outage.

**Fix: the transactional outbox.** Write a row describing the intent into an outbox table in the same transaction as the state change. A separate worker reads the outbox and performs the effect, retrying until it succeeds, with an idempotency key so the retries are safe. The delivery becomes at-least-once, which shape 5 already told you how to handle. Where a framework offers a post-commit hook, that is the simpler version and it is enough for effects that may be lost.

## Cache stampede

A hot key expires. Every request in flight misses at the same moment and every one of them recomputes the value, so the origin receives the entire read volume of that key at once and often falls over, which lengthens the recomputation, which extends the herd.

Mitigations, roughly in order of how much they buy:

- **Single flight.** One computation per key per process, with the rest waiting on the same result. Across processes, a short lock in the cache, set with a create-if-absent flag and a ten second expiry, so exactly one worker recomputes and the others serve stale or wait briefly. Treat this as load shedding, never as a correctness lock: a lock in a cache can be lost.
- **Serve stale while revalidating.** Keep the old value past its logical expiry and refresh in the background. Removes the herd entirely at the price of bounded staleness.
- **Probabilistic early expiry.** Each reader independently decides to refresh slightly before expiry with a probability that rises as expiry approaches, which spreads the refresh across time. The published version is the XFetch algorithm from the 2015 paper on optimal probabilistic cache stampede prevention.
- **Jittered TTLs.** Ten thousand keys written by one batch job with an identical TTL expire together. Add a random spread of ten to twenty per cent to every TTL as a default habit.
- **Negative caching.** Cache the absence of a value too, or a miss on a key that does not exist becomes an uncached origin hit on every request.

## Decision rule: choosing the fix

- **The invariant is expressible as a constraint on stored data.** Use the constraint. Uniqueness, overlap, a bound on a value. Nothing in application code beats a rule the database refuses to break.
- **Conflict is rare and the caller can be told to retry.** Optimistic version compare-and-swap, with the row count checked.
- **Conflict is common, the critical section is short, and everything is inside one database.** Pessimistic row lock, in a transaction that contains no network call.
- **The effect crosses a network or leaves your system.** Idempotency key. Nothing else survives a retry you did not initiate.
- **The invariant spans rows that no single write touches.** SERIALIZABLE with a genuine retry loop, or materialise the conflict into a lockable row. Prefer materialising it if the codebase has no retry discipline today.
- **You cannot tell whether this code runs concurrently at all.** Assume it does, and say why you assumed: more than one application instance, a retrying client, a queue consumer with more than one worker, or a scheduler that can overlap runs. Then ask three specific questions rather than guessing: how many instances of this process run, what the isolation level is, and whether the caller retries. If the answer to all three is genuinely no concurrency, report the finding as latent, name the change that would activate it, which is nearly always scaling to two instances, and do not demand a fix now.

## The checklist over a diff

1. Does any path read a row, decide from it, and then write? That is the shape.
2. Is any uniqueness or limit enforced by a query rather than a constraint?
3. Does any update take the value that was read instead of computing from the stored value?
4. Does any conditional update ignore its affected row count?
5. If this endpoint is called twice with the same payload, what differs? If anything does, where is the idempotency key?
6. Does this handler consume anything delivered by a broker, a webhook, or a scheduler? Then it is at-least-once, and out of order.
7. Is there an HTTP call, an email, or a payment inside a transaction?
8. Does anything lock more than one row, and in a defined order?
9. Does any transaction stay open across application logic that can be slow?
10. Are 40001 and the deadlock error classes handled anywhere, and does the retry re-run the logic rather than the statement?
11. Is anything cached with a fixed TTL and a hot key, with no single-flight and no jitter?
12. Is any state held in a module-level or process-level variable that a second request could observe? In an async runtime, does an `await` sit between the check and the use?

## Worked example, compressed

An endpoint in a billing service that invites a teammate and consumes a seat.

```
seats = SELECT used_seats FROM workspaces WHERE id = ?
if seats >= plan_limit: return 402
if SELECT 1 FROM invites WHERE workspace_id=? AND email=?: return 409
BEGIN
  INSERT INTO invites (...)
  UPDATE workspaces SET used_seats = seats + 1 WHERE id = ?
  send_invite_email(...)
COMMIT
```

**Shape 1, twice.** The seat check and the duplicate-invite check are both check-then-act. Two simultaneous invites on the last seat both pass, and two simultaneous invites to the same address both insert. Fixes: a unique index on `(workspace_id, lower(email))` for the duplicate, and a conditional update for the seat, which is also the next finding.

**Shape 2.** `used_seats = seats + 1` writes back a value read outside the transaction. Two concurrent invites both write the same number and one seat is issued free. Fix: `UPDATE workspaces SET used_seats = used_seats + 1 WHERE id = ? AND used_seats < plan_limit`, then check the affected row count and return 402 on zero. This removes the read entirely and enforces the limit in the same statement.

**Shape 3.** No idempotency. A retried request after a gateway timeout sends a second invitation email and consumes a second seat.

**Transaction boundary.** `send_invite_email` is inside the transaction. If the commit fails the invitee has an email for an invite that does not exist, and if the mail provider is slow the row lock on the workspace is held for the length of that call, serialising every invite in the workspace behind it. Fix: outbox row inside the transaction, dispatch after.

**Isolation.** Nothing in the handler is safe under READ COMMITTED, and raising the isolation level would fix none of it, because both defects are check-then-act rather than write skew. Worth saying explicitly, since the reflex response to a race report is to raise the level.

**Verdict: do not ship.** Two constraint fixes, one conditional update replacing the read-modify-write, one outbox, and an idempotency key on the endpoint. No lock and no isolation change required, which is the point of selecting the fix from the shape.

## Failure modes

**Naming the hazard without naming the shape.** "This could race" tells the author nothing they did not know and gives them nowhere to go.

**Reaching for `SELECT ... FOR UPDATE` every time.** It is correct for exactly one shape. Applied to check-then-insert it locks a row that does not exist yet and fixes nothing.

**Recommending a longer or more careful check.** The gap is the defect. Any fix that leaves a gap is not a fix.

**Wrapping the race in a transaction and declaring it solved.** A transaction gives atomicity and a snapshot. Neither makes two readers see each other.

**Adding a version column and ignoring the affected row count.** The most common half-implemented optimistic lock, and it is indistinguishable from no protection at all in every test that does not run concurrently.

**Treating a queue or a webhook as exactly-once because the vendor page says so.** Read what the guarantee covers. It usually covers delivery within their system for a bounded window, not the effect your handler performs.

**Missing side effects inside transactions because they are one function call deep.** The email send is rarely written inline. Follow the call.

**Flagging every shared variable in a single-threaded runtime.** In an async runtime the yield points are where interleaving happens, and code between two yields is safe. Flagging everything trains the reader to ignore the review.

## What this skill does not do

- It does not prove a race occurs. It argues from the shape of the code, and only a race detector, a concurrent load test, or production data can confirm an interleaving actually happens.
- It does not measure contention, so it cannot tell you whether optimistic control will degrade in your traffic. That needs a number from production.
- It covers races at the level of requests, rows, and messages. Memory ordering, atomics, and lock-free structures are a different discipline and are outside it.
- It cannot see your deployment shape, isolation level, or retry policy, all three of which change the correct fix, so it asks for them rather than assuming.
- It does not fix anything. Every fix here changes behaviour under load, and each one needs a test that runs the handler concurrently before it can be believed.
