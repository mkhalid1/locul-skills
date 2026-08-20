---
name: style-guide-conformance
description: Audits a documentation set or a piece of copy against a style guide, and builds the decision list a guide needs when there is not one yet. Covers the roughly twenty mechanical decisions a guide must settle, a terminology list with one name per concept and its deprecated synonyms, the split between mechanical findings and judgement ones, a situation-by-tone table including error and billing messages, inclusive language as named substitutions rather than as a principle, and the rule for recording a deliberate exception. This skill should be used when auditing documentation for consistency, when writing or revising a style guide, or before a corpus goes to an editor or to translators.
---

# Style guide conformance

## The claim this skill is built on

A style guide that does not decide the small things will be relitigated in every single review.

The obvious approach is to write a voice and tone document. Three paragraphs about being friendly but not casual, clear but not blunt, expert but not condescending. Everyone agrees with it, nobody can apply it, and the next pull request contains a comment asking whether headings take title case. The document decided nothing, so the argument is still live, and it will be had again by different people next month at full price.

The decisions that consume review time are almost all mechanical, and they are enumerable. That is the whole idea here: replace the parts of a style guide that read as opinion with a list of settled answers, then make as many of those answers countable as possible, so a machine reports the exceptions and a human only reads the ones that need a person.

Two consequences follow, and both are uncomfortable. The first is that consistency beats correctness, which means the better choice applied half the time loses to the worse choice applied everywhere. The second is that a synonym introduced for variety, which in most prose is a virtue, is a defect in documentation.

## The decision list

A guide that does not answer each of these will have the question asked again. Answer them in one line each. The reasoning is optional and usually not worth writing down.

1. **Heading capitalisation.** Sentence case or title case, and if title case, which one, since the rules differ on words of four letters or fewer and on subordinating conjunctions. Also whether headings take terminal punctuation.
2. **The serial comma.** Yes or no. There is no third answer and no case-by-case.
3. **British or American spelling**, and the specific families where it bites: the -ise and -ize endings, -our against -or, -re against -er, -ogue against -og, licence and license, practise and practice, defence and defense, and the doubled consonant in words like travelled. Note the traps: Oxford spelling pairs British English with -ize, and several technical words do not vary at all, so a computer program is a program in both, and a disk is a disk. **The overriding rule: prose follows the guide, identifiers never do.** A field named `color` stays `color` in a British-spelling document.
4. **Numbers.** Words or digits, and the threshold. A common answer is to spell out zero to nine and use digits from 10 upwards, with digits always used alongside a unit, in a version, in an interface value and in a measurement. Plus the two edge rules: never begin a sentence with a digit, and do not mix forms inside one comparison.
5. **Date format.** ISO 8601 for anything machine-adjacent or sortable, and one prose form chosen from day-first or month-first. Ban the all-numeric slash form outright, because it is ambiguous between two very large audiences and there is no way for a reader to tell which one they are looking at.
6. **Time and time zone.** Twelve or twenty-four hour, whether a leading zero appears, and the rule that a time without a zone is incomplete. Name the zone and give the offset, and use UTC in logs and API examples.
7. **How to refer to the product.** With or without a definite article, how it is capitalised, whether it may be used as a verb or pluralised, and whether it takes a possessive.
8. **How to refer to the reader.** "You" in anything user-facing, and "the user" only when writing about a third party. Decide separately whether "we" is allowed for actions the product takes, which is where most of the disagreement actually is.
9. **Contractions.** Allowed or not. This is the single most relitigated tone item and it takes one line to settle.
10. **Keyboard shortcuts.** The joining character and its spacing, key name capitalisation, and the platform rule: which modifier on Windows, which on Mac, and whether both are written every time or once at the top of the page. Both platforms get a real answer, not a note that they differ.
11. **File paths.** Whether they take code formatting, which separator, whether directories take a trailing slash, how the home directory is written on each platform, and how a variable segment inside a path is marked.
12. **Menu paths.** The separator character and whether it is spaced, and whether menu names take bold or code formatting. Pick one and never mix.
13. **Buttons and fields.** Bold, quoted or plain, plus the rule that the label is reproduced exactly as it appears in the interface, including its capitalisation, even where that capitalisation breaks the guide.
14. **Click, select, choose, tap or press.** Decide by input device neutrality. A workable answer: press for keys, select for options and controls in general, click or tap only when the pointing device genuinely matters.
15. **Inline code against a code block.** Which things take backticks: identifiers, literal values, file names, commands, HTTP methods, status codes. And the threshold at which a snippet becomes a block, which is usually more than one line or any command carrying flags.
16. **List punctuation.** Full stops on every item or on none, capitalisation of the first word, and the rule that decides it, which is normally whether the items are sentences or fragments. Also whether a conjunction appears before the final item.
17. **Ranges.** An en dash with no spaces, or the word "to". The rule that catches people out: a range introduced with a preposition, as in "from 5 to 10", never takes a dash.
18. **Currency and units.** Where the symbol sits, whether a currency code is required when more than one currency exists, which thousands separator is used, and the space between a number and its unit. The SI convention puts a space before a unit, so 10 MB rather than 10MB, while the percent sign is conventionally closed up. Decide byte units too, since MB and MiB are different numbers.
19. **Feature name capitalisation.** The sneakiest item on the list: whether the dashboard is a dashboard or a Dashboard. Whichever you choose, it belongs in the term list, not in someone's memory.
20. **Abbreviations and Latin.** Whether an abbreviation is expanded on first use per page or per document, and whether to allow "e.g." and "i.e." or require "for example" and "that is". Non-native readers and screen readers both do better with the expansion.
21. **Link text.** No "click here", no bare URL inside prose, and a rule on whether link text must match the title of its destination.

