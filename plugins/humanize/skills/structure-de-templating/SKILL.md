---
name: structure-de-templating
description: Diagnoses and fixes the shape of a document rather than its words, covering eight named template tells including the road-map introduction, symmetric sections, taxonomy heading sets, uniform bulleted lists and the conclusion that restates the introduction. Includes the headings-only diagnostic, the section-length distribution check, the four openings that work, and the test for when a list is hiding an argument. This skill should be used on any long-form draft that reads as generated despite clean sentences, and before line editing rather than after.
---

# Structure de-templating

## The claim this skill is built on

Every word-level edit leaves the shape of a document untouched. You can remove every banned word, vary every sentence, fix every cadence, and the piece will still read as generated, because the thing giving it away is the order of the parts and the symmetry between them.

This is why the most common feedback on generated long-form is unhelpfully vague. The reader says it feels like a machine wrote it, and then cannot produce a sentence they object to, and the writer concludes the reader is imagining it. The reader is not imagining it. They are responding to a shape they have seen four hundred times: an introduction that describes the article, five sections of nearly equal length, each opening with a claim and closing with a transition, and a conclusion that says what the introduction said.

Shape is also the cheapest thing to get right and the most expensive thing to fix late, which is why this pass runs before line editing and not after.

**Not a detector tool.** Structural shape is not what automated detectors measure, their scores are unreliable in both directions, and restructuring a document to move one is effort aimed at the wrong reader. The aim here is a document a person would follow to the end.

## The eight template shapes

Enumerated so they can be checked one at a time. A draft with three of these has a shape problem. A draft with six has no shape at all, only a container.

**1. The introduction that explains what the article will cover.** "In this article, we will explore the key factors behind X, examine the common challenges, and provide practical guidance." It is a table of contents written as prose, and it delays the first real sentence by a paragraph. Cut it entirely. In a document read out of order, replace it with an actual table of contents, which does the same job honestly and takes a tenth of the space.

**2. Every section roughly the same length.** The strongest structural tell and the least noticed. It means an outline was filled rather than an argument made, because a real argument is unbalanced: the contested point needs 800 words and the obvious one needs 90.

**3. Every section with the same internal structure.** Claim, three bullets, example, transition. Once the reader has read two sections they know the shape of all of them, and they start skimming, which is the outcome the structure was presumably meant to prevent.

**4. A heading set that is a taxonomy rather than an argument.** "What is X", "Why X matters", "Benefits of X", "Common challenges", "Best practices", "Conclusion". Every heading is a category of thing that could be said about the topic, and none follows from the one before it. This is the shape the headings-only diagnostic below is built to detect.

**5. A bulleted list where every item has the same grammatical shape and the same length.** Six items, each a gerund phrase of nine to twelve words. Human lists are ragged, because the items are collected rather than generated, and one of them is always noticeably longer because it needed to be.

**6. A conclusion that restates the introduction.** The document closes the loop on itself, adds nothing, and the reader stops before it because they can feel it coming.

**7. A final section titled with a synonym for conclusion.** "Final thoughts", "Wrapping up", "Key takeaways", "The bottom line". The title is a confession that the section has no content of its own, only a function.

**8. A call to action bolted on with no relation to what came before.** Two paragraphs of analysis followed by a request to book a demo, connected by nothing. If the piece has earned an action, name the action the argument actually implies. If it has not, leave it off.

## The headings-only diagnostic

The single highest-yield check in this file, and the one most people have never run.

Strip the document to its headings and read only those, in order, as a list. Then apply the connector test.

**Between each consecutive pair of headings, try to insert a connector: "therefore", "but", "so", "which is why", "except when".** If one of those fits, the second heading follows from the first, and the document is an argument. If the only word that fits is "also", the headings are parallel topics, and the document is a taxonomy: a set of things that can be said about a subject, arranged in no particular order and rearrangeable without loss.

That last property is the giveaway. **If you can reorder the sections without damaging the document, it has no argument.** A taxonomy is fine in reference material, where the reader arrives looking for one item and never reads the rest. It is fatal in a piece meant to be read from start to finish, because a reader who senses that the order is arbitrary stops attending to it.

**The fix, in order:**

1. Write the one claim of the document in a sentence. If you cannot, stop restructuring, because there is nothing to structure.
2. Write the three to six steps a sceptical reader has to be walked through to accept that claim. Those are the sections.
3. Delete anything that is not one of those steps, however good it is. Material that is good and off the path belongs in another piece.
4. Rewrite the headings so each one states its step.
5. Run the connector test again.

## The section-length distribution check

The structural equivalent of measuring sentence length, and it works the same way: the spread is the signal, not the average.

