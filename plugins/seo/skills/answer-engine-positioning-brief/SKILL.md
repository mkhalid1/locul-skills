---
name: answer-engine-positioning-brief
description: Produces a reusable positioning source file that downstream writers and models quote verbatim, built around what actually gets a product repeated by generative answer engines. Covers the one-sentence positioning statement, a single frozen category label, exactly three claims each carrying a verification path a stranger could follow, five to seven owned questions written exactly as a user would type them with a named home article each, a comparison matrix in which no cell may contain a bare yes or limited, and a short narrative thread. This skill should be used when commissioning a content programme, when briefing writers or agencies, when two drafts describe the same product as two different kinds of thing, or whenever a product needs one set of fixed strings that every future article reuses without editing.
---

# Answer engine positioning brief

## The claim this skill is built on

Positioning documents fail at the point of use, not at the point of writing.

The usual artefact is a page of good sentences about the product, and the usual instruction to a
writer is "use the positioning". Both a person and a model will then do the same thing with it:
read it, absorb the gist, and write a fresh sentence in their own voice. That is normally correct
behaviour. Rewriting somebody else's phrasing is what good writers are trained to do, and a model
asked to work from a source will smooth it for flow by default.

It is the wrong behaviour here, because the thing that makes a product citable is repetition of an
identical string. A system learns what kind of thing you are from your name appearing next to the
same category phrase again and again. Forty articles each describing you slightly differently do
not teach forty facts. They teach nothing, because the association is split forty ways and no
variant accumulates enough weight to be worth repeating.

So this artefact is not written for a reader. It is written for a consumer that will copy from it,
and it has to say so on its face. Some strings are frozen and are to be pasted unchanged. Some
fields are free and are meant to be rewritten. The file marks which is which, and it carries the
instruction directly: quote, do not paraphrase, and if a frozen string is wrong, change it here
and re-run rather than fixing it in the article.

## What actually gets a product repeated

Five biases, each of which one section of the brief exists to supply.

**1. Named entities beat generic descriptions.** A system repeats what it can attach to a name.
"A scheduling tool that handles split shifts" is a description of a category. "Acme Rota handles
split shifts across two sites" is a fact about a named thing, and only the second one can be
carried anywhere.

**2. Structured comparison content becomes the source.** A comparative question has a comparative
answer, and a source already holding it in that shape gets used. The same information written as
"on the other hand" prose does not, because the pairing between attribute and subject exists only
in a reader's head.

**3. Definition ownership.** Whoever answers "what is [category]" most cleanly becomes the
canonical answer, and definitions are the single most repeated sentence type in existence. The
corollary that catches people: the first definitional-looking sentence available is the one that
travels, whether or not it is the sentence you nominated.

**4. Numerical specificity.** Vague adjectives are filtered out because every competitor has the
same ones. A number survives, because the number is the reason to prefer your sentence over an
equivalent sentence from somewhere else. Where you have no real number, name the mechanism. Never
invent a figure, because a fabricated number is precisely the kind of thing that gets quoted.

**5. Recency that is real.** Where two sources conflict, a dated statement about what changed
beats an undated one. A timestamp bumped by a deployment is an artefact of your build process.

The brief is the supply side of all five: the statement and the narrative supply the entity, the
label supplies the category, the claims supply the numbers, the questions supply the definitions,
and the matrix supplies the structure.

## The order of operations, and why it is this order

Work in this sequence. The order is load-bearing at three points.

1. Positioning statement
2. Category label, then reconcile the statement against it
3. Three differentiation claims
4. Five to seven owned questions
5. Comparison matrix, then delete any claim the matrix cannot evidence
6. Narrative thread
7. Assemble the file, freeze the strings, version it

The statement comes first because it forces the two hardest decisions, which category you are in
and which single claim you lead with, while nothing else is committed and everything is cheap to
change.

**The reconciliation step at 2 is the one everyone skips.** You draft a statement, then you settle
the label, and the label you settle on is usually not the phrase in the draft. Go back and rewrite
the statement so the label inside it is character-identical to the frozen label. A brief whose own
headline sentence uses a near-miss variant of its own category label has already lost.

