---
name: dataset-cleaning-run
description: Cleans a dataset by fixing the order of operations rather than the techniques, because several cleaning steps destroy information that a later step needs and two of them cannot be undone at all. Carries a twelve-step pipeline with the reason each position is forced, the profile to compute before touching anything, a catalogue of dirty-data patterns with the tell for each, the code points of the invisible characters that break string equality, survivorship rules for deduplication, and the three missingness mechanisms with the rule about what each one permits. Produces a cleaned dataset plus a cleaning log with row counts before and after every transformation. This skill should be used when a raw file, extract or spreadsheet has to become an analysable table and no transformation has been applied yet.
---

# Dataset cleaning run

## The claim this skill is built on

Cleaning a dataset is not hard. Every individual operation in it is a solved problem with a one-line implementation in every language, and a competent model performs each one correctly on request.

The failures come from sequence. Deduplicate before normalising and you keep both spellings permanently, because deduplication has already picked which row survives. Filter before parsing dates and you are filtering strings, where "09/12/2025" sorts after "2025-01-01" and any range boundary silently removes one of the two formats in the file. Impute before detecting outliers and you have imputed from a contaminated distribution. Detect outliers before reconciling units and the rule deletes real values while keeping the ones wrong by a factor of a thousand.

So this file is an ordering with a reason at each position, not a catalogue of techniques. Two steps are irreversible in practice and are named as such, because knowing which operation you cannot take back is the difference between a run you can repeat and a file nobody trusts.

**What you produce.** Two artefacts, not one.

1. **The cleaned dataset**, written to a new file. The input is never modified in place.
2. **The cleaning log.** One line per transformation carrying the step name, the rule in a form somebody could re-implement, rows in, rows out, cells changed, and the count sent to quarantine. It closes with a reconciliation: input rows equals output rows plus collapsed duplicates plus quarantined rows, no residual.

A cleaning run with no log is an edit, and an edit cannot be repeated next month or argued with.

## The ordered pipeline, and why each position is forced

### 1. Load with every column as text

No type inference, no date parsing, no numeric coercion. Read the delimiter, the quoting rule and the encoding, and nothing else.

This is first because it is the only step whose damage cannot be repaired by running the pipeline again. A reader that infers types decides, before you have seen a single value, that a nine-digit identifier is an integer and drops its leading zeros, that a long numeric identifier is a float and rounds off its tail, and that "N/A" is a null and forgets it was ever a string. Every later step can be re-run against the original file. This one destroys the evidence you would need to re-run it.

Record the dialect while you are here. RFC 4180, published October 2005 as an Informational document, specifies CRLF line endings and escaping of an embedded double quote by doubling it, then observes that implementations vary, which is the accurate summary of the format.

### 2. Freeze the input and count it

Keep a byte-identical copy of the source and record its size, its row count with and without the header, and a checksum. Every number later in the log is relative to it, and without it "we lost four thousand rows" is a sentence nobody can verify or refute.

### 3. Profile before changing anything

A profile taken after cleaning describes the cleaning. The null rate after imputation is zero, which is not information about the data.

### 4. Repair encoding, whitespace and invisible characters

This must precede normalisation, deduplication and grouping, all three. A single trailing U+00A0 NO-BREAK SPACE makes two visually identical strings unequal under every comparison operator, so any deduplication, join or group-by run before this step is silently wrong and produces a result that looks plausible. It must also precede type parsing, because a numeric parser that meets a non-breaking space inside "1 234" returns a null, and you then impute a value that was in the file all along.

The characters that do this, with code points, verified against the Unicode Character Database:

