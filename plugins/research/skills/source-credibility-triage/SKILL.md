---
name: source-credibility-triage
description: Rates how far a source can be trusted for a specific claim before it is cited. Selects the right evidence hierarchy for the claim type, since the ranking that applies to an intervention's effect inverts for a claim about what a specification requires. Separates primary from secondary and tertiary sources, handles preprints and their later published versions, identifies predatory journals from metadata rather than design, checks retractions at the identifier rather than in the PDF, reads funding and conflict statements, and applies the same discipline to vendor white papers, industry surveys and consultancy reports. This skill should be used whenever a source is about to be cited, quoted or relied on in work that someone else will act on.
---

# Source credibility triage

## The claim this skill is built on

A source is not credible in general. It is credible for a particular claim, and the ranking of sources changes when the claim changes. Most credibility advice fails because it hands you one ladder and expects you to climb it regardless of what you are trying to establish.

The second failure is subtler. Almost every real check that separates an authoritative source from one that merely looks authoritative lives in the metadata, not in the text: the dates on the article, the funding statement, the editorial board, the identifier, the sample description. A source can read beautifully, cite thirty references, sit on a well-designed site, and still be worthless. Design tells you about the budget. Metadata tells you about the process.

So this skill runs claim type first, then hierarchy, then metadata, and only then content.

## Step 1. Name the claim type, because it selects the hierarchy

Write down, in one sentence, the specific claim the source is being used to support. Then classify it.

**A claim about the effect of an intervention.** Does treatment A change outcome B. Here the classical hierarchy holds, strongest first:

1. A systematic review or meta-analysis of randomised trials, with a stated search strategy and an assessment of risk of bias in the included studies.
2. A single randomised controlled trial, adequately powered and pre-registered.
3. A prospective cohort study.
4. A case-control study.
5. A case series or case report.
6. Expert opinion, editorials, narrative reviews.

Two cautions on this ladder. A systematic review is only as good as the studies in it, so a review of six weak trials is not stronger than one large good trial. And the modern evidence-appraisal frameworks moved away from treating study design as destiny: the GRADE approach rates certainty by starting from design and then adjusting for risk of bias, inconsistency, indirectness, imprecision and publication bias, which means a well-conducted observational study can outrank a badly conducted trial.

**A claim about what a specification, standard, law or contract requires.** The hierarchy inverts. The normative document is primary and nothing outranks it. A conference talk by a member of the working group is secondary. A widely cited tutorial is tertiary and frequently wrong, because tutorials encode the version that was current when they were written. Check the document, check its version, check whether the clause was amended, and check whether the clause is normative or informative, since standards bodies routinely mark sections as non-binding and readers routinely ignore that marking.

**A claim about what an organisation did, said or reported.** Primary is the filing, the transcript, the changelog, the archived page, the court document. A news article about it is secondary.

**A claim about the size of a market, a population or a cost.** No hierarchy saves you. This is provenance work, and a source that reports the figure is worth nothing until you find where the figure was produced.

**Decision rule.** If you cannot classify the claim into one of these, the source cannot be triaged yet, because you do not know what it is being asked to support. Sharpen the claim first.

## Step 2. Primary, secondary or tertiary

The working test is one question: **does this document contain the observation, or does it report someone else's observation?**

- **Primary.** The people who made the measurement, wrote the specification, ran the trial, kept the record. It contains the observation.
- **Secondary.** Reports, synthesises or interprets primary work. Reviews, textbooks, most journalism, most analyst notes.
- **Tertiary.** Summarises the secondary layer. Encyclopaedias, course notes, listicles, most of what a search engine returns first.

A systematic review is secondary and is often the best available source, so secondary is not an insult. The point of the test is that a source cannot be treated as primary just because it is respectable. If it reports someone else's number, its credibility for that number is entirely inherited, and inherited credibility is exactly where the chain breaks.

## Step 3. Preprints

A preprint is a manuscript posted publicly before, or instead of, journal peer review. Established servers include arXiv for physics, mathematics and computer science, bioRxiv and medRxiv for biology and medicine, and SSRN for social sciences.

