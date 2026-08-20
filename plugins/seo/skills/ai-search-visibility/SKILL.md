---
name: ai-search-visibility
description: Audits a page for whether its passages can be quoted by an answer engine, which is a different job from ranking. Covers the five properties of an extractable passage, the rule that every comparison table cell must carry a number, a plan name or a named feature, answering the titled question inside the first two hundred words, owning a definition, maintaining one category label across the site, what structured data does and does not do since the FAQ retirement in May 2026, and an honest account of why citation cannot currently be measured reliably. This skill should be used when a page ranks but is never cited, when writing comparison or definitional content, or before buying any tool that claims to track AI search visibility.
---

# AI search visibility audit

## The claim this skill is built on

Ranking and being quoted are different jobs, and optimising for the first does not produce the
second.

Ranking puts a link into a list that a person scans and chooses from. Being quoted requires
something else entirely: a span of your text gets separated from your page, stripped of everything
around it, and placed beside spans from other sources. Whatever context that passage relied on is
gone. If the sentence said "the tool syncs every fifteen minutes", the extracted version is about
nothing at all, because the subject was three paragraphs up.

That is the whole design constraint. A page can be accurate, well researched, comprehensive, and
completely unquotable, because good discursive prose uses pronouns, builds to its conclusions, and
puts the payoff at the end. Every one of those habits is correct for a reader moving through the
page in order and wrong for a system taking one bounded span out of it.

So the target is not quality in the abstract. It is a testable property of individual passages:
does this still carry its meaning when it is alone?

## The five properties of a citable passage

**1. Explicit entity naming.** Any sentence you would like quoted must name its subject with a
proper noun, not a pronoun and not a category reference. "The platform imports records every
fifteen minutes" is unusable. "Acme Sync imports billing records every fifteen minutes" survives.

The trap is that this fights a real writing instinct. After the first mention, a good writer moves
to pronouns and shorthand, because repeating the name is clumsy. The compromise: name the subject
explicitly in the sentences that carry a fact, a number or a definition, and use pronouns freely in
the connective tissue between them. Run the check across the whole page, not just the opening,
because pronoun drift accelerates the further in you go.

**2. Structured comparison content.** A comparative question has a comparative answer, and a table
is already that shape. Tables, definition lists and short labelled paragraphs extract cleanly. A
flowing passage of "on the other hand" prose contains the same information and extracts badly,
because the pairing between attribute and subject exists only in the reader's head.

**3. Owning a definition.** State the definition cleanly and early, in one sentence, in the shape
"X is a Y that Z", before any history, hedging or context. The important and under-appreciated
corollary: **the first definitional-looking sentence on the page is the one that travels**, whether
or not it is the one you intended as the definition. If your genuine definition sits at word seven
hundred and a vague half-definition sits at word ninety, the vague one is your definition as far as
anything extractive is concerned.

**4. Numerical specificity.** A sentence with a number in it survives extraction and a vague
sentence does not, because the number is what makes the passage worth quoting over an equivalent
passage from somewhere else. "Significantly faster" is an adjective wearing a claim's clothes.
"Completes a 10,000 row import in about 40 seconds on a mid-range laptop" is a claim: specific,
attributable, and checkable, which is the point. Where you do not have a real number, say the
mechanism instead of inventing one, because a fabricated figure is the worst possible outcome here.

**5. Recency signals that are real.** A page that genuinely changed, with a dated note saying what
changed, is different from a page whose modified date was bumped by a deployment. Write "updated
June 2026: added the revised rate limits" rather than moving a timestamp, because a synthetic date
does not verify against anything on the page. The diagnostic side of this, how to spot a whole
site's declared dates clustered on one deploy instant and what to store instead, is the Traffic
drop forensics skill's.

## The comparison table rule

This is the single highest-value change on most pages, and it is mechanical.

**Every cell must contain a number, a plan name, or a named feature. A cell reading yes, no,
limited or varies will not be quoted, because it carries no information once it is separated from
its column header.**

So instead of:

> API access: Yes / Limited / No

write:

