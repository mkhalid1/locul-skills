---
name: topic-cluster-backlog
description: Converts a validated keyword portfolio into a 30-row clustered article backlog that an unattended writer can execute for months without re-researching anything. Groups keywords into three to five clusters with exactly one pillar page each, plans rows to a fixed article-type quota, bands every row by measured search demand and binds word count to the band, orders the schedule so that internal link targets are always scheduled earlier than the rows naming them, and freezes every field except status and published URL. This skill should be used when turning a keyword list into a content schedule, when a planned programme keeps producing internal links to pages that do not exist yet, or when a backlog is too thin for anyone to execute a row without redoing the research.
---

# Topic cluster backlog

## The claim this skill is built on

The obvious way to turn a keyword list into a content plan is to sort it by search volume, take the top thirty terms, and write a title for each. It produces a document that looks exactly like a plan and fails in three specific ways.

It has no shape. Keyword tools return questions, because questions are what people type, so an unconstrained planner produces something like twenty-six how-to articles, three comparisons and a list. There is no hub, so there is no page for authority to concentrate on and nothing for the other rows to point at.

It has no depth rule. Word count gets decided at writing time, by whoever is writing, which reliably produces a corpus where the longest pages are the topics the writer found most interesting and the shortest are the ones with actual demand.

And it has no order. A plan is a sequence in time, and the moment anyone plans an internal link they are making a claim about what already exists. Sorted by volume, a plan says nothing about what exists when.

So this procedure adds exactly three constraints to a keyword list: a fixed type quota, a numeric band that sets depth, and an ordering rule on link targets. Everything else is fields.

## Phase 1: cluster the portfolio, exactly one pillar each

Group the portfolio into **three to five clusters**. Fewer than three and you have one topic wearing a hat. More than five and a thirty-row programme cannot give any cluster enough supporting pages to matter, because a cluster with three children is not a cluster, it is three articles.

**Each cluster gets exactly one pillar page**, which is its hub: the page a newcomer would accept as the place to start, and the page every other row in the cluster links up to.

The boundary test, applied to each keyword: name the pillar a reader searching this term would accept as the "start here" page. If two pillars are equally plausible, the cluster boundary is in the wrong place and the two clusters should be one. If no pillar is plausible, the keyword does not belong to this portfolio and should be dropped rather than forced.

A cluster carrying fewer than four rows in the final plan is not viable. Fold it into its nearest neighbour, or cut it and redistribute the rows.

## Phase 2: the fixed type distribution

Thirty rows, to this composition, counted rather than approximated:

| Type | Rows | Why this count |
| --- | --- | --- |
| Pillar | 4 | One per cluster. These are the hubs and they are not optional. |
| How-to | 8 | The largest bucket, because instructional queries are the most numerous and the cheapest to satisfy properly. They are also the natural link donors. |
| Comparison | 6 | One per **real** competitor. Not one per competitor you can name. |
| Use-case | 6 | One per named segment or job, so the corpus has a page for a reader who knows their situation but not your category. |
| FAQ-cluster | 4 | Each aggregates five to twelve long-tail questions onto one page, rather than one thin page per question. |
| Round-up | 2 | Deliberately capped. Round-ups age fastest and cost the most to maintain. |

**The default is four clusters, which is why the pillar count is four.** If the portfolio genuinely resolves to three or five clusters, the invariant is that the pillar count equals the cluster count, the total stays at 30, and the difference comes out of or goes into the how-to bucket, which is the only bucket large enough to absorb it without distorting. Never split a cluster to reach four pillars, and never give a cluster two hubs to use up a spare row.

**What makes a competitor real.** A competitor earns a comparison row if it appears in the results for your main category term, or it is named unprompted by buyers in at least three separate conversations, support tickets or review-site comparisons. If you have fewer than six real competitors, do not invent the difference. Cap the comparison bucket at the number you actually have and move the surplus rows into use-case, which is the bucket with the most headroom. An invented comparison page is a permanent maintenance liability aimed at a query nobody types.

## Phase 3: demand bands, and the band sets the depth

Assign every row a band from its **measured** monthly search volume:

- **high**: 5,000 searches a month or more
- **medium**: 500 to 4,999
- **low**: below 500
- **unmeasured**: no volume figure from a data source

