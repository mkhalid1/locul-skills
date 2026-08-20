---
name: recurring-job-runbook
description: Writes the operating contract for a scheduled, unattended job: a constants block, a source-of-truth declaration, a three-way failure classification separating failures that abort the run from those that abandon one item from those that are logged and counted, a closed list of skip reasons that are never valid, a debt record the next run repays without double-processing, a reconcile step that establishes what actually happened rather than trusting the job's own log, and a report contract with an inverted alert. Platform and language neutral, and it names no single scheduler. This skill should be used when a job is about to be put on a schedule, when a run has ended partway and somebody is deciding what the next run does, when a scheduled job has been reporting success while producing nothing, or when two instances of the same job have overlapped.
---

# Recurring job runbook

## The claim this skill is built on

A recurring unattended job is not code plus a schedule. It is a contract, and two of its clauses are almost never written down: what happens when something fails, and what the next run owes because of what happened in this one.

The obvious approach is to document the happy path thoroughly and give failure a sentence. That sentence is usually one of two, and neither is a policy. "Log the error and alert" is not a classification, because it treats a rejected credential and one malformed row as the same event. "Retry" is not a policy either, because it does not say how many times, against which operation, or what happens when the retries are exhausted at item 288 of 400.

The consequence is invisible while it accumulates. The job runs, the report is green, and the queue behind one permanently broken item never drains. Or the job aborts nightly on a row that would have been fine to skip. Or a run dies at 60 percent, nobody records what the remaining 40 percent was, and the next run starts the new day's work on top of a hole that no one discovers until a customer asks.

The deliverable below is one document, not the job itself.

## What the runbook contains, in order

Write these sections, in this order, with these names. The order matters: each is read by someone who has already read the one above it, at speed, while something is broken.

1. Constants and identifiers
2. The source-of-truth declaration
3. Pre-flight
4. Selection and per-run budget
5. Per-item gates
6. Write, then verify
7. Failure taxonomy, three classes
8. Legitimate skips, and the excuses that are never valid
9. The resumption contract
10. Reconcile
11. Decisions the job may never make alone
12. The report contract
13. Maintenance runs
14. Changelog

## Part one. Constants, and the two nobody remembers

**Every path, identifier, target set, per-run item budget, retry count, threshold and interval goes in one block at the top. No literal appears inline in any step.** The reason is not tidiness. Whoever edits this file is often not whoever wrote it, and they are editing at speed, at night, because something is broken. A threshold buried in step nine gets changed in step nine and not in step fourteen.

Two constants are almost always missing.

**A run identifier.** Generated at start, printed in the first line of every log and report, and stamped on every record the run writes. Without it, two runs' output cannot be told apart afterwards, which is exactly the situation you are in when you most need to.

**The clock.** State that the job's internal clock is UTC and that any local presentation time is a separate, named constant. A job scheduled at a local wall-clock time inside a daylight saving transition either runs twice or does not run at all, depending on the direction, and both instances of the twice case will report success. Pin the business date the run is processing as its own constant too, derived from UTC, so that a run which starts at 23:58 and finishes at 00:04 processes one day rather than two.

Keep the constants portable. Store a location as a single named constant resolved at start-up rather than as a path with separators written into the document, so the same runbook is true on a Windows host and on a Unix-like one.

## Part two. The source-of-truth declaration, in one sentence

Write this sentence into the runbook, verbatim, adapted to your systems:

> The target system is authoritative for what already exists. The tracker is authoritative for what to do next. When they disagree, the target system wins and the tracker is repaired to match it.

That one rule prevents most duplicate output: tracker drift is normal rather than exceptional, and every recovery decision needs a tie-breaker chosen in advance.

Then the corollary, which is the part people resist:

> The job's own log is authoritative for nothing.

## Part three. Pre-flight, before any state change

List the capabilities each phase needs. Load or check all of them in one pass, then report which are available and which are not. If a required capability is missing, abort with a line naming it, and change nothing. A job that starts, does three items, and then discovers it cannot reach the destination has created a partial run for no reason.

