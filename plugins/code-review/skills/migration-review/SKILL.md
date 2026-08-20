---
name: migration-review
description: Reviews a database schema migration for the lock each statement takes and how long it holds it, rather than for syntax. Classifies every statement as metadata-only, scanning, or rewriting, checks the deploy order of the migration against the application code that depends on it, checks backfills for batching and replica lag, and checks whether the change can be undone. Covers PostgreSQL and MySQL with InnoDB, with the version boundaries attached. This skill should be used when reviewing any pull request that adds or edits a migration file, or when planning a schema change on a table large enough that locking it matters.
---

# Database migration review

## The claim this skill is built on

Migrations are reviewed as code and they fail as operations. The reviewer reads the SQL, confirms it says what the ticket asked for, and approves it. Then it runs against a table with 80 million rows and the service is down for eleven minutes.

The reason the code review misses it is that nothing in the text of the statement tells you what it costs. `ALTER TABLE orders ADD COLUMN currency text` and `ALTER TABLE orders ALTER COLUMN id TYPE bigint` are the same length, take the same lock, and differ by about eleven minutes. The cost lives in the engine's implementation and in the row count, and neither of those is in the diff.

So the review has to do one thing first, before it looks at anything else: put every statement into a lock class. Everything after that follows from the class.

Three classes:

1. **Metadata-only.** The catalogue changes, no rows are touched. Cost is independent of table size.
2. **Scanning.** Every row is read to verify something. Cost is linear in table size, and the lock is held for all of it.
3. **Rewriting.** Every row is written to a new copy of the table and every index is rebuilt. Cost is linear in table size plus disk, and the lock is held for all of it.

## PostgreSQL, by class

Nearly every `ALTER TABLE` form takes ACCESS EXCLUSIVE, which conflicts with everything including plain `SELECT`. The exceptions are worth knowing because they are the escape hatches: `VALIDATE CONSTRAINT` takes SHARE UPDATE EXCLUSIVE, `ADD FOREIGN KEY` takes SHARE ROW EXCLUSIVE on both tables (since PostgreSQL 9.5), and the planner-hint forms such as `SET STATISTICS` take SHARE UPDATE EXCLUSIVE. Everything else is ACCESS EXCLUSIVE and the only question is duration.

**Metadata-only.** `ADD COLUMN` with no default. `ADD COLUMN` with a non-volatile default, since **PostgreSQL 11 (October 2018)**, which stores the default in the catalogue and materialises it on read. Before 11 this rewrote the table, which is why so much advice still says never to add a column with a default. `DROP COLUMN`, which marks the attribute dropped and leaves the bytes in place, so no space comes back. `RENAME COLUMN` and `RENAME TABLE`. `DROP NOT NULL`. `SET DEFAULT`. Widening `varchar(n)` to a larger `n` or to `text`, since PostgreSQL 9.2.

**Scanning.** `SET NOT NULL` reads every row to prove there are no nulls, holding ACCESS EXCLUSIVE throughout. Since **PostgreSQL 12 (October 2019)** it can skip the scan if a valid `CHECK (col IS NOT NULL)` constraint already exists, which is the whole reason for the two-step trick below. `ADD CONSTRAINT ... CHECK` without `NOT VALID` scans. `ADD PRIMARY KEY` or `ADD UNIQUE` without `USING INDEX` builds the index under the exclusive lock.

**Rewriting.** `ALTER COLUMN TYPE` in the general case, including the very common `int` to `bigint`, which rewrites the table and every index and needs room for a second copy on disk. One documented exception: converting `timestamp` to `timestamptz` avoids the rewrite from PostgreSQL 12 onward when the session time zone is UTC. `ADD COLUMN` with a volatile default such as `clock_timestamp()` or `random()`. `SET LOGGED` and `SET UNLOGGED`.

**The two-step tricks, which are the point of the class table.**

To add a not-null column without a scan: add the column nullable, backfill in batches, add `ALTER TABLE t ADD CONSTRAINT t_c_not_null CHECK (c IS NOT NULL) NOT VALID` (instant, exclusive lock held for a moment), then `ALTER TABLE t VALIDATE CONSTRAINT t_c_not_null` (a scan, but under SHARE UPDATE EXCLUSIVE, so reads and writes continue), then on PostgreSQL 12 or later `SET NOT NULL` sees the valid constraint and skips its own scan.