The band then sets the word-count target, and this is the binding that stops depth being a matter of mood:

- **high**: 2,200 to 3,000 words
- **medium**: 1,400 to 2,000 words
- **low**: 800 to 1,200 words

**Pillar override.** A pillar is 2,500 to 3,500 words regardless of its own band. Its job is to cover the whole cluster and hold the internal links, not to satisfy one query, so its depth follows the cluster rather than the term.

Record the band **with the number it came from**, plus the source and the date it was pulled. A band with no figure behind it is an opinion in a column that looks like data, and six weeks later nobody can tell the two apart. If there is no volume source at all, band every row `unmeasured`, leave the word-count targets empty rather than guessing them, and label the backlog unvalidated at the top of the file. Do not estimate a band. The band is load-bearing on depth, and a guessed band is how a topic with forty searches a month acquires a 2,500-word article.

## Phase 4: ordering, priorities 1 to 30

The priority number is both the schedule position and the sort key that the link rule in phase 5 depends on. Assign it deterministically:

1. **Priorities 1 to 4: the pillars**, ordered by total cluster demand descending, meaning the sum of the measured volumes of every keyword in that cluster.
2. **Priorities 5 to 24: comparison, how-to and use-case rows**, sorted by band descending, then by measured volume descending, then by row identifier ascending. Both tiebreakers, always, so two planning passes over the same portfolio produce the same order.
3. **Priorities 25 to 28: the FAQ-cluster rows**, late, because they harvest the questions the earlier rows surfaced and because they are the rows most likely to change shape once real articles exist.
4. **Priorities 29 and 30: the round-ups**, last regardless of volume, because a round-up links to its entries and its entries have to exist.

One additional constraint on step 2: **no more than three consecutive rows of the same type.** Six comparison pages in a row means six weeks in which nothing new links into the how-to spine, and a cluster where all the internal linking arrived at once and then stopped.

Round-ups being last is the rule most often argued with, because a round-up usually has the highest volume of any non-pillar row and the instinct is to publish it early. Publish it early and it either links to nothing, or it links forward, which phase 5 forbids.

## Phase 5: the ordering constraint on internal links

This is the load-bearing rule of the procedure.

**A row may only name link targets whose priority number is strictly lower than its own.**

The reason is that a backlog is a schedule and not a set. When row 12 is written, rows 13 to 30 do not exist. A link planned from row 12 to row 25 has exactly two outcomes, and both are bad. Either the writer notices and drops the link, in which case the internal link plan silently degrades and nobody records that it did, or the writer does not notice, constructs the URL from the slug sitting in the backlog, and ships a link that renders as a perfectly ordinary link and returns 404. That link will be found by a crawler months later rather than by anyone reading a run summary, because internal links on a page nobody has read yet do not generate complaints.

Four consequences follow from the constraint, and they are the reason the ordering in phase 4 is shaped the way it is:

- **The pillars occupy 1 to 4**, so every subsequent row can always legally link to its parent pillar. The parent pillar link is therefore mandatory on every non-pillar row.
- **The link budget grows across the programme.** Row 5 has exactly four legal targets. Row 30 has twenty-nine. Early rows being thin on internal links is correct, not a defect, because there is genuinely nothing yet to link to.
- **Every non-pillar row names two to four targets**: the parent pillar, plus one to three earlier siblings. Never more than four. Past that the anchor text starts repeating and the page reads as a directory.
- **The pillar's own downward links are not planned in the backlog at all.** A pillar is written at priority 1 to 4 with no children published, so it cannot link down without violating the rule. The resolution is that every non-pillar row carries the instruction `hub-update-owed`, meaning: after this row is published, add one inbound link to it from its parent pillar. That is an edit to an earlier page, made at a time when the later page exists. It is the only forward relationship the schedule permits, and it is expressed as maintenance rather than as a link written into a future article.

**Validating it is one pass of arithmetic.** For every row, the maximum priority among its link targets must be less than its own priority. When a row fails, you can move the row later or move the target earlier, and moving the target earlier is usually correct: a page that several rows want to link to is a page that deserves to be published sooner.

## Phase 6: the row schema