Run a liveness check on every dependency: a trivial query, a metadata fetch, an import.

**Then take a lease, not a lock.** A lock file is not safe for an unattended job: a crashed run leaves it behind forever, and the next run either blocks permanently or learns to ignore locks, which is worse. A lease is a record carrying the run identifier, a start time and an expiry, renewed while the run is alive. An expired lease with no terminal report is a crashed run, and that is a state you want to recognise later.

## Part four. Selection, and why the scheduled date is the wrong key

**Do not select work by a scheduled date.** Date gating means a missed run leaves a permanent hole, and clocks, time zones and transitions will disagree with your tracker at least twice a year.

Select by status instead: eligible items are those in a non-terminal state. Sort by priority ascending, then by identifier ascending. Both keys, always, so two runs from the same state select the same items and any failure is reproducible. Cap at the per-run budget.

**Debt owed by a previous run executes first and counts against this run's budget.** This is the rule that stops debt compounding. A half-finished item is worth more than a fresh one because most of its cost is already sunk.

**Quarantine permanently broken items.** If the same item fails the same way for a set number of consecutive runs, three is a workable default, stop retrying it, move it to a quarantined state, and name it in the report. Without this rule, one broken item sitting at the top of the sort order consumes the retry budget every night and the queue never drains, while the job completes and reports success every time.

## Part five. The failure taxonomy, three classes

This is the section the runbook exists for. Every failure the job can encounter is assigned, in advance, to exactly one of three classes.

**Class A, abort the run.** Stop. Do not process further items. Write the debt record. Report.

**Class B, abandon this item, continue the run.** The item is left in its previous state or moved to a failed state, the reason is recorded, and the run moves to the next item.

**Class C, log, count, and continue.** The item is still considered done. The failure is recorded and counted.

### The assignment test

Ask three questions in order, and stop at the first yes.

1. **Does this failure invalidate an input that every remaining item depends on?** A tracker that cannot be read, a governing configuration that cannot be loaded, a rejected credential at the authentication step, a source dataset that is empty when it must not be, an input that fails a schema check, a lease that cannot be acquired because another instance is live. **Class A.**
2. **Would continuing write something wrong rather than nothing?** A destination in an unexpected state, a partially applied migration, a clock skewed beyond the tolerance you set. **Class A**, and the more important of the two questions.
3. **Is the damage bounded to the item in hand?** The primary write for this item failed after its retries, its prerequisite does not exist, it failed validation, it exceeded the per-item timeout. **Class B.**

Everything left is **Class C**: the tracker write-back failed after the real work succeeded, a secondary cross-link failed, one variant of several failed, a metric emit failed, a notification failed.

### Two rules that stop the taxonomy being gamed

**One failing variant never licenses skipping the remaining variants.** If an item produces five outputs and the second fails, the third, fourth and fifth are still produced. This has to be written down because it is the most natural wrong inference in the whole file.

**Class C failures are counted, and a count is a Class A signal.** Set a threshold as a constant. If the same class of Class C failure affects more than a stated share of items, twenty percent is a reasonable starting point, stop the run and report it as Class A. A hundred individually survivable failures of the same kind is not a hundred small problems, it is one large one that has been classified as small a hundred times.

### Why both directions are expensive

Classify a per-item failure as Class A and one malformed row stops the entire run, every night, while the backlog behind it grows. It presents as an unreliable dependency, and the cause is a taxonomy that is too conservative.

Classify a run-level failure as Class B and the job writes wrong or empty output for every item in the window while reporting a high completion count. That is the worse error, because it looks productive: an expired credential misread as a per-item problem fails 400 items in a row and calls it a bad night.

## Part six. Legitimate skips, and the excuses that are always laziness

An unattended job permitted to decide it had a good reason not to do the work will always find one. Not by cheating, but by reasoning its way to a defensible-sounding partial result, which any competent system is good at. The defence is to convert the judgement into a lookup, because a lookup cannot be talked round.

