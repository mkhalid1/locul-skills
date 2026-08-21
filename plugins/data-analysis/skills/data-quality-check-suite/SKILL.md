---
name: data-quality-check-suite
description: Builds a data quality check suite as a specification: one row per check carrying the class, the target, the rule, the threshold and its provenance, the severity, the action on failure and a named owner, plus the escalation policy. Covers ten check classes with what each one silently misses, the method for deriving a threshold from observed history instead of intuition, and the BLOCK, WARN, LOG ladder with the rule for which failures earn a block. Output is implementable in dbt, Great Expectations, Deequ or plain SQL, and it executes nothing itself. This skill should be used when a table or a pipeline needs checks that run on every load, or when an existing suite is being ignored, muted or rewritten.
---

# Data quality check suite

## The claim this skill is built on

The output is a suite specification: a table with one row per check, plus one escalation policy underneath it. Each row carries eight fields, and the last three are the ones normally missing.

| Field | What goes in it |
| --- | --- |
| ID | A stable identifier you can put in an alert message |
| Class | One of the ten classes below |
| Target | Table, column, or the join between two tables |
| Rule | The assertion, written so it can be transcribed into SQL |
| Threshold and provenance | The number, and where it came from: derived from N days of history, a vendor contract, a regulatory limit, or a placeholder pending history |
| Severity | BLOCK, WARN or LOG |
| Action | What happens on failure, naming a person or a rota |
| Position | Source, post-transform or serving layer |

The obvious approach is to sit down with the schema and write every assertion you can think of. It produces a large suite in one afternoon, and three things are wrong with it. Most of the assertions are true by construction, so they can never fire: a not-null check on a column the DDL already declares NOT NULL is a test of the database, not of the data. Several are too tight, because they were set from intuition or from one week of observation. And none of them has an owner, so the first time one fires at an inconvenient hour it gets acknowledged by whoever is nearest and never looked at again. Within a quarter the channel is muted or an inbox rule is deleting it, and the next real incident is found by a person downstream.

The vocabulary is not the difficult part and never was. Great Expectations ships roughly 62 expect_* expectation classes in its core library. dbt ships exactly four built-in generic tests: unique, not_null, accepted_values and relationships. Deequ's constraint vocabulary includes isComplete, isUnique, hasUniqueness, isInRange, hasSize, hasMin, hasMax, hasMean, hasStandardDeviation and hasNoAnomalies. Anyone can enumerate checks. The three hard questions are which small set survives being paged on, what number goes in the threshold column, and what happens when it fires.

For coverage prompting, the six primary dimensions published by DAMA UK are a useful list to read down: completeness, uniqueness, timeliness, validity, accuracy and consistency. Read it as a prompt rather than a standard, because there is still no consensus standardised list of quality dimensions across the field, and treating any one list as settled is how a suite acquires checks that exist to fill a category.

## The ten check classes, and what each one misses

Each class catches a shape of breakage and is blind to another. The blindness is the part worth writing down.

**1. Freshness.** Did the data arrive, and how old is the newest record. Catches the pipeline that did not run, the upstream job that failed silently, the credential that expired. Misses everything about the contents: a load that arrives on time with the wrong data passes. In dbt this is configured as `warn_after` and `error_after`, each with a count and a period. The documentation's worked example uses 12 hours and 24 hours at source level, which is illustrative rather than a default. Note the trap: freshness is skipped entirely unless `loaded_at_field` is set, so an unconfigured source reports no failure rather than a failure.

**2. Volume.** How many rows arrived, against what you expect. Catches the partial file, the duplicated load, the filter that was accidentally left in a query. Misses any change in the shape of the rows: the same count with every value nulled passes.

**3. Schema and type.** Are the expected columns present, with the expected types, and has anything been added. Catches the vendor who renamed a field, the migration applied upstream on a Friday, the type widening that silently truncates. Misses semantic change under a stable schema, which is the more common vendor behaviour: the field is still called status and still a string, and the strings mean something different from Tuesday.

**4. Nullability.** Is the column populated where it must be. Catches the join that failed to match, the optional field that became mandatory, the parser that gave up on a bad row. Misses the null that arrives as a sentinel: an empty string, the text "NULL", a zero, or 1970-01-01 all pass a not-null check. Add the sentinel values to the check explicitly or it is not a nullability check, it is a syntax check.