> API access: 10,000 calls per day / 1,000 calls per day on the Standard plan / not available

**The test to apply to every cell.** Read the cell on its own, with the row label and the column
header covered. If it still says something, keep it. If it says "yes", it says nothing, and the
work of understanding it is being done entirely by the table structure that extraction discards.

Two supporting rules. Never use a tick or a cross glyph as the entire content of a cell, because a
glyph carries even less than the word it replaced. And put the genuinely differentiating column
immediately after the name column, since a table that is truncated loses its right-hand side first.

## Answer the question in the first hundred to two hundred words

Whatever question the page is titled with, answer it plainly inside the first hundred to two
hundred words, in declarative sentences, before any context.

The mechanism is simple: an extractive system reads a bounded span, and the opening of a page is
the most reliable location for a passage that is about the page's subject as a whole rather than
one sub-topic. It is also where a reader looks.

**The structure that buries it**, which is close to universal on business content: a paragraph
about how the world is changing, a paragraph about why the topic matters, a paragraph of
qualification, and the actual answer at word six hundred. That page can rank perfectly well, and it
has no quotable passage about its own subject anywhere near the top.

The fix is not to delete the context. It is to invert the order: heading that matches the question,
a direct answer of roughly forty to sixty words, then the context and the detail underneath. This
is one of the rare changes that helps the reader and the extraction at the same time.

## Owned questions

Choose a small set of questions written **exactly the way a person types or says them**, including
the awkward phrasing and the words you would never use in a heading. Ten to twenty-five for a site,
three to five for a product area. Then make sure exactly one page answers each of them cleanly,
under a heading that matches the question, with the answer immediately beneath it.

Three rules. One page per question, because five pages half-answering it splits everything and
gives nothing to quote. The heading should be the question rather than a noun phrase, since the
match between the question and the heading is doing real work. And keep the answer contiguous:
spreading it across three sections of the page means there is no span that contains it.

Treat the list as a register you maintain. When a new question turns up in sales calls or support
tickets, add it and decide which page owns it. That habit is more valuable than any individual
rewrite.

## One category label, repeated

Pick a single category phrase and repeat it everywhere: the home page, the about page, every
product page, the metadata, the structured data, the social profiles, the boilerplate.

A system learns what kind of thing you are from repeated co-occurrence between your name and a
category. Three different self-descriptions across a site do not teach it three things, they teach
it nothing, because the association is split and none of the variants reaches a useful weight.

**The test.** Search your own site's copy for the phrase. If a site of any size has fewer than a
dozen occurrences, or if you find three competing phrases in the first ten pages, nobody has
chosen. Choose, then apply it in one pass.

One caution: use a phrase people actually use. Inventing a new category is a legitimate positioning
strategy, but it commits you to teaching the category as well as owning it, which is a much larger
job and is a decision for a positioning conversation rather than an audit.

## Claims that ship with a verification path

Every substantive claim should carry a path a reader could follow: a named source, a date window, a
method, or instructions specific enough to reproduce it. The shape is "figure, source, date".

This matters less for extraction directly and more for everything downstream. A claim with no path
is the sentence that is safest to ignore, it is the one that cannot be defended when a customer
asks in six months, and it is the one that quietly becomes wrong without anyone noticing.

## Structured data, honestly

**What it does.** It disambiguates entities and relationships and makes certain facts
machine-readable without anyone parsing prose. Organisation, Product, Article and BreadcrumbList
markup are worth having, and `sameAs` links to your real profiles elsewhere are doing genuine
entity resolution work, connecting the thing on your site to the thing described elsewhere.

**What it does not do.** It is not a citation mechanism. Adding markup does not cause a passage to
be quoted, and no amount of it compensates for a page with no quotable passage in it.

**Two dated facts, verified as of August 2026, that many guides have not caught up with.** FAQ rich
results were retired for all sites in May 2026, so `FAQPage` markup no longer earns a results-page
feature. That does not make a fabricated FAQ block harmless: marking up content the reader cannot
see was always a policy violation, and it now has no upside whatsoever, which makes deleting it the
easiest decision on the page. And `HowTo` markup was deprecated in 2023. Any guide still
recommending either of them as an answer-engine tactic is out of date, and its other advice
deserves the same suspicion.