Preprints are legitimate and useful. They are how a field moves at the speed of the field rather than the speed of the review queue, they are free to read, and in fast-moving areas they are frequently the only source that exists. Refusing to read them is not rigour.

What they are not is reviewed. Three specific checks:

1. **Label it.** If the source is a preprint, the citing document must say so. Presenting a preprint as though it were a peer-reviewed article is the most common single misrepresentation in assisted research.
2. **Check whether it was later published.** Most servers link the published version once it exists, and the identifier resolves to it. If it has been published, cite that.
3. **Check whether the published version differs.** This is the step people skip and it is where the value is. Conclusions get softened in review, effect sizes get recalculated, subgroup claims get removed, sample sizes change when data cleaning is challenged. A number quoted from the preprint may not exist in the paper.

A preprint that has been sitting unpublished for a long time is a fact worth noting rather than a verdict. It may mean rejection. It may mean the authors never submitted it. It may mean the field posts and moves on.

## Step 4. Peer review, honestly

Peer review filters. It does not verify.

What it usually catches: obvious methodological errors, missing controls, claims not supported by the presented data, failures to cite an obviously relevant prior result, and unclear reporting.

What it usually does not catch: fabricated data, selective reporting of outcomes that were measured but not published, undisclosed conflicts of interest, statistical errors that require re-analysis of the raw data, and results that are simply not reproducible. Reviewers are unpaid, work from the manuscript rather than the dataset, and typically cannot rerun anything.

So "peer reviewed" is a floor. It means a small number of people in the field read it and did not object. It is a meaningful floor and it is not a guarantee, and the phrase is routinely used in argument as though it were the end of the discussion rather than the beginning.

## Step 5. Predatory and low-quality journals, identified by metadata

These are identified by process signals, not by how the site looks. Many have competent design.

- **An implausibly short interval from submission to publication.** Most journals print both dates. Genuine review, revision and re-review takes weeks at the fastest and commonly months. A paper received on the 3rd and accepted on the 9th, with no revision history, did not receive review.
- **An editorial board that cannot be verified.** Pick three names and check them independently. Names are frequently listed without consent, and the listed institution often has no record of the person.
- **A scope so broad it covers unrelated fields.** A single journal covering medicine, engineering, education and management is not a journal, it is a payment channel.
- **Aggressive solicitation.** Unsolicited email inviting a submission, flattering a paper the sender has clearly not read, promising rapid publication.
- **An article processing charge with no visible editorial process.** A fee is not itself a red flag, since reputable open access publishing runs on fees. A fee with no named editor, no stated review policy and no retraction policy is.
- **A name or a site that closely mimics an established title.** A word added, a word dropped, a plausible-looking metrics badge that is not from a recognised indexing service.

**The positive check matters more than the negative one.** Rather than trying to prove a journal is bad, check whether it appears in a directory that applies published criteria: the Directory of Open Access Journals for open access titles, and membership of the Committee on Publication Ethics for the publisher. Think. Check. Submit. is a free cross-publisher checklist built for exactly this question. Note the asymmetry: presence in a directory is meaningful, absence is weak evidence, because new and small legitimate journals are frequently not listed yet.

## Step 6. Retractions and corrections

A retracted paper keeps accumulating citations for years. The mechanism is mundane: the notice appears on the publisher's page for the article, while the copy most people read is a PDF saved on a personal site, a repository mirror, a course page or a shared folder, and that file is unchanged. It carries no watermark, no notice and no hint. Someone citing it has no way to know from the file alone.

The practical procedure:

1. Take the digital object identifier, not the file. Resolve it and read the publisher's landing page for the article.
2. Look for a retraction, expression of concern, correction or erratum. Crossref's Crossmark service exists to surface exactly these updates against the identifier, and the Retraction Watch database, which Crossref made openly available in 2023, is the largest curated list of retractions in existence.
3. Check post-publication commentary. PubPeer carries reader-reported concerns that frequently precede a formal retraction by years.
4. If the paper is being used as evidence, check its key references too. A retraction upstream does not propagate downstream automatically.