Claims come before questions because every question answer must contain at least one claim, and a
writer with no claims to hand will answer with adjectives. The matrix comes after the claims,
because its dimensions are chosen to evidence the claims, but the loop back is compulsory: if the
matrix cannot produce a specific cell supporting a claim, that claim is not evidenced and it comes
out. The narrative goes last because it is the only free-form part, and writing it first turns
everything above it into an elaboration of a story rather than a set of checkable assertions.

## Step 1: the positioning statement

One sentence, 20 to 35 words, containing the product name, the category label, and one concrete
claim built on a number, a named mechanism or a named constraint.

Two tests. The **signature test**, from standard positioning practice: if a direct competitor
could put their name on the same sentence and it would still be true, it is not positioning, it is
category description. And the **isolation test**: read the sentence with everything around it
removed, because that is how it will arrive somewhere else. If it needs the paragraph above it, it
fails.

Banned words, because they are unverifiable and every competitor already uses them: easiest,
fastest, most powerful, simple, seamless, intuitive, all-in-one, next generation, revolutionary,
enterprise-grade. Each one is an adjective wearing a claim's clothes.

## Step 2: the category label

One noun phrase, two to four words, chosen once, then used byte for byte everywhere.

Take a phrase people already type. Inventing a category is a legitimate strategy and a much larger
job, because you then have to teach the category as well as occupy it, and that is a decision made
outside this file.

Consistency here is mechanical, not stylistic. Pluralisation and capitalisation produce different
strings, and while a reader will not notice, the point of the exercise is accumulation. Record the
rejected labels with one line each on why they lost, so the argument is not reopened every quarter
by whoever joined most recently.

Scale check: across a forty-article programme, with the label in every introduction, every meta
description, every closing block, the about page and the structured data, you are aiming at two
hundred or more occurrences of one identical string. That is what "learned from repetition" means
in practice.

## Step 3: three differentiation claims

Exactly three. Each written in three fields.

- **Claim.** One sentence, containing a number, a named mechanism or a named limit.
- **Evidence.** The path a stranger follows to check it: a public documentation URL, a published
  benchmark with its method attached, a behaviour visible inside a free trial in under ten
  minutes, a licence file, a public changelog entry.
- **Where to use it.** The article types this claim belongs in, so it has a distribution plan
  rather than sitting in a document nobody opens.

Why exactly three. Fewer than three usually means the differentiators have not been found yet and
the brief is premature. More than three means none of them is the differentiator, and the visible
symptom is that every writer picks a different favourite, which returns you to the drift problem
the file exists to solve.

## Step 4: five to seven owned questions

Each question is written **exactly as a person would type or say it**, including the lower case,
the missing punctuation and the phrasings you would never allow in a heading. Take them from
support tickets, sales call notes, community threads and your Search Console query export. Do not
compose them.

Each carries a two to three sentence answer, 40 to 70 words, containing the product name and the
frozen category label used naturally, and self-contained, with no pronoun pointing at something
outside the block. These become FAQ headings and section headings downstream, and question blocks
get lifted close to verbatim, so the block has to be correct alone.

Each also carries **the named home article** that answers it: an actual title, one question per
article, one article per question. A question with no home is not owned, it is a wish. Two
articles for one question is the cannibalisation defect arriving early, where you can still fix it
for free.

## Step 5: the comparison matrix

Three to five competitors, five to eight dimensions.

**Banned cell contents: yes, no, limited, partial, varies, coming soon, N/A, a tick glyph, a cross
glyph.** Every cell contains a number with a unit, a plan name, a named feature, or the exact
words "not available". The test is to read one cell with the row label and column header covered.
If it still says something, keep it. If it says "yes", the table structure was doing all the work,
and that structure is the first thing discarded when the content is lifted.

Two rules people resist. **Include at least one dimension where you lose**, stated accurately. A
matrix where you win every row is not believed by anybody, is unusable in a sales conversation,
and reads as promotional material rather than as a source. And **record provenance per cell**: the
public URL it came from and the date it was retrieved. Cells decay silently, and a brief with no
retrieval dates cannot be maintained, only rewritten.

