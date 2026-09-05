---
name: recruiter-outreach-writer
description: Produces recruiter and hiring manager outreach in two passes: a search surface of exact strings that makes a profile and an application findable in the systems recruiters actually search, then a rationed set of approaches deciding which targets earn one of a free account's scarce personalised connection notes, which get a note-free invitation, which get followed first, which get an InMail, and which get nothing. It writes to the correct character ceiling for the account type, tests every opener against name substitution, and keeps the sending pattern inside documented invitation limits. This skill should be used when an application has gone in and messaging the hiring manager is the next instinct, when a recruiter target list exists but nothing has been sent, or when a profile reads well to a human and returns nothing to a search.
---

# Recruiter and hiring manager outreach writer

## The claim this skill is built on

Outreach advice is written as though the recruiter is sitting in front of an inbox deciding which message to open. Often they are not. They are running a search over a database of candidates, and whether your name is in the result set was decided before anybody read anything.

That makes this two problems, not one. Being findable is mechanical, documented per vendor, and mostly a matter of which literal strings exist in fields you control. Being persuasive is a writing problem with a hard character budget attached. Advice that treats the whole thing as writing loses the expensive problem, because a message can be resent and a search you did not appear in never happens again.

Do the findability pass first. It changes what the outreach has to achieve, and sometimes removes the need for it.

## Pass one: the search surface

The output is eight to twelve exact strings, each one you would accept a recruiter typing into a search box, placed into your headline, About section, experience entries and skills, and into the application itself.

### 1. Carry the acronym and the expansion, both

This single rule survives three vendors that behave differently, which is why it is the rule.

In an AI match, synonym expansion is documented. Greenhouse compares related terms, SAP identifies additional skills related to those in the job description, and LinkedIn's skills graph ran "over 374,000 aliases (e.g. 'data analysis' and 'data analytics')" in its engineering post of 30 November 2022, with a later post of 13 December 2023 citing more than 41,000 skills. There is no official figure after 2023, so quote those with their dates or not at all.

In a recruiter's keyword search, expansion is not guaranteed and is sometimes explicitly absent. Greenhouse Talent Filtering states: "To appear in your results, the keyword from your search must exactly match the keyword in the application." LinkedIn goes the other way and expands acronyms bidirectionally, documenting that "doing a keyword search for SaaS will also return members who list 'Software as a service' on their profile without using the acronym SaaS". SAP goes the other way again and advises its recruiters to "Enter 'Information Technology' instead of 'IT' in your search filters", because its matching engine drops stopwords.

Carrying both forms is the only instruction correct in all three cases. Write it once as the acronym and once expanded, in prose a human would read, not as a bracketed pair repeated in every bullet. All facts here checked August 2026.

### 2. Write the string, because nothing will expand it for you

LinkedIn documents that it "does not support braces { }, brackets [ ], angle brackets < >, or wildcards like asterisks *". So there is no stem that catches analyst, analytics and analysis at once. If you want to be found by all three, all three have to exist somewhere in your text.

The same logic governs spelling variants. Postgres and PostgreSQL are different strings, and so are front-end, frontend and front end. Pick the vendor's own spelling as primary and place the common variant once elsewhere. Under an exact-match rule, a variant you did not write is a search you do not appear in.

### 3. Do not let a stopword carry your distinctiveness

LinkedIn publishes its stopword list: and, or, the, of, at, by, to, for, with, in, they, have, from, not, but, after. Its own example is that searching "after sales" returns profiles containing only "sales", so a headline built on "after sales support" is, to the engine, a headline about sales. Read your headline and About section once looking only for this: any phrase whose specificity depends on one of those sixteen words needs a second, load-bearing word.

### 4. Two forms is the ceiling, and stuffing is documented as counterproductive

LinkedIn states that if a profile "appears overly optimized, it may be impacted by spam detection systems". Greenhouse documents the collapse directly: "multiple terms can map to the same calibrated skill, so a longer list of matched terms doesn't always mean a higher match score", and recommends its recruiters calibrate on only four to six key skills, since the match score is spread across the ones selected.

So the ceiling is real and low. Each concept earns the acronym and the expansion, once each, and a headline that is a comma-separated keyword list is a documented risk rather than merely bad taste.

### 5. Know what the destination cannot do

- **Workday documents no Boolean query language at all.** Its model is facets, filters and text. Do not build a plan that assumes a recruiter there can run a nested query, and do not assert that the capability exists.
- **Greenhouse Boolean is off by default** and has to be switched on. It supports AND, OR, NOT, quotes, parentheses and the wildcard `*`.
- **Lever sorts search results by recency, not relevance, and its tags are case-sensitive.** Being recent is being ranked, and a tag with the wrong capitalisation is a different tag.
- **LinkedIn Recruiter publishes its bands:** high qualification relevance is 70 per cent or more of qualifications matched, medium is 30 per cent or more, low is under 30 per cent.

## Pass two: the approach

### The trigger taxonomy

