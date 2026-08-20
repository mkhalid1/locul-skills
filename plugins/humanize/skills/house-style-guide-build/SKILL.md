---
name: house-style-guide-build
description: Produces a house style guide derived from an organisation's own published corpus rather than imported from someone else's. Counts roughly two dozen conventions across a sample of real work, applies split thresholds that separate conventions the corpus has already settled from ones that still have to be decided, splits candidate rules into mechanical rules with an executable check and judgement calls that ship as annotated example pairs, gives every rule an identifier plus a correct and an incorrect example drawn from real material, builds a separate term list covering product names and their variants, and writes the whole thing as a delta from a named public base guide with an enforcement plan and a review cadence attached. This skill should be used when there is no written guide, when a rename or a new writer has exposed that the standard lives only in one reviewer's memory, or before a prose linter is introduced.
---

# House style guide build

## The claim this skill is built on

A style guide is worth what its checks are worth, and its rules have to come from your own writing.

Two failures follow from ignoring either half. A guide written from general principles contains rules nobody can fail, so it is never cited except by a reviewer looking for cover for an objection. A guide imported wholesale contains hundreds of decisions your team has never read, so it is nominally adopted and actually ignored, and its existence blocks anyone from writing the twenty rules that would have mattered.

The corrective is unglamorous. Count first, decide second, write third, and put a check next to every rule you keep.

## Why the obvious approach fails

Ask for a style guide and what arrives is: be clear, be concise, prefer the active voice, write for your reader, avoid jargon, keep sentences short. Every one of those is true and none of them is a rule, because none of them can be failed by a specific sentence in a way two people would agree on.

Meanwhile the arguments that actually recur in review are about the serial comma, whether headings are sentence case, whether the interface verb is click or select, whether the feature is capitalised, and whether a list item ends in a full stop. A guide that omits all of those and covers clarity instead has settled nothing, and the cost is paid every review, forever, by whoever cares least about being right.

## Step 1. Choose the corpus by outcome, not by volume

Twenty to forty pieces, from the last eighteen months, spread across every surface you publish on: documentation pages, release notes, marketing pages, in-product strings, error messages, support replies, and outbound email.

Each constraint earns its place. **Eighteen months** because older material encodes decisions you have since reversed and names you have since changed, and a count that includes them measures your history rather than your practice. **Every surface** because a guide derived only from marketing pages produces rules that break error strings, and an error string is the surface where a violation is most visible and least editable: it has a character budget, it appears at the worst possible moment, and changing it may require a release.

Do not select the pieces you are proudest of. A corpus of the best twelve documents, all written by the same person, produces that person's guide with a sample attached for credibility.

## Step 2. Split the corpus into exemplars and rejects

Fifteen to twenty-five exemplars, and five to ten rejects. Annotate each reject with the single thing wrong with it, in one sentence.

This step is skipped almost universally, and it is why so many in-house guides are full of rules that cannot be violated. Without counter-examples from your own output, every rule you draft sounds reasonable, because you are testing it against prose you already like. The rejects are what tell you where the real boundary sits, and they supply the incorrect examples that every rule needs later.

## Step 3. Count, before you decide anything

Measure these across the corpus, per surface as well as in total. The inventory is the asset, and it is longer than people expect.

1. **Heading capitalisation**, sentence case against title case, counted per heading.
2. **Serial comma**, present or absent, in lists of three or more.
3. **Contraction rate** per thousand words, recorded separately per surface.
4. **Person and voice in instructions**: second person, first person plural, or agentless passive.
5. **Spelling variant set**: British or American forms, and the count of exceptions inside the majority.
6. **Numerals**: the threshold at which digits replace words, and whether it survives at the start of a sentence.
7. **Date format**, and whether any all-numeric ambiguous form appears anywhere.
8. **Time format**, and whether a time zone is ever stated.
9. **Units**: the space before a unit symbol, the per cent word against the symbol, and byte abbreviations.
10. **Product and feature names**: every observed spelling and casing of each. This is the single most valuable count in the exercise.
11. **Interface references**: bold, quoted, or plain, and the verb used, with counts for click, select, choose, tap and press. This one carries an argument that is not aesthetic: click assumes a mouse, tap assumes touch, and select covers keyboard, mouse and touch on Windows, macOS and mobile alike.
12. **Code and literal strings**: inline code formatting against quotation marks.
13. **Link text**: descriptive text, bare URLs, or the words here and read more.
14. **List items**: terminal punctuation, capitalisation of the first word, and whether items are fragments or full sentences.
15. **Latin abbreviations**: e.g., i.e. and etc. against for example, that is, and so on.
16. **Ampersands** outside proper nouns.
17. **Acronyms**: whether expansion on first use actually happens, counted per document rather than assumed.
18. **Callout labels in use**: Note, Tip, Warning, Caution, Important. More than four distinct labels is a taxonomy nobody applies consistently.
19. **Emoji**, observed rate per surface, which is routinely not what the team believes it to be.
20. **Currency, locale and number separators.**
21. **Version numbers and release naming**, including whether a leading v appears.
22. **Heading depth**, and the deepest level anyone actually uses.
23. **Trademark and legal suffixes**, and where they are required rather than habitual.
24. **Sentence and paragraph length per surface**, recorded as a baseline rather than as a rule.