Sixteen fields. The test for whether the schema is sufficient is concrete: a writer picking up row 19 eight weeks from now, with no memory of the planning session and no access to a keyword tool, must be able to write it. Every field that fails that test causes a re-research, and re-research is where the plan quietly becomes a different plan.

1. **id**: stable, assigned once, never renumbered.
2. **priority**: 1 to 30. The schedule position and the link ordering key.
3. **cluster**: the cluster name.
4. **type**: one of the six.
5. **title**: as it will appear on the page. Not a topic label.
6. **focus_keyword**: exactly one, unique across the entire backlog.
7. **supporting_keywords**: pipe-separated, three to eight of them.
8. **demand_band**: high, medium, low or unmeasured.
9. **measured_volume**: the figure, plus source and date.
10. **intent**: informational, commercial, transactional or navigational.
11. **word_count_target**: the range, written as numbers rather than as a reference to the band.
12. **competition_note**: one sentence naming what the current leading result does and where the gap is.
13. **link_targets**: pipe-separated row ids, every one strictly lower priority.
14. **parent_pillar**: a row id, always one of 1 to 4.
15. **status**: planned, in-progress, published, blocked or retired.
16. **published_url**.

Plus a free-text **notes** column for anything that does not fit, which in practice carries the `hub-update-owed` flag and any `split-candidate` markers from the decision rule below.

## Phase 7: approve the first ten before planning the remaining twenty

Plan rows 1 to 10, render them as a readable table rather than as raw delimited text, and get an explicit yes before continuing.

The columns a human can actually judge are: priority, cluster, type, title, focus keyword, band and link targets. Do not show all sixteen at the gate, because a sixteen-column table is not reviewed, it is nodded at.

The reason for the gate is the cost curve. A wrong cluster boundary, a distribution error or a duplicated focus keyword is ten rows of rework if it surfaces at row 10, and thirty rows of rework plus an argument if it surfaces at the end. The first ten rows contain all four pillars, which is exactly the part where a structural mistake is both most likely and most expensive.

## Phase 8: freeze everything except two fields

Once approved, **status and published_url are the only mutable fields.** Every other field is frozen for the life of the backlog. A backlog whose titles drift from run to run is not a plan, it is a diary.

When a row genuinely must change, because the premise went stale or a competitor was acquired, do not edit it in place. Other rows point at its id. Instead:

- Set its status to `retired` with a reason in notes.
- Append a **new** row at the end with a **new** id, and give it the retired row's priority slot.

**The retire gate.** A row that is named as a link target by any already-published row may not be retired until either a replacement is published at the same or an earlier priority, or every published row naming it has been edited. Retiring a link target without doing one of those two things converts a live internal link into a 404, which is precisely the defect the whole ordering rule exists to prevent, arriving by the back door.

## Decision rule: does this keyword get its own row?

1. **Same intent, and the same page would answer both?** Fold it in as a supporting keyword on the existing row.
2. **Different intent on the same noun**, for example an informational "how X works" against a commercial "best X tools"? Two rows, two different types.
3. **Same intent, different audience, and the answer differs in more than the examples?** Two rows, both use-case type.
4. **Volume below 100 a month with no distinct intent?** Never its own row. It goes into the nearest FAQ-cluster row as one of its five to twelve questions.
5. **You cannot tell.** The two terms return overlapping but not identical leading results, or you have no volume data for one of them, or the difference is a synonym you cannot confirm the search engine treats as a synonym. In that case: **do not create the second row.** Fold it in as a supporting keyword and record `split-candidate: <term>` in notes. The asymmetry decides it. Splitting later costs one new row appended at the end. Merging later costs a redirect, a rewrite, a decision about which URL survives, and the loss of whatever either page had earned. Cheap error, expensive error, and they are not close.

## Worked example, compressed

A fictional product: a self-hosted uptime monitoring tool. The portfolio is 50 keywords with volumes pulled from a keyword data source in the same week.

**Clustering.** Four clusters emerge: monitoring fundamentals, alerting and escalation, self-hosting and operations, and migrating off a hosted monitor. Each takes one pillar. A fifth candidate cluster around status pages carries only two viable keywords, so it folds into monitoring fundamentals rather than becoming a hub with nothing under it.