## Terminology consistency, the highest-value item

Everything above is worth doing. This is worth doing first.

Build a term list where **each concept has exactly one name.** For each entry: the term, a one-line definition, its part of speech, its capitalisation, its plural, and the deprecated synonyms listed right beside it with a "use instead" instruction. Thirty to sixty entries covers most products, and the first ten come straight from the interface labels rather than from anyone's preference.

**A synonym introduced for variety in prose is a bug in documentation.** In an essay, calling the same thing a job, a task and a run is good writing. In documentation it is three failures at once: search stops working, because a reader looking for "task" misses every page that says "job"; translation memory fragments, so the same sentence gets translated three times and diverges; and, worst, the reader assumes two names mean two things, and starts looking for the difference between a job and a task. They will find one, because they will invent it.

Two rules keep the list honest. **The interface wins.** Where the documentation and the product disagree on a name, either the documentation changes or a ticket is filed against the product, and never document an aspiration. And **a renamed feature enters the list as a deprecated synonym on the day of the rename**, not later, because that is the moment the old name exists in dozens of pages simultaneously.

## Why consistency beats correctness here

Two defensible choices applied consistently beat the better choice applied half the time, and this is not a compromise, it is what readers actually experience.

Nobody notices the serial comma. They notice it appearing on page 3 and vanishing on page 4, because a difference in a text reads as a difference in meaning. That is the same instinct that makes the job-and-task problem expensive, and it costs the reader attention they were spending on your product.

The second argument is about cost. Every undecided item gets re-argued in every review, by people whose attention is finite. Reviewer attention spent on capitalisation is attention not spent on the incorrect default value three lines below it, and that trade is being made silently, in every review, right now.

So the guide's job is to end the argument, not to win it. Record the choice. Record the reasoning only where it is one short sentence.

## The conformance pass, as a mechanical procedure

