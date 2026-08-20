---
name: ai-tell-removal
description: Runs a mechanical lexical pass over a draft using an enumerated ban list of 48 words and phrases with replacements, seven named construction tells, and a punctuation count with a target of zero that covers alt text, captions and meta descriptions as well as the body. Includes the keep rule for correct technical usage and the re-run rule that makes the pass valid. This skill should be used when editing any draft before it is published or handed over, and again after any later edit, including a one-word fix.
---

# AI tell removal, the word list

## The claim this skill is built on

Asking for prose that sounds human does not work, because the instruction has no target. What does work is a list, because the lexical layer of the problem is finite. There is a bounded set of words, a smaller set of sentence constructions, and two punctuation characters that appear far more often in generated prose than in prose people write by hand. You can count all three. A count has a target, an instruction to sound human does not, and this is the whole difference between an edit that converges and an edit that wanders.

Two warnings before the list.

**This is not a detector-evasion tool.** Automated detectors of generated text are unreliable in both directions: they clear machine prose and they accuse human prose, and their scores move for reasons that have nothing to do with quality. Writing against a score produces text that is worse to read and merely scores differently, and it also produces the specific damage of stripping correct words out of technical writing because a classifier disliked them. The goal of this pass is prose a person would want to read. If a detector likes it afterwards, that is a coincidence and not evidence.

**The vocabulary is the smallest part.** Removing every banned word from a draft whose sentences all restate each other leaves you with a draft whose sentences all restate each other. The constructions in Tier 2 do more work than Tier 1, and the layers above this one, rhythm and document shape, do more work again.

## How to run it

Run it as counts, not as impressions. Impressions are why the pass fails: you read the draft, nothing jumps out, you declare it clean, and six of the tells were on the second page.

1. **Fix the surface list first.** Write down every piece of language in the deliverable. Body text, headings, alt text, image captions, table cells, footnotes, the meta description, link title attributes, the social preview text, code comments if they ship. This list is the scope, and it is the step people skip.
2. **Count Tier 1**, the ban list, over the whole surface list.
3. **Count Tier 2**, the constructions, which needs reading rather than matching.
4. **Count Tier 3**, the punctuation, which is pure matching and should be automated.
5. **Apply the keep rule** to every Tier 1 hit before changing it.
6. **Write the report**, with before and after counts.
7. **Check the timestamp.** If the file has been edited since step 2, the report is void. Go back to step 2.

## Tier 1, the ban list

Forty-eight entries. Each is a lookup: the left column is what to search for, the right column is what to do instead. The right column matters more than the left, because the failure of every ban list is a synonym swap that keeps the emptiness.