**5. Uniqueness.** Is the grain what you say it is. Catches the double-run, the retried load, the join that fanned out. Misses the duplicate that differs in one field, which is what a retry with a new surrogate key produces. Test uniqueness on the business key, not on the surrogate.

**6. Referential integrity.** Does every foreign key resolve. Catches the late-arriving dimension, the deleted parent, the partial backfill. Misses orphans in the other direction: a parent with no children is invisible to this check, and it is what a failed child load looks like.

**7. Accepted values and ranges.** Is the value in the permitted set or interval. Catches the new enum member nobody told you about, the negative quantity, the amount in the wrong currency unit. Misses the wrong value that is inside the range, which is the single most expensive class of defect and is not machine-checkable without a reference source.

**8. Distribution drift.** Has the shape of a column moved relative to a reference period. Catches gradual instrumentation decay, a changed default, a segment that stopped reporting. Misses causality entirely: drift tells you the distribution moved, never whether that is a bug or a good quarter. Real behaviour drifts, so this class produces the highest false alarm rate of the ten and belongs at WARN or LOG until it has proved itself.

**9. Cross-field consistency.** Do fields agree with each other: does the total equal the sum of the parts, is the end date after the start date, does the tax fall in a plausible ratio to the net. Catches transformation bugs that no single-column check can see. Misses anything that is internally consistent and externally wrong, which is what a unit error looks like.

**10. Reconciliation against an external total.** Does your count or sum match the source system, the vendor dashboard or the finance ledger. This is the only class in the list that can detect a wrong-but-plausible value, because it is the only one with an independent reference. Misses anything the reference also gets wrong, and it is the most expensive check to build and to keep alive.

## Deriving a threshold instead of choosing one

This is the part that decides whether the suite is alive in six months.

A hard equality is brittle and a wide band catches nothing. Between them sits a derived band, and the derivation has five rules.

**Derive from observed history, not intuition.** Pull the last 90 days of loads for the target and look at the actual distribution of row counts, nulls, and arrival times. If you cannot pull that history, you cannot set a band, and the correct row for now is a floor check with a note.

**Make volume relative, never absolute.** A band of plus or minus 20 per cent against the same weekday's recent loads survives growth. An absolute band of 40,000 to 60,000 rows is wrong the month the business grows, and its failure mode is that somebody widens it rather than replacing it.

**Stratify by the weekly cycle.** Most operational data has a strong day-of-week pattern, so a naive daily band derived from all days fires every Sunday, and a suite that fires every Sunday teaches its audience to ignore Sunday. Compare each Monday against recent Mondays. Where a month-end or a billing date creates its own spike, exclude those dates from the derivation and give them their own row.

**Replay before shipping.** Run the proposed threshold backwards over the history you already hold and count how many times it would have fired. If it would have fired repeatedly on days where nothing was wrong, the number is wrong and no amount of explaining will make the alert credible. This step costs one query and it is the step that is always skipped.

**Start loose, then tighten on a date.** Set the initial band so that it would not have fired at all on the history you replayed, ship it, and write a date in the row for the first tightening. A suite that fires in its first week gets disabled in its second. A suite that stays silent for two weeks and then fires once earns the attention needed to tune it.

Two notes on borrowed numbers. Published defaults exist and they disagree, because they were derived from different data: Evidently's drift defaults pivot at 1,000 rows, using Kolmogorov-Smirnov at 0.05 for numeric columns below that count and normed Wasserstein at 0.1 above it, with Jensen-Shannon distance at 0.1 for categorical columns above 1,000 rows, and a dataset-level `drift_share` default of 0.5 meaning the dataset is flagged when at least half its columns drift. TensorFlow Data Validation, by contrast, publishes no default drift thresholds and Google's documentation states the user must set them. dbt publishes no universal numeric default threshold either. Deequ's anomaly example uses 3 standard deviations for a warning and 4 for an error, which is a reasonable starting shape and a poor one for a seasonal series, since a standard deviation computed across a strong weekly cycle is wide enough to hide a real drop.

Second, use failure-count thresholds where the framework offers them. In dbt, severity is warn or error, and `error_if` and `warn_if` set the failure count at which each fires, so a check can tolerate three bad rows and escalate at thirty. Turn on `store_failures` for anything you expect to triage, because a check that reports a count without the offending rows generates a second query every time it fires.

## Severity, action and escalation

