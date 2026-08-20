---
name: claim-and-hedge-audit
description: Audits a draft claim by claim for two opposite failures: overqualification, where hedges pad sentences without carrying uncertainty, and fake precision, where numbers and attributions appear with no source behind them. Carries an inventory of hedges with a deletion test for each, the two legitimate uses of a hedge, the falsifiability test, attribution rules, weasel constructions, and a three-way confidence calibration pass. This skill should be used before any draft containing factual claims is published or sent for expert review.
---

# Claim and hedge audit

## The claim this skill is built on

Generated prose fails in two opposite directions at the same time, and both are mechanical enough to
fix with a procedure rather than an instinct.

It is **overqualified**. Hedges attach to claims that need no hedging, because a qualifier is always
the safe move when you are not sure. The result reads as careful and is not: a paragraph where every
sentence is softened has not stated anything, so it cannot be wrong, which also means it cannot be
useful.

It is simultaneously **falsely precise**. Numbers arrive with no provenance. Percentages appear that
match nothing in the source material. Ranges get presented as findings. This looks like the opposite
of hedging and comes from the same place: the shape of authoritative writing includes numbers, so
numbers appear.

Both failures survive a normal editing pass because neither one looks wrong. Nothing is
ungrammatical, nothing is off-topic, and the paragraph reads fine at speed. They only surface when
someone asks what exactly the sentence commits to.

## Step 1. Extract the claim list

Before editing anything, list every claim in the draft as a separate line. A claim is any sentence
that could be true or false about the world. Instructions, definitions and descriptions of the
document itself are not claims and do not go on the list.

Working threshold: on a 1,200 word explanatory piece expect somewhere between fifteen and forty
claims. Far fewer than that and the piece is not saying much. Far more and some of them are
restatements, which is worth knowing on its own.

Extracting the list first matters because it strips the connective prose. Read as a bare list, a
weak claim is obvious in a way it never is inside a well-turned paragraph.

## Step 2. The hedge inventory

Twenty-four constructions. For each, the question is the same: is this hedge carrying real
uncertainty, or padding a sentence?

**The deletion test.** Remove the hedge and read the sentence. If the sentence is now false, the
hedge was load-bearing and stays. If the sentence is still true, the hedge was padding and goes. If
you cannot tell which without checking a fact you do not have, that is the third branch and it is
handled below.

| Hedge | What it usually is | What to do |
|---|---|---|
| arguably | Padding. Signals the author expects disagreement and will not name it. | Cut, or name who disagrees and why. |
| perhaps | Padding in an assertion, legitimate in a proposal. | Cut from claims, keep in suggestions. |
| it could be argued | Padding, and evasive. Nobody has argued it. | Cut, or attribute to whoever argued it. |
| some would say | Weasel attribution. Who? | Name them or cut the sentence. |
| in many cases | Vague scope. Which cases, how many? | Replace with the actual scope or cut. |
| often | Frequency claim with no basis. | Replace with a count, or state the basis. |
| generally | Same, plus it invites an exception you have not thought about. | Replace or cut. |
| typically | Same, and it is the most common of the three in generated prose. | Replace or cut. |
| tends to | Softens a causal claim into an unfalsifiable one. | State the claim, or state the exception. |
| may | Legitimate for genuine possibility, padding when the thing simply does. | Deletion test. |
| might | As above, weaker. | Deletion test. |
| can be | Nearly always padding. Almost anything can be almost anything. | Cut and assert. |
| is likely to | Legitimate only if you can say how likely and on what basis. | Give the basis or cut. |
| one of the most | Unfalsifiable superlative. One of how many? | Give the rank or cut. |
| among the best | Same. | Cut. |
| relatively | Relative to what? Usually nothing. | Name the comparator or cut. |
| fairly | Padding. | Cut. |
| quite | Padding, and ambiguous in British English, where it can mean both very and somewhat. | Cut. |
| somewhat | Padding. | Cut. |
| various | Vague quantity pretending to be a description. | Give the number and the list. |
| several | Vague quantity. If you know it is three, say three. | Replace with the number. |
| a number of | The worst of the three, since it carries no information at all. | Replace with the number. |
| up to | Upper bound presented as a result. See fake precision below. | Give the range and the typical value. |
| as much as | Same pattern, more emphatic. | Same fix. |

Two notes on running this. First, do not batch. The deletion test is per sentence, and a global
strip converts careful claims into false ones, which is a worse document than the one you started
with. Second, count the survivors. If more than about one sentence in five still contains a hedge
after the pass, either the topic is genuinely uncertain, which should be said once at the top rather
than twenty times throughout, or the pass has not been run honestly.

## The two legitimate uses of a hedge

A hedge earns its place in exactly two situations.

**Genuine uncertainty about a fact.** You do not know, and the reader needs to know that you do not
know. This is correct and the sentence is worse without it.

**A deliberate scope limitation.** The claim holds for some cases and you are marking the boundary.
Also correct.

