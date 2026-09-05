---
name: ats-resume-tailorer
description: Rewrites a CV against one specific job advert and produces both the revised document and a record of every change made and why. It runs the eligibility and knockout questions before touching the CV, fixes the delivery layer so the text is actually parsed, selects a small number of capabilities to promote, and applies vendor-documented parser and recruiter-search behaviour rather than folklore about keyword density. This skill should be used when tailoring a CV to a named job advert, when preparing to submit through a company careers site or an applicant tracking system, or when a CV has been rebuilt with columns, graphics or a design tool and needs to be made machine readable again.
---

# ATS resume tailorer

## The claim this skill is built on

Two beliefs govern almost all CV advice, and both trace to the same company, which no longer exists.

The first is that roughly 75 per cent of CVs are rejected by an applicant tracking system before a human sees them. Its earliest traceable publication is a CIO.com article by Meridith Levinson dated 1 March 2012, and the original sentence is not the one in circulation: these systems "kill 75 percent of candidates' chances of landing an interview", attributed to Preptel, a company selling resume-optimisation software to job seekers. That is a claim about interview probability, made by an interested party, not a claim that three quarters of applications are auto-rejected. No methodology was ever published, and Preptel folded in August 2013.

The second is that an ATS cannot read PDFs. Same vendor, a blog post of 26 May 2011 claiming "PDF files can't be read correctly by about 90% of the software being used in the employment market", with a journalist rather than a test as its stated basis. It predates essentially every parser in use. As of August 2026 Greenhouse's own candidate FAQ answers "What resume file format works best?" with "Upload a PDF for best results", and Ashby recommends converting to PDF before uploading.

Replacing that folklore is one structural fact. **The system that scores a CV and the system that rejects an applicant are, in every vendor examined, different systems.**

Greenhouse states it twice: "It does not automatically advance or reject candidates", and, answering "Does Talent Matching auto-reject candidates with a low match score?", "No." Lever: "Talent Fit is not intended to filter out candidates." Meanwhile the documented automatic rejection in those same products keys on structured form answers. Greenhouse's Auto-Reject "uses custom application questions", configurable only on yes/no, single-select and multi-select types. Workable restricts auto-disqualification to yes/no questions. SAP has a status named Auto-Disqualified for prescreen failures, and a Required Score threshold below which an applicant is disqualified automatically.

And the two halves do not touch. Greenhouse: "The matching algorithm is only using data coming from the resume. Currently, structured application form data is not included in this calculation." Lever: "Talent Fit uses only the Job Description and the candidate's resume."

Two exceptions to carry. Workday makes no public statement either way about Candidate Skills Match, so anyone claiming Workday denies auto-rejection is inventing it. And Workable documents an Agent setting that auto-disqualifies against criteria "marked as Disqualify if not met", off by default and which Workable advises leaving off.

The consequence is the whole method: **rejection lives in the application form, and the CV is a ranking and findability problem.** So the form comes first.

## Phase 0: work out what you are applying into

Five minutes, before any editing. The domain serving the application form usually names the vendor outright. Check whether the advert discloses AI screening, which Ontario has required in publicly advertised postings since 1 January 2026, employers of fewer than 25 people exempt.

Record three things: the vendor if identifiable, whether AI screening is disclosed, and the formats the upload control accepts. Learning nothing is a valid outcome, and the cannot-tell branches below are written for it.

## Phase 1: the knockout sheet, before you touch the CV

First, because it is the only stage where a mistake is documented to end an application automatically, and a failure here makes every later phase wasted work.

Extract from the advert every requirement that is a fact about you rather than a quality: work authorisation, a named licence or certification, location or willingness to relocate, minimum years where the employer states a number, security clearance, notice period. In a 2025 study of 25 United States talent acquisition professionals, run by the resume service Enhancv between September and October 2025, all 25 reported using knockout questions for exactly these checks. Small sample and an interested publisher, so indicative rather than definitive.

Produce a two column sheet, requirement and accurate answer, then apply the rule.