### The evidence test

A skip is legitimate only when the work **could not** be done, never when it **was not worth** doing. Two properties, both required:

1. It names a condition external to the run, in the world rather than in the run's own preferences.
2. It carries an artefact generated at the time that a reader can check afterwards, normally a verbatim error string.

### The four legitimate skips

1. **Documented repeated failure of a named operation.** Not one attempt. A set number, against the same operation, with the verbatim error text captured each time. Use three attempts for an expensive step such as a generation or an upload, and one for a cheap per-variant call. If the errors differ between attempts, report all of them, because a changing error is diagnostic and a repeated one is not.
2. **A mid-run loss of the session or the connection.** Nothing failed on its merits. The record names the last completed step and enumerates exactly what is owed.
3. **A genuine per-invocation output ceiling.** Everything is healthy and nothing errored: the run simply could not emit more in one turn. The record states what was emitted, what remains, and the exact resumption point.
4. **A precondition only a human can supply, absent for this item.** A credential, an approval, a signature. This counts as a skip only if it is also recorded as a blocker with a named owner and a place to obtain it. Note the asymmetry: missing for the whole job it is a Class A pre-flight abort, missing for one item it is a Class B skip with an owner attached.

### The excuses that are never valid, as a closed list

These are never valid reasons to do less work than the budget:

- **"Context budget pressure."** This is a concern about an input window. It is not an output limit and it is not a failure.
- **"The response was too large."** The call succeeded. The output exists somewhere. Read it back from where it was persisted and continue.
- **"The run is taking too long."** Duration is not an error. If duration genuinely matters, it belongs in the per-run budget as a number, decided before the run started.
- **"I will do it next run,"** offered with no retry evidence and no debt record. This is not deferral. It is silence with a promise attached.

And the family test, so the list stays closed as new members are invented: anything of the form *the remaining items were lower value*, *the earlier ones were good enough*, *the tracker can be caught up later*, *this one did not feel ready*. Those are not failures. They are preferences expressed in the grammar of constraints. The test on any candidate reason is whether it names something outside the run and carries an artefact somebody else can check. If not, it is an excuse, and the work is done rather than skipped.

## Part seven. The resumption contract

**The debt record is written at the moment the work is abandoned, not reconstructed afterwards.**

The reason is mechanical. Once the run ends, the process is gone and so are the error strings, the attempt counts and the ordering. The only reconstruction available is a diff of the target system against the plan, and that diff cannot distinguish "never done" from "done but not recorded", cannot recover why, and cannot tell you whether retrying is safe. A debt record written later is a guess with a timestamp on it.

Every debt record carries:

- the run identifier that incurred it
- the item identifier
- the last completed step, named from the fixed step list rather than described
- **the remaining outputs owed, enumerated by identifier**, never summarised as "the rest"
- which of the four legitimate skips applies
- the verbatim evidence
- a UTC timestamp
- an age counter, in runs

Write it to two places: the tracker, because that is what the next run reads, and the report, because that is what a human reads.

**Repayment.** The next run clears debt before starting new work, counting it against the budget, and its report states the run identifier of the run that incurred it. If it cannot clear the debt, the record is re-emitted with the original run identifier and the age incremented, so that a debt ageing past a stated number of runs, three is a reasonable default, escalates to a person instead of quietly living forever.

**Not double-processing** is why the enumeration matters. Repayment executes only the enumerated outputs, never the whole item again, and every write is still guarded by the idempotency key from part eight. An item described as "half finished" cannot be safely resumed. An item recorded as "outputs 3, 4 and 5 of 5 remain, last completed step: upload" can.

## Part eight. Reconcile, and why the log is the least trustworthy artefact

Before acting on the tracker or on any debt, establish what actually exists by querying the target system. Compare three things: the target system's state, the tracker's state, and the previous run's report. The target system wins, the tracker is repaired to match, and the report is treated as evidence about intent only.