To add an index: `CREATE INDEX CONCURRENTLY`. It takes SHARE UPDATE EXCLUSIVE, makes two passes over the table, and waits for older transactions to finish between them. Three things to check in review. It cannot run inside a transaction block, so the migration file must opt out of the framework's automatic wrapping. **If it fails, it leaves an invalid index behind**, which is still maintained on every insert and update and is never used by a query, so the failure is silent and permanent until someone runs `DROP INDEX CONCURRENTLY`. And a unique index built this way fails on the first duplicate, leaving exactly that mess. A migration that runs `CREATE INDEX CONCURRENTLY` without a documented recovery step for the invalid-index case is incomplete.

To add a unique constraint: `CREATE UNIQUE INDEX CONCURRENTLY`, then `ALTER TABLE ... ADD CONSTRAINT ... UNIQUE USING INDEX`, which adopts the existing index instead of building one.

**The lock queue, which causes more outages than any statement in the list.** PostgreSQL grants conflicting lock requests broadly in order of arrival. A pending ACCESS EXCLUSIVE request therefore blocks every request behind it, including ordinary `SELECT`, which needs only ACCESS SHARE. So a genuinely instant `ADD COLUMN`, queued behind a twenty-minute analytics query, takes reads on that table offline for twenty minutes. The statement was innocent. The queue was not.

The mitigation is mechanical and belongs in the migration file: `SET lock_timeout = '2s'` in the same session as the DDL, wrapped in a retry loop with backoff, so the statement gives up and tries again rather than parking at the head of the queue. `statement_timeout` bounds the scanning and rewriting classes separately. Both must be set in the same transaction as the statement, which several migration frameworks make awkward, and that awkwardness is the reason it is usually missing. Before running anything on a busy table, check `pg_stat_activity` for long transactions and for sessions idle in transaction, because those are what the queue will form behind.

## MySQL with InnoDB, by algorithm

MySQL exposes the class directly, which is the good news, and picks one silently when you do not, which is the bad news.

**ALGORITHM=INSTANT.** Introduced in **MySQL 8.0.12 (July 2018)** for adding a column at the end of the row. Extended in **MySQL 8.0.29 (April 2022)** to adding a column at any position and to dropping a column. Also instant: setting or dropping a column default, adding or dropping a virtual generated column, renaming a table, and appending values to the end of an `ENUM` where the storage size does not change. The limit that surprises people, and the unit in it is the part that is usually reported wrongly: a table supports a bounded number of **row versions**, not of columns, and a new row version is created **once per `ALTER TABLE` statement** regardless of how many columns that statement touches. Adding five columns in one statement therefore costs one version, and adding them in five statements costs five. The ceiling is **64 in MySQL 8.0 and 8.4, raised to 255 in MySQL 9.0**, and hitting it raises `ERROR 4092 (HY000): Maximum row versions reached`, at which point only a full table rebuild resets it. `ROW_FORMAT=COMPRESSED` tables are not eligible for instant DDL at all.

**ALGORITHM=INPLACE.** Adding or dropping a secondary index, renaming a column, adding and dropping foreign keys, changing a column from nullable to `NOT NULL`. Note that in place is not the same as no rebuild: several in-place operations still rebuild the table, they simply do it while permitting concurrent reads and writes.

**ALGORITHM=COPY.** Changing a column data type, dropping a primary key, changing the character set. Blocks writes for the duration and needs a full second copy of the table on disk. This is the class that needs an external tool: gh-ost, which replays the binary log and requires row-based logging, or pt-online-schema-change, which uses triggers and therefore adds overhead to every write on the table while it runs.

**The habit to enforce in review: state the algorithm.** `ALTER TABLE t ADD COLUMN c INT, ALGORITHM=INSTANT` fails loudly if the server cannot do it instantly. The same statement without the clause silently falls back to a copy. One clause converts a production incident into a failed migration, and it is visible in a diff.

**And the MySQL equivalent of the lock queue.** Every online DDL still takes a brief exclusive metadata lock at the start and the end. `lock_wait_timeout` governs how long it waits for that lock and **its default is 31536000 seconds, which is a year**. Left at the default, a metadata lock request behind one long transaction will wait effectively forever with the entire application queued behind it. Set it to something in the range of 5 to 30 seconds and retry.

MariaDB's instant DDL rules diverge from MySQL's and were introduced on a different schedule, so do not carry these version numbers across.

## Expand and contract, in six phases

The pattern exists because during a rolling deploy both the old and the new version of the application are live at once, against one database. Every phase below is a separate deploy, and the ordering is the part people get wrong.

