---
name: voice-match-edit
description: Matches a draft to a specific author's voice by measuring it rather than describing it. Builds a corpus from the author's real writing, extracts a fingerprint of countable features including sentence and paragraph length distributions, contraction rate against opportunities, pronoun proportions, punctuation inventory, opener patterns, the small closed set of transitions the author actually uses, and hedging rate, then measures the draft on the same features and edits down the list by size of gap. This skill should be used when a draft has to sound like a particular person and a corpus of that person's writing exists.
---

# Voice match from a corpus

## Whose voice you may match

Settle this before you collect a single piece of writing, because the rest of the file assumes it is
already settled.

Match a voice you are entitled to write in. In practice that means one of three things: your own
voice; a voice you have been engaged to ghostwrite, with the named author's knowledge and their
sign-off on what goes out; or a house voice belonging to an organisation that has asked you to write
in it.

It is not for reproducing the voice of a public figure, a competitor's founder, a former colleague,
or anybody else who has not agreed. A corpus being publicly readable is not consent. Writing is
published to be read, not to be used as a template for sentences its author never wrote.

Say this plainly: the technique works well enough that the distinction matters. A close match
attached to someone's name, or placed anywhere a reader would reasonably assume they wrote it, is
impersonation whatever the intention behind it was, and the better the match the worse the problem.
If you cannot name the person who agreed, stop at this section.

## The claim this skill is built on

"Write in my voice" contains no information. It names a target without giving coordinates, so the
model does what anyone would do with no coordinates: it produces competent, mid-register prose and
labels it as yours. The same is true of every adjective people reach for next. Conversational,
punchy, warm, authoritative, human. Each of these is a direction rather than a destination, and two
readers will not agree on where any of them lands.

A voice is a set of habits, and habits are frequencies. How often this person contracts. How often
they start a paragraph with a conjunction. Whether they have ever used a semicolon. How long their
shortest sentences are and how often they use one. These are all countable, which means the gap
between a draft and a voice is measurable, which means the edit can be prioritised by size of gap
instead of by whatever the editor notices first.

This skill is that measurement. It has three parts: build a corpus, extract a fingerprint, then
compare and close. The order matters, because a fingerprint taken from the wrong corpus is worse
than no fingerprint at all: it gives you a precise target that points somewhere the author has
never been.

## Step 1. Build the corpus

**Include** pieces the author wrote themselves, in the same register as the draft, ideally from the
last two years. Blog posts, newsletters, internal memos they actually typed, long-form social posts,
conference talk transcripts if the draft is a talk.

**Exclude, and this list is where most corpora go wrong:**

- Anything ghostwritten. You will be matching the ghost.
- Anything heavily edited by someone else. A piece that went through a magazine's copy desk carries
  the copy desk's punctuation habits, not the author's. If you cannot tell how much was changed,
  leave it out.
- Anything from a different register. Legal correspondence, formal announcements, performance
  reviews, regulatory filings. These are written in the register's voice, not the person's, and
  including them drags every feature towards the middle.
- Co-authored work, unless you know which paragraphs are theirs.
- Anything generated or partly generated, which is now common enough that you have to ask.

**How much.** As a working threshold: at least ten pieces and roughly five thousand words. Below
about two thousand words you can measure the coarse features, sentence length and paragraph length
and pronoun proportions, and nothing else, because the sparse features do not appear often enough to
have a rate. A writer who uses a semicolon once every three thousand words will show zero semicolons
in a two thousand word corpus, and you will confidently instruct the draft never to use one.

**Why a corpus of one is worthless.** A single piece cannot separate a stable habit from the
occasion that produced it. Short sentences in that piece might be the author's rhythm or might be
what that particular argument needed. With one document you have no variance, and without variance
you cannot tell which features are the author and which are the topic. Any feature that varies more
between the author's own pieces than it does between the author and the draft is not a voice feature
at all, and you can only discover that with several pieces.

## Step 2. Extract the fingerprint

