---
name: ai-disclosure-audit
description: Decides whether a piece of work needs to disclose that a generative model was involved, and what a real disclosure has to say. Maps the settings where disclosure is required rather than optional, including academic publishing, advertising and endorsement rules, platform policies for synthetic media, institutional policy in employment and education, and statutory transparency obligations. Carries a taxonomy of seven kinds of use that carry different obligations, and a list of outputs that must never be produced at all. This skill should be used before publishing, submitting or shipping any work a model helped produce.
---

# AI disclosure audit

## The claim this skill is built on

"Should I disclose that I used AI?" is not one question. It is at least five, they have different
answers, and the reason people find it unanswerable is that they are trying to answer it about the
tool rather than about the use and the setting.

Two things make it decidable. The first is knowing which policy family you are in, because the
obligation comes from a publisher, a regulator, a platform or an institution, and never from a
general principle. The second is being precise about what the model actually did, because using a
model to find papers and using it to generate the data in a figure are not the same act and no
policy treats them as the same act.

**Everything in this file is a structural map, not legal advice.** Obligations differ by
jurisdiction, by publisher, by platform and by employer, and this area has been changing quickly for
several years. Every policy detail below must be re-checked against its source at the point of
publication. Where a rule matters to you, read the actual document, not this summary of it.

## The map: where disclosure is required rather than polite

### Academic and scholarly publishing

The most settled branch, and the one with the clearest consensus.

**Authorship.** The major journal bodies and the large publishers have converged on the position
that a generative tool cannot be listed as an author. The reasoning is consistent across them:
authorship carries accountability for the work, including the ability to approve the final version
and to answer for its integrity, and a tool cannot hold any of that. This position was set out by
the Committee on Publication Ethics and by the International Committee of Medical Journal Editors in
2023, and adopted by the large commercial publishers over the same period. Check the current wording
at the source, because the surrounding requirements have been revised since.

**Description of use.** Where a tool was used, journals generally require it to be described, in the
methods if it touched the research and in the acknowledgements if it touched only the writing. Many
require the **specific tool and version** to be named, and some ask for the dates of use and the
purpose. Some also require a statement that the authors reviewed and take responsibility for the
output.

**Images and data.** Several publishers apply stricter rules to generated images than to generated
text, in some cases prohibiting them outright outside of papers about the technology itself. If your
submission contains a generated figure, this is the specific thing to check, because a policy that
permits drafting assistance may still refuse the figure.

**Peer review.** A separate and often stricter rule. Several publishers and funders prohibit putting
a manuscript under review into an external tool at all, on confidentiality grounds, which is a
question about permission rather than disclosure.

### Advertising, endorsements and reviews

Here the issue is usually not that a model wrote the copy. It is whether the content misrepresents a
relationship or an experience.

- **Material connections** between an endorser and a brand must be disclosed under the endorsement
  rules of most advertising regulators, and this is true whoever or whatever wrote the words.
- **Synthetic testimonials and reviews** are the live risk. A review attributed to a customer who
  does not exist is a false review, and consumer protection regulators have moved specifically
  against fake reviews and testimonials in recent years, including a dedicated rule finalised in the
  United States in 2024 and provisions in United Kingdom consumer law from the same period. Check the
  current rule in your market. The safe reading is simple: do not generate a review, a testimonial,
  or a quoted customer experience.
- **Generated depictions of people** in advertising bring in both the endorsement rules and, in some
  jurisdictions, publicity and likeness rights.

### Platform policies for synthetic media

The largest video and social platforms introduced disclosure or labelling requirements for realistic
synthetic media during 2023 and 2024, generally aimed at content that could mislead a viewer into
thinking a real event happened or a real person said something. Several also apply automatic labels
using embedded provenance metadata, and the open standard most of them reference is Content
Credentials from the C2PA. Requirements differ per platform and change, so check the current help
page for the platform you are posting to. Political advertising is usually a separate and stricter
category.