1. **Expand the schema.** Add the new column, table, or index. Nullable, no constraint, no default that the old code would be confused by. Deploy this alone, with no application change.
2. **Dual write.** Deploy code that writes both the old and the new location and still reads the old one. Now every new row is correct in both places.
3. **Backfill.** Batch through the historical rows. This is a job, not a migration, and it must be resumable.
4. **Read from the new location.** Deploy code that reads the new one, ideally with a comparison or a fallback for a period, and keep writing both.
5. **Stop writing the old location.** Deploy code that only touches the new one. Nothing now reads or writes the old column, but it is still there.
6. **Contract.** Drop the old column, in a later release.

**The ordering rule, stated once:** additive schema changes ship before the code that needs them, destructive ones ship at least one full release after the code that stopped needing them, and neither ever ships in the same deploy as that code. The failure this prevents is specific. If the migration and the code go out together, then for the minutes of a rolling deploy the old application version is running against the new schema. Old code that does `SELECT *` against a table that just lost a column crashes. Old code that inserts without the new not-null column crashes. And if you have to roll the code back, the schema does not roll back with it.

## Backfills

Never one statement. A single `UPDATE` over a large table fails in four ways at once. In PostgreSQL, every updated row leaves a dead tuple, so the table can double on disk and stay there, while the long transaction holds back the cleanup horizon and stops autovacuum reclaiming anything anywhere in the database. In MySQL, the undo log grows for the length of the transaction. Replicas receive the change as one lump and can fall minutes behind, so read traffic goes stale. Every touched row is locked for the whole duration. And if it fails at ninety per cent it rolls back to zero per cent, and you pay the cost again.

The discipline instead:

- **Batch by primary key range, not by `OFFSET`.** `OFFSET 500000` reads and discards half a million rows every time. Keep the last processed id and use `WHERE id > ? ORDER BY id LIMIT ?`.
- **1,000 to 10,000 rows per batch** as a starting range, one transaction each, tuned down if the batch takes longer than a second or two.
- **Sleep between batches**, on the order of 50 to 200 milliseconds, or better, throttle on a measured signal.
- **Throttle on replica lag.** Fix a pause threshold and a resume threshold in advance, for example pause above 10 seconds of replay lag and resume below 2, and check it between batches. Otherwise the backfill is invisible on the primary and fatal on the replicas that serve reads.
- **Resumable and idempotent.** Record progress durably. The job will be killed at some point, and restarting it from zero on a table that takes six hours is not an option.
- **Run it outside the migration.** A migration framework that wraps each file in a transaction will hold that transaction open for the length of the backfill, which reintroduces every problem above.

## Rollback, and the asymmetry nobody plans for

A forward, additive migration is reversible: drop what you added and the schema is where it was. The data written into the new column since it shipped is gone, which is usually acceptable because nothing depended on it yet.

A destructive migration is not reversible in any meaningful sense. `DROP COLUMN` in PostgreSQL is metadata-only, which is exactly why it feels harmless, but the values are unreachable from that moment. The down migration that recreates the column recreates the shape and nothing else. Recovering the values means a restore from backup, and a restore is an incident.

Two rules follow.

**Never drop a column in the same release that stops writing to it.** Leave a full release cycle, at minimum, so that the version of the application which still reads the column is definitely no longer running anywhere and so that the previous release remains a valid rollback target. If you cannot roll back to last week's build without a schema restore, you do not have a rollback.

**Confirm nothing reads it before you drop it**, rather than assuming. Query logs, statement statistics, or a temporary trigger that records access are all better evidence than a search of the codebase, because the codebase does not include the analytics job, the admin tool, or the reporting query somebody saved.

## The decision rule

Classify first, then size. Table size only changes the answer for two of the three classes.

- **Metadata-only, any table size.** Ship it. Add a `lock_timeout` and a retry, because the queue is the risk, not the statement.
- **Scanning, table under roughly one million rows.** Seconds on ordinary hardware. Ship it with `lock_timeout` and `statement_timeout` set, preferably outside peak traffic.
- **Scanning, table over roughly ten million rows.** Do not run it as a plain statement. Use the `NOT VALID` then `VALIDATE CONSTRAINT` path, or `CREATE INDEX CONCURRENTLY`, or the MySQL online algorithm, and state which.
- **Rewriting, any table you cannot afford to lock.** Not as a migration. Expand and contract, or an external copy tool.
- **A brand new table with no rows.** Every class is free. Do not apply any of this to it, and do not let a reviewer waste the author's time doing so.
- **You cannot tell how big the table is in production.** Assume large, and say so explicitly in the review rather than silently. Then split the verdict: approve the metadata-only statements, because their cost does not depend on size, and hold the scanning and rewriting ones pending an actual number. Ask for `SELECT reltuples FROM pg_class WHERE relname = ?` and `pg_total_relation_size(?)` on PostgreSQL, or `DATA_LENGTH` and `TABLE_ROWS` from `information_schema.TABLES` on MySQL. A review that guesses here is worse than one that asks, because the author will believe the guess.