- **Every hard requirement met.** Proceed. Answer each form question accurately and completely, and leave none blank.
- **A hard requirement is not met and is genuinely binary**, such as a licence you do not hold. Stop. Nothing in the CV changes this.
- **A requirement is soft or numeric**, such as a stated years-of-experience minimum. Proceed, and note it. The Harvard Business School and Accenture study "Hidden Workers: Untapped Talent" (3 September 2021, 2,275 executives across three countries) found years of experience among the criteria employers configure their systems to rank and filter on.
- **You cannot tell whether a requirement is hard.** Adverts routinely list preferences in the same block as requirements. Default to proceeding and write the honest answer. Where the form itself puts the question as yes/no, that is your signal it is hard.

The employment gap is the one filter with real evidence behind it. In the same study, 48 per cent of employers reported filtering middle-skills candidates on gaps of more than six months, the resume being "automatically screened out by their RMS or ATS, based on that consideration alone". Handle one explicitly in Phase 4.

A hard prohibition, because the misattribution is everywhere: that study says nothing about parsing or formatting. A search of the full report for "parsing" and for "resume format" returns zero hits. Do not cite it as evidence that formatting gets CVs rejected.

## Phase 2: the delivery layer

Keywords inside text that was never extracted are invisible to both the score and the recruiter's search, so this comes before the writing.

**Verify the text layer.** Open the file, select all, paste into a plain text editor, and read it in the order it arrives. That order is what the parser receives. Affinda publishes the threshold: it applies OCR across the whole document when fewer than 25 words are found in the text layer, and names the symptom of a corrupt layer as "duplicated text, garbled output, or wildly incorrect values despite the document looking fine visually".

**Never rely on OCR.** Textkernel returns a specific error for an image-only document, "You will need to enable OCR in your account to parse this document", and states that around 5 per cent of documents require it. OCR is a paid add-on rather than a base feature, so whether it runs at all depends on what the employer bought.

**Mind the size gap, which is silent.** The upload limit and the parse limit are different numbers, and the upload succeeds either way. Greenhouse publishes both of its own: "Candidate uploads can be up to 100 MB", and "Greenhouse Recruiting can't parse resumes larger than 2.5MB". A factor of 40, so a file between the two uploads successfully, shows no error, and is never parsed. Ashby has the same gap, accepting 50 MB but processing only files "16MB or smaller". Oracle Taleo parses only 100 kilobytes, customer-configurable, plausibly the real mechanism behind the "Taleo cannot read PDFs" folklore, since a PDF with embedded fonts clears 100 KB easily where the same CV as .doc fits. Oracle does not say that itself. Keep the file under 1 MB and none of this applies. SAP adds one hard blocker: "You can't attach protected or secured PDFs."

### Decision rule: which file you send

- **The form names one format, or lists one first.** Send that. It overrides everything below.
- **The destination is Greenhouse or Ashby.** Send PDF, which both tell candidates to prefer. Greenhouse adds the fallback: "If an upload fails, try re-saving your file as a PDF and reducing its file size."
- **The destination is SAP SuccessFactors.** Send DOCX. SAP names Textkernel as its parsing vendor, and Textkernel tells integrators, under the heading "Documents That Can Cause Problems", "If you want to minimize conversion problems, don't use PDF documents", while conceding in the same paragraph that "Many PDFs convert/parse fine".
- **You cannot tell what system it is.** Send a PDF exported directly from a word processor, provided it passed the text check above. The two vendors that address candidates directly both point at PDF, and the documented PDF risk is a broken text layer, which that check has ruled out. Send DOCX instead whenever you cannot run the check, or the file fails it and you can rebuild it.
- **The file is scanned, image-only, secured, or fails the text check.** Do not send it in any format. Rebuild the document from text.

## Phase 3: choose four to six capabilities, not forty

Greenhouse recommends its own recruiters select only four to six key skills when calibrating a role, "since the match score is spread across the selected skills". That is the shape of the target, much smaller than a typical keyword list.

Greenhouse also documents that stuffing does not work on its own mechanism, because "multiple terms can map to the same calibrated skill, so a longer list of matched terms doesn't always mean a higher match score". LinkedIn warns that a profile which "appears overly optimized" may be hit by spam detection.

So list every capability the advert names, then cut to the four to six you can evidence, ranked by how often it repeats them.

