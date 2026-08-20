---
name: literature-search-strategy
description: Designs a literature search as a reproducible artefact rather than typing a query into one search engine. Builds concept blocks joined with AND and synonyms joined with OR, combines controlled vocabulary such as subject headings with free text because each catches what the other misses, handles truncation and phrase and field restriction traps, records a protocol with databases and exact strings and dates and counts so the search can be rerun, sets inclusion and exclusion criteria before screening begins, runs backward and forward citation snowballing, covers grey literature and the bias caused by omitting it, and includes a scaled-down two-hour version for a commercial question. This skill should be used before the first query is typed for any question whose answer someone will rely on.
---

# Literature search strategy

## The claim this skill is built on

A search is a designed artefact with a written record. A query is what you type when you have not designed one.

The difference shows up in one place: whether anyone can say what the search missed. A designed search has a stated scope, stated sources, stated terms and a stated stopping point, so its gaps are known even when they are large. A query has an unknown miss rate, and unknown is worse than large, because you cannot report it, cannot budget for it, and cannot tell whether the absence of evidence you are about to write about is a property of the literature or a property of your search box.

Most of what follows is the full apparatus, which is built for a review. **If you have two hours and a commercial question, skip to the last section before the worked example.** That is the version most readers need and following the rest of it will cost you the afternoon for no gain.

## Why one search engine is not a search

Databases index different things, and they differ in ways that are not visible from inside any one of them.

They differ in **which journals they cover**, in **how far back they go**, in **whether they index conference proceedings, theses, reports and preprints**, in **whether they index abstracts or full text**, and in **whether they apply a controlled vocabulary at all**. Two databases in the same discipline routinely return substantially different result sets for equivalent queries, and neither is wrong.

There is a second problem with using a general academic search engine as your only source. Its result counts are not reproducible, its ranking is opaque, it caps how many records you can page through, and it will not give you the full result set in an exportable form. That makes it excellent for finding a specific known paper and unsuitable as the spine of a search you intend to report, because the central promise of a reported search is that someone else can rerun it and get the same records.

**The rule.** At least two indexed databases with genuinely different coverage, plus at least one source for grey literature. One is not a search, it is a lookup. If your field has a subject-specific database, it is nearly always the highest-yield of the set and the general ones are the supplement, not the other way round.

## Building the query

**Concept blocks.** Decompose the question into two to four concepts. One block per concept. Inside a block, synonyms joined with OR. Between blocks, AND.

```
(concept A term OR synonym OR synonym OR heading)
AND
(concept B term OR synonym OR synonym OR heading)
AND
(concept C term OR synonym OR synonym OR heading)
```

Three blocks is the usual sweet spot. Four blocks is where searches quietly become too narrow, because every added AND multiplies the chance that a relevant paper failed to use one of your words. If a block exists only to make the results tidier rather than to define the question, delete it and screen the extra records instead.

**Populating a block.** Include the term the field uses now, the term it used ten years ago, the plural, the noun and verb forms, the British and American spellings, the abbreviation and the expansion, and any brand or product name where the generic term did not exist first. Terminology drift across a decade is a leading cause of a search that misses the foundational work.

**Truncation and wildcards.** A truncation symbol, usually an asterisk, matches word endings: `randomis*` retrieves randomise, randomised, randomisation. Three traps:

- **Over-truncation.** `cat*` matches cattle, catheter and catastrophe. Truncate at the longest stem that is still unambiguous, and read the first page of results specifically to see what the truncation dragged in.
- **Left truncation is frequently unsupported.** Matching word beginnings is not available in many interfaces, which matters for compound terms.
- **Truncation can switch off automatic mapping.** In at least one major biomedical interface, using truncation, quotation marks or a field tag disables the automatic term mapping that would otherwise have expanded your word to its subject heading. The search silently becomes narrower than the same words without the asterisk. Check the current help page for the interface you are in, because this behaviour is interface-specific and it changes.