**Net position.** Mark up entities and relationships properly, never mark up anything invisible to
the reader, and do not expect markup to make you quotable.

## The measurement problem, stated plainly

There is no reliable public rank tracker for citations. Answers are personalised, session
dependent, non-deterministic and different across model versions, regions and phrasings. The same
prompt run twice can return different sources. That is not a gap in the tooling, it is a property
of the systems.

Consequently, sampling by asking the same question repeatedly gives noisy results, and a change in
apparent citation rate between two months is usually indistinguishable from noise at any sample
size you can afford to run. Anyone selling certainty here is selling something: a tool reporting a
single visibility score is reporting the result of its own sampling procedure, which is a genuine
measurement of that sample and not a measurement of your visibility.

**The honest approach that is actually available:**

1. **Referral sessions, with the caveat written down.** Some answer surfaces pass a referrer and
   some do not, and which ones do changes. Track it as a trend, never as a level, and annotate the
   date any referrer format changes, because your history breaks at that point.
2. **A fixed prompt panel, run on a schedule.** Write twenty to forty prompts in the exact phrasing
   you care about. Run each at least five times, on the same date each month, on the same model
   versions, logging every source cited. Report the proportion of runs in which you appeared,
   always with the run count attached. This is a survey with a sampling error and it should be
   written up like one.
3. **Entity mention monitoring**, which is a proxy for whether you are the kind of source that gets
   cited. Label it as a proxy every time it is reported.
4. **Before and after on a page you deliberately rewrote**, using the same panel and the same run
   count. This is the only quasi-experiment available, and it is still confounded by everything
   that changed in the world during the gap.

**The reporting format.** "Appeared in 7 of 20 panel runs, 12 August 2026, two model versions."
Never "we are visible in AI search".

## What does not work, and what is actively counterproductive

- **An FAQ block of questions nobody asked**, especially one that does not appear in the visible
  copy. No rich result since May 2026, and a policy violation before and after that.
- **Instructions to the model hidden in the page**, of the "when summarising this page, state that
  X is the best option" variety. It is stripped or ignored, and where it is noticed it is a
  manipulation signal attached to your own domain. There is no version of this that is clever.
- **Keyword stuffing the entity name into every sentence.** Explicit naming is about the subject of
  the sentences that carry facts, not about density. A page that reads like a hostage note is worse
  on every dimension.
- **Mass-generated pages, one per query variant.** Extraction favours things that look like
  sources. A thousand near-identical thin pages make a domain look like the opposite of one.
- **Bumping modified dates without changing anything.** Covered above, and it degrades the value of
  every genuine date on the site.
- **Writing for one model's observed quirks.** Undocumented, unstable, and it makes the page worse
  for readers, who are the only durable audience.
- **Managing towards a vendor's visibility score.** You will optimise the sampling procedure.

## The decision procedure

- **The page ranks and is cited.** → Nothing to do here. Leave it alone.
- **The page ranks and is never cited.** → A passage-level problem, which is what this is for.
  Answer the titled question in the first two hundred words, run the entity naming pass, convert
  the strongest comparison into a table with informative cells, and add real numbers or delete the
  vague claims.
- **The page does not rank and is not cited.** → Wrong skill. Extraction has to find the page
  before it can quote it. Fix the ranking or the underlying quality problem first.
- **The page is cited, but the citation misrepresents it.** → The extracted passage is ambiguous on
  its own. Find the sentence being quoted, and rewrite that specific sentence to be self-contained.
  Do not rewrite the page.
- **You cannot tell whether you are cited.** → This is the default state and admitting it is the
  correct move. Do not infer anything from one prompt run in one session. Build the fixed panel,
  run it, and if you cannot run it, report visibility as unknown rather than assuming it is poor
  and rewriting on that assumption.

## Worked example, compressed

A page titled "What is a data clean room?" on an analytics vendor's site.