## Worked example, compressed

One migration file in a billing service, four statements, plus an application change in the same pull request.

```sql
ALTER TABLE invoices ADD COLUMN currency varchar(3) NOT NULL DEFAULT 'GBP';
UPDATE invoices SET currency = accounts.currency FROM accounts WHERE invoices.account_id = accounts.id;
CREATE INDEX idx_invoices_currency ON invoices (currency);
ALTER TABLE invoices DROP COLUMN legacy_currency_code;
```

**Statement 1.** Metadata-only on PostgreSQL 11 and later: the default is a constant, so no rewrite. Safe, and the reviewer should say why it is safe, because half the team believes it is not. Add `lock_timeout`.

**Statement 2.** A single unbatched update across the whole table, inside the migration transaction, joined to another table. This is the worst statement in the file. Dead tuples for every row, a held transaction, replica lag, all-or-nothing failure. It must leave the migration and become a resumable batched job keyed on `invoices.id`.

**Statement 3.** A plain `CREATE INDEX` takes a lock that blocks writes for the length of the build. Must be `CREATE INDEX CONCURRENTLY`, which means this statement cannot live in the same transaction as the others, which means the file has to be split regardless of anything else. And the review must ask what happens if it fails, because the answer is an invalid index that slows every write and serves no query.

**Statement 4.** Destructive, and shipped in the same pull request as the code change that stops using `legacy_currency_code`. During the rolling deploy the previous application version is still selecting that column and will error. Rolling the code back will not restore it. This is the finding that matters most and it is not visible in the SQL at all: it is visible in the fact that the diff also touches the application.

**Row count.** Not stated anywhere in the pull request. Applying the "cannot tell" branch: statement 1 is approved regardless, statements 2 and 3 are held pending the actual count and size.

**Verdict: hold.** Split into four deploys. Release A: statement 1 alone. Release B: the dual-write code. Then the backfill as a job, throttled on replica lag. Release C: the concurrent index, in its own non-transactional migration, with a documented check for an invalid index afterwards. Release D, no earlier than one release after the reading code is gone: the drop.

## Failure modes

**Reviewing the syntax and approving the operation.** The statement is valid SQL, does what the ticket says, and takes a table offline. Syntax is not the review.

**Assuming the current engine version.** Half the dangerous advice about migrations was correct before PostgreSQL 11 or before MySQL 8.0.12 and is now wrong, and half the safe-looking advice assumes a version the team is not running. Check the version before applying the rule.

**Missing the queue.** Concluding a statement is instant and stopping there, without noticing that instant statements still queue, and that everything else queues behind them. The outage is caused by the wait, not the work.

**Treating `CREATE INDEX CONCURRENTLY` as free.** It is not free, it is slower, it takes two passes, it can fail, and the failure is silent and leaves debris that costs write throughput indefinitely.

**Reviewing the migration without the diff around it.** The deploy-ordering defect is never inside the migration file. It is in the relationship between that file and the application change shipping beside it.

**Accepting a down migration as a rollback plan.** For anything destructive the down migration restores the schema and not the data, and writing one creates the belief that rollback is available when it is not.

**Backfilling inside the migration.** Even correctly batched, a backfill inside a framework-wrapped migration is one long transaction, which is the thing the batching was meant to avoid.

**Sizing the table from the fixture.** The development database has 400 rows. The staging database has 40,000. Neither tells you anything about the statement that is about to run against 80 million.

## What this skill does not do

- It does not know your table sizes, row widths, index counts, write rates or replica lag, and every estimate of duration depends on all of them. It classifies and it asks.
- It does not run anything. It will not connect to a database, take a lock, or verify that the statement behaves as described on your version.
- It covers PostgreSQL and MySQL with InnoDB only. Other engines have different lock tables and different escape hatches, and guessing across engines is how the wrong advice spreads.
- It does not review the schema design: types, naming, normalisation, and whether the column should exist are all outside it.
- It does not replace a linter in CI. The mechanical checks should fail a build automatically, long before anyone is reading a diff.
