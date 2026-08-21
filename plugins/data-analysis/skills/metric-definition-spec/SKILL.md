---
name: metric-definition-spec
description: Writes one specification document per metric, enumerating the decisions the metric name hides: grain, eligibility, the action, the day boundary, the window, deduplication, late arrival, the denominator, null handling and restatement, each recorded with the choice made and the reason. Adds a reference query, a register of known divergences from other systems, and a naming rule that forces a metric to carry its own window and population. This skill should be used when a metric is about to be built or when two systems report different numbers for the same metric name.
---

# Metric definition spec

## The claim this skill is built on

The output is a specification document, one per metric. It contains the definition, the decisions listed with the choice made, the reference query, the known divergences from other systems, and the restatement policy. It is a written artefact, not a query and not a dashboard.

The obvious approach is to write a better sentence. "Monthly active users: the number of unique users who used the product in a month" reads as complete and specifies almost nothing. It does not say what a user is, what used means, when the month starts, which timezone that start is in, what happens to a person with two accounts, or what happens to an event that arrives four days late. Every argument about a number traces back to one of those unstated decisions, and no amount of rewriting the sentence closes them, because the sentence was never where the ambiguity lived.

The evidence that a metric name specifies almost nothing is public. Meta's 10-K for the 2021 financial year defines a daily active user as a registered and logged-in user who visited through the website or a mobile device, or used Messenger, on a given day, and defines monthly active users on a trailing 30-day window rather than a calendar month. The same filing estimates duplicate accounts at approximately 11 per cent and false accounts at approximately 5 per cent of worldwide monthly active users, based on an internal review of a limited sample, with a stated margin of approximately 3 per cent, and discloses that model recalibrations rather than user behaviour added roughly 40 million and 60 million to a people-based metric in two separate quarters. Snap's 10-K for the 2025 financial year defines a daily active user over "a defined 24-hour period", pointedly without naming a timezone. Twitter's 10-K for the 2018 financial year defined monetizable daily active users as those reached through surfaces "able to show ads", which builds revenue relevance into a user count.

Those are four different meanings of the word active, all in filed documents, all correct inside their own definitions. Your two dashboards are doing the same thing with less paperwork.

## The decision list, which is the asset

Work through these in order. Each one gets a written choice and one sentence of reason. An unanswered item stays in the document marked open, because an open decision that is visible is safe and an open decision that is invisible is the next argument.

**1. Grain.** State what one row of the underlying table represents: one event, one session, one account day, one aggregate. Everything downstream inherits it. A metric built on a daily rollup cannot answer an hourly question, and a metric built on raw events counts a retry as an event unless you say otherwise.

**2. Eligibility.** Which population counts. Name internal and staff accounts, automated test accounts, partner sandboxes, bots and crawlers, trial users, and churned accounts whose integrations still authenticate every night. Then say where the exclusion is applied, at the source table or as a final filter, because that decides whether every other metric inherits it. Meta's filing shows the honest shape of this answer: a stated estimate, a stated method, and a stated margin, rather than silence.

**3. The action.** What used means, written as a concrete event set rather than an adjective. This is where passive usage hides: an open browser tab that polls, a background sync, a mobile app resuming in the background, a push notification received, an email pixel firing. Each of those can make a person active without a person being present. Netflix published the magnitude of getting this wrong in one direction: moving from 70 per cent of a single episode to two minutes of viewing made the metric about 35 per cent higher on average, per its shareholder letter for the fourth quarter of 2019, dated 21 January 2020. It changed again in 2021 to hours viewed over a first-28-days window. The definition of the action is not a rounding decision.

**4. The day boundary.** Which timezone the day is cut in. Timezone offsets in current use span UTC-12:00 to UTC+14:00, a 26-hour span, so at any instant two calendar dates are live somewhere. Roughly 13 of the offsets in use are not whole hours, including +05:45 for Nepal and +12:45 for the Chatham Islands, which breaks the habit of thinking in whole-hour shifts. In a zone that observes daylight saving, exactly two days a year are not 24 hours long, one of 23 and one of 25, so a per-day count in local time has two days a year that are structurally not comparable. The IANA time zone database changes several times a year, so a zone identifier is a moving target while a fixed offset is not. Google Analytics 4 documents the practical version of this: the day boundary is the property's timezone regardless of where the visit originates, changing it affects only data going forward, and the change produces a visible flat spot or spike at the switch. Write the choice as an identifier from the IANA database or as an explicit offset, and use ISO 8601 for every date in the document.

**5. The window.** Rolling or calendar, and how long. A trailing 30-day window and a calendar month are different metrics wearing one name. Calendar months are 28 to 31 days, so a February decline of about a tenth in any monthly count is arithmetic rather than behaviour. A 28-day rolling window holds the weekday composition constant and a 30-day one does not, which matters for any product with a weekday shape.

**6. Deduplication.** What identity means. One person with two accounts, one account with ten users, one household on one subscription, one person on three devices, and one anonymous identifier that gets promoted to a known user at sign-in. State the dedup key, and state whether pre-login activity is stitched to the user afterwards. Stitching is the dangerous one, because it changes history retroactively and it does so silently.