1. **Extract the checkable rules** from the guide into a rules file: a pattern and an expected form for each. Anything that cannot be expressed that way goes straight to the judgement queue and is not pretended to be mechanical.
2. **Exclude the untouchable regions first:** code blocks, inline code, quoted error strings, interface labels, third-party names and anything inside a block quote. Doing this second instead of first is how a conformance pass breaks a product.
3. **Run the rules as counts, not as a list of hits.** Report the shape: how many headings exist and how many are in each case, how many dates take each form. Counts tell you something a hit list cannot, which is whether the corpus or the guide is the odd one out.
4. **Treat a lopsided count as evidence about the guide.** If the guide says title case and 90 per cent of the corpus is sentence case, the cheap fix may be to change the guide.
5. **Check five hits by hand before any mass fix.** A rule producing more than roughly 50 hits on its first run is either a real systemic gap or a badly written rule, and the two look identical in a summary.
6. **Fix mechanically, one commit per rule,** so a rule that turns out to be wrong can be reverted without unpicking anything else.
7. **Keep the judgement queue under about twenty items.** A longer queue is one that never gets worked, and its contents quietly become permanent.
8. **Record the rule count and the date of the pass** at the top of the guide, so the next person knows what has already been enforced.

**The decision rule for a single finding:**

- The guide decides it and the text disagrees → mechanical fix.
- The guide is silent and the corpus is consistent → adopt the corpus convention, write it into the guide, change nothing in the text.
- The guide is silent and the corpus is split → judgement queue. Decide once, write it down, and it is mechanical from the next pass onwards.
- **You cannot tell whether the text is a quotation, an identifier or an interface label** → do not touch it, and flag it for whoever owns it. Rewriting a literal string is a worse defect than an inconsistent one, because an inconsistent document is untidy and an altered literal is wrong.

## Voice and tone, treated separately

Voice is constant: it is who the product sounds like, and it does not change between a welcome screen and a payment failure. Tone varies with the reader's situation. The useful artefact is not an essay, it is a small table of situations and the tone for each: first-run and onboarding, reference, an empty state, a success confirmation, an error, a billing message, a deprecation notice, and an incident update. Eight rows, one line each.

**Two rows are always got wrong.**

**The error state.** One line is enough here, because the full rule belongs to the error message writing skill, which sets out the three parts every error has to carry: humour and apology are both wrong, and an error with no next action is unfinished.

**The billing message.** Warmth reads as manipulation the moment money is involved. Be plain and exact: the amount, the date, the card, the consequence and the action. No euphemism for a failed payment, no softening of a price rise, and never an upsell inside a failure notice. This is the row where a friendly product voice does the most damage, because the reader is already suspicious and the tone confirms it.

## Inclusive language, as specifics

Handle this as a list of substitutions, not as a paragraph about respect, because a paragraph cannot be checked and a list can.

**Settled, as of August 2026.** Allowlist and blocklist rather than the colour terms. Primary and replica, or main and secondary, rather than the ownership pair. Main as the default branch name. Singular "they" for a person whose gender is unknown. "Everyone" or "all" in place of "guys". A quick check or a confidence check rather than a sanity check. A placeholder rather than a dummy value. Carried over or legacy rather than grandfathered. Cancel or stop rather than abort in user-facing copy, while noting that abort survives in plenty of APIs and stays there.

**Contested, and worth deciding once rather than per review.** Whether to rename identifiers in existing APIs, where the inclusive change and the breaking change are the same change. Blind in the sense of a blind test, which is a term of art. Master in senses unrelated to ownership. Person-hours against man-hours. Disabled as an interface state, which is settled as fine for a control and avoided for people.

**The accessibility-adjacent ones, which get missed most often.** Never use "see below" or "as shown above" as the only pointer, because reading order is not spatial for everyone; name the section and link it. Never let colour carry meaning alone, in prose or in an interface. Cut "simply", "just", "easy" and "obviously", which do nothing for a reader who is succeeding and tell a stuck reader they are the problem. Make link text meaningful out of context, since screen reader users list links in isolation. Give decorative images empty alt text as a decision, and describe the point of an informative image rather than its appearance.

## The escape hatch

There are five cases where breaking the guide is correct: the rule makes the sentence wrong or unreadable; the text is a verbatim quotation, an error string or an interface label; an identifier in code disagrees with the prose convention, in which case the code wins; a legal or regulatory phrase has a fixed form; or a third-party brand has a fixed form you do not control.

