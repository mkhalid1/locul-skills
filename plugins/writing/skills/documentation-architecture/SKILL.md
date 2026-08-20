---
name: documentation-architecture
description: Classifies and restructures a documentation set using the four modes of Diátaxis, by Daniele Procida. Assigns every page exactly one mode, identifies pages that mix two modes and says which sentences belong where, reports empty or thin quadrants and what each one costs, and decides the top-level navigation shape. Includes the tells that identify a mixed page from the outside and a decision rule with a branch for pages that cannot be classified. This skill should be used when auditing an existing documentation set, when planning the structure of a new one, or before writing a page whose mode has not been settled.
---

# Documentation architecture

## The claim this skill is built on

Most documentation that is judged as bad is not badly written. It is badly sorted. The sentences are fine, the examples work, the person who wrote it knew the product. What has gone wrong is that one page is trying to serve two readers who are in different states, and neither of them is served.

The obvious approach makes this worse. The obvious approach is to organise documentation by feature, and to write, under each feature, everything there is to say about it: what it is, why it exists, how to set it up, every option it takes, and a walkthrough. That page is comprehensive and it is unusable. The beginner cannot find the starting point under the option table. The expert scrolling for one default value has to read a narrative. The person deciding whether to adopt the feature at all gets no argument, only mechanics.

The fix is structural and it is old. This skill is built on Diátaxis, the documentation framework devised by Daniele Procida and published in full at diataxis.fr. The framework itself is free and short, and this file does not restate it for its own sake. What this adds is the operational part: how to tell from the outside that a page is mixed, what to do with a page that resists classification, and what an empty quadrant costs you.

The rule underneath everything below: **one page, one mode.** A page that needs two labels is a defect with a known fix, and the fix is a split, not a rewrite.

## The two axes, and why the grid generates rather than lists

The four modes are not a list of four good things to write. They fall out of two binary questions about the reader, and that is what makes the scheme usable under pressure.

**Axis one, practical or theoretical.** Is the reader acquiring a skill, something they can do afterwards, or acquiring knowledge, something they understand afterwards?

**Axis two, study or work.** Is the reader here to learn, with no goal of their own beyond learning, or here to get something done, with a goal they brought with them?

Cross them:

- practical and study: **tutorial**
- practical and work: **how-to guide**
- theoretical and work: **reference**
- theoretical and study: **explanation**

This is generative rather than a convenient list because both axes describe the reader's state, not the content. Every reader who opens a page is in exactly one cell at that moment: they are either at a keyboard or not, either pursuing their own goal or not. There is no fifth state to argue for, and no page that legitimately occupies two cells, because a single reader cannot be in two states at once. That is the whole force of it. A list of four things can always be argued up to five by someone who wants a "concepts and examples" category. A grid of two binary questions cannot.

The second use of the axes is diagnostic. When a page is wrong, flip one answer and the grid tells you where the material should go. Explanation inside a tutorial is the study column with the wrong practical-theoretical answer, so it moves to explanation. A worked example inside reference is the theoretical row with the wrong axis-two answer, so it moves to a how-to.

## The four contracts

State each mode as a contract, with an obligation and a prohibition. A label is arguable. A contract is checkable.

**Tutorial.** A lesson, where the reader is guaranteed to succeed and the author controls every variable. The obligation is that a reader who follows it exactly arrives at a working result, every time, on a machine the author has never seen. The prohibitions are severe: no choices, no alternatives, no edge cases, no explanation of why, no "you may prefer". The tutorial is not about the subject, it is about the reader's first success. Its title promises a finished result: "Build a working scheduled job in ten minutes".

**How-to guide.** Solves one real problem for someone who already knows the domain. The obligation is that it addresses a goal the reader arrived with, and gets them to it without detours. It may assume knowledge. It must not assume a starting state, because the reader's system is already in some condition the author did not create. Its title names the goal, and starts with a verb or the word "how": "Rotate an API key without downtime". The prohibition is teaching, and the prohibition is completeness. A how-to that covers every case is a reference page wearing a verb.