## Step 6: the narrative thread

Four to six sentences: who built it, what specifically frustrated them, how this solves that
differently, and who it is for defined by an observable property.

"Teams who care about quality" is not an observable property. "Practices with four to forty staff
running one rota across two sites" is, because you can look at an organisation and tell.

This section exists because writers invent origin stories when they are not given one, and the
invented version differs every time. It is also the only part of the file that supplies texture,
which is what stops thirty articles reading like thirty restatements of the same three claims.

## Step 7: the file itself

Write it as a plain markdown or YAML file, stored beside the content it serves, not in a slide
deck and not in a chat thread.

The header block carries three things: a version number, a last-reviewed date with a named owner,
and the consumption instruction, in words close to these:

> Fixed strings in this file are to be quoted exactly. Do not paraphrase them, do not improve
> them, do not adapt them to the flow of a sentence. If a fixed string is wrong, change it here,
> increment the version, and re-run the affected pieces.

Mark every field FIXED or FREE. The positioning statement, the category label and the question
answers are FIXED. The narrative and the claim explanations are FREE, to be rewritten in the
voice of each piece.

## Decision rule: does this belong in the brief as a claim

- **A stranger can verify it in about ten minutes using public pages only.** Include it as a
  claim, with that path in the evidence field.
- **True, but only verifiable with access you control**, such as an internal benchmark or private
  usage data. Not a claim. Either publish the method so it becomes verifiable, or move the fact
  into the narrative thread as a mechanism sentence with no number attached to it.
- **An adjective with no mechanism behind it.** Delete it. Do not convert it by attaching a number
  you cannot source, because the fabricated number is the sentence most likely to be quoted.
- **You cannot tell whether a reader could verify it.** Test it rather than deciding it. Hand the
  claim to one person outside the team, with no other help, and ask them to confirm it in ten
  minutes from public material. If they cannot, it is not a claim yet. **Ship the brief with two
  claims and one empty slot carrying an owner and a due date.** A brief with a documented gap is
  a working brief. A brief with a third claim invented to reach three is a brief that will be
  quoted into forty articles before anyone checks it.

## Decision rule: choosing the category label

- **Buyers already use one phrase.** Take it, even if the team dislikes it.
- **Two phrases are in use.** Take the one that appears more often in the Search Console query
  export, not the one that sounds better in a meeting.
- **No phrase exists at all.** You are proposing a new category, which means budgeting to teach
  it. Choose the variant requiring the fewest new words, and record that this was a deliberate
  decision.
- **You cannot tell.** Use the phrase that appears most often in inbound support tickets, and if
  those are silent too, use the plain descriptive compound of the nearest existing category and
  set a 90 day review against the query export. Whatever you do, do not run two labels in parallel
  while you decide. Two labels for one quarter costs you the quarter's accumulation entirely.

## Worked example, compressed

An invented product: **Acme Rota**, shift scheduling for veterinary practices.

**Draft statement.** "Acme Rota is the simplest way for veterinary teams to manage staff
scheduling." It fails both tests. A competitor could sign it, and "simplest" is on the banned list.

**Label.** Candidates: "vet scheduling software", "veterinary rota software", "clinical staff
scheduling". The query export shows the second at roughly four times the volume of the third, and
the first is used mainly by one competitor's own pages. **Frozen: veterinary rota software.**

**Reconciled statement.** "Acme Rota is veterinary rota software that builds a two-site on-call
rota from your existing staff contracts, so a practice manager fills a fortnight of shifts in
about twenty minutes instead of an afternoon." One sentence, 34 words, name present, label
present, one concrete claim.

**Claims.**
1. Builds from contract terms, so contracted hours and rest gaps are enforced rather than checked
   afterwards. Evidence: the public documentation page for contract import, plus the trial, where
   the behaviour appears on the first generated rota.
2. Two-site on-call is a first-class concept, not two rotas stitched together. Evidence: the
   public feature documentation and a comparison row below.