### Employment and education

**The governing rule is the institution's own policy, and it overrides every general principle in
this file.** A university's academic integrity policy, an employer's acceptable use policy, a
client's contract terms. If a policy exists, it is the answer. If none exists, ask in writing, which
also creates the record that protects you.

Adjacent obligations exist that are not really about disclosure at all: several jurisdictions
regulate automated tools in hiring, including a New York City law enforced from 2023 that requires
bias auditing and candidate notice for automated employment decision tools. If a model is in a
decision path about people, that is a different and heavier regime.

### Statutory transparency obligations

A growing set of laws require that content generated or manipulated by these systems be marked or
disclosed.

The most developed example is the **European Union's AI Act**, which entered into force in August
2024 and applies on a phased timetable. Its transparency provisions cover systems that interact with
people and content that is artificially generated or manipulated, including a machine-readable
marking duty and a disclosure duty for deepfakes, with exceptions including artistic and satirical
work. The timetable for the transparency tier has itself been the subject of revision and
implementation guidance, so **check the current position rather than relying on any date quoted
here**, including the dates in this paragraph.

Other jurisdictions have their own: China has had rules on deep synthesis services since 2023 and
introduced a labelling measure for synthetically generated content afterwards, and several United
States states have enacted disclosure rules for synthetic media in election advertising.

**One structural point that resolves a lot of confusion.** Some of these obligations fall on the
provider of the system rather than on the person using it. A law requiring a large model provider to
embed provenance metadata is not a law requiring you to add a footnote. Before assuming a duty is
yours, check who it binds.

## The taxonomy of use

This is what makes disclosure decidable. Name what the model actually did.

| Kind of use | What it means | Where the obligation usually sits |
|---|---|---|
| **Research assistance** | Finding sources, summarising a field, suggesting search terms. | Usually low. The output is your reading, and everything cited must be verified. |
| **Drafting** | The model produced text that survives in the final work. | The main disclosure case in academic and institutional settings. |
| **Editing a human draft** | Grammar, tightening, restructuring text you wrote. | Often exempt, and several policies say so explicitly. Check, because the boundary between heavy editing and drafting is where policies differ most. |
| **Translation** | Rendering your work into another language. | Increasingly treated as its own category. Some journals require it to be stated, since translation carries meaning decisions. |
| **Summarising** | Condensing a source, a transcript, or your own work. | Depends on whether the summary is presented as your reading of the source. If it is, verify it against the source first. |
| **Image generation** | Any generated or heavily manipulated visual. | The strictest category almost everywhere. Often prohibited in scholarly figures and usually subject to platform labelling. |
| **Data generation** | Producing numbers, tables, synthetic samples, or filling gaps in a dataset. | The most serious category. Generated data presented as measured is fabrication, not a disclosure question. |

The reason people cannot answer the disclosure question is that they compress all seven into "I used
AI". Separate them and most cases answer themselves: research assistance and light editing rarely
require anything, drafting usually requires a statement in a governed setting, generated images
require checking before you commit, and generated data requires stopping.

## The responsibility rule

**You are accountable for every factual claim in the output regardless of how it was produced.**

This single rule resolves most of the cases people find hard, because it separates two questions
that get muddled. Disclosure is about the reader's right to know how the work was made. Verification
is about whether the work is true. They are independent, and the second is not satisfied by the
first.

The consequence is blunt: **anything unverified must not ship, disclosed or otherwise.** A footnote
saying a model helped write the piece does not license an unchecked citation, an unchecked number,
or an unchecked quote. Disclosure is not a warning label that transfers risk to the reader.

## Things that must never be produced

Not a disclosure question. These do not become acceptable when labelled.

- **Fabricated quotes attributed to a real person.** Including a plausible paraphrase of what they
  would probably have said, and including public figures.
- **Fabricated citations.** A reference that does not exist, or a real reference that does not
  support the claim attached to it. This is the most common serious failure in generated academic
  writing and it is checkable in minutes.