**7. Late arrival.** What happens to events that arrive after the period closed. The Dataflow Model, Akidau et al., PVLDB 8(12), 2015, established the distinction between event time and processing time and states that for most real-world distributed data sets the system lacks the knowledge to establish a fully correct watermark. Apache Beam and Apache Flink both default allowed lateness to zero, which means late events are silently dropped rather than late-added. That default is the reason most numbers never restate, and it is worth saying plainly in the document. Write a close time: the number is provisional until T plus some stated interval, final after it, and late events beyond that are counted in the period they arrive or discarded, named explicitly.

**8. The denominator.** For any rate, name it precisely. An activation rate can have a denominator of every signup, every signup that reached the application, or every signup that was eligible to activate, and the three differ by more than most of the arguments they cause. Beware any rate whose denominator is itself a behavioural metric, such as a share of accounts active this month, because the rate then moves when the denominator moves and nobody can tell which half changed.

**9. Null and unknown.** Three options, and they are not equivalent: exclude from the denominator, count unknown as its own category, or impute. Dropping unknowns makes shares sum to 100 per cent of a smaller universe, which is the most common way a segment chart lies without anybody editing it.

**10. Restatement.** Whether history may change, under what circumstances, who approves it, and who is told. Twitter's 10-Q for the first quarter of 2022 disclosed that an account-linking error had overstated its monetizable daily active user metric from the first quarter of 2019 through the fourth quarter of 2021, by roughly 0.9 per cent, undetected for twelve quarters, and that earlier periods could not be recast because of data retention policies. Its 10-K for the 2017 financial year disclosed a separate case where a third-party software development kit had caused three years of overstated monthly active users. Retention policy, not intention, decides whether restatement is even possible, so record the retention window in the spec.

## The reference implementation

A definition with no runnable reference query is prose, and prose does not get copied into a dashboard. The query does. Specify exactly one reference query per metric, name it, version it, and store it beside the document.

Three rules keep it honest. Every decision above appears in the query as a visible clause with a comment naming the decision, so the eligibility filter is not an unexplained WHERE. The prose and the query are checked against each other on a stated schedule, because they drift, and the drift is always in the direction of the query, since that is what people run. And the document states which one wins when they disagree: if the prose is authoritative, a divergent query is a bug to be fixed, and if the query is authoritative, say so out loud, because that is a decision to hand the definition to whoever last edited the SQL.

The four questions worth answering in the document, whatever else it contains, come from the SEC's guidance on key performance indicators, Release Nos. 33-10751, 34-88094, FR-87, effective 25 February 2020. It requires a clear definition of the metric and how it is calculated, why it is useful to investors, how management uses it, and disclosure of any change in definition along with the reason, the effect, and whether prior metrics should be recast. It names daily and monthly active users explicitly as in scope. You are probably not filing anything. The four questions are still the right spine, and the recast question is the one every internal document skips.

## Divergence, treated as a first-class output

Two systems will report different numbers for the same metric name, and often both are right. The billing system counts a subscription that was cancelled on the last day of the month, the product database counts it as churned on that day, and the two reports differ by exactly those accounts, every month, forever.

The spec carries a divergence register: for each known conflict, name the two systems, the cause, the expected direction, and the rough size. Two lines each. An undocumented divergence is not a one-off cost, it is a recurring one, because it gets rediscovered in a meeting every quarter by a different person, and each rediscovery costs the same afternoon.

Where a divergence cannot be explained, record it as unexplained with the date it was last checked. That is a genuine state and it is far better than the alternative, which is a footnote nobody can source.

## Naming, so the number cannot travel alone

The name should carry the qualifiers the number needs, so a figure quoted without its window looks wrong on sight.

- Include the population, the action, the window and the boundary: `sending_accounts_28d_utc_excl_internal` rather than `active_accounts`.
- One name, one definition. Two definitions get two names. A single name plus a footnote loses the footnote the first time somebody pastes it into a slide.
- Never reuse a name across a definition change. Version it, keep the old series under the old name, and let the two overlap for at least one full window so the size of the change is visible rather than argued about.
- If a metric feeds a contract, a bonus or an external report, mark it in the name or the header. Those metrics need the strictest boundary rule and the tightest restatement policy, and the marker is what stops a well-meaning change landing on one.

## Decision rule: choosing the day boundary

- **The product is used in one timezone.** Use local time, name the IANA identifier, and say so in the metric name. The days line up with the working day of everyone reading the number, which is the whole point.
- **The product is global and the metric feeds a financial, contractual or externally reported process.** Use a single fixed offset, UTC unless there is a reason. Reproducibility beats intuition here: a fixed offset gives the same answer when recomputed next year, and a set of local boundaries does not, since the IANA database changes several times a year and the rules for a past date can be revised.
- **The metric answers a per-user behavioural question**, such as whether people use the product on weekends. User local time is genuinely more meaningful and is also much more expensive, because each row needs a resolved user timezone and the daylight saving edge has to be handled per user. Choose it only when somebody will act on the difference between that and a fixed offset.
- **You cannot tell.** Use the fixed offset, put it in the metric name so nobody assumes otherwise, and record the question as open with the person who can answer it. This is the branch you will hit most often, and the failure is not choosing wrong, it is choosing silently.