In both cases the rule is the same: **state it precisely rather than vaguely.** A hedge that
survives should name the shape of what is unknown.

- "This often causes a rebuild" becomes "This caused a rebuild in all three of the projects we
  checked, and we have not tested it beyond those."
- "Results may vary by platform" becomes "We only ran this on Windows. Behaviour on other platforms
  is unverified."
- "It is likely to improve throughput" becomes "It improved throughput in the one benchmark we ran,
  which is not representative."

Notice what happens. The vague hedge is shorter and says nothing. The precise version is longer,
commits to something, and can be checked. That is the trade, and it is worth taking every time.

## Fake precision

The opposite failure, and the more damaging one, because a fake number travels further than a vague
sentence.

**A number with no source.** Any figure in the draft that you cannot trace to a document is a
liability. It does not matter that it sounds right.

**The percentage that appears in many articles and originates in none.** A figure gets repeated
across an entire content genre with no primary source anywhere in the chain. It reads as common
knowledge precisely because it is everywhere. Tracing it is a research job, and the research
category here covers the citation chain properly. The job in this file is only to flag it as
unsourced and refuse to let it ship unmarked.

**A range presented as a finding.** "Between 20 and 60 percent" is not a result, it is the absence
of one. If the range is that wide, the honest sentence says the measurement is inconclusive.

**The upper bound restated as a typical value.** This is the specific pattern to watch, because it
happens across retellings rather than within one document. A source says "up to 40 percent". The
first retelling says "as much as 40 percent". The second says "around 40 percent". The third says
"40 percent". Nobody lied at any step, and the final sentence is false. Whenever you see a round
figure asserted flatly, check whether the original was a bound. In advertising this pattern has
drawn regulatory attention in several jurisdictions, so if the copy is commercial, check the rules
that apply to you rather than relying on this paragraph.

**Precision beyond the measurement.** "37.2 percent" from a sample of forty is precision the data
cannot support. Round to what the method justifies.

## The falsifiability test

For every claim on the list, ask: **what would have to be true for this to be wrong?**

If you can answer, the claim is real and the answer tells you what evidence it needs. If you cannot,
the claim is not a claim. It is content in the shape of a claim, and a page full of these reads as
filler even when every sentence is defensible.

Typical unfalsifiable claims, all of which look substantive:

- "Good documentation improves developer experience."
- "The right approach depends on your context."
- "Understanding your users is essential."

Each could be deleted with no loss. The fix is to make it specific enough to be wrong: name the
approach, the context, the mechanism, the thing that happens if you get it wrong.

## Attribution discipline

**"Research shows" is the single most common unsupported construction in generated prose.** It
carries the authority of a citation with none of the content. So do "studies suggest", "experts
agree", "it is widely believed", and "data indicates".

The rule has three branches, and every claim takes one:

1. **A named source with a date.** The author or organisation, the year, and ideally the title.
   "Research shows" becomes "a 2019 paper by the university's software engineering group found".
2. **A stated basis in your own experience.** Perfectly legitimate and often stronger than a weak
   citation, provided it is marked as what it is. "In the four migrations I have run" is a good
   sentence. It tells the reader exactly how much weight to give it.
3. **Removal.** If it has neither a source nor a personal basis, it goes. There is no fourth option,
   and the temptation is always to invent a fourth option by hedging it instead.

Never invent a citation, an author, a date, or a section number to satisfy branch one. A fabricated
reference is a different and much more serious failure than an unsupported claim, and it is covered
in the ai-disclosure-audit skill under things that must never be produced.

## Weasel constructions

Related to hedging, distinct enough to check separately.

**The agentless passive.** "Mistakes were made." "The feature was deprioritised." "It was decided."
Test: can you name the subject? If yes, name it. If no, say that you do not know who decided, which
is a real and interesting sentence. Not every passive is a weasel: the passive is correct when the
actor is genuinely unimportant or unknown, and blanket passive removal is its own mistake.

**Attribution to an unnamed group.** "Many experts", "critics argue", "some have suggested". Name
one, or cut.

**Belief without a believer.** "It is widely believed", "there is a growing consensus", "it is
generally accepted". Same fix.

**Nominalisation that hides the action.** "There was a reduction in errors" instead of "errors fell
by a third". The nominalised version conveniently removes the need for a number.

## Confidence calibration

The last pass. Go through the claim list and mark each one:

- **C, certain.** You can point at evidence now.
- **P, probable.** You believe it, with reason, and could be wrong.
- **S, speculative.** A hypothesis, an extrapolation, or a prediction.

Then check the language against the mark. Certain claims should be stated flatly with a source.
Probable claims should be stated with the reason attached. Speculative claims should be marked as
speculation in the sentence itself.

**Most drafts have this inverted somewhere.** The pattern is consistent enough to look for
deliberately: the speculative claim gets asserted flatly because it is the most interesting sentence
in the piece and hedging it would spoil it, while the certain claim gets hedged because stating
something obvious feels like condescension. Find the inversions and swap the treatments.