- **Invented data presented as measured.** Filled-in survey results, plausible benchmark numbers,
  gap-filled time series. If it was generated, it is not a measurement, and calling it an estimate
  does not fix it unless the method is stated.
- **Synthetic reviews or testimonials.** Covered above under consumer protection rules, and wrong
  regardless of the rules.
- **Impersonation of a real individual's voice or likeness**, in text, audio or image, outside
  clearly marked satire where the law of your jurisdiction permits it and the marking is impossible
  to miss.

## The decision rule

Work down in order. The first branch that fires is your answer.

1. **A policy applies and names this use.** Disclose exactly as it specifies, in the place it
   specifies. Do not improve on the wording.
2. **A policy applies but is silent on this use.** Disclose at the quality bar below, and ask the
   publisher, institution or client in writing. The written answer is the point.
3. **No policy applies, but the work could be mistaken for a first-hand human account, or depicts a
   real person, or is a review, endorsement or testimonial.** Disclose, or do not publish it.
4. **No policy applies and none of the above is true.** Disclosure is optional. See the good
   practice section.
5. **You cannot tell whether a policy applies.** Treat it as though it does, and ask before
   submission. Never resolve this branch by omission, because the cost of an unnecessary disclosure
   is mild embarrassment and the cost of a missing one, in a governed setting, is a retraction, a
   misconduct finding, or a regulatory action.

## The disclosure quality bar

A real disclosure answers three questions:

1. **Which tool**, named, with the version where the policy asks for it.
2. **For what part of the work**, specifically. Drafting section three. Translating the abstract.
   Generating the illustration on page two. Not "in the preparation of this work".
3. **Who checked it**, and what they checked. This is the part almost every disclosure omits and it
   is the part that carries the accountability.

A footnote saying that artificial intelligence was used somewhere is not a disclosure. It is a
shrug. It gives the reader no way to calibrate anything, and it protects the author rather than
informing the audience, which is the opposite of the purpose.

A serviceable form: "The first draft of the discussion section was produced with [tool, version] in
[month, year]. All sources were retrieved and read by the authors, and all figures are original. The
authors reviewed and edited the text and take responsibility for its content."

## When nothing requires disclosure

Most writing is in branch four. Good practice there is short.

- **Keep a private record** of what was used where. If you are asked in six months, you want an
  answer rather than a reconstruction.
- **Keep the human review step real** rather than nominal. The claim that you checked it should be
  true.
- **Never claim first-hand experience you do not have.** This is the line that matters far more than
  a footnote. Writing "in the migrations I have run" about migrations you have not run is a lie
  whether a model wrote the sentence or you did.
- **Be more transparent as the reader's decision depends more on a human having done the work.** A
  product description does not need it. An account of a personal experience, a recommendation, or a
  professional opinion does.

## The argument against detector-driven writing

Detectors have both false positives and false negatives at rates that make them unsafe for
consequential decisions about individuals. Three specific things are worth knowing rather than
asserting generally.

They **misfire on non-native English writers**. A study published in the journal Patterns in 2023,
"GPT detectors are biased against non-native English writers", found that detectors misclassified a
large share of essays by non-native writers as generated, while classifying native-writer essays
correctly. The mechanism is that the detectors keyed on textual simplicity, which is a property of
second-language writing as well as of generated text.

The vendors themselves have withdrawn products for accuracy reasons. The best-known example is the
AI text classifier withdrawn by its own developer in 2023, with low accuracy given as the reason.
Several universities disabled detection features in their integrity tooling over the same period
after false-positive incidents.

And the direction of the error is the problem, not just the rate. A detector that is wrong 5 percent
of the time, applied to a thousand students, produces fifty accusations, and the accused has no way
to prove a negative.

**Optimising a draft against a detector produces worse writing.** The features that lower a score
are not the features that make prose good: introducing errors, inflating vocabulary, and breaking
rhythm on purpose. You end up with prose that is harder to read and still generated.