- **U+00A0 NO-BREAK SPACE.** The commonest by a distance, from copied web pages and locale-aware number formatting. **U+202F NARROW NO-BREAK SPACE** and **U+2007 FIGURE SPACE** arrive the same way, as thousands separators in some locales.
- **U+200B ZERO WIDTH SPACE**, **U+200C ZERO WIDTH NON-JOINER**, **U+200D ZERO WIDTH JOINER** and **U+2060 WORD JOINER** render as nothing at all, so no amount of looking at the screen will show you one.
- **U+FEFF ZERO WIDTH NO-BREAK SPACE.** As a byte order mark it lands on the first character of the first header cell, so the first column name is not the name you typed.
- **U+00AD SOFT HYPHEN**, invisible unless the renderer breaks the line there. **U+2018**, **U+2019**, **U+201C** and **U+201D**, the curly quotation marks a word processor substitutes, so a name that should carry an apostrophe carries something else.
- **U+3000 IDEOGRAPHIC SPACE** and **U+1680 OGHAM SPACE MARK.** Space-like, so they defeat a trim that strips only ASCII space and tab.

Then the confusables. Unicode Technical Standard #39, Unicode Security Mechanisms, revision 32 dated 4 September 2025, defines a skeleton transformation for exactly this: convert to NFD, remove characters with the Default_Ignorable_Code_Point property, substitute each character's prototype from the published confusables data, re-apply NFD. Two strings are confusable if and only if their skeletons are equal. The data file behind it, confusables.txt version 17.0.0 dated 22 July 2025, carries roughly six and a half thousand mappings. The ones that turn up in business data are the Cyrillic letters shaped like Latin ones: U+0430 maps to a, U+043E to o, U+0435 to e, U+0441 to c, U+0440 to p, U+0445 to x, U+0456 to i. A value pasted out of a document can carry one and will then match nothing, forever, while looking correct in every screenshot.

Finally, normalisation form. A precomposed e with an acute accent, U+00E9, and a plain e followed by U+0301 COMBINING ACUTE ACCENT render identically and compare unequal. Pick NFC or NFD, apply it to every text column once, record which.

### 5. Normalise representations

Case folding, punctuation and accent handling, and an explicit synonym map for known variants of a category. This follows step 4 for the reason above, and it must precede deduplication. Two rules govern it: normalise into a new column and keep the original so the merge is auditable, and never merge two values without evidence they are the same thing, which is the failure named later as Normalisation Overreach.

### 6. Parse types with an explicit format

Never inference. Name the format: the date pattern, the decimal separator, the thousands separator, the currency symbol handling, the timezone. This must follow normalisation, because a decimal comma or a thousands separator has to be resolved before the parse, and it must precede every filter, range check, sort and outlier rule, because a comparison on text is lexicographic and "9" is greater than "10".

The date case costs the most. A date written as A/B/C is unambiguous only when A or B exceeds twelve, and the ambiguous cases count directly: twelve possible first components below thirteen multiplied by twelve possible second components is 144 of the 365 dates in a common year, so roughly two dates in five are ambiguous under a slash format. Determine the format from the rows where a component exceeds twelve, apply it to the whole column, log the rule, and quarantine the column entirely if those unambiguous rows disagree with each other, because that means the column holds two formats.

### 7. Resolve sentinels and encode missingness properly

Sentinels are real values standing in for absence: minus one, zero, 999, 9999, the strings "N/A", "NULL", "unknown", "none" and "-", the empty string, a whitespace-only string, and dates on an epoch.

This follows parsing, because you cannot see a pile of rows on an epoch date until the column holds dates. It precedes outlier detection, aggregation and imputation, because a minus one left in a numeric column is subtracted from every sum and pulls every mean, and no downstream step flags that as an error.

Replace each sentinel with a typed null and log the count per sentinel per column, separately. The count is the point: six thousand rows of minus one is a different fact about the data than six rows.

### 8. Deduplicate

Now, and not before. See the dedicated section below.

### 9. Reconcile units, currencies and grain

After deduplication, because a unit conversion writes a derived value that makes the original row harder to match against its twin. Before outlier detection, because a column mixing grams and kilograms produces outliers that are units rather than values, and an outlier rule applied there deletes the correct rows. Grain belongs here too: if some rows are orders and some are order lines, no cleaning rule fixes it, so split the table.

### 10. Handle outliers

After sentinels and after units, so what remains is a candidate for being genuinely extreme rather than an artefact. Flag, do not delete. An outlier is a question, and the answer is in the decision rule below.

### 11. Impute, if at all