## The decision rule

For each hedge, in order:

- **If deleting it makes the sentence false, keep it**, and rewrite it into a precise scope
  statement rather than leaving it vague.
- **If deleting it leaves the sentence true, delete it.**
- **If you cannot tell without a fact you do not have, do not guess in either direction.** Mark the
  claim as unverified, and take one of three actions: go and get the fact, downgrade the sentence to
  a precise statement of what you actually observed, or cut the claim. Leaving the vague hedge in
  place is the one option that is always wrong, because it hides the gap from the next reader as
  effectively as it hides it from you.

## Worked example

A paragraph from a draft on database migrations, audited claim by claim.

> Zero-downtime migrations are generally considered one of the most challenging aspects of database
> operations. Research shows that a significant number of outages are caused by schema changes, with
> some estimates suggesting up to 40 percent. Teams may find that a phased approach tends to reduce
> risk, and various tools can be helpful in this process.

Claim by claim:

1. "generally considered one of the most challenging aspects" **C? No: unfalsifiable.** Considered by
   whom, one of how many. Cut or make specific.
2. "Research shows" **Unsupported attribution.** No source. Branch three applies unless one is found.
3. "a significant number of outages" **Vague quantity.** Significant compared with what.
4. "up to 40 percent" **Upper bound as a finding**, attributed to "some estimates", which is a weasel
   attribution stacked on fake precision. Two failures in five words.
5. "may find that a phased approach tends to reduce risk" **Double hedge on a causal claim.** Deletion
   test: "a phased approach reduces risk" is still true if the author has seen it, so both hedges are
   padding, and the claim then needs a basis under attribution.
6. "various tools can be helpful" **Vague quantity plus padding.** Says nothing.

Rewritten:

> Zero-downtime migrations are hard because the schema and the code have to be compatible in both
> directions for the length of the rollout. I have not found published figures on how often schema
> changes cause outages, so treat the common claim that they cause a large share of them as
> unsourced. A phased approach, expand then migrate then contract, removed the failure mode in the
> three migrations I have run, all on the same stack, so treat that as one team's experience rather
> than a general result.

**Verdict on this example: six claims audited, one kept and made specific, one converted to an
explicit statement of what is unknown, one restated with its basis and scope, three cut.** The
rewritten version is a similar length, commits to three things instead of nothing, and is checkable.

## Failure modes

**Stripping hedges globally.** A find-and-replace over the inventory turns careful sentences into
false ones. The deletion test is per sentence and there is no batch version of it.

**Trading a vague hedge for a false certainty.** The pass removes "may reduce costs" and writes
"reduces costs", when the evidence supported neither. The correct output is the precise scope
statement, not the flat claim.

**Keeping the number and losing the source.** The audit flags an unsourced figure, someone finds a
plausible source, and nobody checks that the source actually says that number. This is the failure
the research category exists to catch, and it looks like success from inside this pass.

**Marking everything certain.** The calibration pass is worthless if C is the default. If nothing in
a 2,000 word piece is speculative, either the piece is very dull or the marking was done to make the
document look finished.

**Auditing the prose and not the headings.** Headings, pull quotes and summary boxes carry claims
too, they get read more often than the body, and they are almost always the most overstated text on
the page.

**Confusing a hedge with politeness.** "You might want to check the logs" is a hedge in form and an
instruction in function. The inventory applies to claims about the world, not to how you address the
reader, and running it over instructions makes prose brusque for no gain.

**Removing the passive everywhere.** The agentless passive is a weasel only when there is an actor
being hidden. Where the actor is genuinely unknown or irrelevant, the passive is the correct
sentence and a mechanical rewrite invents an actor.

**Editing to a hedging target.** Voice matching is a separate job. If the author hedges more than
this pass leaves, that is a rate question for the voice-match-edit skill, and the epistemic pass
should run first so the hedges that survive are the ones that were doing work.

## What this skill does not do

- It does not check whether claims are true. It checks how they are phrased and whether they carry a
  source. Verifying the source is the research category's job, and this file will happily pass a
  well-attributed falsehood.
- It cannot trace a laundered statistic to its origin. It flags the pattern and stops there.
- It has no domain calibration. In clinical, legal, safety or regulatory writing the required
  hedging is set by rules this file does not carry, and a qualified reviewer overrides it.
- It does not assess legal or advertising exposure. Converting a bounded claim into a flat one can
  change what the sentence is in advertising terms, and that decision belongs to whoever signs off
  the copy.
- It does not fix rhythm, vocabulary tells or document shape. Those are the sentence-rhythm-edit,
  ai-tell-removal and structure-de-templating skills.
- It does not decide whether a piece needs a disclosure that a model was involved. That is
  ai-disclosure-audit, and passing this audit is not a substitute for it.