**The acronym rule follows from a documented split.** In an AI match, synonym expansion happens: Greenhouse compares related terms, SAP identifies additional skills related to those in the job description, and LinkedIn's skills graph ran over 374,000 aliases as of 30 November 2022, pairing "data analysis" with "data analytics". In a recruiter's keyword search it does not. Greenhouse's Talent Filtering states plainly: "To appear in your results, the keyword from your search must exactly match the keyword in the application." SAP advises its own recruiters to type "Information Technology" rather than "IT" because its engine drops stopwords, while LinkedIn expands acronyms bidirectionally. Opposite behaviour, and you cannot know which one decides.

**So write both forms, once each, in the same line.** "Search engine optimisation (SEO)" satisfies an exact-match search under either spelling and costs a synonym-expanding matcher nothing.

Two further behaviours matter. Workday's skills match explicitly does not consider how recently a skill was acquired, how long it was used, or years of total work experience, so framing a claim as "ten years of" moves nothing there. And Workday cannot score an application that is not in English, returning "Low or Unable to Score", which puts a non-English CV in the same bucket as a bad match.

## Phase 4: the rewrite

**Structure.** Single column, top to bottom, all across the page. Greenhouse publishes its own causes of an unsuccessful parse, the strongest formatting evidence in the subject because it comes from an ATS vendor rather than a resume service: "graphics, photos, or word art"; a resume "uploaded as an image, rather than a document"; "tables, headers, and footers"; "the name and contact information in the header, footer, or text box"; "a columned layout"; a resume "without clear sections and differing formats throughout each section"; and "spaces between the letters". That covers icons, skill bars and rating dots, none with a documented upside. Greenhouse adds that these issues usually produce "a partial resume parse" rather than a total failure.

**Headers and footers, where vendors disagree.** Greenhouse names header and footer content as a parse-failure cause, while Oracle documents the opposite for Taleo, whose parsing "can detect text in the header and footer". The instruction that survives both is to put nothing load-bearing there. Contact details belong at the top of the body regardless, which is what Textkernel's code 311 checks.

**Titles and employers.** Two items on that list have nothing to do with layout and are widely missed: "Company names that don't include identifying words such as Inc., Co., LTD, or LLC", and "Resumes with incomplete job titles. For example, Sr. Account Exec instead of Senior Account Executive." Write both in full. The exact-match search rule in Phase 3 points the same way, so an abbreviation is a documented parse failure and a findability problem at once.

**Dates.** Each range on the same line as its role. Textkernel's code 418 flags date ranges written vertically across multiple lines, and code 419 a work history with no dates. No vendor states a required date format, so pick one unambiguous form and use it throughout.

**Length.** Code 417 documents that a long CV may have "only the first WORK HISTORY section" parsed. Code 331 flags exceeding a threshold of 30 jobs. The material at the end is the material at risk, so the roles you want read go first.

**Gaps.** Where a gap exceeds six months, state what it was in one line on the CV itself, given the 48 per cent figure above. A stated gap is a fact a human can weigh; an unexplained one is a pattern a filter can act on.

**Do not tailor to the advert alone.** Greenhouse's match score is calculated against skills, experience and job titles the hiring team selected during calibration, not against the published advert. The advert remains the best proxy available and it is what a human reads, but it is not the scoring key.

**Finish before you submit.** SAP documents that skills compatibility is calculated at snapshot and "is not recalculated" by changes to the application resume or the candidate profile. Uploading a better CV after applying does not change that score.

## Phase 5: the change record

Output two artefacts. The revised CV, and a change record listing every edit with its reason: the capabilities selected and why, each acronym pair added, every structural change and the parse-failure cause it addresses, the format chosen and the branch that chose it, and the knockout sheet ready to transcribe. The record makes the next application take twenty minutes instead of ninety, and tells you later whether a change was reasoned or superstitious.

## Worked example, compressed

An invented case. A support engineer of eight years applying to a billing service for a role advertised as "Senior Technical Support Engineer, hybrid, two days on site". The CV is a two column layout from a design tool: an icon row for contact details, a five-dot rating bar per skill, and a fourteen month gap between the two most recent roles.

**Phase 0.** The form sits on a Greenhouse-hosted careers domain, with no AI disclosure and none required in that jurisdiction. The upload control accepts PDF, DOC and DOCX.