**The correct response to a detector result is the work itself.** Drafts, notes, version history,
the ability to talk about the argument. If you are the one running a detector, treat a score as a
prompt to have a conversation, never as evidence.

## Worked example

Three decisions, run through the rule.

**Case one. A journal submission.** The model was used to find literature and to tighten the
discussion section. No generated figures.

Taxonomy: research assistance plus editing, with some drafting in the discussion. Branch one, since
the journal's author instructions cover generative tools. Action: no authorship, a statement in the
acknowledgements naming the tool and version and the sections affected, and every retrieved source
independently verified before citation, because the research-assistance category carries the highest
fabricated-citation risk. **Verdict: disclose in the acknowledgements, verify all references, no
methods entry, because nothing touched the research itself.**

**Case two. A product landing page.** The copy was drafted by a model and edited by a marketer. It
includes a quote from a named customer.

Taxonomy: drafting. Branch four for the copy itself, since no general rule requires disclosing who
drafted marketing text. But the quote is a different object. If it came from a real customer with
their permission, fine. If it was generated or improved, it becomes a synthetic testimonial and the
consumer protection rules apply. **Verdict: no disclosure required for the drafting, and the quote
must be verbatim from a real customer with a record of consent, or removed.**

**Case three. A blog post with a generated illustration showing a recognisable public figure.**

Taxonomy: image generation, depicting a real person. Branch three, and then the never list. The
platform's synthetic media labelling policy applies to the image, and the depiction of an
identifiable real person is the disqualifying element. **Verdict: do not publish the image.
Labelling does not cure it. Replace it with something that does not depict a real individual.**

## Failure modes

**The blanket footnote.** "AI was used in the creation of this content." It names no tool, no part
of the work, and no reviewer, so it cannot be acted on by anyone. It exists to protect the author.

**Disclosure as absolution.** The piece discloses the drafting and ships four unverified citations.
The disclosure makes this worse, not better, because it demonstrates the author knew a model was
involved and did not check its output.

**Disclosing at submission and not at revision.** The tool changed between draft and final, or was
used again during revision, and the statement now describes something that is not what happened.

**Over-disclosure that hides the material case.** Every trivial use is disclosed at equal weight, so
readers stop reading disclosures, and the one that mattered is buried among the spellchecks.

**Assuming the provider's duty is yours.** A transparency law binding large model providers gets
read as a personal labelling obligation, producing pointless notices, while the actual applicable
policy, the publisher's, goes unread.

**Confusing permission with disclosure.** The real question was whether the material was allowed
into the tool at all: a manuscript under peer review, client data under contract, personal data
under a protection regime. Disclosing afterwards does not repair a confidentiality breach.

**Detector-driven rewriting.** A draft is degraded on purpose to move a score, and the score is
unreliable in both directions anyway, so the writing gets worse and nothing is proven.

**Retro-fitting.** A disclosure is quietly added after publication rather than the record being
corrected. In a governed setting this is usually treated more seriously than the original omission.

## What this skill does not do

- It is not legal advice, and it does not know your jurisdiction, your contract, or your employer's
  policy. Every branch ends in reading the specific policy that applies to you.
- Its policy detail ages. Dates, thresholds and requirements in this area have changed repeatedly
  and will change again, so treat everything here as a pointer to a source rather than as current
  fact.
- It cannot detect generated text, and it argues that nothing else reliably can either. If you need
  to establish provenance, the evidence is drafts, history and process, not a score.
- It does not cover data protection, confidentiality, or copyright and ownership of output. Those
  are separate questions and frequently the more serious ones.
- It does not verify claims. Checking whether a citation exists and says what you claim is the
  research category's job, and the claim-and-hedge-audit skill covers the language side of the same
  problem.
- It will not help you evade a policy, a detector, or an obligation. If the honest answer is that
  the work should not be published as it stands, that is the answer it gives.