Fourteen features. For each one, measure it the way described, not the obvious way.

| Feature | How to measure it | What the number tells you |
|---|---|---|
| **Sentence length distribution** | Not the mean. Report the middle value, the quartiles, the proportion under 8 words, the proportion over 30, and the longest sentence in the corpus. | The mean hides everything. Two writers with the same average can look nothing alike: one writes every sentence at 17 words, the other alternates 6 and 34. |
| **Paragraph length distribution** | Sentences per paragraph, same shape: middle, range, and the proportion that are a single sentence. | The one-sentence-paragraph rate is a strong stylistic marker and varies enormously between writers. |
| **Contraction rate** | Contractions divided by contractions plus expanded equivalents. Count the opportunities: every "it is", "do not", "we are", "that is". | A per-thousand-words rate confounds the habit with how often those verbs occur. Rate against opportunity is the honest number. Many writers sit near 0 or near 1 rather than in the middle. |
| **Person mix** | First person singular, first person plural, and second person, each per thousand words, plus their proportions of each other. | "I" versus "we" is often a deliberate positioning choice, and a draft that swaps them reads as a different author entirely. |
| **Punctuation inventory** | Per thousand words: semicolons, colons, parentheses, exclamation marks, question marks, ellipses. Also the proportion of documents containing at least one of each. | The document proportion matters as much as the rate. A writer with one semicolon in ten pieces is not a semicolon user, and one semicolon in your draft will stand out. |
| **Register proxy** | Proportion of words with three or more syllables, which is the "hard words" definition used by the Gunning Fog index published in 1952, plus a count of Latinate suffixes: -tion, -ment, -ity, -ance, -ise, -ency. | A practical stand-in for Latin versus Germanic vocabulary without needing an etymological dictionary. Track the direction, not the absolute value. |
| **Vocabulary variety** | Type-token ratio computed on fixed windows of 500 or 1,000 tokens, then averaged. | Raw type-token ratio falls as text gets longer, so comparing a 6,000 word corpus with an 800 word draft directly is meaningless. Fixed windows fix this. |
| **Piece openers** | The first sentence of every piece in the corpus, listed verbatim. Classify: anecdote, direct claim, question, definition, scene, number. | Openers are the most patterned thing most writers do and the thing a generated draft gets most obviously wrong. |
| **Paragraph openers** | The first word of every paragraph. Count how many start with "The", "It", "This", "But", "And", "So", "There". | Two numbers to carry: the proportion starting with a conjunction, and the proportion starting with a subordinate clause. |
| **Transition set** | Every explicit transition used, with counts. | Almost every writer has a small closed set, often five or six items, used repeatedly. The presence of a transition outside that set is more diagnostic than the frequency inside it. |
| **Hedging rate** | Hedges per hundred sentences. Count qualifiers on claims: perhaps, arguably, tends to, generally, may, somewhat. | The single most reliable separator between a generated draft and most human writing, and the direction is nearly always the same. See the sibling file below. |
| **Stock formations** | Recurring phrases, idioms, sentence shapes, and any construction appearing in three or more pieces. | This is the part that reads as personality. It includes bad habits, and you should record them anyway. |
| **Humour** | Whether it appears at all, what kind, and where it sits: in asides, in parentheses, at the end of a section, never in the opening. | Position is more transferable than content. A writer who only jokes in parentheses will look wrong with a joke in a topic sentence. |
| **The never list** | Constructions absent from the entire corpus despite many opportunities. | Often the most diagnostic feature in the whole table. Zero rhetorical questions in 6,000 words is a decision. Zero exclamation marks is a decision. |

The never list deserves the emphasis. Presence features tell you what is possible for this writer.
Absence features tell you what is impossible, and violating an impossible feature is what makes a
reader say "they would never write that" without being able to explain why.

## Step 3. The comparison pass

Measure the draft on all fourteen features using identical methods, then build one table with three
columns: corpus value, draft value, and gap. Sort by gap size.

