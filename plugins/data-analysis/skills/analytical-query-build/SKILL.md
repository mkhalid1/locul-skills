---
name: analytical-query-build
description: Builds an analytical SQL query together with a runnable assertion block that proves its grain and its row accounting, so a fan-out, a null-swallowed filter or a mis-framed window fails loudly instead of returning a plausible number. Carries the row-multiplier check, the difference between NOT IN and NOT EXISTS, the ON versus WHERE outer-join downgrade, half-open date intervals, the default window frame, and the points where Snowflake, BigQuery and Redshift diverge. This skill should be used when a SQL result is about to be quoted, charted or pasted somewhere nobody will ever see the query again.
---

# Analytical query build

## The claim this skill is built on

A query with a bug usually runs. It returns a number of roughly the right magnitude, in the right unit, formatted the way the last one was, and it gets believed. Nothing in the toolchain objects: the planner is happy, the row count looks sane, and the only signal that anything is wrong is that the total is 1.4 times what it should be.

This is why performance advice does not help here. Indexing a wrong query makes it a fast wrong query. What helps is a catalogue of the constructions that fail silently, and an assertion that converts each silent failure into a visible one.

So the output of this skill is two things, always shipped together: the query, and an assertion block. The assertion block is runnable SQL, not comments. A comment saying "one row per order" is a hope. A query that returns the row count and the distinct key count side by side is a claim somebody can check in four seconds.

## Step 1. State the grain, before you write a single join

Grain is what one row means. Before any join, write one sentence for every table in the query and one for the result:

- `orders`: one row per placed order.
- `order_items`: one row per line on an order, so many rows per order.
- `customer_dim`: intended as one row per customer, but it is a slowly changing dimension, so it is one row per customer per version.
- Result: one row per segment per calendar month.

That third line is where most wrong numbers are born. A table named like a dimension is not necessarily unique on its key, and nobody finds out until a sum is 40 per cent high.

The grain assertion for the result:

```sql
SELECT count(*)                              AS rows_out,
       count(segment)                        AS non_null_keys,
       count(DISTINCT segment || '|' || month_start) AS distinct_keys
FROM final_result;
```

Pass condition: `rows_out = non_null_keys = distinct_keys`. If `distinct_keys` is lower, the result is not at the grain you declared and something upstream fanned out. If `non_null_keys` is lower, a key is null and your GROUP BY has quietly created a bucket nobody asked for.

Note the null asymmetry that makes that last check necessary. Nulls collapse for GROUP BY, so every null segment lands in one group. They do not collapse for a unique constraint: PostgreSQL documents that two null values are not considered equal in a unique constraint, so duplicate rows containing a null in a constrained column are storable. That default only changed with the `NULLS NOT DISTINCT` clause added in PostgreSQL 15.0, released 13 October 2022, and it is still opt-in. MySQL documents the other half of the pair, that two nulls are regarded as equal for grouping. A key column can therefore be non-unique in the table and look perfectly unique in your output.

## Step 2. Join fan-out, the expensive one

A one-to-many join multiplies rows. Every subsequent aggregate is computed over the multiplied set, so counts inflate and sums inflate more, because a 300-unit order joined to three lines contributes 900.

Looker's documentation defines this as a fanout and works an example where joining orders to order items and summing the order value produces 223.44 against a correct 124.84. Nothing errors. The number is simply eighty per cent too high.

The check is row accounting across each join:

```sql
WITH base AS (
  SELECT count(*) AS n FROM orders
  WHERE ordered_at >= TIMESTAMP '2026-07-01 00:00:00'
    AND ordered_at <  TIMESTAMP '2026-08-01 00:00:00'
),
joined AS (
  SELECT count(*) AS n
  FROM orders o
  JOIN order_items i ON i.order_id = o.order_id
  WHERE o.ordered_at >= TIMESTAMP '2026-07-01 00:00:00'
    AND o.ordered_at <  TIMESTAMP '2026-08-01 00:00:00'
)
SELECT base.n AS rows_before,
       joined.n AS rows_after,
       CAST(joined.n AS numeric) / NULLIF(base.n, 0) AS multiplier
FROM base, joined;
```