**Idempotency.** Every write carries a deterministic key derived from stable attributes of the item plus the identity of the output, and never from the run time or a random value. A key of the form `<item id>|<business date>|<output name>|v<version>` is repeatable, so a repeat write is detectable. Where the target supports a conditional create, an upsert on a natural key, or a unique constraint, use it, because that is the only guarantee that survives two instances running at once.

**Why the job's own log is worth less than everything else in the room:**

1. It is written by the same process whose failure you are investigating, so whatever killed the process also truncated the log.
2. The log write and the system write are not atomic together. The gap between them is precisely where partial runs live: a line saying an item was written can precede a write that never landed, and a write that landed can precede a line that never flushed.
3. Buffered output is lost on abrupt termination, so the last lines, the ones about the failure, are the ones most likely to be missing.
4. A retried step writes its line more than once, so counts taken from the log overstate the work.
5. Nothing reconciles the log against reality, which is why a job that has done nothing for three weeks can produce three weeks of clean logs.

## Part nine. The decision rule: did the previous run complete?

Run this at the top of every run, before selection.

1. **A terminal report exists, no debt records, and reconcile shows the target system matching the plan.** Complete. Start new work.
2. **A terminal report exists and carries debt records.** Partial and described. Repay the enumerated debt first, then new work up to the remaining budget.
3. **No report at all, and no lease record.** The run never started. Treat the window as unworked, reconcile, and work the queue from the top.
4. **A lease record exists and has not expired.** Another instance is live. Do not start. Exit with a line reading "overlap, skipped" and the live run's identifier. This is not a failure and must not be reported as one, or the overlap alert becomes noise.
5. **A lease exists, has expired, and there is a terminal report.** The run finished and the lease was not released. Apply branch 1 or 2 on the report.
6. **You cannot tell.** The report is missing, truncated or contradicts the tracker; or the tracker says done and the target system disagrees; or the lease expired with no terminal report; or two instances may have run concurrently.

   **Enter reconcile-only mode.** Do no new work this run. Query the target system for every item in the candidate window. Compare each against the tracker. Repair the tracker to match the target system. Write a reconciliation report listing every repaired item with both values, before and after. Emit the entire window as debt for the next run, with the reason class recorded as an unresolved prior run. Then exit successfully.

   The asymmetry that justifies doing nothing: a run that does no work costs one interval, and the work is still in the queue. A run that duplicates or overwrites costs an unbounded manual cleanup, and the cleanup is done by a person reading rows one at a time.

   **If reconcile-only mode fires on two consecutive runs, it is not transient.** Suspend the schedule and escalate. A job that cannot establish its own state is a job that should not be writing.

## Part ten. What your scheduler does and does not do for you

Missed-run behaviour is per-scheduler, off by default in several, and silently different when the job moves host. These are documented behaviours as of August 2026, and defaults change, so confirm against the version you run.

- **cron** has no concept of a missed run. If the machine was off, the run simply did not happen. On Linux, `anacron` exists precisely to cover that gap.
- **systemd timers** support `Persistent=true`, which fires the unit immediately once the machine is up again if the elapsed time was missed.
- **Windows Task Scheduler** has a per-task setting, "Run task as soon as possible after a scheduled start is missed", which is not enabled by default.
- **launchd** on macOS runs a missed calendar interval when the machine next wakes or starts.
- **Kubernetes CronJob** exposes `concurrencyPolicy` with Allow, Forbid and Replace, plus `startingDeadlineSeconds`; the controller stops starting the job and logs an error once it counts more than 100 missed schedules.
- **Scheduled workflows in hosted CI**, GitHub Actions among them, are documented to be delayed during periods of high load, and inactive repositories have their scheduled workflows disabled automatically.
- **Managed cloud schedulers** generally offer a retry policy and a delivery window, with limits varying by provider.

**The load-bearing point: do not let the runbook depend on any of it.** Catch-up behaviour differs, is often off, and disappears when someone moves the job. The debt record from part seven is the portable mechanism, and it behaves the same on every host.