An approach needs a trigger: a specific, dated, publicly checkable event giving this message a reason to exist now.

**Triggers that justify an approach.** A live requisition you applied to within roughly the past two weeks. A published artefact by the target: a talk, an engineering post, a conference session, a repository. A company event with a stated date: a funding round, a new office, a launch, a published roadmap item that maps onto work you have done. A referral path where a mutual contact has agreed to be named. A prior process at the same employer that reached a late stage. A job advert naming a problem you have solved, with evidence you can attach.

**Triggers that do not.** They viewed your profile: an observation, not an invitation, and the most common false trigger in a job search. They posted "we are hiring" with no role you match. A shared university, city or group. A work anniversary surfaced by the platform. An email open recorded by a tracking pixel, which is a fact about your software rather than about them. Their presence on your list, the weakest reason of all and the one that produces every message that reads like the others.

A trigger older than roughly six weeks is stale, and congratulating someone on a round announced eleven months ago tells them exactly how you found them.

### The substitution test

The opening line must reference something only someone who actually read the target's work could know.

Test it by swapping the name and company for another target on your list. If the sentence still reads true, it is a variable in a template rather than personalisation. Then the second test: could this line have been written from the job advert alone? If so, it proves you read the advert, which every applicant did. A line that passes usually carries a number, a method or a decision from the target's own material.

### Rationing the notes

As checked in August 2026, LinkedIn's Help Centre does not agree with itself about the free allowance: its most recently updated page says a personalised message can be added to up to three connection requests per month, while two older pages say five. Treat it as a small handful per month, three or five depending on which of LinkedIn's own pages you read. Premium members are unlimited.

The character ceiling differs too: 200 on free or Basic, 300 on Premium. The near-universal published figure of 300 is the Premium number, so a template written to it and pasted into a free note box loses its ending, which is where the ask sits.

The ration rule: order targets by trigger strength multiplied by decision power, and spend a note only where a justified trigger meets someone who can start a process. Everyone else gets a note-free invitation, which remains available because LinkedIn documents that "Restrictions and limits on standard invitations are separate from the limits placed on personalized invitations to connect". Three notes and nine qualifying targets means three notes and six note-free invitations.

On volume: a weekly invitation ceiling of roughly 100 is consistently reported but not documented by LinkedIn, which confirms limits exist without publishing a number, so treat it as observed rather than as a rule. The consequences are documented: a restriction typically lasts one week, withdrawing pending invitations does not lift it, Support will not tell you the reason, and after withdrawing an invitation you cannot re-invite that person for up to three weeks. LinkedIn also lists invitations "ignored, left pending, or marked as spam" as a cause of restriction, which is the real basis for caring about acceptance rate. No threshold is official, so do not aim at a percentage and be suspicious of any source that gives you one.

### The three-message shape

**Message one carries no ask.** It names the trigger, states one line of relevance, and stops, inside 200 characters on a free account, which is roughly two sentences. The absence of an ask is the point: an ask in message one forces a decision from someone with no reason to make it, and the reply you want is a signal, not a yes.

**Message two, three to seven days later and only after a reply or a clear signal**, carries the single ask, phrased as a named next step with an explicit out. One artefact at most.

**Message three, seven to ten days after two, once.** It says this is the last message, leaves the door open and repeats nothing. There is no fourth: a thread of three unanswered messages already reads as one.

One documented detail worth holding: a sent direct message can be edited (text only) or deleted within 60 minutes, and edited messages show an "Edited" label. That window is a messaging rule and does not carry over to comments, which LinkedIn documents as editable with no stated time limit. Wrong company name in a message means one hour; in a comment, longer. The two get conflated constantly.

**InMail**, if you hold credits, is a Premium feature with a 200-character subject line. Write the body to 1,900 characters: LinkedIn's most recently updated page says 2,000 while two Recruiter pages say 1,900, so 1,900 fits everywhere. You cannot send a second InMail to the same person until they respond. Credits expire after 90 days and one is returned when the recipient accepts, declines or replies inside that window, so an InMail costs you nothing except when it is ignored. A member who has opted out of InMail cannot be reached that way at all.

### The decision rule

For each target, in order:

1. **Justified trigger, decision power, and a note left this month.** Spend a personalised note. Draft to 200 characters on free or Basic, 300 on Premium.
2. **Justified trigger but the notes are gone, or the trigger is second tier.** Send a note-free invitation and hold message one until it is accepted.
3. **No trigger yet, but the target publishes.** Follow, read and engage for two to three weeks before any invitation, which converts a cold approach into one with a trigger.
4. **Out of network, top-tier trigger, no shared path, and you hold credits.** Send one InMail. Subject to 200, body to 1,900, and no second attempt until they answer.
5. **No trigger, no public output, no shared path, or they have opted out of InMail.** Do not approach. Apply through the process and spend the effort on pass one instead, which is where the return is.
6. **You cannot tell.** If you do not know your account type, how many notes remain this month, how many invitations went out in the past seven days, or whether this person owns the requisition, send nothing today. Run three cheap checks: open the note box on any profile and see what it offers and counts, count the last seven days of invitations, and confirm the trigger from a primary source such as the target's own post rather than a platform notification. Until then default to branch 2, because a note-free invitation costs nothing from a scarce allowance.