Read the multiplier as follows. Exactly 1.0 means the join was one-to-one and every aggregate downstream is safe. Below 1.0 means an inner join dropped rows, which is sometimes intended and is never intended silently. Above 1.0 means fan-out, and every SUM and every COUNT in the query is now wrong by an amount that varies per group, which is why the total still looks plausible.

Probe the key before you join, not after:

```sql
SELECT customer_id, count(*) AS n
FROM customer_dim
GROUP BY customer_id
HAVING count(*) > 1
LIMIT 5;
```

Zero rows returned means the key is unique in this snapshot of the data. It does not mean the key is unique by constraint, which is a different and stronger claim you can only get from the schema.

**Three legitimate fixes, in the order to try them.**

First, aggregate to the grain before joining. This is the default and it composes:

```sql
WITH item_rollup AS (
  SELECT order_id,
         sum(quantity)  AS units,
         count(*)       AS line_count
  FROM order_items
  GROUP BY order_id
)
SELECT o.segment,
       sum(o.order_total) AS revenue,
       sum(r.units)       AS units
FROM orders o
LEFT JOIN item_rollup r ON r.order_id = o.order_id
GROUP BY o.segment;
```

Second, use a semi-join when you only need existence. If the question is "revenue from orders that contained hardware", you do not need the item rows at all:

```sql
SELECT o.segment, sum(o.order_total) AS revenue
FROM orders o
WHERE EXISTS (
  SELECT 1 FROM order_items i
  WHERE i.order_id = o.order_id AND i.category = 'hardware'
)
GROUP BY o.segment;
```

Third, deduplicate explicitly to the claimed grain, with the tie-break written down. `QUALIFY row_number() OVER (PARTITION BY customer_id ORDER BY valid_from DESC) = 1` is available in Snowflake, BigQuery and DuckDB; PostgreSQL has no QUALIFY as of version 18, so the same thing goes in a subquery with a WHERE on the row number, or in a `DISTINCT ON (customer_id)` clause, which is PostgreSQL-specific.

**COUNT(DISTINCT) is not a fourth fix.** It repairs a count after a fan-out and leaves every sum in the same query wrong. If you find yourself adding DISTINCT to make a number look right, you have located a fan-out and patched one symptom of it.

## Step 3. Three-valued logic

Null is not a value. It is the absence of one, and comparisons against it are unknown rather than false. PostgreSQL states it plainly: ordinary comparison operators yield null, not true or false, when either input is null, so `7 = NULL` yields null and so does `7 <> NULL`. `IS DISTINCT FROM` is the operator that treats null as a normal value: `NULL IS DISTINCT FROM NULL` is false, and `1 IS DISTINCT FROM NULL` is true.

Four consequences that produce wrong numbers rather than errors.

**NOT IN over a nullable column returns nothing.** If there are no equal right-hand values and at least one right-hand row yields null, the result is null rather than true, so the WHERE clause admits no rows. There is no error and no warning. The PostgreSQL wiki's "Don't Do This" page recommends NOT EXISTS instead and notes the plan difference between the two can cost orders of magnitude.

```sql
-- returns 0 the instant one refunds.order_id is null
SELECT count(*) FROM orders
WHERE order_id NOT IN (SELECT order_id FROM refunds);

-- correct, and usually faster
SELECT count(*) FROM orders o
WHERE NOT EXISTS (
  SELECT 1 FROM refunds r WHERE r.order_id = o.order_id
);
```

**An inequality filter drops nulls.** `WHERE status <> 'cancelled'` removes every row whose status is null, because the comparison is unknown. Write `WHERE status IS DISTINCT FROM 'cancelled'` if unknown status should be kept, and either way record the size of the set you dropped:

```sql
SELECT count(*)                                              AS all_rows,
       sum(CASE WHEN status <> 'cancelled' THEN 1 ELSE 0 END) AS kept_by_inequality,
       sum(CASE WHEN status IS NULL       THEN 1 ELSE 0 END) AS unknown_status
FROM orders;
```