On the last one: the fuller rhythm and voice fingerprint is a different instrument aimed at one author rather than an organisation, and it is not repeated here.

### The split thresholds

The counts tell you which job you are doing, and the two jobs use the same words for different work.

- **More lopsided than about 90 to 10.** The corpus has decided. Adopt the majority, and the minority becomes a fix list rather than a rule debate.
- **Between 90 to 10 and about 80 to 20.** A working convention with drift. Adopt the majority and attach a check, because without one it drifts back.
- **Closer than 80 to 20.** The organisation has not decided anything. You are legislating rather than describing, so the entry needs a one-line rationale and a date, because an undecided convention gets re-argued every quarter until it is written down with a reason someone can disagree with.
- **Fewer than about ten instances in the whole corpus.** You have no evidence. Take the base guide's answer and move on rather than inventing a house position from four examples.

**A split by surface is not a conflict.** If marketing runs fourteen contractions per thousand words and documentation runs two, that is two rules, one per surface, not an average that misrepresents both. Averaging across surfaces is the most common way a derived guide ends up describing nothing.

## Step 4. Split rules into mechanical and judgement

**The two-reviewer test.** Would two reviewers who have never spoken to each other reach the same verdict on a borderline case? If yes, the rule is mechanical: it can be a regular expression, a term list entry, a linter rule, or a count against a threshold, and it goes in the rules section with a check.

If no, it is a judgement rule, and writing it as a rule is a mistake. Judgement belongs in an annotated examples section: a before, an after, and one sentence saying what changed and why. For judgement, an example is a better instrument than a rule, because a reviewer can point at it and the argument ends, whereas a rule invites a debate about interpretation every time.

**The trap that produces bad guides.** "Use the active voice" reads mechanical, because you can search for a passive construction. It is not. Passive is correct when the actor is unknown, when the actor is irrelevant, and when the object is the topic of the paragraph, which in technical writing is often. Encoded as a mechanical rule it flags correct prose repeatedly.

**The false-positive rule.** Apply a candidate mechanical rule to twenty real instances from your corpus and count the flags that are wrong. More than two of twenty, and it is demoted to a judgement example. This is not fussiness. A rule that flags correct writing trains people to dismiss flags, and the dismissal transfers to the rules that were right, so one over-mechanised rule costs you the credibility of the whole file.

And rules with no possible check at all, such as avoid jargon or write for your audience, are decoration. Decoration is not harmless: it dilutes the rules that can be checked, and it lets any objection be justified after the fact by pointing at it.

## Step 5. The rule entry, six fields

Every rule ships with:

- **ID**, short and stable, such as ST-14.
- **Rule**, one imperative line.
- **Rationale**, one line, because a rule with no reason gets reopened.
- **Correct example**, from the real corpus where possible.
- **Incorrect example**, likewise.
- **Check**, the exact search, term entry, linter rule or count. If this field says human review, the rule is a judgement example that has not admitted it yet.
- **Exceptions**, named, including the disputed cases found during the false-positive count.

**Why both examples, always.** A rule with only a correct example does not say what it rules out, and every argument in review is about the boundary. The incorrect example is the boundary. Take it from real material: a fabricated one is usually a strawman, and the mistake your team actually makes is subtler than the one you would invent.

**Why identifiers.** So a review comment can say ST-14 and the argument ends rather than restarting from first principles. Identifiers also make the guide diffable, and they let you count which rules are ever cited, which tells you which to delete at the next review.

## Step 6. The term list is a separate artefact

Fields per entry: preferred term, variants to replace, capitalisation, whether it can be used as a verb, plural form, and a do-not-use list with a reason for each entry.

It is the highest-value output and the fastest to rot, because it is the only part of a guide that a product release can invalidate. So it lives closest to the code, is owned by whoever ships renames, and is reviewed on every release rather than twice a year. A stale term list is worse than no term list, because people follow it.

## Step 7. Write the delta, not the guide

Adopt a public base guide and write only your differences from it. Two serious free options are named in the alternatives on this page, and the choice between them is mostly about register.

Your deliverable then is: the base guide named and linked, twenty to forty overrides, the term list, and the annotated examples. **If the rules section runs past roughly two thousand words, nobody reads it,** and compliance falls back to whatever the reviewer remembers, which is the state you started in. The public guides run to hundreds of pages and work anyway, because they are searchable reference material. Yours is an onboarding document, and onboarding documents are read once.

## Step 8. The enforcement plan is part of the guide

Every mechanical rule maps to a named check with an owner: a linter rules file in the repository, a term list in the spell checker, a continuous integration job, a pull request template item, or a named manual review step. A rule with no owner becomes an opinion the first time someone objects to it.

Cadence: term list on every release, rules twice a year, corpus refreshed annually. Keep a changelog with dates, because a rule with no date cannot be argued with and therefore cannot be improved.