**Phrase searching.** Quotation marks force adjacency. Useful for a term that is meaningless when split, harmful when the field writes it two ways. Where an interface supports a proximity operator, terms within a stated number of words of each other is usually a better instrument than a rigid phrase, and proximity syntax differs in every interface.

**Field restriction.** Restricting to title and abstract is precise and misses papers where the concept is present but not in the abstract, which is common for a secondary outcome or a method. Full text searching is recall-heavy and returns papers that mention your term once in a discussion. The usual answer is title and abstract plus subject headings for the main run, with a separate full text pass only when the main run returns implausibly little.

## Controlled vocabulary

A thesaurus of subject headings is a fixed list of terms that human or machine indexers assign to each record, regardless of the words the authors used. The best known is MeSH, the Medical Subject Headings maintained by the US National Library of Medicine, which is the example most people will meet. Embase has its own thesaurus, and other fields have their equivalents, including subject classification schemes used by the computing and engineering indexes.

Why it matters: a heading catches every paper about your concept, including the ones that called it something else entirely. That is the single biggest recall gain available.

Three operational points:

- **Headings usually explode.** Searching a heading normally includes all its narrower terms automatically. That is what you want by default, and there is syntax to switch it off when a narrower branch is genuinely irrelevant.
- **Indexing lags publication.** A record entered last month may not have headings assigned yet, so a heading-only search systematically misses the newest work, which is often the work you most want.
- **Not everything is indexed.** Records outside the indexed set, in the wrong database, or from grey sources carry no headings at all.

**The rule: use both.** Controlled vocabulary catches the papers that used a different word. Free text catches the papers that have not been indexed yet and the sources with no vocabulary. Each covers the other's blind spot, and a search using only one has a known, avoidable gap.

## The written protocol

Record all of this, in a file, as you go rather than afterwards:

- Every **database and interface**, since the same database behaves differently through different interfaces.
- The **exact query string** for each, copied and pasted, not paraphrased. Strings do not port between interfaces and a paraphrase cannot be rerun.
- The **date each search was run.** Databases update continuously, so a count without a date is not a fact about anything.
- **Every filter applied**: date range, language, publication type, and the reason for each. A language restriction is a decision with a bias attached, not housekeeping.
- The **number of results at each stage**: retrieved, after deduplication, after title and abstract screening, after full text, included.
- Any **deviation** from the plan, and when it happened.

The reason is not bureaucracy. A search that cannot be rerun cannot be updated when someone asks the same question next year, and it cannot be checked when someone disputes the conclusion. Those are the two things that actually happen to research, and both of them fail on a protocol that says the search was performed in the usual databases.

## Screening

**Write the inclusion and exclusion criteria before screening starts.** This is the single ordering rule that changes the result, and the reason is not discipline, it is that criteria written afterwards are written with knowledge of which papers they would admit. Once you have seen a paper you want to include, you will write a criterion that includes it.

Criteria should be checkable by someone else: population, intervention or exposure, comparator, outcome, study design, timeframe, language, setting. Each one either applies to a record or does not.

Then screen in two passes:

1. **Title and abstract.** Fast, generous, aimed at excluding only what is clearly out.
2. **Full text.** Slow, aimed at applying the criteria properly. Record a reason for every exclusion at this stage, because those reasons are reported.

**The decision rule at title and abstract:**

- Clearly meets the criteria, or clearly might. **Include for full text.**
- Clearly fails a stated criterion. **Exclude**, and note which criterion.
- **You cannot tell from the title and abstract.** **Include for full text.** Never resolve ambiguity by excluding. The cost of an unnecessary full text read is a few minutes; the cost of a wrongly excluded paper is invisible and permanent, and it is invisible precisely because nobody ever finds out.

Where two people screen independently and resolve disagreements, do that. Where only one person is available, screening a sample twice and comparing your own two passes will tell you how noisy your criteria are.

## The flow record

Report how many records were found, how many remained after duplicates were removed, how many were screened out at each stage and why. This is the part that lets a reader see the shape of the evidence rather than only its conclusion, and it is where an inconsistency between stages becomes visible.