The portable `CASE` form is used here on purpose. `count(*) FILTER (WHERE ...)` is standard and works in PostgreSQL, while BigQuery spells it `COUNTIF` and Snowflake `COUNT_IF`.

**A CASE with no ELSE returns null.** PostgreSQL documents it: if no WHEN condition yields true and the ELSE clause is omitted, the result is null. Those nulls then vanish from any aggregate you feed them to, so a bucketing expression that forgot one category quietly shrinks the denominator.

**Aggregates ignore nulls, and count is the exception.** Except for count, aggregate functions return null when no rows are selected, so `sum` over an empty set is null rather than zero. `count(*)` counts rows and `count(expression)` counts non-null values, which means `AVG(col)` is not `SUM(col)/COUNT(*)` whenever col has nulls, and the two are trivially easy to mix in one report. Within one family the policies even disagree: `array_agg` includes nulls, `string_agg` excludes them.

## Step 4. The filter that changes the join

Put a predicate on the right-hand table of a LEFT JOIN in the WHERE clause and you have written an inner join. PostgreSQL's documentation gives the mechanism directly: a restriction placed in the ON clause is processed before the join, while a restriction placed in the WHERE clause is processed after it, and that does not matter for inner joins but matters a lot for outer ones. The unmatched rows are added by the join with nulls in the right-hand columns, and the WHERE clause then evaluates the predicate against those nulls, gets unknown, and discards exactly the rows the outer join existed to keep.

```sql
-- customers with no July orders silently disappear
SELECT c.customer_id, count(o.order_id) AS july_orders
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id
WHERE o.ordered_at >= TIMESTAMP '2026-07-01 00:00:00'
GROUP BY c.customer_id;

-- the restriction belongs in ON, and zero-order customers survive with a 0
SELECT c.customer_id, count(o.order_id) AS july_orders
FROM customers c
LEFT JOIN orders o
       ON o.customer_id = c.customer_id
      AND o.ordered_at >= TIMESTAMP '2026-07-01 00:00:00'
GROUP BY c.customer_id;
```

The check is a row count of the left table against the row count of the result. For a LEFT JOIN at one-to-one grain they must be equal. The only WHERE predicate on an outer-joined table that is safe is an explicit `IS NULL` test, which is the anti-join idiom and is deliberate.

## Step 5. Time, which has more edges than it looks

**Choose the zone-aware type.** In PostgreSQL, `timestamptz` is stored internally as UTC and the original zone is not retained. `timestamp` without time zone silently ignores any time zone indication in the input string, so an ISO-8601 value with an offset loses that offset with no complaint. Both types are 8 bytes with microsecond resolution, so the zone-less type buys nothing and loses information. `AT TIME ZONE` is two different operators sharing one name: applied to a zone-aware value it produces a zone-less one, and applied to a zone-less value it produces a zone-aware one, so the direction of the conversion depends on the type of its input.

**Use half-open intervals.** `>= start AND < next_start`, never BETWEEN, on any timestamp column. BETWEEN treats both endpoints as included, being exactly `a >= x AND a <= y`. Two failures follow. A date literal against a timestamp column is a timestamp at midnight, so `BETWEEN DATE '2026-07-01' AND DATE '2026-07-31'` admits one instant of the last day and drops the other 86,399.999999 seconds of it. And two adjacent closed ranges share their boundary, so any row landing exactly on it is counted in both months.

**Never compare a timestamp to a date without deciding what you meant.** `date_trunc('day', ordered_at)` or a cast to date first, in the zone you have named out loud, is the version somebody else can read.

## Step 6. Window frames, where the default is not what people assume

With an ORDER BY and no frame clause, the default framing is `RANGE UNBOUNDED PRECEDING`, equivalent to `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`. That sets the frame to all rows from the start of the partition through the current row's last ORDER BY **peer**, not through the current row.

The consequence is specific. A running total written as `SUM(x) OVER (ORDER BY d)` over a column with ties returns the same fully-summed value for every tied row, so a running total by day where a day has five rows shows the day's completed total five times rather than accumulating through them.