| # | Word or phrase | Do this instead |
|---|---|---|
| 1 | delve, delve into | "look at", "go into", or cut the verb and start with the object |
| 2 | dive in, deep dive, let us explore | Cut all three. A heading that states the finding replaces them |
| 3 | tapestry, rich tapestry | Name the actual set of things |
| 4 | realm, in the realm of | "in", or the field's real name |
| 5 | landscape, navigate the landscape | "market", "the options". Say who is choosing between what |
| 6 | navigate, of anything not physical | "handle", "work through", "decide between" |
| 7 | ecosystem, outside biology | Name the parts: the plugins, the API, the docs |
| 8 | journey, outside travel | "process", or the stage: "the first week" |
| 9 | myriad | A number, or "many" |
| 10 | plethora | "too many", or a number |
| 11 | underscore, underscores the importance of | "shows", "confirms", or cut the sentence |
| 12 | pivotal | "important", then say to whom |
| 13 | holistic | List the parts you are including |
| 14 | synergy, synergies | Name the two things and what one does for the other |
| 15 | streamline | Name the step you removed |
| 16 | empower | "let", "allow", "give access to" |
| 17 | transformative | State the before and the after |
| 18 | unprecedented | Verify it, then give the previous high |
| 19 | robust | Say what it survives: "keeps working when the connection drops" |
| 20 | seamless | Say what the user no longer has to do |
| 21 | leverage, as a verb | "use". The financial noun stays |
| 22 | harness | "use" |
| 23 | unlock, unlock the potential of | "make available", "let you" |
| 24 | elevate | "improve", and name the measure |
| 25 | embark | "start" |
| 26 | foster | "encourage", "cause", "support" |
| 27 | cutting edge, state of the art | Give the version number and the date |
| 28 | a game changer | Say what changed, and for whom |
| 29 | a testament to, stands as a testament to | State the evidence and stop |
| 30 | plays a crucial role, plays a key role | Say what it does |
| 31 | crucial, vital, essential | Keep only where the thing fails without it, and say what fails |
| 32 | it is worth noting, it is important to note, notably | Delete the frame, keep the fact |
| 33 | ever-evolving, ever-changing | Cut, or give the rate of change |
| 34 | fast-paced | Cut the clause entirely |
| 35 | in today's world, in the modern era | Cut |
| 36 | in conclusion, to sum up, to wrap up | Delete the phrase. Keep the sentence only if it adds something |
| 37 | at the end of the day, the bottom line is | Cut, and keep the claim that follows |
| 38 | when it comes to | Restructure around the real subject |
| 39 | in order to | "to" |
| 40 | utilise | "use" |
| 41 | facilitate | "run", "help", "make possible" |
| 42 | comprehensive | Say what is covered, and what is not |
| 43 | actionable insights, key takeaways | "findings", or just the action |
| 44 | best practices | Name the practice and who follows it |
| 45 | curated, hand-curated | "chosen", and say by whom |
| 46 | meticulous, meticulously | Cut, or name the check that was performed |
| 47 | showcase, boasts | "shows", "has" |
| 48 | resonate, resonates with | "matches", "appeals to", or the evidence that it did |

The list is a snapshot dated August 2026. It drifts, because usage drifts and because these words become tells partly by being flagged. Treat it as maintained rather than fixed, and add house entries at the bottom rather than editing the numbering.

**Density heuristic.** Above roughly twelve hits per 1,000 words, you are not editing, you are rewriting, and it is usually faster to restate the argument from the facts than to repair the sentences one at a time. Below about three per 1,000 words, stop looking. Both numbers are working heuristics from ordinary editing rather than measured thresholds.

## Tier 2, the construction tells

These are stronger signals than any single word, and they survive a vocabulary pass untouched. Seven patterns.

**1. Negated then corrected.** "This is not just a tool, it is a system." "The problem is not the code. It is the process." One instance is a legitimate rhetorical move. Three in a document is a signature. Fix by deleting the negated half, which almost never carries information, and asserting the second half directly.

**2. The rule of three as filler.** Three items where the third adds nothing: "faster, cheaper, and more efficient". Test each item by deleting it. If the sentence loses nothing, the item was rhythm rather than content. Two real items beat three where one is padding.

**3. The "whether you are A, B, or C" opener.** "Whether you are a solo developer, a growing team, or a large organisation." It is an attempt to address everyone, which addresses no one, and it appears in the first paragraph. Replace it by naming the one reader you actually mean.

**4. The rhetorical question as a transition.** "So what does this mean in practice?" "But how do you actually do it?" A question the reader did not ask, used to change subject. Delete it and let the next sentence start the new subject. Keep a question only where a real reader would have asked exactly that one.

**5. The sentence that restates the previous sentence.** Usually the opener of a paragraph, recapping the end of the paragraph before. It reads as continuity and is actually a stall. Delete it. If the connection is genuinely unclear without it, the fix is a transition of four words, not a whole sentence.

**6. The paragraph that ends by telling the reader what they just read.** "In short, the deployment process has three stages." Cut the closing sentence. Then check whether the paragraph actually delivered the thing the closing sentence claimed, because sometimes the summary is the only place the point appears, and then the fix is to move it up and delete the rest.

**7. Appositive stacking.** Every noun trailing a descriptive clause: "the dashboard, a central hub for team activity, displays the queue, a prioritised list of pending items, in real time." One appositive is fine. Two in a sentence is a tell, and three is unreadable. Promote one to its own sentence and delete the others.

## Tier 3, punctuation counted to zero

The em dash, Unicode U+2014, is the single strongest punctuation signal in current generated prose. The en dash, U+2013, is second and is often missed because it is visually similar to a hyphen. Both are countable, and the target is zero.