Last, and usually not at all. This is the second irreversible step, and it is irreversible for a social reason rather than a technical one: once an imputed value sits in the same column as measured values with no marker, no downstream consumer can separate them again, and none of them will know to ask. So every imputed field gets a companion boolean column named for it, and the log records the method and the count. If you will not add the companion column, do not impute.

### 12. Validate, then write the log

Validation runs against stated constraints so the log can record the reject counts. If you want a checklist for what to constrain, DAMA UK's white paper "The Six Primary Dimensions for Data Quality Assessment", published October 2013, is the usual one: completeness, uniqueness, timeliness, validity, accuracy and consistency. Note where it does not help. Accuracy is agreement with the real world, which no file can establish about itself, so it is a dimension you can only test against a second source. The other five are testable inside the file, and those are the five worth writing rules for here.

The log is then written, and it is the deliverable that makes the cleaned file worth more than a cleaned file.

## The profile, and what each line detects

Compute this per column, before step 4, and write it down.

- **Null rate, with each sentinel counted separately.** Distinguishes a column that is empty from one full of the string "unknown".
- **Distinct count, and distinct count over row count.** A distinct count of one means a constant, which usually means an upstream filter nobody mentioned. A distinct count equal to the row count on a column that should be categorical means free text or an accidental identifier. A distinct count slightly above expectation, nine where the business says five, is the whitespace and case variant tell, and it is the most useful line in the profile.
- **Top twenty values by frequency, and bottom twenty.** Sentinels announce themselves at the top. Typos and homoglyph variants sit at the bottom with a count of one or two.
- **Length distribution for text columns.** An identifier should have one or two lengths. Three means truncation or inconsistent padding. A maximum length of exactly 255, 100, 50 or 32 is a column a database truncated, and those rows are unrecoverable.
- **Character class summary per text column.** Which scripts, whether any character falls outside a stated allowlist, whether any has the Default_Ignorable_Code_Point property. This is the line that catches the invisible characters.
- **Minimum, maximum and quantiles for numeric columns.** A minimum of exactly minus one or zero on a quantity that can be neither is a sentinel floor.
- **Minimum and maximum for date columns.** A pile on 1970-01-01 means a zero was read as a Unix timestamp, and a pile in late December 1899 means a zero was read as a spreadsheet date serial. Microsoft documents Excel's deliberate treatment of 1900 as a leap year, inherited from Lotus 1-2-3 for compatibility, so serial dates before 1 March 1900 are not trustworthy anyway.
- **Count of values failing the column's expected pattern.** The number that decides whether a repair rule is worth the hour.

## Deduplication done properly

Three kinds, and they are not variations of one thing.

**Exact duplicates.** Every column identical. Safe to collapse, but count them and log the count, because a large exact-duplicate block almost always means the extract ran twice or overlapped a previous run, and collapsing them quietly conceals the real bug in the export.

**Key-based duplicates.** The business key repeats and other columns differ. A survivorship rule is required and must be written before it is run. The usable rules: most recent by a named timestamp column, most complete by a stated count of non-null fields, highest trust by a stated ranking of source systems, or field-by-field merge with a rule per field. Whichever you choose, the hard rule is that if you cannot state in one sentence why the surviving row survived, the deduplication is not reproducible and will produce a different answer on the next load without anyone noticing.

**Fuzzy duplicates.** Free-text entry, so "Northgate Logistics Ltd", "Northgate Logistics Limited" and "northgate logistics" are one customer. Two rules:

- **Block before you score.** Comparing all pairs is quadratic, and a 200,000 row table is twenty billion comparisons, which is the job that runs overnight and is abandoned half done, leaving a partially merged file. Block on something cheap and exact first: the first three characters of the normalised name plus the postcode, a phonetic key, or a sorted-token fingerprint. Score only inside blocks.
- **Two thresholds, not one.** An auto-merge threshold and a review threshold, with everything between them going to a review file. A single threshold forces every borderline pair into a decision the score cannot support.

One rule applies to all three: a merge is irreversible unless the surviving row carries the source identifiers of every record folded into it. Keep them in a column. It costs nothing and it is the only way anybody can ask later what happened to a specific record.