Work down that list. Do not work in the order you notice things, and do not start with whatever is
easiest to fix, because the easy fixes are usually the small gaps. The biggest gap is where the
draft stops sounding like the author, and it is frequently a feature nobody would have raised in
conversation: the draft hedges four times as often, or it uses eleven distinct transitions where the
author uses five, or it opens every paragraph with "The".

## The decision rule

For each feature, in order of gap size:

- **If the draft's value falls outside the range observed across the corpus pieces**, change it. This
  is the strongest signal available: the author has never, in any piece, done this.
- **If the value is inside the range but far from the centre**, change it only when the feature is
  high salience. The high salience four are contraction rate, hedging rate, sentence length spread,
  and paragraph openers. Readers notice these. They do not notice type-token ratio.
- **If the corpus contains fewer than five instances of the feature, or fewer than three pieces
  containing it at all, you cannot tell.** Leave the draft alone and record the feature as
  unmeasured. Do not construct a target from two observations. This branch fires most often on
  semicolons, exclamation marks, and humour, and the temptation to guess is strongest exactly where
  guessing is least supported.
- **If the corpus varies more within itself on a feature than the draft differs from the corpus
  average**, ignore the feature. The author is not consistent on it, so it is not part of the voice.

## The two features hardest to match

**Confidence level.** A generated draft hedges more than almost any human writer. It qualifies
claims that need no qualification, attributes opinions to unnamed groups, and softens the ends of
sentences. Matching an author who asserts things flatly means deleting the qualifiers and accepting
the exposure that creates, and it is the single change most likely to make a draft sound like a
person. This overlaps with the claim-and-hedge-audit skill, which does the epistemic version of the
job: that file asks whether a hedge is carrying real uncertainty, this one asks whether the rate
matches the author. Run the epistemic pass first, then the rate match, because a hedge that survives
the epistemic pass is one you should keep even if it pushes the rate up.

**Omission.** Every real writer leaves things out. They skip the definition, they do not explain the
obvious counterargument, they let a reference go unexplained because their reader will get it. A
generated draft covers. It answers the objection nobody raised and defines the term everyone knows,
because completeness is the safe default. You cannot measure this from a fingerprint. What you can
do is look at what the corpus pieces do not contain, and cut the parts of the draft that fill in
gaps the author would have left open.

## The derived style guide

Once the fingerprint exists, write it down as instructions with numbers in them: contract at roughly
this rate, keep one sentence paragraphs at about this proportion, never use a semicolon, open with a
claim rather than a question, use these six transitions and no others.

A derived guide beats a declared one because people do not accurately describe their own writing.
Asked to characterise their style, writers report aspirations and self-image. They say they write
short sentences when the corpus says otherwise. They say they avoid jargon while using six terms of
art per page. They are not lying, they simply have no access to their own frequencies. Ask a writer
for a style guide and you get what they wish they wrote. Measure the corpus and you get what they
write.

Where the declared guide and the derived one disagree, the derived one wins for matching the
existing voice. The declared one may still win as a target if the author is deliberately changing
how they write, and that is a different job, which this skill does not do.

## Worked example

An author's newsletter, twelve pieces, roughly 9,000 words. A draft has come back on a new topic.

| Feature | Corpus | Draft | Gap |
|---|---|---|---|
| Sentence length, middle value | 14 | 19 | +5 |
| Under 8 words | 22% | 4% | -18pt |
| Over 30 words | 6% | 14% | +8pt |
| Contraction rate | 0.81 | 0.22 | -0.59 |
| One-sentence paragraphs | 31% | 6% | -25pt |
| Semicolons | 0 in 12 pieces | 3 | outside range |
| "I" per 1,000 words | 12.4 | 1.1 | -11.3 |
| Hedges per 100 sentences | 4 | 23 | +19 |
| Distinct transitions | 6 | 14 | +8 |
| Paragraph opens with "The" | 9% | 38% | +29pt |