**The count covers everything the document contains, not everything the reader sees in the body.** Alt text, image captions, the meta description, table cells, footnotes, list items, headings, the social preview text, and any string that ships alongside the prose. A document can read perfectly clean and still carry six of them in alt text.

Replace U+2014 with a comma, a colon, a full stop, or a restructured sentence. Two of them in one sentence usually means the sentence wanted to be two sentences. Replace U+2013 in a numeric range with the word "to", which is clearer in prose anyway.

**Curly quotes.** These are not banned, they are a consistency check. A document with curly quotation marks in twelve places and straight ones in three has been assembled from more than one source, and the mismatch is a visible sign of that. Pick one convention for the document and count the exceptions to zero. Watch apostrophes specifically, because a straight apostrophe among curly ones is easy to miss at reading speed and obvious in a diff.

**Do the count with a script, not with your eyes.** A one-liner that runs the same way on Windows and on macOS:

```
node -e "const t=require('fs').readFileSync(process.argv[1],'utf8');const n=(s)=>t.split(s).length-1;console.log('U+2014 em dash', n('\u2014'));console.log('U+2013 en dash', n('\u2013'));console.log('U+2019 curly apostrophe', n('\u2019'));console.log('U+0027 straight apostrophe', n('\u0027'));" draft.md
```

**Why this is a rule and not a preference.** A document can go out carrying dozens of em dashes after being declared finished, with a passing report attached to it, because the check ran before the last round of edits rather than after and that last round put them all back. Nothing about a check like that is wrong except when it happened.

## The re-run rule

This pass is invalidated by any subsequent edit to the document. Any edit. A one-word fix from a reviewer, a corrected figure, a new sentence in the third section, a retitled heading.

The reason is specific to how the tells get introduced. A later edit is usually written into a finished document at speed, without the pass in mind, and it is exactly where a reintroduced em dash or a "it is worth noting" lands. The edit is small, so nobody thinks of it as new prose, and the report from the earlier run is still sitting there looking valid.

Two operational consequences. First, this is the last pass before publication, after structural edits, after fact checks, after review. Second, the report is only meaningful with a timestamp attached and a statement that no edit has landed since. A report without a timestamp is a claim about a file that no longer exists.

## The keep rule, for false positives

Several banned words are correct in a technical sense, and a pass that removes every instance produces prose that is mangled in a new and more embarrassing way. Robust has a precise meaning in statistics. Leverage is a real financial noun. Harness is a physical object and a test fixture. Realm is a named concept in several authentication systems. Journey and landscape are literal in travel writing. Streamline is literal in fluid dynamics.

The branching test, applied to every hit before you touch it:

- **KEEP** if the word carries a technical meaning in this domain that no substitute carries. Test by substituting the plain replacement and asking whether the sentence now means something else or nothing. "Robust standard errors" does not survive becoming "strong standard errors".
- **CUT or REPLACE** if the word is doing praise, emphasis, or scale rather than reference. "A robust solution" is praise. "A robust estimator" is reference.
- **CANNOT TELL**, which happens most often in a domain you do not know well: leave the word alone, list it in the report with its full sentence, and let the author decide. Never rewrite a sentence whose meaning you cannot verify. A flagged word left in place costs nothing. A flattened technical term costs the author their credibility with the only readers who noticed.

## What the report looks like

```
Tell pass, draft v4, 1,180 words, run 14:02, no edits since
  Ban list hits           9   3 cut, 4 replaced, 2 kept as technical
  Construction tells      5   2 negated-corrected, 1 rule of three, 2 restating openers
  U+2014 em dash          0   was 6
  U+2013 en dash          0   was 1, numeric range, now "to"
  Quote consistency       0   was 2 straight apostrophes in a curly document
  Surfaces checked        body, 3 alt texts, 2 captions, meta description, 1 footnote
  Author call needed      1   "robust" in "robust standard errors", left in place
  Density                 7.6 hits per 1,000 words before the pass
```

Counts before and after, the surfaces covered, and the timestamp. Anything softer than this is an impression.

## Worked example

A product page paragraph, 78 words.