## Missingness, treated honestly

The taxonomy is Donald Rubin's, from "Inference and missing data", Biometrika volume 63, issue 3, 1976, pages 581 to 592. It is worth knowing because the three categories permit different things.

**Missing completely at random.** The probability of a value being missing depends on nothing, observed or unobserved. A sensor dropping packets. Analysis on complete cases is unbiased, just less precise.

**Missing at random.** Missingness depends on data you have, such as income unreported more often by younger respondents where age is recorded. Conditioning on the observed variables is enough, so imputation using them is defensible.

**Missing not at random.** Missingness depends on the unobserved value itself, such as high earners declining to state income. No imputation built from the observed data repairs this, and every imputed value carries the same bias deleting the rows would have carried.

**The operational point people skip.** You cannot determine which you have from the data alone. Missing at random versus missing not at random is not identifiable from the observed values, because the test would need the values you do not have. So the output is a written assumption with its direction stated: "we assume missing at random conditional on segment and tenure, and if it is not, revenue per account is overstated because the accounts that stopped reporting were the small ones."

The default remains: represent missing as missing. A typed null, not a zero, not a mean, not an empty string. Zero is a value and it will be averaged.

## The decision rule for a row that fails validation

Three branches, and the third is the one that matters.

**Branch A. The failure is a representation problem and the true value is recoverable from the row.** A postcode carrying a non-breaking space, an amount with a currency symbol embedded, a date in the file's second format disambiguated by the source system column. Repair it, log the rule and the count of rows it touched, keep the row.

**Branch B. The value is genuinely absent.** Mark it null, keep the row, do not drop it. Dropping rows for a missing field silently changes every denominator downstream, and not proportionally: removing three per cent of rows does not move a rate by three per cent, it moves it by however much those rows differed from the rest, which nobody can recover once they are gone.

**Branch C. You cannot tell whether the value is wrong or merely unusual.** A single order of 4,000 units where the next largest is 90. A birth date in 1902. A transaction dated after the extract ran. Quarantine it: move the row to a rejects file naming the rule it failed, keep it out of the analysis, and report the count and its share of the total in the log. A quarantine you can count is honest, a deletion you cannot count is not, and the two produce the same cleaned file.

**The stop condition on branch C.** Name a quarantine ceiling before you start, as a share of rows, somewhere in the region of one to five per cent depending on what the analysis has to bear. If the rejects exceed it, stop cleaning and go back to the source, because past that volume the rejects are the finding and cleaning them away deletes the answer.

## Worked example

A mid-size logistics company exports 412,000 transaction rows across fourteen columns from a billing system. The task is monthly revenue by customer segment over eight quarters. Everything here is invented.

**Three findings from the profile.**

1. **customer_id.** 38,904 distinct, and the length distribution shows exactly two lengths, eight and nine, where every nine-character value begins with the letter C. Two upstream systems, one prefixing. A representation split, not a data error, and it means the distinct customer count is wrong.
2. **amount.** Minus one occurs 6,214 times, a spike in the frequency table rather than the tail of a distribution. It is a sentinel for "not billed". Genuine refunds are also negative but carry a credit_note_id, which the sentinel rows do not.
3. **segment.** Eleven distinct values where the business says six. The extras are "Enterprise " with a trailing space, "enterprise", "SMB " with a trailing space, "Small/Medium", and one value whose first character is U+0415 CYRILLIC CAPITAL LETTER IE, pasted from a document, matching nothing.

**Two steps that had to be reordered.**

The first draft deduplicated on customer_id and transaction_date before resolving the C prefix, so every customer with rows from both systems stayed two customers and no duplicate between them was ever found. Deduplication moved after the identifier normalisation, taking distinct customers from 38,904 to 31,140.

The draft also filtered to the last eight quarters before parsing transaction_date, still text in two formats: 340,102 rows as YYYY-MM-DD and 71,898 as DD/MM/YYYY. Lexicographic comparison against the boundary string kept every YYYY-MM-DD row and removed the entire slash-format block, and the result looked completely normal. The filter moved after the explicit parse. Of the slash rows, 44,205 have a first component above twelve and are unambiguously day first; the remaining 27,693 are resolved by the source_system column, which is day first for every unambiguous row it produced. That rule is logged, not inferred per row.