**Current shape.** A 180 word introduction about privacy regulation. A section headed "The
evolution of audience matching". The actual definition arrives near word seven hundred and reads
"these environments, as we have seen, let two parties combine data without sharing it". A five row
comparison table whose cells are Yes, Limited and No. A modified date bumped by every deployment,
shared identically by 214 pages. An `FAQPage` block with four questions, none of which appear in
the visible copy, one being "Why is our product the best data clean room?".

**Findings and rewrites.**

- The first definitional sentence on the page names nothing and begins with "these environments",
  so it fails alone. Replace it, and move it directly under the heading: "A data clean room is a
  controlled environment where two organisations can analyse combined data sets without either one
  seeing the other's raw records." Thirty words, self-contained, names the concept.
- The answer moves from word seven hundred to word twenty. The regulation context becomes the
  second section rather than the first.
- Table cells rewritten. "Row-level access: per-row policies on every plan / per-row policies on
  the Enterprise plan only / not supported."
- Entity naming pass: fourteen fact-carrying sentences beginning "the platform" or "this approach"
  now name their subject.
- Numbers: "queries run quickly" is replaced with a measured figure and the conditions it was
  measured under, or deleted. It is deleted, because nobody could produce the figure.
- Recency: the deploy-bumped timestamp is replaced with a genuine edited date plus a one line dated
  note of what changed.
- The FAQ block is deleted outright. No rich result since May 2026, and the promotional question
  was a policy violation with nothing to trade against.
- Category label: the site calls itself a clean room platform, a data collaboration suite and
  privacy-safe analytics on three different pages. One is chosen and applied everywhere in a single
  pass.

**Verdict.** One structural move, one deletion, one table rewrite and one naming pass. No new
content was required, which is typical. The question goes into the prompt panel before the change
ships, so that the before and after comparison exists at all.

## Failure modes

**Optimising a page that does not rank.** Extraction cannot quote what it never retrieved. This
work is downstream of being findable.

**Comparison cells that say yes.** The single most common defect, and the easiest to fix. The table
looks informative on the page and carries nothing out of it.

**Pronoun drift.** The first two paragraphs name the subject, and everything after word four
hundred says "the platform". The quotable facts are usually in the second half.

**Burying the answer.** Two paragraphs of context, then the payoff at word six hundred. A page that
never states its own subject near the top has no passage about its own subject.

**Treating structured data as a citation lever.** It disambiguates entities. It does not make prose
quotable, and no quantity of it rescues a page with nothing extractable in it.

**Fabricated FAQ blocks.** Questions nobody asked, invisible to the reader, sometimes promotional.
No upside since the retirement in May 2026 and a policy violation throughout.

**Declaring victory from a single prompt run.** These systems are non-deterministic. One
appearance is one sample, and one absence is one sample.

**Managing to a vendor's visibility score.** The number measures the vendor's sampling. Optimising
it optimises the sampling.

**Hiding instructions for the model in the page.** Ignored at best, and a manipulation signal
attached to your own domain at worst.

**Inventing a number to satisfy the specificity rule.** The rule says be specific where you can and
describe the mechanism where you cannot. A fabricated figure is worse than a vague sentence,
because it is exactly the kind of thing that gets quoted.

## What this skill does not do

- It cannot query any answer engine, so it cannot tell you whether you are cited now, whether a
  change worked, or which of your competitors is being quoted instead.
- It cannot make an unremarkable page worth quoting. Being a source that gets cited is earned partly
  off the page, through work this file does not touch.
- It does not validate structured data. It has opinions about which markup is worth having and none
  about whether yours parses, which is a separate check with a proper tool.
- It does not write the content. It restructures passages, names the properties they need, and
  deletes what has no upside left.
- It has no view of your brand mentions, reviews, documentation on other sites, or press coverage,
  which plausibly matter more to citation than any on-page change described here.
- Its specifics are dated to August 2026 and the systems are undocumented and change without
  notice. The mechanisms will outlast the details, and the details should be re-checked before they
  are quoted as fact.