## Part eleven. The report contract, and the inverted alert

One report per run, in a fixed format, emitted even when the run did nothing.

- A header line: run identifier, runbook version, schedule interval, business date, and counts for done, skipped, quarantined and owed.
- One block per item, in one of exactly three forms: **success**, with the resulting identifiers or locations; **documented failure**, with the operation, the attempt count and every verbatim error string; **owed**, with the enumerated remaining outputs, the reason class and the age.
- A closing list of what the next run will do first, and a list of anything deferred to a human with an owner's name against it.

**A report in a non-compliant format is itself a violation**, because the format is what makes the report machine-readable and comparable across runs. State that in the runbook.

**Invert the alert.** Do not alert on failure, alert on the absence of a success report within the interval plus a grace period. A failure alert has to be sent by a job alive enough to send it, which excludes the failure you care most about. And make the report carry a number a human can compare against an expectation: "0 items processed" against an expected 400 is an alert, whereas "completed" is decoration.

## Part twelve. Maintenance runs, forbidden decisions, and the changelog

**Every Nth run, do no new work.** Fourteen is a workable interval for a daily job. Instead, re-check the oldest outputs against current reality and repair what has drifted, because unattended output rots and nothing else is looking at it.

Use the same run to **test the alert channel**: emit a deliberate test alert and require a human acknowledgement within a stated window. An alert channel nobody reads is undetectable by any other means, since its symptom is silence and silence is also what success looks like.

**Decisions the job may never make alone.** Enumerate them: touching anything outside its item set, changing price, legal or safety-relevant content, deleting rather than marking, inventing a category not in the governing document, skipping a mandatory step, widening its own budget, changing its own schedule, or disabling its own alerts. Close with the standing instruction: when tempted, skip the item and flag it rather than improvise.

**Changelog at the bottom, versioned, with dates.** Every report names the runbook version it executed, so a behaviour change can be tied to an edit. A runbook edited without a changelog quietly loses rules, and nobody notices which one went missing.

## Worked example, compressed

**The job.** A nightly usage export for a workspace analytics service. At 02:00 UTC it exports the previous day's usage rows for each of about 400 customer accounts, uploads one file per account to that account's storage destination, and records a row in an export tracker. Per-run budget: all eligible accounts. Retries: three for the upload, one for the tracker write-back.

**Run 41.** Lease taken. Reconcile passes. Work starts.

- **Account 137**: the destination returns a rejection on the account's own credential, three times, the same message each time. Bounded to this item, so **Class B**. The item is abandoned, the three verbatim errors are recorded. It has now failed identically on three consecutive runs, so it is **quarantined** and named in the report rather than retried again tomorrow.
- **Account 210**: the file uploads and verifies, then the tracker write-back fails. The real work is done, so this is **Class C**: counted, and a debt record is written for the write-back alone.
- **Account 288**: the source database refuses the connection. Every remaining account depends on that connection, so question one of the assignment test answers yes. **Class A, abort.** The run enumerates the 112 remaining accounts by identifier, writes one debt record per account naming the last completed step and the outputs owed, and reports.

Report: 285 exported, 1 quarantined, 1 write-back owed, 112 owed under documented repeated failure with the verbatim error.

**Run 42.** Reads the debt first. Reconcile before repayment finds four of the 112 already carrying a file stamped with yesterday's business date: the abort landed between the upload and the tracker write for those four. Their idempotency keys, of the form `<account id>|<business date>|usage-export|v3`, already exist at the destination, so the writes are refused rather than repeated, and the tracker is repaired to match. The remaining 108 are exported, the owed write-back is completed, and only then does the run start the new day's work with what is left of the budget.

**Run 43.** No report from run 42 at all, and an expired lease. Branch 6. **Reconcile-only mode**: no new work, the full window is compared against the destination, three tracker rows are repaired, a reconciliation report lists each with its before and after value, and the whole window is emitted as debt. Run 44 starts from a tracker that has been checked rather than assumed.