**One quarantine.** 1,133 rows, 0.27 per cent, carry a transaction_date after the extract timestamp. Nothing in the file determines whether a clock is skewed or a future-dated invoice is legitimate, so branch C applies: rejects file, counted, under the one per cent ceiling set at the start.

**Row accounting in the log.** 412,000 loaded, zero lost to type coercion. 6,214 sentinel amounts nulled with the rows kept. 8,806 exact duplicates collapsed, traced to the extract overlapping the previous run by two days. 1,133 quarantined. Output 402,061, and 402,061 plus 8,806 plus 1,133 is 412,000 exactly, no residual. Segments eleven to six. No imputation, so no companion columns.

**Verdict.** Three profile findings, two reorderings, one quarantine, no imputation. Every input row sits in exactly one of four buckets. The revenue figure a reader will quote rests on a stated denominator, 402,061 minus the 6,214 never billed, which is 395,847, and that sentence is in the log rather than in somebody's memory.

## Failure modes

**Coercion On Read.** The identifier column lost its leading zeros before anybody looked at the file. The tell is a join that matches most rows and not all, with the misses concentrated among the shortest identifiers. It is unrecoverable from the loaded copy, which is why loading as text is step one.

**Premature Deduplication.** Deduplication ran before normalisation, so "Acme Ltd." and "ACME Ltd" both survived. The tell is a distinct count that is stubbornly higher than the business expects even after a deduplication that reported collapsing thousands of rows.

**Silent Row Loss.** A join or a filter removed rows nobody counted. The tell is that the final row count does not reconcile with the input, and the reason nobody catches it is that no step in the pipeline was ever asked to reconcile.

**Imputation Laundering.** Imputed values sit in the same column as measured ones with no marker. From the outside it looks like a complete dataset with a suspiciously smooth distribution, and six months later a model is trained on it by somebody who has no way to know.

**Sentinel Arithmetic.** A minus one or a 9999 survived into an aggregate. The tell is a mean that sits slightly below every individual value a person spot-checks, or a total that is oddly short of the sum of its parts.

**Normalisation Overreach.** Two genuinely different categories were merged because their names were similar. This one is worse than the others because the merge looks like progress: the distinct count fell, the profile improved, and a real distinction is gone with no record that it existed.

**Irreversible In-place Edit.** The source file was overwritten. There is now no way to re-run anything, no way to check a disputed value, and the only description of what happened is whatever somebody remembers.

**Blocking-free Fuzzy Matching.** An all-pairs comparison was started on a large table, ran for hours, and was killed part way through, leaving a file where some duplicates are merged and some are not, with no record of which. The tell is a merge count that stops at a round number.

**Log Absence.** The cleaned file exists and nothing describes how it got that way. It is undetectable from the file itself, which is the whole problem, and it surfaces the first time somebody asks why the report shows fewer customers than the CRM.

## What this skill does not do

- It does not do entity resolution across sources at scale. Reconciling three systems' views of the same organisations is a purpose-built problem with a literature and dedicated tools, and blocking plus a threshold is where the resemblance ends.
- It does not judge whether a finding is real. Base rates, mix shift and survivorship survive cleaning untouched, and a perfectly clean dataset supports a false conclusion as readily as a dirty one.
- It cannot recover information destroyed before the file arrived. A truncated column, an already-coerced identifier and a date the source system flattened are losses the pipeline records rather than repairs.
- It is not a data quality monitoring system. This is one run against one file. A feed that arrives nightly needs tests that fire nightly, with alerting and ownership, which is a different artefact.
- On a large or recurring dataset, a framework with a declarative validation vocabulary will do the checking half better than prose will, and it produces machine-readable failures instead of a paragraph.
- It says nothing about the export format, the delimiter you should write, or how to hand the cleaned table to somebody who will open it in a spreadsheet without corrupting it again on the way in.