**Reference.** A description of the machinery, accurate and complete, organised to be looked up rather than read. The obligation is that its structure mirrors the structure of the thing it describes, so a reader who knows the code can predict where a fact lives. It is austere on purpose: neutral, uniform, repetitive, with the same fields in the same order for every entry. The prohibitions are instruction, persuasion, opinion, and narrative examples. A one-line canonical usage snippet is fine, because it is part of the description. A scenario is not.

**Explanation.** The discussion of why, which the other three must not contain. The obligation is to give the reader the context and the reasoning: alternatives that were considered and rejected, history, constraints, trade-offs, the shape of the problem. It is the only mode read away from the keyboard, and the only place where an opinion belongs. The prohibition is steps. The moment an explanation page carries a numbered sequence of commands, a how-to has grown inside it.

## The mixing failure

Mixing is the most common documentation defect there is, and it is invisible from the inside because every sentence in the mixed page is a true and useful sentence.

**A tutorial that starts explaining.** The reader who wanted to succeed at something is now reading architecture. The tell is textual and searchable: "under the hood", "internally", "it is worth understanding why", "there are three ways to do this", or a paragraph with no action in it at all sitting between step 4 and step 5. A diagram of system architecture inside a tutorial is nearly always this defect. **The fix:** move the paragraph to an explanation page and leave a single link, placed *after* the step rather than before it, so the reader who is following along is not stopped mid-flight.

**A reference page with a worked example in the middle.** It becomes unusable for lookup, because scanning breaks the moment the uniform structure breaks. The tell is a prose paragraph between two entries, an opening like "For example, imagine you are building", or entries that are no longer in the order the machinery is in, because the example forced a narrative sequence on them. **The fix:** move the example to a how-to and link to it from the entry. Keep at most the minimal canonical call, one line, no scenario around it.

**A how-to guide that stops to teach concepts.** It fails the person who has a problem right now, which is the only person who opens a how-to. The tell is a first step that is not an action, usually "First, understand how X works", or a "Background" section, or the word "let's", or a guide four times longer than the task it describes. **The fix:** move the concepts to explanation, and replace them with a prerequisites list the reader can check in ten seconds.

**An explanation with steps in it.** The tell is a numbered list of commands. **The fix:** move the commands into a how-to and link. The explanation may say what the operation achieves. It may not say how to run it.

**A tutorial that offers choices.** The tell is "if you prefer", "depending on your setup", or an alternative in parentheses. **The fix:** choose one, say plainly that you are choosing one, and mention the alternative nowhere on this page.

**The universal rule for all five: move the material, do not delete it.** Every one of these paragraphs was written because a real reader needed it. Delete it and it will be rewritten, worse, within a year, and probably in the same wrong place. Moving it also does something better than tidying: it populates the quadrant that was thin. A documentation set's first three explanation pages are usually already written, scattered inside its tutorials.

## The rules that follow

- **A tutorial must not offer choices, because a choice is a place to fail.** The corollary bites: pin the version, name the exact tool, and where the platform genuinely diverges, give the Mac branch and the Windows branch in full, both complete, rather than writing one and adding "adjust the paths for your platform". A branch the reader has to construct is a choice.
- **A how-to may assume knowledge and must not assume a starting state.** Open with preconditions written as checkable facts, not as reassurance: what must already exist, what permission is needed, what version this applies to.
- **Reference must be generated from, or mechanically checked against, the thing it describes, or it rots.** Hand-written reference is accurate on the day it is written and decays with every release. If generation is impossible, the minimum acceptable substitute is a test that fails when the documented default and the real default disagree.
- **Explanation is the only place where an opinion belongs.** If you find yourself writing "we recommend" in a reference entry, that sentence has an explanation page waiting for it, and the entry gets a link instead.

## Navigation and the top level

**The argument for organising the top level by mode.** A reader arrives in a state, not with a feature. They know whether they are learning or working before they know which component they need, and a top level of Tutorials, How-to guides, Reference, Explanation lets them say so in one click. It also makes gaps visible to the team: an empty section on the front page is embarrassing in a productive way, whereas a missing tutorial inside a feature folder is invisible for years.