**PRISMA**, the Preferred Reporting Items for Systematic Reviews and Meta-Analyses, is the published standard for this. Its current version, PRISMA 2020, carries a 27 item checklist and the flow diagram most people recognise, and there is a dedicated extension covering how to report the search itself. It is free and authoritative, and it is designed for systematic reviews. **For most work it is overkill**, and using the diagram for a two-hour search is theatre. What is not overkill at any scale is the underlying discipline: numbers at each stage that reconcile, and a reason recorded for every full text exclusion.

Deduplicate by persistent identifier first, then by title, first author and year for the records that have no identifier. Duplicate rates between overlapping databases are high enough that a count taken before deduplication is not a meaningful number.

## Snowballing, in both directions

**Backward.** Read the reference lists of the papers you have included. This finds the foundational work that your terms missed because it predates the vocabulary the field settled on.

**Forward.** Find everything that cites each key paper, using a citation index. This is the step people skip and it is the one that finds what keyword search structurally cannot: a paper that engages with the same idea using none of your words, a paper in an adjacent field with different terminology, or work on a concept that has no agreed name yet and is referred to only as the thing that paper described. No keyword can reach those, because the keyword does not exist in them. The citation link does.

Do both from your best three to five papers, then repeat once on anything new that qualifies. A second full round rarely earns its cost.

## Grey literature

Grey literature is everything not published through commercial channels: theses and dissertations, conference abstracts and proceedings, preprints, government and agency reports, standards, trial and study registries, regulatory submissions, working papers and technical reports.

**Omitting it biases the result in a known direction.** Studies with positive or striking findings are more likely to be written up and published than studies that found nothing, so a literature restricted to published journal articles over-represents positive findings. If your question is whether something works, grey literature is not optional, because the studies that found it did not work are disproportionately in it.

Where to look: preprint servers for your field, institutional repositories and the aggregators that harvest them, trial and study registries, the publications pages of relevant government departments and agencies, conference proceedings, professional body publications, and thesis collections. Grey sources are poorly indexed by design, so expect to browse rather than to query, and expect the searches to be less reproducible. Record what you did anyway.

## Knowing when to stop

Saturation is an observable condition, not a feeling. Declare it when all three hold:

1. The last thirty to fifty records screened produced no new inclusions.
2. Adding a further database produced no unique inclusions, only duplicates.
3. Backward and forward snowballing from the included set returned only records you already have.

If you are still finding new inclusions, you have not saturated, however tired you are. If you saturated after twelve records, your search is probably too narrow rather than the literature being thin, and the check for that is whether you can name three papers you know exist and confirm the search retrieves them.

## The scaled-down version, which is the one most readers need

For a commercial question with two hours behind it and no review at the end. Everything above still applies in principle and almost none of it applies in practice. Do this instead:

1. **Write the question and the inclusion criteria in three lines, before searching.** This costs two minutes and it is the highest-value step in the whole method. Everything else here is optional and this is not.
2. **Two sources with different coverage, plus one grey source.** For a commercial question that usually means one academic index, one industry or standards source, and one place where practitioners write, such as a conference proceedings archive or a preprint server.
3. **Concept blocks still, but two or three synonyms each and no thesaurus work.** Write the string once, keep it, paste it into both sources.
4. **Cap the reading.** Twenty to twenty-five records per source, screened on title and abstract only. If nothing relevant appears in the first ten, the terms are wrong, so fix the terms rather than reading further.
5. **Do one forward citation pass on the single best thing you find.** This is the highest yield per minute in the entire method and it takes about ten minutes.
6. **Keep a five line log**: sources, exact strings, date, counts, and what you excluded and why. Five minutes, and it converts a search you cannot repeat into one you can.
7. **Stop when the last ten records return nothing new**, and write down that this was the stopping condition, so the next person knows how thorough the search was rather than guessing.

What you give up is a defensible recall claim. State that in the output: this was a scoped search of two sources on a stated date, not an exhaustive one. That single sentence is the whole difference between honest scoped work and an implied completeness you did not earn.

## Worked example, compressed