**The requirement is to record it.** One line in an exceptions register: the location, the rule and the reason. This is not bureaucracy, it is the only thing that stops the exception being "fixed" by the next contributor, reverted by the person who made it, and re-fixed a quarter later.

The register earns its keep a second way. **A rule that accumulates more than about five exceptions is a wrong rule**, and the register is the evidence for changing it.

## Worked example, compressed

A 40-page documentation set for a hosted job scheduler, with a one-page style guide that covers voice, the serial comma and nothing else. All counts below are invented for this example.

**Counts from the mechanical pass.** 212 headings: 148 sentence case, 64 title case, and the split runs by author rather than by section. 39 dates: 22 in ISO form, 11 month-first, 6 in the ambiguous all-numeric form. 84 keyboard shortcut references, of which 19 give a Mac modifier only. 27 instances of "click" and 14 of "select" for the same action. 61 list items ending in full stops out of 190. Currency appears in three formats across the billing pages.

**Terminology.** The same object is called a job, a task and a run, across all three of the tutorial, the reference and the interface. The interface says "run". 46 instances of the two other names.

**Mechanical findings:** the six ambiguous dates, the 19 Mac-only shortcuts, the list punctuation, the currency format. All fixable without reading for meaning, one commit per rule.

**Judgement queue, eleven items:** heading case for the whole corpus, click against select, contractions, whether the product name takes an article, and the term list.

**Excluded and not touched:** 14 apparent violations inside code blocks, and 3 interface labels whose own capitalisation breaks the guide. Flagged, not edited.

**Verdict: fix mechanically, then decide five things.** The terminology collision is the finding that matters, and it outranks everything else on the list, because a reader who meets a job and a task will go looking for a difference that does not exist. The 46 instances get normalised to "run", the two old names go into the term list as deprecated, and the heading case decision goes to whoever owns the guide with the count attached, since the corpus has already voted 148 to 64 and following the corpus is a one-line change instead of a 64-page one.

## Failure modes

**The guide that decides only tone.** Three paragraphs about being friendly, nothing about the serial comma, and a review queue that argues both forever.

**Autofixing a literal.** A find and replace that rewrites a string inside a code block, an error message or an interface label. The document now describes a product that does not exist, and it will be believed.

**Mass-fixing before checking five hits by hand.** One badly written pattern, applied to 300 places, in one commit, is a very efficient way to make a corpus worse.

**A term list that documents an aspiration.** It uses the name the team wishes the feature had, the interface uses the other one, and the list has now added a synonym rather than removed one.

**Elegant variation.** A writer with genuine skill alternates between three names for one thing because repetition felt clumsy, and every downstream reader pays for the elegance.

**A judgement queue nobody works.** It grows to 60 items, becomes a backlog rather than a decision list, and its contents get settled by whoever writes the next page.

**Running a style pass on a set that is structurally wrong.** Perfectly consistent capitalisation across pages that are in the wrong places. Fix the architecture first, because most of the style work will be redone when pages are split or merged.

**Inclusive-language replacement inside identifiers.** A well-meant rename that touches a config key, a branch name or a database column, and breaks something. The prose change and the identifier change are different decisions with different costs, and only one of them is free.

## What this skill does not do

- It does not run on every commit. A linter with a rules file does that, and it is the mechanism that makes any of this survive past the first month.
- It cannot see the product, so it cannot confirm that a term list matches the real labels, or that a documented button exists.
- It does not settle the judgement items for you. It puts them in a short queue, phrased as decisions, and someone with authority still has to choose.
- It does not evaluate whether the writing is good. A perfectly conformant page can be badly organised, wrong, or aimed at nobody. Structure belongs to `documentation-architecture` and whether a tutorial actually works belongs to `tutorial-reproducibility-audit`.
- It has no view of legal, regulatory or contractual wording, which in some products cannot be changed at all regardless of what the guide says.