> In today's fast-paced digital landscape, teams must navigate an ever-evolving array of tooling choices. Our platform is not just a documentation tool, it is a comprehensive knowledge ecosystem, designed to empower teams, streamline workflows, and unlock the full potential of institutional memory. It is worth noting that the platform is robust, seamless, and built to elevate how modern teams collaborate. So what does this mean for you? Let us dive in.

Hits: fast-paced (34), landscape (5), navigate (6), ever-evolving (33), comprehensive (42), ecosystem (7), empower (16), streamline (15), unlock (23), it is worth noting (32), robust (19), seamless (20), elevate (24), dive in (2). Fourteen Tier 1 hits in 78 words, a density of about 180 per 1,000 words, far past the rewrite threshold. Tier 2: the negated-then-corrected construction in sentence two, the rule of three twice, and the rhetorical question as a transition in sentence four. Every single word of the paragraph is claim and none of it is fact, which is why the rewrite is shorter than the repair would have been.

Rewritten from the facts underneath it, 54 words:

> Teams choose a documentation tool about every three years, usually after an outage nobody could explain afterwards. This one stores decisions next to the code they affect, so searching a filename returns the argument that produced it. It works offline, and a repository rename does not break the links.

Changes: the opening clause is deleted rather than replaced, because it contained nothing. The negated-corrected sentence becomes one assertion. Both rules of three are gone, replaced by two concrete capabilities. Robust and seamless become the two things they were standing in for, working offline and surviving a rename. The rhetorical question and the closing invitation are cut with no replacement.

**Verdict: rewrite rather than edit, and 24 words shorter.** At a density above about 12 per 1,000 words the repair costs more than restating the facts, and this paragraph was fifteen times that. Note that the rewrite is only possible because the facts existed. Where they do not, this pass will tell you that too, and that is the more useful finding.

## Failure modes

**The blanket find and replace.** Every instance of a banned word swapped without reading the sentence, so a finance document loses its leverage ratios and a statistics document loses its robust estimators. This is the failure that makes people distrust the whole idea, and it is prevented only by applying the keep rule to every hit individually.

**The synonym swap that keeps the shape.** "Delve into" becomes "explore in depth", "leverage" becomes "utilise", and the sentence is exactly as empty as it was. The right column of the table exists to prevent this. If your replacement is another abstract verb, you have not finished.

**Running the pass before the last edit.** The most common and most expensive failure, and the one that lets dozens of em dashes back into a document that has already passed. The report was true when written and false when read.

**Editing only what the reader sees in the body.** Alt text, captions, meta descriptions, footnotes and table cells routinely carry the punctuation the body no longer has. The surface list in step 1 is the fix, and skipping it makes the whole count a fiction.

**Over-correction into a new register.** Every sentence clipped to seven words, every abstraction removed, nothing left but a list of blunt facts. This is just as recognisable as the original, and it usually reads as brusque rather than plain. Removing a tell does not mean removing the sentence.

**Treating the list as complete.** It is 48 entries dated August 2026. New tells appear, old ones fade as they get flagged, and a document that avoids all 48 while sounding wrong needs the constructions in Tier 2 and the layers above this one, not more vocabulary.

**Stripping hedges as if they were tells.** "It appears that" and "the data suggests" are sometimes filler and sometimes the honest description of a weak claim. This pass has no way to tell the difference and should not try. Leave hedges to a pass built for them.

**Writing against a detector score.** Iterating until a classifier is satisfied optimises for a number nobody reads, damages the prose, and produces confident nonsense about what is safe to say. The number is not the goal and it is not evidence.

## What this skill does not do

- It does not evade detectors, and any file that promises that is selling you a number rather than an edit.
- It does not touch sentence rhythm. A draft can pass every count here with all its sentences still the same length. That is the sentence-rhythm-edit pass.
- It does not touch document shape, which is the layer that survives every word-level edit and is usually the real reason a piece feels generated. That is the structure-de-templating pass.
- It has no opinion about hedged claims, voice matching, or disclosure. Those are separate passes with separate rules, and this one deliberately leaves them alone.
- It cannot verify a fact. Replacing "robust" with "keeps working when the connection drops" is only an improvement if that is true, and checking it is your job.
- It does not know your house style. Add entries at the bottom of the list rather than assuming the 48 cover your particular register.