Distinguish the three outcomes, because they are not the same. A **correction** fixes an error and usually leaves the conclusion intact. An **expression of concern** means the journal has doubts it has not resolved, so the paper is unusable as load-bearing evidence until it is. A **retraction** means the paper should not be relied on at all, and it may be for misconduct or for honest error, which the notice usually states.

## Step 7. Funding and conflict of interest

Read the funding statement and the competing interests declaration. In most journals they sit at the end, before the references, and most readers never scroll there.

- **Who paid.** Funding by a party with a commercial interest in the result is not disqualifying and it is material. It changes what caveat the citation needs.
- **Who was involved.** Employment, consulting, patents, shareholdings, paid advisory roles, and writing assistance provided by a third party.
- **Disclosed versus undisclosed.** A disclosed conflict is a fact you can price in. An undisclosed conflict you later discover is a reason to distrust everything else in the paper, because the omission was a choice. Where a stated declaration of no competing interests turns out to be false, that is a much more serious finding than a large disclosed one.
- **Trial registration.** For clinical work, a study registered in advance with its primary outcome stated is far more trustworthy than one where the outcome reported is chosen after the data are in.

## Step 8. Non-academic sources, where most work actually happens

Most commercial research never touches a journal. The same discipline applies, with different questions.

**A vendor white paper.** Who ran the analysis, and does the vendor sell the thing it recommends. Is there a methodology section at all, or only a findings section. Are the comparisons against named alternatives or against an unnamed composite. Treat it as a hypothesis with a source of funding attached.

**An industry survey.** Four questions, in order: who was sampled, how many, how were they recruited, and what was the response rate. Recruitment is the one that decides everything. A survey advertised to a vendor's own mailing list measures that mailing list. If the sample size is given and the recruitment method is not, that is a deliberate omission, because the sample size is the flattering half.

**A trade publication.** Useful for what happened and who said it. Frequently a rewrite of a press release, so check whether the wording matches the release. Named sources beat unnamed, and a link to the underlying document beats a description of it.

**A consultancy report behind a paywall.** The pattern is a headline figure circulating freely while the methodology sits behind a five figure price. Anyone can quote it and nobody can check it. Cite the figure only if you can see how it was produced, and if you cannot, say that the figure comes from an unpublished methodology.

**A press release from an institution.** Frequently overstates the underlying study. Where a release and a paper disagree, the paper wins.

## Step 9. The single most useful question

**What would this source have said if the opposite were true?**

If the answer is that it could not have said anything else, the source is not evidence. It is a position. A vendor survey whose questions cannot produce a finding against the vendor, an editorial written to defend an existing commitment, an evaluation commissioned by the people whose programme is being evaluated: all of these can be honest, competent, and still incapable of surprising anyone.

The related question for a study: was there a result this design would have failed to detect. An underpowered trial reporting no difference has not shown there is no difference.

## The triage procedure

For each source, in order. Stop early when a step settles it.

1. State the claim in one sentence and classify it. Select the hierarchy.
2. Primary, secondary or tertiary for that claim.
3. Identify the type: journal article, preprint, specification, filing, vendor document, survey, trade article, blog post.
4. Run the type-specific metadata checks above.
5. Check the identifier for retraction, correction or concern.
6. Read the funding and conflict statements.
7. Ask the opposite question.
8. Output a rating and a one-line reason.

**The rating, with explicit branches:**

- **Strong.** Primary or a high-quality synthesis for this claim type, verified metadata, no unresolved conflict. Cite it directly.
- **Usable with a stated caveat.** Real evidence with a named weakness. Cite it, and write the caveat into the sentence, not into a footnote. "A vendor-funded survey of self-selected respondents reports X."
- **Weak, do not lead with it.** Tertiary, undisclosed methodology, or an unresolved concern. Use it for background or to find better sources. Never let it be the only support for a load-bearing claim.
- **Do not cite.** Retracted, predatory venue, fabricated or unverifiable authorship, or a claim the source does not actually make.
- **Cannot assess.** The material needed to judge is not reachable: paywalled methodology, dead link with no archived copy, an organisation with no traceable existence. **This is a legitimate result, not a failure.** Record what was checked, what was missing, and what would settle it. A cannot-assess that is honestly reported is more useful than a confident rating built on nothing, and it tells the next person exactly where to start.