Three severities, and nothing else. More severities means the middle ones get ignored.

- **BLOCK.** The pipeline stops. Downstream consumers keep yesterday's data. Somebody is notified immediately.
- **WARN.** The data flows. A notification goes to a named owner within the working day, and where there is a dashboard it carries a visible flag.
- **LOG.** The result is recorded and nobody is notified. This is where new checks live while their thresholds are still being derived, and where drift checks live indefinitely.

The rule for BLOCK: a check earns it when a downstream consumer makes an irreversible decision from the data, and only then. Irreversible means money moves, a message is sent, stock is committed, or a model retrains on it. Everything else is a WARN.

State the trade plainly, because it is not obvious. Blocking is not automatically safer. A blocked pipeline leaves consumers reading yesterday's numbers, and if the serving layer does not display its own staleness the block has converted a loud failure into a silent one. So every BLOCK row in the suite requires a paired staleness indicator on the serving layer. If you cannot show staleness, downgrade the check to WARN, because at least a warning arrives with a timestamp attached.

The escalation policy sits below the table and answers four questions in writing. Who is notified for each severity, by which channel. What the acknowledgement window is. What happens when nobody acknowledges, which needs a named second person and not a group address. And the demotion rule: any check that fires three times without a genuine data defect behind it is automatically demoted to LOG and returned for re-derivation, with the date recorded in the row. That last rule is what stops the suite decaying into noise, because it removes the option of leaving a bad check running while intending to look at it.

## Where the check runs

The same rule, checked in three positions, tells you three different things.

**At the source, before transformation.** Catches vendor and ingestion problems at the point of entry, so the alert names the culprit. Cannot see anything your own transformations do. Freshness, volume, schema and accepted values belong here.

**After transformation.** Catches your own logic: the fan-out join, the filter that dropped a segment, the unit conversion applied twice. Uniqueness, referential integrity and cross-field consistency belong here.

**At the serving layer.** Catches everything, including problems introduced by the final aggregation or the caching, and it is the only position that sees what the consumer sees. The rule to remember: a check at the serving layer tells you the problem exists but not where it entered. With ten upstream steps, that is a full day of manual bisection, which is why serving-layer checks are for reconciliation and for the things you cannot check earlier, never as the whole suite.

## Decision rule: should this failing check block?

- **A downstream consumer makes an irreversible decision from this data.** BLOCK, and pair it with a staleness indicator on the serving layer.
- **Consumers only read dashboards and reports.** WARN, and annotate the dashboard. Do not block. A blank chart generates more incorrect conclusions than a flagged one, because a reader who sees no data assumes the metric is zero or assumes the tool is broken, and both are guesses made without a timestamp.
- **You cannot tell who consumes it.** WARN, and spend the next iteration finding out: read the query logs on the table, list the dashboard subscribers, and check which downstream tables reference it. Blocking an unknown consumer is how a quality programme loses its mandate in week one, and the political cost of one wrongly blocked pipeline is larger than the cost of the incident that prompted it.

## Worked example

A mid-size logistics company ingests a daily shipment export from a carrier into a warehouse table, `shipments_daily`. Roughly 40,000 to 70,000 rows per weekday, near zero on Sundays, a spike on the last working day of each month. Consumers: an operations dashboard, and a billing job that issues invoices on the fifth of the month. Everything here is invented.

**History pulled.** 90 days of loads. Weekday counts sit between 41,200 and 68,900. Sunday counts sit between 0 and 3,100. Month-end weekdays run about 1.6 times the ordinary weekday level. Arrival time is between 02:10 and 04:55 local, with two loads past 06:00 in 90 days.

**Three checks, with derivation shown.**

1. **Freshness, source, BLOCK.** Newest record no older than 8 hours at 09:00. Derived: the latest observed arrival in 90 days was 06:20, so an 8 hour window at 09:00 leaves roughly three hours of margin over the worst arrival seen. Replayed over history it fires zero times. It blocks because the billing job is irreversible, and the dashboard carries a last-updated stamp so the staleness is visible.

2. **Volume, source, WARN.** Row count within minus 35 to plus 60 per cent of the trailing four same-weekday loads, with month-end excluded and given its own row. Derived from the observed same-weekday variation, then widened deliberately for the first month. Replayed, it fires twice in 90 days, both on a public holiday, so the holiday calendar is added as an exclusion and it then fires zero times.