```sql
-- ties share one value, because the frame runs to the last peer
SELECT ordered_at, amount,
       sum(amount) OVER (ORDER BY ordered_at) AS running_total
FROM daily_orders;

-- deterministic: ROWS framing, and a unique tie-break in the ORDER BY
SELECT ordered_at, amount,
       sum(amount) OVER (ORDER BY ordered_at, order_id
                         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM daily_orders;
```

Ordering by a value with ties also makes `row_number()` non-deterministic across runs, which is how a deduplication step keeps a different row each time it executes. Add a unique key to the ORDER BY whenever the result of the window is going to be filtered on.

BigQuery and SQL Server match the RANGE default. Snowflake diverges for `FIRST_VALUE`, `LAST_VALUE` and `NTH_VALUE`, defaulting them to `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`, and Snowflake's own documentation states this does not comply with the ANSI standard. Write the frame explicitly and the divergence stops mattering.

## Step 7. Numbers that will not add up twice

PostgreSQL documents that comparing floating-point values for equality might not work as expected, and recommends `numeric` for monetary amounts. Money goes in `NUMERIC(12,2)` or the dialect equivalent, never in `double precision`, and equality on a float is replaced by a tolerance test.

There is a worse version at warehouse scale. BigQuery documents that the result of `SUM` over floats depends on the order in which values are accumulated, and that order is not deterministic, so two executions of the same query on the same tables might return different results. A revenue figure that changes in the fourth decimal place between refreshes is that, not a data change.

Approximate distinct counts are a separate trap because they are sometimes the default and sometimes not. BigQuery's plain `COUNT(DISTINCT)` is exact and `APPROX_COUNT_DISTINCT` is not. Snowflake documents an average relative error of about 1.62 per cent for its HyperLogLog implementation, and Redshift documents around 2 per cent for its approximate form. On a distinct user count of 400,000 that is a few thousand users of drift, which is invisible in a chart and fatal in a reconciliation.

## The decision rule for a join

Apply this to every join in the query, one at a time.

- **The right-hand key is provably unique**, by a primary key or unique constraint you have read in the schema. Join freely, and record the row multiplier anyway so the assertion block documents the claim.
- **The right-hand key is not unique**, or you have read the schema and there is no constraint. Aggregate to the grain first, or replace the join with an EXISTS semi-join if you only need existence, or deduplicate explicitly with a written tie-break.
- **You cannot tell.** You do not have the schema, the table is a view over something you cannot see, or the constraint exists but the data predates it. This is the common branch and the answer is not to guess: run the uniqueness probe from Step 2 and add it permanently to the assertion block. It costs about two minutes. The alternative costs the meeting in which somebody notices the total moved.

## The assertion block

Ship it with the query, above it, as runnable SQL. Five checks, each with a pass condition written next to it:

1. **Grain.** Row count, non-null key count and distinct key count of the result. All three equal.
2. **Uniqueness.** One probe per join key that is not backed by a constraint. Zero rows returned.
3. **Row accounting.** A multiplier per join. Expected value stated, usually 1.0.
4. **Null census.** Count of nulls in every column that appears in a WHERE, a JOIN or a GROUP BY. A number you have looked at, not a number you assume is zero.
5. **Reconciliation.** The headline figure recomputed by a different route, such as a total from the fact table with no joins at all, compared against the sum of the grouped result. Difference of zero, or a difference you can name.

## Worked example

A mid-size logistics company wants revenue by customer segment for July 2026. Three tables: `orders` at one row per order, `order_items` at one row per line, and `customer_dim`, which everybody calls a dimension. Everything here is invented.

**First draft.** Orders joined to items to pick up a category filter, joined to `customer_dim` for the segment label, summed on `order_total`. It runs in under a second and returns 4.12 million.

**The assertions.** The uniqueness probe on `customer_dim.customer_id` returns 5 rows, so the key is not unique: it is versioned, one row per customer per change of segment, and 1,204 customers have two versions. The row accounting reads `rows_before` 18,442 and `rows_after` 26,109, a multiplier of 1.42. The grain assertion on the result passes, because grouping to seven segments hides everything.