## Worked example, compressed

A candidate is applying for a staff data engineer role at a mid-sized logistics software company. Their profile headline reads "Building the future of data, at scale", and their About section mentions ELT, dbt, and "after hours reliability work". The employer's careers page hands off to a Greenhouse application domain.

**Pass one.** The headline contains no string a recruiter would type. ELT appears without "extract load transform", dbt without "data build tool", and the one distinctive phrase, "after hours reliability", rests on "after", a published LinkedIn stopword, so the engine reads "hours reliability". Under Greenhouse's exact-match rule, a recruiter typing Airflow will not find "workflow orchestration". Fix: a headline reading "Data engineer, Airflow and dbt (data build tool), ELT and extract load transform pipelines", with "after hours" replaced by "overnight batch reliability". Four new strings, one stopword removed.

**Pass two.** Two targets. Target A is the named hiring manager, who published an engineering post three weeks ago about cutting a nightly pipeline from six hours to ninety minutes. Target B is a recruiter who viewed the profile yesterday.

Target B fails the taxonomy: a profile view is not a trigger, so no approach. Target A is a top-tier trigger plus decision power, so one of the month's notes is spent. The note, 187 characters: "Your write-up on taking the nightly pipeline from six hours to ninety minutes matched a rebuild I ran on a 40-table ELT job last year. Applied for the staff data engineer role on Tuesday."

It survives substitution: no other target on the list published that. It contains no ask.

**Verdict.** One note spent of three, one approach cancelled outright, four search strings added, one stopword removed. The cancelled message to the recruiter is the most valuable line of the output, because it is the one that felt most obviously worth sending.

## Failure modes

1. **The mail-merge tell.** The opener survives the substitution test: "I loved your post on leadership", or "your work in the logistics space is impressive", sendable to anyone on the list. From the recipient's side it is indistinguishable from automation and gets treated as such.
2. **Writing to 300 on a free account.** The note arrives ending mid-sentence with the ask amputated, because the template came from an article quoting the Premium ceiling.
3. **The stopword headline.** A headline that reads beautifully and returns nothing, because the word carrying its specificity is one of the sixteen LinkedIn drops. Nothing tells you this happened. The searches simply do not include you.
4. **Wildcard thinking.** Listing one form of a term and assuming the engine catches the family. LinkedIn documents no wildcard support, so only the literal strings present get found, and the profile silently loses to a worse one that spelled things out.
5. **The over-optimised profile.** A skills wall and a headline that is a keyword list. LinkedIn documents that a profile appearing overly optimised may be hit by spam detection, and Greenhouse documents that synonyms collapse to one calibrated skill, so the extra terms buy nothing and carry a real risk.
6. **Asserting Boolean where none is documented.** Advice built on the assumption that a recruiter in Workday can run a nested query. Workday documents no query language at all, so the claim is unverifiable and the tactic it justifies is imaginary.
7. **Spending the ration on the easy names.** All three notes gone by Tuesday on the most findable people, so the hiring manager who published the post gets a note-free invitation and no context.
8. **The volume tool.** The account is restricted for a week with no stated reason, withdrawing pending invitations does not lift it, and the withdrawn targets cannot be re-invited for up to three weeks. LinkedIn documents that suspected use of an automation tool can lead to suspension or restriction and that repeated suspensions may result in permanent restriction.
9. **The fourth message.** Three unanswered messages sit in a thread and a fourth is added. Nothing can be withdrawn, and the thread is the first thing the recipient sees if they ever open it.
10. **The stale trigger.** An approach built on an announcement from last year, which dates precisely when and how you found them.

## On the ethics of this, plainly

The restrictions above exist because of volume tooling. LinkedIn's documented triggers for restricting an account are excessive invitations, invitations left ignored or marked as spam, and suspected automation. Every scarce allowance in this file is a response to people sending at scale: the reason a careful individual now gets three notes a month is that other people sent three hundred.

So the method is deliberately manual, and not only as a compliance preference. Sending by hand at a volume you can defend is also the version that works, because the one durable advantage in outreach is having actually read the thing you are writing about.

## What this skill does not do

- It does not tell you whether your application will be read. Documented automatic rejection in these systems keys on structured application questions rather than on messages or CV keywords, and this file sees neither.
- It does not write your CV or tailor it to a job description. That is a different document read by a different system, and the search surface built here is not a substitute for it.
- It cannot verify a trigger. It knows only what you paste in, so a surfaced notification and a genuine public artefact look identical to it.
- It does not know today's numbers. Every LinkedIn figure here was checked in August 2026, one of them is contradicted by LinkedIn itself, and the weekly invitation ceiling is observed rather than documented.
- It does not get you a referral, which beats everything in it. If you know somebody inside, use them and spend the effort on the profile their recruiter will open.