Count the words in each section, including its heading. Then look at the ratio of the longest to the shortest.

- **Ratio under about 1.5, meaning every section sits within roughly 20 percent of the mean:** the document was generated from an outline with a length target per section. This is the flat case, and it is worth fixing even when every section is individually good.
- **Ratio between roughly 2.5 and 5:** normal for an argued piece. Some points needed evidence and got it, others needed one paragraph and got one.
- **Ratio over about 8, with one section holding most of the document:** not necessarily a fault, but check that the long section is one section rather than three wearing one heading. Apply the one-thing test to it.
- **You cannot tell** when the document has fewer than four sections, or is deliberately a reference list where each entry is by design the same size. Do not compute the ratio. Read the headings instead and use the connector test, which works at any length.

The fix is never to pad the short sections. It is to ask why the long one is long, which is usually because that is where the actual argument lives, and then to consider whether the short ones are needed at all.

## Openings

**What a real opening does:** it gives this specific reader a reason to continue that could not have been written for any other piece on the topic. Two sentences is the budget.

**The test:** if the first paragraph could be moved to a different article on the same subject without changing a word, it is not an opening. It is a warm-up, and the piece actually starts in paragraph three.

**The four that work:**

1. **A specific moment.** One event, with a time and a detail. "The alert fired at 04:12, and the only person who could read the dashboard was on a plane." Concrete beats general, always, and a moment gives the reader something to hold while the abstraction arrives.
2. **A concrete claim at odds with the obvious one.** "Adding a second reviewer made the reviews worse." The reader continues to find out how that can be true. This only works if you can actually defend it.
3. **A number that is surprising, with its source.** Surprising means it contradicts what the reader would have guessed. The source is not optional, because an unsourced surprising number reads as invented, and often is.
4. **A question the reader is actually asking, in their words.** Not a rhetorical question you invented as a transition. The difference is whether the reader had already formed the question before they arrived.

**The ones that do not work:** the definition opener, which starts by defining a term the reader already knows. The road map, which describes the article. The historical sweep, which begins centuries before the subject. The importance claim, which asserts the topic has become increasingly important without evidence. And the "whether you are a beginner, a professional, or somewhere in between" opener, which addresses everyone and reaches nobody.

## Endings

**What a genuine ending does:** one of three things. It lands the claim in its strongest form, now that the reader has the evidence to accept it. It adds the thing that could not be said earlier, which is usually the implication, the caveat, or the cost. Or it names the next action the argument has actually earned.

**Why a summary is almost never it.** The reader just read the document. In anything under about 1,500 words they still have all of it in working memory, so a summary tells them what they already know while occupying the most emphatic position on the page. A summary at the end is usually a sign the writer had no last point and needed the section to exist.

**The specific case where a summary is correct:** long procedural or reference material, over roughly 3,000 words, that the reader will return to and re-scan rather than read again, or a decision document where a reader who skipped to the end must be able to act from the last section alone. Both are cases where the summary is a navigation aid for a non-linear reader rather than a closing argument for a linear one. A summary at the top, as an abstract, is a different device and is often right where a summary at the bottom would be wrong.

## List discipline

**A list is the right form when** the items are genuinely parallel, the order does not matter, and the reader will scan and pick one. Options, parameters, error codes, alternatives, a checklist to be worked through in any order.

**A list is avoidance when** it lets you skip the connective tissue. The test: **if the items have an order that matters, or a relationship to each other, prose is usually correct and the list is hiding the argument.** Try inserting "because", "but", "so" or "which is why" between consecutive items. If those connectors fit, the relationships exist and the bullets have deleted them, and the reader now has to reconstruct what you knew.

Two secondary tells. **Uniform items:** when every bullet has the same grammatical shape and lands within a word or two of the same length, the items were generated to fill a list rather than collected because they were true. **The chosen number:** a list of exactly three, five, seven or ten, in a domain with no reason to produce a round count, usually means the number came before the items and the last one or two were manufactured to reach it.

## Headings that do work

A heading that states the finding beats a heading that names the topic, for a reader going start to finish. "Cache invalidation is where the latency comes from" carries the point. "Caching" carries a subject only, and the reader has to read the section to find out what is being claimed about it.

**The trade-off is scannability, and it is real.** Topic headings are better when readers arrive from a search engine or an internal index looking for one specific item, better as anchor links, and better as navigation in a long document. Finding headings are better in a piece read in order, because the heading set doubles as the argument.

**The decision rule:**

- **Reader arrives looking for one item:** topic headings, keyword first.
- **Reader reads start to finish:** finding headings.
- **You cannot tell, or both audiences are real:** use a finding heading that contains the topic keyword inside it, which is what "Cache invalidation is where the latency comes from" does. Keep it under about 70 characters, since longer headings wrap awkwardly in sidebar navigation and get truncated in most table-of-contents components.