## Worked example

An invoicing tool for small businesses wants an activation metric. Everything here is invented.

**The naive definition.** "Activation rate: the percentage of new accounts that activate in their first week." One sentence, and it currently reads 61 per cent on the growth dashboard and 44 per cent in the monthly report.

**Decisions surfaced.**

1. *Eligibility.* The denominator includes staff accounts, demo accounts made during sales calls, and a partner sandbox that creates several accounts a week. Choice: exclude all three, applied at the source table so every metric inherits it. Effect small, roughly two points.
2. *The action.* Activate currently means an invoice row exists. The onboarding tour auto-saves a draft invoice for every new account, so that event fires for nearly everybody. Choice: activation is one invoice sent to an external email address. Effect large. This is the gap between the two dashboards, since the monthly report was already filtering to sent invoices without saying so.
3. *The window.* First week means seven days from the signup timestamp, and the cohort is not complete until seven days after the last signup in it. The growth dashboard shows the current week's cohort, which is always partial, so the newest bar is always low and somebody worries about it every Monday. Choice: cohorts are reported only once complete, and the in-flight cohort is shown separately and labelled partial with the number of days elapsed.
4. *Late arrival.* Invoice-send events come off a queue with retries, and the pipeline's allowed lateness is the default of zero, so events landing after the window closes are dropped rather than counted. Choice: the cohort number is provisional for 72 hours and final after that, with late events counted into the correct cohort during that window only, and the close time stated on the dashboard.

**Reference query, specified.** Denominator is accounts created in the cohort week, excluding the three eligibility classes at source. Numerator is distinct accounts with at least one external invoice send within 168 hours of their own signup timestamp, evaluated in UTC. Cohort weeks are ISO 8601 weeks. One row per cohort week, with a completeness flag.

**Verdict.** The rate is 38 per cent, not 61 and not 44. The 61 was drafts counted as activation, the 44 was a complete cohort measured against an incomplete denominator, and neither dashboard was wrong about its own arithmetic. Two decisions moved the number materially and two moved it by about two points combined. The document now names one metric, `activated_accounts_7d_utc`, with a partial-cohort rule, a 72 hour close and one divergence recorded against the billing system, which counts an account as activated on first payment and will therefore always read lower.

## Failure modes

**The Same Name Twice.** One name, two definitions, in two tools. Nobody notices until the two numbers are on one slide. From the outside it looks like a data quality problem and it is a naming problem.

**Midnight Drift.** The warehouse cuts days in UTC, the analytics property cuts them in the head office timezone, and the two daily counts differ by a slice of evening traffic. The signature is a stable percentage gap that widens with the share of users on the far side of the boundary, and it survives every attempt to explain it as sampling.

**Silent Restatement.** Last month's number changes and nothing announces it. Somebody quotes the old figure from an old email and looks careless. The cause is usually a backfill, and the fix is a close time in the spec rather than a rule against backfills.

**Denominator Ambiguity.** A rate is quoted with no denominator anywhere near it. Two people defend different numbers, both compute correctly, and the conversation lasts twenty minutes before anyone asks what the bottom of the fraction is.

**Bot Inclusion.** Automated traffic sits inside the active count. The tell is a flat weekend, or a perfectly regular hourly pattern, or a country appearing in the top five that has no customers in it.

**Identity Collapse.** Two accounts get merged, or anonymous sessions get stitched to a known user, and history changes retroactively. Last quarter's count drops with no code change and no incident, because the past was rewritten by a deduplication job.

**Definition By Dashboard.** The definition is whatever the chart happens to do. There is no document, so the SQL in the tile is the specification, and it was edited by four people who each fixed one thing.

**Late Arrival Amnesia.** Events arriving after the window are dropped by default and nobody knows. The number is quietly low for any period where the pipeline had a delay, and because the loss is silent there is no alert and no gap in the chart.

**The Partial Period.** The current day, week or month is reported alongside complete ones. Every trend ends in a decline and somebody investigates it monthly.

## What this skill does not do

- It does not choose which metrics matter. That is a product decision, and a rigorous definition of a metric nobody should be watching is wasted work done carefully.
- It cannot see your event schema, your table names or your instrumentation. It supplies the questions and somebody with warehouse access supplies the answers.
- It does not implement a semantic layer, and where one exists the definition belongs in it rather than in a document. A document cannot stop a dashboard from disagreeing with it, and a semantic layer can.
- It will not resolve an organisational disagreement that is really about incentives. If two teams need different numbers to hit different targets, the definition argument is a proxy and settling it changes nothing.
- It does not cover experiment analysis or charting. What a defined metric means inside a test, and how it should be drawn, are different jobs with different failure modes.
- It cannot make history restatable. Where raw events were dropped or aged out, the spec records that the series is not recastable before a given date, which is a fact to publish rather than a problem to solve.