**The honest counter-argument.** Most readers never see your top level. They arrive from a search engine on a deep page, so the front page shape matters less than it feels when you are redesigning it. And for a product that is really a suite of independent components, mode-first pushes the component to the second level, which means a reader who wants "the billing API" has to answer a question about their own state before they can answer the question they actually have.

**The decision rule.** If the product is one coherent thing, put mode at the top level and the component inside it. If it is a suite of largely independent components, put the component at the top level and repeat all four modes inside each one. Either way, add a per-component landing page carrying exactly four links, one per mode, so the shape is visible from wherever the reader lands. What never works is mixing a third scheme into the same level, such as Tutorials, Reference, and Billing.

## The audit procedure

Order matters here, because step 2 is contaminated if you do step 1 badly.

1. **Inventory.** One row per page: URL, title, and where it currently sits in navigation. Do not classify yet.
2. **Classify each page into exactly one mode, from its content only.** Ignore the section it currently sits in. Reading the current label first is the single fastest way to reproduce the existing mistakes.
3. **Record the classification failures.** Three kinds: pages that need two labels, pages that fit none, and pages whose content contradicts their location.
4. **Count by mode.** An empty or very thin quadrant is a finding in its own right, even when every individual page is well made.
5. **Check the entry point.** Can a reader who has never used the product reach the first tutorial in one click from the documentation root? If not, that is a finding regardless of how good the tutorial is.
6. **Order the findings by cost:** missing tutorial, then mixed pages, then misfiled pages, then naming and tone.

**The decision rule for classifying one page.** Ask what the reader is doing while reading it.

- Following along at a keyboard, with an outcome the author has already made work → **tutorial**
- At a keyboard, working on a problem they brought with them → **how-to guide**
- Looking up one fact and leaving → **reference**
- Away from the keyboard, deciding or trying to understand → **explanation**

If that is ambiguous, use the title as the tiebreak. A goal in the title means how-to. A thing in the title means reference. A promised finished result means tutorial. A question word, especially "why", means explanation.

**If you still cannot tell, the page is mixed, and that is the finding rather than an unresolved case.** Mark it MIXED, list which sentences belong to which mode, and propose the split. One constraint on the split: if a resulting half comes to less than roughly 150 words, it is not a page, it is a section belonging to a sibling page, and it gets merged there instead. Splitting a page into two stubs is a worse outcome than leaving it mixed.

**Pages that are none of the four.** Do not force them. Release notes and changelogs are their own genre and sit outside the grid. A glossary is reference. A migration guide is a how-to with a version pair in its title. An FAQ is almost always a symptom: classify it entry by entry, and expect every entry to belong somewhere else. An FAQ exists because nobody could decide where its answers lived, and an entry that keeps drifting back into it is evidence that its proper home is hard to find.

## What a missing quadrant means

**No tutorials: a new user cannot start.** The symptom is support questions phrased as "where do I begin", and the cost is invisible, because the people it fails leave without filing anything.

**No reference: everyone reads the source.** The symptom is answers in the issue tracker that quote line numbers, and heavy users who know more about the defaults than the documentation does. It is survivable for an open-source library and fatal for a closed product.

**No explanation: every design question is answered in a chat thread and lost.** The symptom is the same question re-asked every quarter, new maintainers relitigating decisions that were settled years ago, and constraints whose reasons live in one person's head.

**No how-to guides: the documentation feels complete and nobody can do anything.** The symptom is a tutorial that has been quietly edited over two years into a fake how-to, accumulating branches and caveats until it no longer guarantees success for anyone.

If you can only build one quadrant first, build how-to guides for your five most common support questions. They pay back immediately. The tutorial is second, and it is the one most worth spending a week on.

## Worked example, compressed

A documentation set for a hosted job scheduler. Fourteen pages, currently organised by feature. All page names and counts below are invented for this example.