## Worked example, compressed

Four sources gathered for a claim that a particular working practice reduces defects. All figures below are invented for illustration.

**Source A, a meta-analysis in an established journal.** Claim type is intervention effect, so the classical hierarchy applies and this sits at the top. It states its search strategy, its databases and its inclusion criteria, and it assesses risk of bias in the included studies. Funding is a public research council. The identifier resolves cleanly with no update notice. Two of its eleven included studies are small and the review says so. **Strong.**

**Source B, a preprint on a public server, posted eighteen months ago.** Widely quoted for an invented figure of a 40 per cent reduction. Checking the server record, a published version exists in a journal. In the published version the reduction is stated as 22 per cent with a confidence interval that includes values close to zero, and the subgroup that produced the 40 per cent figure was removed in review as underpowered. The widely quoted number does not exist in the final paper. **Usable with a stated caveat, and the caveat is that the correct figure is the published one.**

**Source C, a paper in a journal nobody in the team recognises.** Submission and acceptance dates are eleven days apart with no revision history. The scope covers computing, education and clinical medicine. Three editorial board members cannot be verified at the institutions listed. The publisher is not a member of any ethics body and the journal is not in the open access directory. Article processing charge is stated, review policy is not. **Do not cite.**

**Source D, a vendor white paper.** Reports that an invented 78 per cent of teams saw improvement. Sample size given as 1,200. Recruitment method not stated anywhere in the document, and there is no methodology appendix. The vendor sells tooling for the practice being evaluated. Applying the opposite question: no configuration of this survey could have produced a finding against the practice. **Weak, do not lead with it.** If the underlying methodology is requested and refused, the correct entry is cannot assess, recorded as such.

**Overall verdict.** The claim is supportable at the published effect size from A and the journal version of B, stated with its interval. C and D are removed. The document should say 22 per cent, not 40, and should not repeat the vendor figure at all.

## Failure modes

**Rating the source instead of the pairing.** A source is rated for a claim. The same journal article can be strong for its own result and worthless for the background sentence in its introduction, which is somebody else's work reported at second hand.

**Applying the intervention hierarchy to a specification question.** Preferring a well-cited review over the normative document, which is exactly backwards, and inheriting whatever version the review was written against.

**Judging a journal by its design.** Predatory publishers buy templates too. Every real signal is in the dates, the board and the policies.

**Reading the PDF instead of the record.** The single most common way a retracted paper survives. The file cannot tell you it was retracted.

**Treating peer reviewed as a verdict.** It is a floor, and using it to close an argument is a category error rather than a small overstatement.

**Quoting a preprint figure that the published paper contradicts.** Common precisely because the preprint is the version that circulated and got quoted, so it is the version everyone has seen.

**Accepting a sample size without a recruitment method.** The number of respondents is the flattering half of the description. How they were found is the half that decides whether the result means anything.

**Refusing to output cannot assess.** Producing a confident rating from unreachable evidence, which converts a known gap into an invisible one.

## What this skill does not do

- It does not detect fabricated data, image manipulation or statistical inconsistency in reported values. Those need specialist tools and, usually, the raw data.
- It cannot read what it cannot reach. Paywalled methodology, supplementary files behind a login and dead links with no archived copy all resolve to cannot assess.
- It does not know which journals a specific discipline respects. That knowledge is local, it changes, and a subject librarian or an active researcher in the field has it.
- It does not settle whether a claim is true. It rates how much weight one source can carry, which is a different question, and a strong source can still be reporting a result that later fails to replicate.
- It does not trace a number back to its origin. When the source reports someone else's figure, credibility triage stops and provenance tracing starts.