**Distribution.** Four pillars, one per cluster. The comparison bucket is filled against six real competitor archetypes that buyers actually name: the market-leading hosted monitor, the open-source incumbent, the all-in-one observability suite, a cloud provider's own built-in checks, a status-page vendor that added monitoring, and a legacy on-premises tool. A seventh candidate is rejected because no buyer has ever named it. Six of six, so no overflow into use-case is needed.

**Banding.** Of the thirty rows, three band high, eleven medium, sixteen low. The lowest-volume row, at 90 searches a month, would have been a how-to. Under rule 4 of the decision rule it becomes one question inside an FAQ-cluster row instead.

**Ordering, first conflict.** A how-to lands at priority 9 and names a comparison row that sorted to priority 17. The check `max(17) < 9` fails. The comparison row is wanted as a target by four other rows, so it moves to priority 7. The how-to keeps 9. Now `max(7) < 9` holds.

**Ordering, second conflict.** The round-up "best self-hosted uptime tools" has the highest measured volume of any non-pillar row, 6,400 a month, and was drafted at priority 6. It names six targets, five of which sort later. Under phase 4 round-ups go last regardless of volume, so it moves to 29. Its band is still high, so its word-count target is still 2,200 to 3,000. Depth follows demand; position follows dependency.

**Caught at the approval gate.** Two rows in the first ten carry the focus keyword "self-hosted uptime monitoring": the self-hosting pillar, and a how-to. The pillar keeps it. The how-to is retargeted to a longer-tail variant that has its own measured volume of 320 a month, which rebands it from medium to low and drops its word-count target from 1,400 to 2,000 down to 800 to 1,200.

**Verdict: thirty rows at 4/8/6/6/4/2, four clusters each with exactly one pillar, zero forward link references, zero duplicate focus keywords, twenty-six rows carrying between two and four targets each, and a word-count target on every row derived from a recorded figure rather than from a preference.** The backlog is executable without supervision, because by the time any row is written every page it links to has already been published.

## Failure modes

**The pillarless cluster.** From the outside: a section where every page ranks for its own long tail and nothing ranks for the category term. Six children and no hub, so authority has nowhere to concentrate and the children have nothing to point at.

**Two hubs in one cluster.** Two pages both written as the definitive overview, both linked from everything, alternating positions in the results for the same query. This is cannibalisation planned in advance, and it is usually the result of a spare row being spent on a second pillar.

**The forward link.** A published article containing a link that renders normally and returns 404, discovered by a crawler months later rather than by anyone reading a summary. Caused by a link target with a higher priority number than the row that named it.

**The imaginary competitor.** A comparison page targeting a term nobody searches, against a product no buyer has considered, which nevertheless has to be re-verified every time either product ships a release. It never ranks and it never stops costing.

**The eyeballed band.** A 2,500-word page on a topic with forty searches a month sitting next to an 800-word page on a topic with eight thousand. The corpus is inverted: the depth is everywhere the demand is not, and every individual decision seemed reasonable at the time.

**The duplicate focus keyword.** Two rows in one backlog aimed at the same term, most often a pillar and a how-to. Nothing is visibly wrong until both are live, at which point neither ranks where either would have alone.

**The thin row.** A row reading "how to set up alerts, medium priority". The writer opens a keyword tool, researches it again, arrives at a slightly different angle, and the cluster the planner designed quietly stops existing one row at a time.

**The renumbered backlog.** Somebody sorts the sheet by title. Every link_targets field now points at a different row. The damage is total, silent, and shows up in a diff that looks like a tidy-up.

## What this skill does not do

- It does not do keyword research. It consumes a portfolio and will faithfully schedule a bad one.
- It does not write anything. Every row is a brief, and the competition note in particular is a placeholder for real work against the actual results page.
- It cannot see the live site, so it cannot tell you that row 22 already exists as a published page under a different title.
- It does not track rankings, indexation or traffic, and it has no mechanism for noticing that a published row underperformed. Nothing here reacts to results.
- It plans link targets, not links. Whether the anchor sentence reads naturally in the paragraph it lands in decides whether the link helps or reads as filler, and a person has to read that sentence.
- The arithmetic it depends on is better enforced by a script or a few spreadsheet formulas than by a model, permanently and without being reminded.