That combination is the signature. The result looks correctly shaped and the money inside it has been counted 1.42 times on average, unevenly, so per-segment shares are wrong in different directions and no single ratio corrects them.

**Second issue, found by the null census.** The query excludes refunded orders with `order_id NOT IN (SELECT order_id FROM refunds)`. The census shows `refunds.order_id` has 3 nulls from a failed backfill. That subquery therefore returns no rows at all for the whole month, and the reason the total was not zero is that this predicate had been commented out during debugging and never restored.

**Corrected query.** Items pre-aggregated to one row per order in a CTE. `customer_dim` reduced to the version in force at `ordered_at` by a dated join in the ON clause. Refunds handled with NOT EXISTS. The July window written half-open as `>= '2026-07-01 00:00:00' AND < '2026-08-01 00:00:00'`. Revenue cast to `numeric`.

**After.** Multiplier 1.00 on both joins, uniqueness probe on the reduced dimension returns zero rows, reconciliation against the unjoined fact total differs by zero, and the answer is 2.87 million.

**Verdict.** The wrong number was 44 per cent high, ran fast, had the right shape and would have survived any review that read the SQL for style. Two assertions caught it, and both of them are four lines long.

## Failure modes

**Silent fan-out.** A one-to-many join multiplies rows and every sum inflates by a factor that varies per group. From the outside it looks like growth, which is why nobody questions it.

**Distinct as plaster.** Somebody adds `COUNT(DISTINCT id)` because the count looked too high. The count is now right, every sum in the same query is still wrong, and the DISTINCT is the evidence that the fan-out was seen and not fixed.

**Outer join downgrade.** A WHERE predicate on the right-hand table converts a LEFT JOIN into an inner join. The tell is a report of customers where nobody with zero orders ever appears, and zero-order customers are usually the point.

**Null filter drain.** An inequality filter removes the null rows too. The row count drops by an amount nobody predicted, and the missing rows are the ones whose status was never set, which is often exactly the population under investigation.

**NOT IN blackout.** A single null on the right-hand side of a NOT IN makes the whole predicate return nothing. Zero rows and no error, so the reading in the room is "there aren't any", which is a very confident way to be wrong.

**Boundary double count.** Closed intervals on consecutive periods share an endpoint. The row that lands on midnight appears in June and in July, so the two months sum to more than the year.

**Frame assumption.** A running total over a column with ties repeats the same value across every tied row. On a daily chart it produces flat steps that look like a data pause rather than a framing default.

**Float money.** Totals differ in the fourth decimal between two runs of the same query, and an equality test against a computed float never matches. Both are the type, not the data.

**Grain assumption undeclared.** Nobody wrote down what one row means, so two people extend the query in incompatible directions and the disagreement surfaces a month later as two dashboards that will not reconcile.

**Stage confusion.** A filter is applied outside a subquery that already computed a window function, so the window saw the unfiltered set. The percentages in the filtered output are shares of a population that is no longer on screen.

**Approximate passed off as exact.** An approximate distinct count is used in a reconciliation. It is within about 2 per cent, which is close enough to look right and far enough to never balance.

## What this skill does not do

- It does not tune anything. Performance belongs to the planner and to an EXPLAIN plan, and several fixes here trade speed for correctness on purpose.
- It does not know your schema. Every uniqueness claim is a query you have to run, and the file cannot tell whether you ran it.
- It does not define your metrics. It will produce a provably correct count of a thing two teams define differently, which is a different problem and a different document.
- It does not assess whether the finding means anything. No effect sizes, no significance, no causal identification.
- It does not cover data cleaning. Encodings, malformed dates and identifiers that lost their leading zeros happen before the query and are out of scope here.
- It covers dialect divergence only where it is named. Facts here were checked against PostgreSQL 18 documentation as of August 2026, with the specific Snowflake, BigQuery, Redshift, MySQL and Looker points cited where they differ, and no claim is made about the engines it does not name.