**Verdict: the design that mattered was not the retry count.** It was two things. The abort at account 288 enumerated 112 accounts by identifier at the moment it aborted, so run 42 knew exactly what it owed rather than having to guess from a diff. And the idempotency key was derived from the business date rather than the run time, so the four already-uploaded files were recognised as duplicates instead of being sent a second time.

Without the debt record, run 42 exports only the new day and 112 accounts carry a permanent one-day hole that surfaces weeks later as a customer question. With a run-time-derived key, the four accounts get a second file each and somebody spends an afternoon working out which one is real.

## Failure modes

**The green run that does nothing.** The job has been failing for weeks while reporting success. From the outside: reports arrive on time, the exit status is clean, and the counts either drift down slowly or were never in the report at all. The usual mechanism is a selection query returning zero rows, and zero is not an error. Discovered when someone asks where the data is.

**The alert into an empty room.** Alerts route to a channel nobody reads: a rotated on-call that no longer exists, a muted channel, a mailbox belonging to someone who left. From the outside it looks like an unusually reliable job. The only detection is a scheduled test alert requiring an acknowledgement.

**The retry storm that is indistinguishable from an outage.** A transient error triggers immediate retries across every item at once. Your own job now saturates the dependency, so the errors become real and are caused by you. From the outside it looks exactly like a provider outage, and the instinctive response, retry again, makes it worse. Bounded attempts, exponential backoff with jitter, a run-level cap on total attempts and a stop threshold on repeated error classes are what prevent it.

**Overlapping instances.** A run whose duration drifts past its interval overlaps the next one. Both select the same items, both write, and the tracker records whichever finished last. It presents as duplicated output near an interval boundary and a self-contradicting tracker rather than as an error, which is why it runs for weeks.

**Debt reconstructed after the fact.** Nobody recorded what was skipped at the time, so the next run diffs the target system against the plan. That diff cannot separate "never done" from "done but not recorded", carries no error text and has no ordering. It presents as slow duplication and as work redone for reasons nobody can explain.

**The item that eats every run.** One permanently broken item sits at the top of the sort order and consumes the retry budget nightly. The job runs, completes, reports success and never progresses. Quarantine after N consecutive identical failures is the fix, and its absence is invisible in every individual report.

**Abort on a per-item failure.** The taxonomy is wrong in the conservative direction. One malformed row stops the whole run, night after night, and the backlog behind it grows. The team concludes the dependency is unreliable.

**Continue on a run-level failure.** The taxonomy is wrong in the other direction. An expired credential or an empty source is treated as per-item, and the job writes empty or wrong output for the entire window while reporting a large completion count. The worst of the set, because it is productive-looking and the output is downstream before anyone checks.

**Daylight saving duplication.** The job is scheduled at a local wall-clock time inside a transition. It runs twice, or not at all, and in the twice case both instances report success and neither mentions the other.

**The runbook edited with no changelog.** A rule is removed to get past one incident and never restored. Weeks later the failure it existed to prevent returns, and nothing connects the two events because nothing records that the rule was ever there.

## What this skill does not do

- It writes a document, not an implementation. It does not configure a scheduler, hold a lease, implement backoff, or enforce a unique constraint, and a rule with no enforcement behind it is a request.
- It cannot see whether the target system supports a conditional create, an upsert on a natural key or a uniqueness constraint. Without one of those, the idempotency section is a convention rather than a guarantee, and a database constraint written by an engineer is the real defence.
- It is not observability. Metrics, tracing, log aggregation, dashboards and paging rotas are a separate discipline with better tools, and the inverted alert here is a specification for something else to implement.
- Its retry counts, thresholds, quarantine limits and maintenance interval are starting defaults with no evidence behind them. Replace each one with a number measured against your own dependency's error behaviour.
- It cannot fix a job whose real problem is upstream data quality. It will make that job resumable and auditable, which means the bad data now arrives on schedule with a clean report attached.
- It has no view on whether the job is worth running. Applied to the wrong work, it makes the wrong work reliable.