**Phase 1.** Hard requirements: right to work in country, on site two days a week, and "5+ years in a customer-facing technical role". All met, the years requirement soft rather than binary. Four knockout questions, all answerable accurately. Proceed, gap flagged for Phase 4.

**Phase 2.** Select all, paste into a plain text editor. The contact details arrive as nothing, because they sit inside the icon graphics, and the two columns interleave, so a role title lands mid-sentence inside an unrelated bullet. The file is 8.4 MB: it uploads without complaint to a system accepting 100 MB and sits more than three times over Greenhouse's 2.5 MB parse limit. Rebuild in a word processor, 96 KB, and ship PDF since the destination is Greenhouse.

**Phase 3.** The advert names, in order of repetition: incident triage, SQL, API troubleshooting, customer communication and a ticketing platform. Five capabilities, acronyms written both ways.

**Phase 4.** Single column, contact block as plain text at the top, date ranges beside each role, rating dots replaced with a one-line evidence phrase per skill, "Sr. Support Eng." expanded to "Senior Support Engineer" and the employer written with its "Inc.". The gap gets one line: "Career break, March 2024 to May 2025, full-time caring responsibilities."

**Verdict.** One PDF at 96 KB with a verified text layer, five capabilities carrying both acronym and expansion, one gap stated rather than hidden, one eligibility sheet completed before a word of the CV was edited, and a change record of 14 entries. The claim is not that this wins an interview. It is that every documented mechanism that could have quietly lost the application, an unparsed contact block, an interleaved work history, a file above the parser limit and an unexplained gap, is gone.

## Failure modes

**Keyword panic.** Looks like: forty terms lifted from the advert stuffed into a skills block, a match score that does not move, and a growing conviction that the system is rigged. Greenhouse documents that synonyms collapse to one calibrated skill, so the longer list buys nothing.

**Tailoring the CV and rushing the form.** Looks like: ninety minutes on the document, ninety seconds on the eligibility questions, and a rejection within the hour. Those questions are the only documented automatic rejection, which is why the ordering exists.

**The beautiful unreadable CV.** Looks like: a two column design with icons and skill bars that a person compliments and a parser receives as interleaved fragments with no contact details. Greenhouse names every one of those elements as a parse-failure cause.

**The successful upload that was never parsed.** Looks like: a confirmation screen, no error, and silence. Greenhouse accepts 100 MB and parses 2.5 MB, so a file in that gap uploads as cleanly as one that works.

**Fixing it after submitting.** Looks like: spotting a better wording an hour later and uploading version two. On SAP the score was taken at snapshot, so the new file changes nothing about the number it was meant to improve.

**Citing the wrong study.** Looks like: a formatting recommendation justified by the Hidden Workers research, which measured employer-configured criteria such as employment gaps and returns zero hits for "parsing" or "resume format". The advice may be right; that is not the evidence for it.

**Assuming your ATS is the one you read about.** Looks like: advice built on Workday behaviour applied to a Greenhouse form, or a claim that Workday states it does not auto-reject, which it says in neither direction. Vendor documentation describes what software can do, never what an employer configured.

## What this skill does not do

- It cannot tell you whether the employer's system scores CVs at all, or how it was configured. Every vendor fact here describes a capability, and the gap between capability and configuration is unbridgeable from outside.
- It does not verify your file. The text extraction check in Phase 2 needs a person or a tool with access to the document, and skipping it invalidates most of what follows.
- It makes no claim about interview rates. No study located measures whether tailoring a CV increases them, and this file optimises for documented mechanisms, not a measured outcome.
- It does not cover the cover letter, interview answers, recruiter outreach or pay discussions.
- Its legal references are jurisdiction specific and dated, and the field moves fast enough that they may have changed since 30 August 2026.
- It cannot promise a parse failure is harmless everywhere. On direct upload routes it costs discoverability rather than the application: Greenhouse leaves an unparsed resume attached to the candidate for a recruiter to enter manually, and Oracle Taleo stores an unparsed image resume while warning the employer that keyword search will miss it. Greenhouse documents one exception, its email-forwarding route, where a parse failure means "the candidate won't be added to Greenhouse Recruiting" at all.