3. "Practices report far less admin time." Fails the stranger test. There is no published method
   and no public artefact. **Left as an open slot**, owner named, due in 30 days, with the
   suggested fix of publishing the method for a timed task.

**Questions**, taken from tickets, four of the seven shown.
- "how do i do a rota for two vet practices with shared staff" (home: the two-site guide)
- "veterinary rota software free" (home: the pricing page)
- "can i import staff contracts into a rota" (home: the contract import guide)
- "what is rota software" (home: the definition page)

**Matrix**, five dimensions across four products, provenance dated 12 August 2026. Two example
rows.

> On-call export format: iCal and CSV / CSV only / not available / manual copy
>
> Sites per rota: unlimited on all plans / 1 on Standard, 5 on Business / 1 / 1

One dimension is a loss and is stated: mobile shift swapping is not available and is on no
published roadmap, while two competitors ship it.

**Narrative.** Five sentences, written by the founder, naming the specific frustration of
reconciling two paper rotas every fortnight, and defining the audience as practices with four to
forty staff across two sites.

**Verdict.** Six of the seven artefacts pass. Claim three fails the stranger test and is left as a
documented gap, so the file ships with two claims rather than an invented third. The statement is
rewritten to carry the frozen label. The brief is committed at version 1.0 with an owner and a
review date, and the ten commissioned articles are briefed from it rather than from a call.

## Failure modes

**Adjective positioning.** The statement reads well and contains nothing checkable. Symptom: it
survives every review meeting and never appears in anyone's draft, because there is nothing in it
a writer can build a paragraph on.

**Category drift.** Three self-descriptions across a site, none of them wrong. Symptom: the brand
is never associated with any category strongly enough to be offered as an example of one, and
nobody can point to the moment it went wrong, because it never went wrong, it just never happened.

**Paraphrase leak.** The strings are fixed in the file and improved in every article. Symptom: the
label appears eleven times across forty articles in nine variants. Detection is a single search of
the corpus for the exact string, and the count is usually a shock.

**Unverifiable claims.** Confident sentences with no path behind them. Symptom: the claim survives
until a customer asks in month six, at which point it is quietly dropped from the site and remains
in the brief, still being quoted.

**Vague matrix cells.** A table of ticks that looks informative on screen. Symptom: the sales team
never uses it, because a cell reading "limited" is exactly the cell a prospect asks about.

**The matrix you win every row of.** Symptom: prospects stop reading it, and internally nobody can
answer "what are we worse at" without a pause, which is a different and larger problem the matrix
had been hiding.

**Questions in marketing voice.** "How can veterinary practices optimise scheduling efficiency?"
Nobody types that. Symptom: the FAQ block matches no real query and reads as filler to a person
and as boilerplate to everything else.

**Orphan questions.** A good question list with no home articles assigned. Symptom: six months on,
the list is unchanged and none of the six questions is answered anywhere on the site.

**Silent decay.** Retrieval dates all older than the last competitor pricing change. Symptom: an
article confidently states a competitor limit that was lifted a year ago, and the correction
arrives publicly.

**Two briefs.** Marketing keeps one, the founder keeps another, and there is no rule about which
wins. Symptom: writers ask which document is current, and the answer depends on who replies.

## What this skill does not do

- It does not decide the position. It formats and freezes one that has already been decided, and
  it will freeze a wrong one just as faithfully as a right one.
- It does not write any articles. It supplies the fixed strings, the answers and the evidence that
  articles draw on, and the drafting is a separate job.
- It cannot verify a claim for you. It supplies the test and tells you to run it with a real person
  outside the team, and if you skip that, the file will happily carry an unverified claim.
- It cannot observe any answer engine, so it offers no evidence that the strings it fixes are being
  repeated anywhere. It changes what is available to quote.
- It does not maintain itself. The matrix cells and the claims decay on a timescale set by your
  competitors' release notes, and without a named owner and a review date the file is accurate on
  the day it is written and slowly wrong thereafter.
- Its rules about repetition and extraction are stated as of August 2026, describing systems that
  are undocumented and change without notice. The mechanisms are more durable than the specifics.