Placement matters too. A rules file next to the content beats a wiki page, because a wiki page is opened when somebody remembers it exists.

## The decision rule, with the branch for when you cannot tell

For each candidate rule:

- **Two reviewers would agree on a borderline case.** Mechanical. Write the check.
- **They would not.** Judgement. Write an example pair instead, and do not phrase it as a rule.
- **You cannot tell.** Run it against twenty real instances from the corpus and count the disputes. More than two disputed, it is judgement. Two or fewer, it is mechanical and the disputed cases become named exceptions. Do not skip to a decision on instinct here: the whole point of the count is that instinct about checkability is unreliable, which is why active voice keeps ending up in rules sections.

## Worked example, compressed

**Situation.** A company with a documentation site, a marketing site and a support inbox. Corpus of 28 pieces: 14 documentation pages, 6 release notes, 4 marketing pages, 4 support macros, all from the last 18 months. Five of the 28 are marked as rejects. All counts below are invented to show the shape.

**Counts and what they decided.**

- Heading case: sentence 61, title 39. Closer than 80 to 20, so nothing has been decided. Legislated: sentence case, rationale that it matches the chosen base guide and removes a per-heading judgement. Dated.
- Serial comma: present 88, absent 12. In the drift band. Adopted, with a linter rule, because it will drift back otherwise.
- Interface verbs: click 140, select 96, choose 22, tap 4. Undecided by the count. Legislated as select, on the platform-neutral rationale rather than an aesthetic one. Noted cost: this creates a fix list of 166 instances, which is a real piece of work and is recorded as such rather than pretended away.
- Contractions: 14 per thousand words on marketing, 2 in documentation. Recorded as two rules by surface, not averaged.
- Product names: one feature found under five distinct spellings. Straight into the term list, along with 45 other entries.
- Callout labels: 7 distinct labels in use. Reduced to 3, each with a one-line definition of when it applies, because seven cannot be applied consistently by anyone.
- Emoji: believed to be zero, found on 3 of the 4 marketing pages. Decision required, and it is a decision, not a finding.

**The demotion check.** Two candidate rules were tested against twenty instances each. A rule banning the passive voice produced six wrong flags out of twenty and was demoted to an annotated example pair. A rule requiring descriptive link text produced one and was kept as mechanical.

**Verdict.** 29 mechanical rules, of which 19 map to linter rules and 6 to term list entries, plus 8 judgement examples and a term list of 46 entries. The whole thing is written as a delta from a named public base guide, the rules section runs to about 1,300 words, and the enforcement plan names an owner for each check and puts the term list on the release checklist. Two open items are recorded as unresolved with the names of the people who have to settle them, rather than being written up as though the corpus had decided.

## Failure modes

**The imported guide.** Adopted wholesale, never read, quietly ignored, and its existence prevents anyone writing the twenty rules that would have mattered.

**The undecidable rule.** Write clearly. Cannot be failed by any specific sentence, so it is cited only when a reviewer needs cover for an objection they had already formed.

**The rule with no counter-example.** Silent exactly where every argument happens, which is the boundary.

**False-positive fatigue.** One over-mechanised judgement rule flags correct prose often enough that people stop reading flags, and the credibility loss spreads to the rules that were right.

**Describing the split instead of deciding it.** Writing "both forms are acceptable" for a 55 to 45 count. That is a report, not a guide, and it guarantees the argument recurs next quarter.

**The stale term list.** A rename shipped and the guide did not change, so the guide is now actively wrong, which is worse than absent because people follow it.

**Marketing rules applied to interface strings.** Contractions and second person are right on a landing page and wrong in a truncated string with a character budget and a translation cost per word.

**The guide nobody can reach.** It lives in a wiki nobody opens rather than beside the work, so it is consulted only by people who already agree with it.

**No owner.** Every rule reverts to an opinion at the moment somebody senior objects, and the guide loses the argument it was written to end.

**Length inflation.** The guide grows into reference material without acquiring the search that reference material needs, and compliance quietly reverts to memory.

**Prose rules with no identifiers.** A review comment has to restate and re-justify the rule every time, so the guide never actually shortens a review.

**The flattering corpus.** Twelve hand-picked pieces by one writer, which produces that writer's preferences with a sample attached for credibility.

## What this skill does not do

- It does not enforce anything. No linting, no continuous integration, no commit hook, and a guide with no enforcement decays within a couple of release cycles.
- It cannot see your product. The term list is a draft until a person checks every label against a running build on both Windows and macOS, where the same control is sometimes named differently.
- It does not rewrite the corpus. The fix list it produces is an estimate of that job, not the job, and the estimate is usually larger than anyone expects.
- It cannot decide a taste question with authority. It can force the decision to be explicit, dated and attributed, which is most of what is missing.
- It does not handle localisation. A terminology base and a translation style guide are separate artefacts, and rules derived from one language rarely survive the crossing.
- It does not know your legal, regulatory or trademark wording obligations, which override everything here and appear nowhere in any count.