| Page | Classified as | Finding |
| --- | --- | --- |
| Getting started | MIXED | Product pitch, install steps, and a 700-word architecture section. Three modes in one page. |
| Concepts | Explanation | Correct, but unreachable: nothing links to it from the top two levels. |
| Scheduling your first job | Tutorial | Offers a choice of two runtimes in step 3. Prohibited. Pick one. |
| API reference | Reference | Two long scenario examples embedded between entries. Move to how-to. |
| Webhooks | MIXED | Half reference for the payload fields, half tutorial for setting one up. Clean split available. |
| Best practices | Explanation | Correctly explanation. Title hides it. |
| FAQ | None | Nine entries: four are how-to, three reference, two explanation. |
| Retries and backoff | Explanation | Good page. Contains one numbered command sequence that belongs in a how-to. |
| CLI reference | Reference | Hand-written and already contradicts the API reference on two default values. |
| Deploying to production | How-to | Opens with "First, understand how our queue works". Move that out. |
| Timezones explained | Explanation | Correct and well placed. |
| Troubleshooting | How-to (collection) | Fine as a set of small how-to guides. Rename by symptom. |
| Migrating from v1 to v2 | How-to | Correct. Version pair in the title, as it should be. |
| Changelog | Outside the grid | Leave it alone. |

**Counts:** tutorials 1, how-to 3, reference 2, explanation 4, mixed 2, unclassifiable 1.

**Verdict: the tutorial quadrant is the failure.** One tutorial exists, it breaks its own contract by offering a choice, and there is no path from the documentation root to it in one click. Everything else on the list is cheaper. The ordered plan: fix the choice in the one tutorial and link it from the root; split "Getting started" three ways, which alone produces the second tutorial and a real explanation page; split "Webhooks"; dissolve the FAQ into its nine homes; generate the CLI reference from the command definitions so the two reference pages stop disagreeing. The two reference pages disagreeing on defaults is the most serious *content* finding here, and it is out of scope for a structural audit: it goes to whoever owns the CLI, today.

## Failure modes

**The tour disguised as a tutorial.** It walks through the product's features in the order the navigation lists them, rather than driving one task to a working result. The reader finishes having done nothing.

**The reference page with a story in it.** One scenario, added by a helpful contributor, and the page stops being scannable. The next contributor adds a second, because there is now a precedent.

**The how-to that starts with a concept.** Written by someone who found the concept hard, aimed at someone who has an incident open.

**Documentation organised by team.** Three sections that map to three engineering groups, and a reader who has to know your org chart to find anything. The tell is a top-level heading that is a team name or an internal system name.

**The FAQ as a landfill.** It grows because it is the one page with no contract, so nothing can be rejected from it. The count of entries is a decent proxy for how hard the rest of the set is to navigate.

**Reference written by hand.** Accurate on the day it ships and wrong within two releases. It rots silently, because nobody reads a reference page end to end and so nobody notices.

**Over-application.** Splitting a page that was working, because it is 60 per cent how-to and 40 per cent reference and a rule was applied to it. Under roughly 150 words per half, the split makes things worse. The framework is a diagnosis for pages that hurt, not a tidiness standard for pages that do not.

**Classifying by location rather than by content.** Reading the current section header before reading the page, and reproducing the existing structure exactly. This is the reason step 2 of the procedure says content only.

## Related skills

Two things this deliberately does not cover, and where they live:

- **Whether the tutorial actually works** on a machine that is not the author's. That is a different procedure with a different checklist: clean-machine assumptions, version pinning, every command running as written, Mac and Windows both. See `tutorial-reproducibility-audit`.
- **Whether the pages agree with each other in wording**, once they are in the right places: terminology, heading capitalisation, how a path or a keyboard shortcut is written. See `style-guide-conformance`. Run this skill first. Reorganising after a style pass wastes most of the style pass.

## What this skill does not do

- It does not judge whether the prose is any good. A well-classified set of badly written pages passes this audit cleanly.
- It cannot verify accuracy. It will flag hand-written reference as a rot risk, but it has no way to know that the documented timeout of 30 seconds is really 60.
- It does not know which missing pages matter. Quadrant counts are structural evidence, and only your support queue and your analytics can rank the gaps by real cost.
- It does not write the pages. The output is an inventory, a classification, a split list and an ordered plan, and someone who knows the product still has to write.
- It has no opinion on your tooling. Which generator, which reference extractor, which hosting, and whether docs live beside the code are all outside it.