**Question.** Does pair programming reduce defect density in professional software teams, as opposed to in student cohorts? All counts below are invented to show the shape.

**Criteria, written first.** Include: empirical studies, professional or industrial settings, reporting a defect or quality outcome, any year, English. Exclude: student-only populations, opinion pieces, studies with no comparison condition.

**Concept blocks.**

```
("pair programming" OR "pair-programming" OR "collaborative programming" OR "peer programming")
AND
(defect* OR fault* OR bug* OR "code quality" OR "software quality" OR error*)
AND
(industr* OR professional* OR commercial OR practitioner* OR "real world")
```

The third block is the risky one: it exists to separate professional settings from student cohorts, and it will exclude relevant papers that never used any of those words. Decision taken and recorded: run the search both with and without block three, and screen the difference rather than trusting it.

**Sources.** Two computing indexes with different coverage, one of which indexes conference proceedings heavily, since in this field the conference literature is primary rather than supplementary. Plus a preprint server and a thesis repository for grey literature. Controlled vocabulary here is a subject classification scheme rather than a medical thesaurus, and the principle is identical: add the classification terms alongside the free text.

**Counts, all invented.** Retrieved 412. After deduplication term 289. After title and abstract screening 41. After full text 12 included. The run without block three added 6 more records, of which 2 were included, which retrospectively justifies having screened the difference rather than trusting the filter.

**Snowballing.** Backward from the 12 included papers surfaced 3 older studies that predate the term pair programming entirely and describe the practice under another name. Forward citation searching from the two most cited included papers surfaced 4 recent industrial reports, none of which use the word defect anywhere in the title or abstract, so no keyword search using the stated blocks could have reached them.

**Stopping.** The final 38 records screened produced no new inclusions, the fourth source added only duplicates, and one further snowball round returned nothing new. Saturation declared and recorded.

**Verdict.** 19 included studies, a search protocol that can be rerun by pasting four strings on a stated date, and a stated known gap: no non-English literature and no access to two paywalled full texts, both recorded as unretrieved rather than as excluded.

## Failure modes

**Searching before writing the criteria.** The criteria then get written around the papers you have already decided you like, and nobody afterwards can tell that happened.

**One database.** The miss rate is not small or large, it is unknown, and unknown cannot be reported.

**A general academic search engine as the spine.** Results are not reproducible, the count changes, the full set cannot be exported, and the search cannot be rerun, which defeats the point of writing it down.

**Free text only, or headings only.** Free text only misses the papers that used another word. Headings only misses everything indexed recently and everything not indexed at all.

**Over-truncation nobody checks.** A stem too short quietly pulls in an unrelated field, and because the count goes up rather than down it reads as thoroughness.

**Too many concept blocks.** Each AND is another chance a relevant paper failed to use one of your words. Four blocks is usually one too many.

**Excluding on ambiguity.** Resolving a title you cannot classify by leaving it out. The cost is invisible, which is exactly why it keeps happening.

**Recording a paraphrase of the string.** Searched the usual databases for pair programming and defects is not a protocol. It cannot be rerun, so it cannot be checked.

**Skipping forward citation searching.** The one step that reaches papers your keywords structurally cannot, and it is usually the first thing cut for time.

**Treating grey literature as optional for an effectiveness question.** It is where the studies that found nothing disproportionately live.

## What this skill does not do

- It does not run the search. It has no subscription to any indexed database, so the strings have to be executed by someone with access.
- It does not know your field's current terminology as well as someone working in it, and terminology is most of what decides recall.
- Its syntax is not authoritative. Operators, field tags, truncation rules and proximity syntax differ by interface and change over time, so every string needs checking against the current help page.
- It does not assess the studies it finds. Risk of bias, study quality and whether the evidence supports the conclusion are separate tasks with their own frameworks.
- It cannot deduplicate or screen at scale. Purpose-built screening software does that better and keeps the counts reconciled.
- It cannot see behind a paywall, so records it finds may be unretrievable, and unretrieved is a different thing from excluded and must be reported as such.