## The one-thing test

For each section, write one sentence beginning with a verb that says what the section is for. No "and" in it.

- If you write the sentence easily, the section is sound.
- If the sentence needs an "and", it is two sections, and splitting them usually reveals that one of the two is much weaker.
- If you cannot write the sentence at all, the section is not needed. Cut it and see whether anything upstream breaks. Usually nothing does.
- If the sentence you produce is nearly identical to another section's sentence, they are one section that an outline split in two.

## Worked example

An 1,800 word draft on incident response. Eight headings, section word counts of 210, 240, 225, 260, 235, 220, 250 and 160.

Before:

> Introduction / What is incident response? / Why incident response matters / Key components of an incident response plan / Benefits of a strong plan / Common challenges / Best practices / Conclusion

Run the connector test. Between "What is incident response?" and "Why incident response matters", only "also" fits. Between "Benefits" and "Common challenges", only "also" fits. Every pair is parallel. The sections can be reordered freely with no loss, which confirms the diagnosis: this is a taxonomy. Section lengths range from 160 to 260 words, a ratio of 1.6, which is the flat case. Shapes present: 1, 2, 4, 6, 7, and the list inside "Best practices" has seven items of near-identical length, which is 5 and the chosen number together. Six of the eight tells.

After, once the claim was written down as "most incidents are lost in the first twenty minutes, and the fix is ownership rather than tooling":

> Nobody owns the first twenty minutes / So the first decision is who decides, not what broke / The rota only works when it is boring / Which is why the runbook has to be shorter than you want / What to cut from it first / The review is the only part that compounds

Six headings instead of eight. The connector test now passes on every pair: "so", "which is why" and "except" all fit where they are needed, and none of the joins works with "also". Reordering breaks the piece, which is the property you want. The introduction and conclusion headings are gone: the opening is now the specific moment from the third section of the old draft, and the ending is the compounding claim, which is the thing that could not be said until the reader had accepted the rest. Section lengths after the rewrite are 120, 340, 180, 520, 240 and 300 words, a ratio of 4.3, because the runbook argument was the contested one and needed the evidence.

**Verdict: taxonomy, restructured into an argument.** Two headings deleted, one section split, one cut entirely, the length ratio moved from 1.6 to 4.3. Note what the pass did not do: it did not change a single sentence. That comes afterwards, and doing it first would have wasted it.

## Failure modes

**Rewriting the headings without reordering the content.** The heading set now states findings, the sections underneath are still parallel topics, and the connector test still fails. This is the most common half-done version of this pass and it produces a taxonomy with better titles.

**Deleting the summary from a document that needed one.** Long reference material and decision memos have non-linear readers, and removing their closing summary because summaries are a tell makes the document worse for the people who use it most.

**Manufactured asymmetry.** Padding one section to move the length ratio is the structural version of adding a long sentence to raise the variance. The ratio is a diagnostic, not a target, and a document padded to hit it has acquired a new fault to hide an old one.

**Converting every list to prose.** Options, parameters and error codes belong in lists, and turning them into paragraphs produces a wall of text the reader has to parse serially to find one item. Apply the ordering test rather than a blanket rule.

**Headings that over-claim.** A finding heading asserts something, and if the section does not deliver it, the reader notices immediately and trusts the rest of the document less. A topic heading cannot fail this way, which is part of why it is safer.

**Losing search and navigation.** Finding headings that contain none of the words a reader would search for make a page harder to find and harder to link to. The hybrid heading exists for this, and on a page whose traffic arrives from search, the trade needs to be made deliberately.

**Restructuring in place of thinking.** The diagnostics reveal that a document has no argument. They cannot supply one. A piece reordered without a claim is the same taxonomy in a new sequence, and the effort is a way of feeling productive while avoiding the hard part.

**Running this after line editing.** Every sentence you polished in a section you then delete or split was wasted work, and the reordered document needs its transitions rewritten anyway. Structure first, always.

## What this skill does not do

- It does not evade detectors, and structure is not what they look at in any case.
- It does not supply an argument or check whether your claim is true. It reveals the absence of one, which is useful and is not the same thing.
- It is wrong for reference documentation, runbooks and specifications, where symmetric sections and predictable headings help a reader who is navigating rather than reading.
- It does not touch vocabulary or sentence rhythm, which are separate passes at separate layers, and both should run after this one rather than before.
- It has no view on length. A well-shaped document can be far too long, and this pass will not tell you so.
- It cannot see your publishing constraints. House templates, required sections and content management systems that mandate a summary field are real, and some of the tells here are contractual obligations you do not get to remove.