3. **Uniqueness, post-transform, BLOCK.** `tracking_number` plus `carrier_scan_date` unique. No threshold to derive: a duplicate at the declared grain is a defect at one row. This is the check that would have caught the class of incident this pipeline actually suffers, which is a retried load appending rather than replacing, doubling every row and inflating the invoice run. Note that volume alone would also have flagged it, but volume flags it as a WARN while uniqueness blocks the billing job, and the difference between those two is an invoice that goes out wrong.

**One check deliberately not written.** Average parcel weight within 2 standard deviations. It was proposed and rejected for now: the series has a strong seasonal component, the promotional period shifts the mix towards small parcels, and replaying a 2 standard deviation band over the 90 days fires eleven times, every one of them a real change in the business. It is written into the suite at LOG severity with a note to revisit after two full seasonal cycles, so the history accrues without anyone being notified.

**Verdict.** Nine candidate checks became three live rows, one LOG row and five rejected, of which three were rejected as true by construction because the columns are already NOT NULL in the DDL with no sentinel values observed. Two checks block, one warns, and the billing job is the only reason anything blocks at all. The escalation policy names one owner in operations for the WARN and the on-call data rota for the two BLOCKs, with a demotion rule attached to the volume band.

## Failure modes

**Alert fatigue.** The channel has an inbox rule deleting it, or a rota member acknowledges every alert within seconds of arrival without opening anything. The tell is that acknowledgement time is uniformly short and no ticket ever follows.

**Green suite.** Every check passes, always, and the team feels covered. Look at the checks: they assert things the schema already guarantees. A suite that has never fired has not proved the data is clean, it has proved nothing is being tested.

**Threshold superstition.** A number in the threshold column that nobody can source. Ask where it came from and the answer is that it seemed reasonable, or that it came from a tutorial. Both mean the threshold describes somebody else's data.

**Seasonality blindness.** The alert fires on the same day every week, or on the same date every month, and the team has learned to skip it. Every subsequent alert on that day is now invisible, including the real one.

**Ownerless alert.** The alert names a table but no person. It is seen by everybody and actioned by nobody, and the diffusion is worse in a large channel than a small one.

**Late detection.** All the checks sit at the serving layer, so the alert says the number is wrong and cannot say where it went wrong. Symptom: every incident starts with an hour of bisecting upstream steps by hand.

**Silent skip.** The check ran and passed because it ran on nothing. A uniqueness check on an empty table passes. A freshness check with no `loaded_at_field` configured in dbt is skipped rather than failed. The fix is a precondition: the volume check runs first, and a zero-row load fails the suite rather than satisfying it.

**Coverage illusion.** Forty checks on one table, none on the join between two. The counting metric was checks written rather than paths covered, and the incident arrives through the uncovered join.

**Blocking cascade.** One over-eager BLOCK stops the pipeline, downstream tables go stale, their own freshness checks fire, and the channel fills with twenty alerts describing one cause. Symptom: an incident whose alert count is proportional to the size of the dependency graph rather than to the size of the problem.

**Stale green.** The pipeline is blocked, the dashboard is serving yesterday's numbers with today's date on it, and every check is passing because nothing new arrived to fail. This is the specific failure that makes blocking not automatically safer.

## What this skill does not do

- It does not fix the data. Everything here detects, and repairs belong to a cleaning pass with its own ordering rules and its own irreversible steps.
- It cannot tell you the business meaning of a drift. A distribution moved is a fact. Whether that is a bug, a pricing change or a good quarter is a question for the person who owns the process, and no threshold answers it.
- It is not lineage. Without a lineage graph, a failing check tells you where the problem surfaced, not where it entered, and the gap between those two is the length of your pipeline.
- It does not replace a framework. A suite written in prose and never transcribed into dbt, Great Expectations, Deequ or scheduled SQL runs zero times, and the specification is worth exactly nothing until something executes it.
- It will not catch a value that is wrong but plausible, which is the most expensive class. Only reconciliation against an independent reference reaches it. ISO 8000-8:2015 names three levels of data quality, syntactic, semantic and pragmatic, and states that pragmatic quality requires interaction with the users who validate the data, which is the formal version of the same limit: automated monitoring reaches the first two levels and stops.
- It says nothing about the cost of running the checks, the retention of their results, or the compute bill of a full-table scan on every load.