Sorted by size and salience, the order of work is: contraction rate, hedging rate, first person,
one-sentence paragraphs, paragraph openers, short sentence proportion, semicolons. Sentence length
middle value is last, because it will largely fix itself once the short sentences come back.

Draft paragraph as received:

> It is generally advisable to consider the possibility that a pricing change may have downstream
> effects on customer retention; in many cases, organisations find that the impact is not
> immediately apparent. The data suggests that a measured approach is typically warranted.

Corrected against the fingerprint, closing the four largest gaps:

> Change the price and you will find out about retention three months later. I have watched it
> happen twice. Both times the first month looked fine.

Contraction rate is not represented here because no opportunity arose, which is the correct
outcome: you do not insert contractions, you stop avoiding them. Hedges went from four to zero. The
semicolon is gone. First person appeared. Two of three sentences are under eight words, matching a
corpus where 22 percent are.

**Verdict on this example: six of the seven features in the work order closed, contraction rate
left alone because the rewrite gave it no opportunity, and one feature unmeasured.** Humour could
not be scored, because the corpus contained three instances across twelve pieces, which is below
the threshold, so the draft was left alone on that feature rather than given a joke.

## Failure modes

**Matching the ghost.** The corpus included two pieces written by an agency. Every feature is now
pulled towards a house style the author has never had, and the author will reject the output without
being able to say why.

**Register bleed.** Blog posts and legal correspondence in the same corpus. The fingerprint lands
between the two and matches neither, producing prose that is too stiff for the newsletter and too
loose for the letter.

**Measuring the mean and stopping.** Sentence length averaged to 16 in both corpus and draft, so the
feature was marked closed, while the corpus alternated 5 and 30 and the draft sat flat at 16
throughout. This is the failure the sentence-rhythm-edit skill exists to catch, and averaging is how
you hide it from yourself.

**Inventing a target from two observations.** Two semicolons in a corpus became a target semicolon
rate, and the edit inserted semicolons into a draft by a writer who barely uses them. The "you
cannot tell" branch exists for exactly this and gets skipped because a blank cell in a table feels
like a failure.

**Faithful reproduction of a bad habit.** The author opens 40 percent of paragraphs with "The", and
the match dutifully reproduces it. This is correct behaviour for a voice match and wrong for the
piece. Flag it, do not silently fix it, and let the author decide.

**Cargo-culting the stock phrases.** Recurring formations get inserted at three times their natural
rate because they are the most visible part of the fingerprint. The result reads as a parody, which
is the characteristic failure of impersonation: the signature moves are all present and the
frequencies are wrong.

**Confusing voice with format.** The corpus is newsletters with a sign-off and a postscript; the
draft is a landing page. Structural habits get transplanted into a form that cannot carry them. The
structure-de-templating skill handles document shape, and the two jobs should not be merged.

**Editing until every gap is zero.** A perfectly matched fingerprint is not a good piece of writing.
Some gaps exist because this topic genuinely needs longer sentences. Close the gaps that are habits
and leave the ones that are the argument.

## What this skill does not do

- It does not supply judgement. Matching the surface of a voice cannot tell you what this author
  would have thought was worth saying, which is most of what makes them recognisable.
- It cannot work without a corpus, and it degrades quietly rather than loudly when the corpus is too
  small: you get a fingerprint with confident numbers on features that had four observations.
- It does not distinguish a habit worth keeping from one worth losing. It reproduces both, and the
  author has to make that call.
- It does not transfer between registers, and using a blog fingerprint on a talk or a filing makes
  the output worse rather than neutral.
- It does not remove generated tells or fix document shape. Those are the ai-tell-removal,
  sentence-rhythm-edit and structure-de-templating skills, and this one assumes they have already
  run.
- It has nothing to say about whether the piece should disclose that a model was involved. That is
  the ai-disclosure-audit skill, and matching a voice well makes the disclosure question more
  pressing rather than less.
