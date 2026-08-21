---
name: dependency-risk-triage
description: Turns the output of a dependency scanner into a triaged worklist: every finding assigned one of four dispositions with a recorded reason, the small set that is genuinely urgent this week, and a written policy that decides the next scan automatically. Carries the current CVSS v4.0 metric groups and nomenclature, live EPSS and CISA KEV counts with their dates, the three levels of reachability evidence, and the four real options for a transitive finding you cannot upgrade directly. This skill should be used when a vulnerability scan, a Dependabot alert queue or an SBOM report has produced more findings than anyone can act on and somebody has to decide what ships this week.
---

# Dependency risk triage

## The claim this skill is built on

A dependency scan on a real application returns more findings than the team can work through, and the default sort is severity, descending. That sort is close to random with respect to your risk, because the score describes the defect in an unspecified deployment rather than your exposure in yours.

This is not a contrarian reading. It is what the people who publish the score say. The CVSS v3.1 User Guide, from FIRST, carries a section titled "CVSS Measures Severity, not Risk", stating that "CVSS is designed to measure the severity of a vulnerability and should not be used alone to assess risk". The score is one input, and sorting on it alone is using it alone.

Volume is why this matters more each year. Counting NVD records by publication date, 2024 saw 40,704 CVE records and 2025 saw 49,972, a rise of 22.8%. The first half of 2026 alone carried 37,126, already 74% of the whole of 2025. Quote that measurement basis with the numbers: NVD records by publication date, including rejected entries, so they will not match CVE.org totals exactly.

You finish holding two artefacts. A **worklist**, where every finding carries one of four dispositions and the field that disposition requires. And a **policy**, three quarters of a page, which is the durable half, because it decides most of the next scan without anyone reading it.

## Step 1. Sort by exploitation probability, not severity

One number does most of the work. EPSS, the Exploit Prediction Scoring System, is run by a Special Interest Group at FIRST and outputs a probability that a given CVE will be exploited in the wild within the next thirty days. Scores regenerate daily and are published free with an API.

As of the 2026-08-19 model run, EPSS scored 362,257 CVEs. Of those, **17,311, or 4.78%, score above 0.1**. Only **4,317, or 1.19%, score above 0.5**.

That 4.78% is the argument. Fewer than one in twenty scored vulnerabilities carry even a one-in-ten modelled probability of exploitation activity in the coming month. Any sort ignoring this number discards the cheapest discriminator available, one API call against identifiers you already have.

Three things to get right.

- **The window is thirty days and the score moves.** A score is a snapshot, not a property. Pull it at triage time, record the value and the date beside the finding, and re-pull rather than caching. A 0.03 in March can be 0.6 in April.
- **It predicts observed exploitation activity, not impact.** A high score on something that cannot touch your data is still not urgent. First sort key, not verdict.
- **No CVE identifier means no score.** Findings from private advisories, vendor bulletins and malicious-package reports arrive with this signal missing and get decided on the other two.

## Step 2. Read the severity score accurately, then set it aside

You still need to read the score, because it is on every finding and somebody will ask.

**CVSS v4.0 is current**, published by FIRST, specification version 1.2 dated 2024-06-18. It has **four metric groups**, not three: Base, Threat, Environmental and Supplemental. Version 4.0 renamed v3.1's Temporal group to Threat. The qualitative bands are None at 0.0, Low 0.1 to 3.9, Medium 4.0 to 6.9, High 7.0 to 8.9, Critical 9.0 to 10.0.

The part worth carrying is v4.0's nomenclature, because it names an imprecision people commit daily.

| Name | What it includes |
| --- | --- |
| CVSS-B | Base metrics only |
| CVSS-BT | Base plus Threat |
| CVSS-BE | Base plus Environmental |
| CVSS-BTE | Base plus Threat plus Environmental |

Almost every published score, in an advisory, in a scanner, on a dashboard, is CVSS-B. Quoting a bare number as "the CVSS score" without the suffix is imprecise, and the two suffixes that would make it about you, BE and BTE, are exactly the ones nobody computes, because they require describing your own deployment. That is the whole gap between a severity number and your risk, and it is structural.

Nor is the score a single agreed value. CVE-2025-32711, the Microsoft 365 Copilot flaw published as EchoLeak, carries **9.3 CRITICAL from Microsoft and 7.5 HIGH from NVD**, two vectors for one defect differing only on the Scope metric. Same bug, two competent scorers, a band apart. If a remediation deadline hangs on which side of 9.0 a finding falls, it hangs on a metric two organisations disagreed about.

## Step 3. Check the catalogue of things already exploited

CISA's Known Exploited Vulnerabilities Catalog lists vulnerabilities with reliable evidence of active exploitation in the wild. Entry requires a CVE identifier, that evidence, and clear remediation guidance. It is free, published as JSON and CSV.

At catalogue version 2026.08.19 it held **1,671 entries**, of which **349, or 20.9%, carry the knownRansomwareCampaignUse flag**. Additions by year: 311 in 2021, 555 in 2022, 187 in 2023, 186 in 2024, 245 in 2025, and 187 in 2026 up to 19 August.

It is deliberately small. Set 245 additions in 2025 against roughly 50,000 CVEs published that year and about **0.49%** of the year's volume reached it. That ratio is what makes membership a strong signal: it is not a list of bad bugs, it is a list of bugs somebody has been attacked with.

Read it in one direction only. **Presence means someone is already using it.** Absence means nothing, because the catalogue records confirmed exploitation rather than its absence. Matching your lockfile against it is the cheapest useful step here: one file, one join on CVE identifier, usually returning either nothing or a very small number of rows that reorder your week.

### The deadline everybody quotes was revoked in June 2026

**BOD 22-01, the 2021 directive that created the KEV catalogue and attached the fourteen-day and six-month remediation deadlines, was revoked on 10 June 2026**, superseded by **BOD 26-04, "Prioritizing Security Updates Based on Risk"**. A policy citing BOD 22-01's fourteen-day KEV deadline as current guidance is stating something no longer true.

What replaced it is more interesting. BOD 26-04 drops flat deadlines for a **risk-tiered table informed by SSVC**, the Stakeholder-Specific Vulnerability Categorization decision-tree approach. The fastest tier is **three calendar days, and additionally requires forensic triage of the affected asset**, on the reasoning that something that exposed and that actively exploited may already have been used against you, so patching alone answers the wrong question. The lowest tier is **fix on system upgrade**: no separate clock at all. The rollout runs in three phases, phase I immediate, phase II within 60 days, phase III within 180 days.

Two mechanics are worth copying whatever sector you are in. **The timelines are dynamic**: adding a vulnerability to KEV shortens its clock, so a deadline is not fixed at discovery. And **the clock starts at whichever comes first**, CISA adding it to KEV or the agency identifying it on one of its own assets, which closes the gap where a known-exploited bug sits unowned because nobody had scanned yet.

Copy the shape rather than the numbers: tiers instead of a flat deadline, a dynamic clock, an earliest-of-two start, and an investigation obligation rather than a patch obligation on the top tier.

## Step 4. Reachability, in three levels

Reachability eliminates most of the list and costs the most to establish. Split it into three questions, because they differ in cost and get conflated.

**Level 1. Is the package present at all?** A scanner reading a manifest answers a different question from one reading a lockfile. A manifest declares a range; only the lockfile says what is installed.

**Level 2. Is the vulnerable version the installed version?** The lockfile decides, and it can hold more than one copy of a package at different versions. Check each resolved entry, not the package name.

**Level 3. Is the vulnerable code reachable from your entry points?** The real question, at three price points.

- **One search, about two minutes.** The advisory usually names the affected function, class, endpoint or configuration option. Search your source for that symbol and for the import of the module containing it. Zero hits in your code and zero in any dependency that would call it is weak evidence of unreachability; any hit ends the question. Record the exact search string and the date, because that record is the whole value of the finding later.
- **One hour.** Trace the call path by hand from each entry point to the import, noting which entry points are network-facing and which are reachable only from a build agent. This is where a development dependency separates properly from a runtime one.
- **Properly.** A static call-graph tool computing reachability from your declared entry points into the vulnerable symbol, which is what commercial reachability products do and, on a mature ecosystem, far better than a search.

**Be careful what you cite about how much collapses.** The often-repeated figures for what fraction of vulnerabilities are reachable mostly do not survive a check back to a primary source. The honest citation is Endor Labs, who sell a reachability product and describe the effect in their own public marketing as "likely less than 1/10 of them can be exploited in your code", presented as an illustration rather than a dataset finding. That is a vendor claim, so label it one.

Their 2023 open-source dependency report carries two checkable figures worth more than the illustration. **71% of the code in a typical Java application comes from open source components, while applications use only 12% of the code they import.** And **45% of applications have no calls to security-sensitive APIs in their own code, falling to 5% once dependencies are included.** The 12% is why reachability filters so much; the drop from 45% to 5% is why "we do not call that" is a claim about your dependencies rather than your code.

**Unreachable today is not unreachable next sprint.** Every not-applicable disposition on reachability grounds carries the search that produced it, the date, and a trigger that reopens it.

## Step 5. Direct versus transitive, and the four real options

Triage against the lockfile, always. A manifest states intent; a lockfile states what is installed and at whose request. Most findings are transitive, so you cannot fix them by editing your own manifest, and a worklist assigning "upgrade to 2.4.1" to a package you do not declare is assigning work that does not exist.

For a transitive finding there are four options and no fifth.

1. **Upgrade the parent.** Cleanest, because you stay inside a resolution the parent's maintainers tested. Requires a parent release that pulls the fixed child. Verify it in the lockfile, not the changelog.
2. **Override the resolution.** Every major ecosystem has this: `overrides` in npm, `pnpm.overrides`, `resolutions` in Yarn, a constraints file in Python, `dependencyManagement` in Maven, a resolution strategy in Gradle. Fast, and it makes you the owner of a combination the parent never tested, so pair every override with a test that exercises the call sites.
3. **Vendor a patch.** Pin and carry a local patch. Highest maintenance cost, and the only option that expires silently, because the next upgrade drops it unless something enforces it.
4. **Accept with an expiry**, naming an owner and a date.

One rule changes the default: **check whether the parent is still maintained before waiting for it.** If its most recent release predates the advisory by a year or more, waiting upstream is not a plan and the override becomes first choice rather than fallback. Open a separate ticket to replace that parent, because it will do this to you again.

## Step 6. The disposition set, which is what makes it a record

Every finding gets exactly one of four dispositions, each with a mandatory field. A row without its field is not triaged.

1. **Fix now.** Mandatory: the target version and the intended merge date.
2. **Fix in the next scheduled window.** Mandatory: which window, by date.
3. **Accept with an expiry.** Mandatory: a named owner, an expiry date, and the condition that reopens it early.
4. **Not applicable.** Mandatory: the evidence, written out, and the trigger that reopens it.

"Monitoring", "won't fix" and "low risk" are not dispositions. The rule that does the work: **an acceptance without an expiry date is a decision to never look again, written to look like a decision to look later.** The same applies to a scanner ignore file, where most undated acceptances actually live. An ignore entry with no expiry and no reason is the most durable object in your repository.

The not-applicable field matters as much. A finding closed as "not reachable" and nothing else is reopened by the next scan, argued again, and closed by somebody with less context. Closed with the search string, the date and the trigger, it stays closed until the trigger fires.

## Step 7. The decision rule, with a cannot-tell branch

Should this finding be fixed this week?

- **On the KEV catalogue, and the lockfile says the vulnerable version is installed.** Fix now, regardless of severity band, EPSS score or reachability. Follow BOD 26-04's shape and pair the fix with a log review of the affected service, because the question is not only whether you are exposed but whether you have already been visited.
- **Not on KEV, EPSS at or above your threshold, and the vulnerable code is reachable.** Fix now.
- **Provably unreachable, with the evidence recorded.** Not applicable, with the search and the reopening trigger written into the row.
- **Cannot tell whether it is reachable.** The common case, and the branch that decides whether the process is honest. Two tiebreaks: the EPSS score, and whether a network-facing entry point sits on a plausible path. High EPSS plus a network-facing path, treat it as reachable and put it in the next window. Otherwise accept it with a ninety-day expiry and a named owner. Do not close it and do not leave it unlabelled, because an unlabelled finding is indistinguishable next quarter from one nobody read.
- **No EPSS score at all**, because there is no CVE identifier. Decide on reachability and network exposure alone, and halve the expiry to thirty days, on the grounds that you are working with one signal instead of three.

## Step 8. The policy, which is the durable output

The point of triage is not this queue. It is the rule that makes the next queue mostly automatic. Write it now, while the reasoning is fresh, in about three quarters of a page: the signals and their thresholds as numbers, a tier table, the standing overrides, and where the record lives.

| Condition | Disposition | Response time |
| --- | --- | --- |
| On the KEV catalogue and installed per the lockfile | Fix now | 3 working days, plus a log review of the affected service |
| EPSS at or above 0.1, reachable, network-facing path | Fix now | 7 days |
| EPSS at or above 0.1, reachability undetermined | Fix in the next window | 30 days |
| EPSS below 0.1, reachable | Fix in the next window | Next scheduled dependency window |
| EPSS below 0.1, unreachable with recorded evidence | Not applicable | Reopened by its trigger |
| No EPSS score available | Accept with expiry | 30 days, then re-triaged |

Then the standing rules, which stop the table being gamed.

- A KEV entry overrides every other row, and only ever in one direction: it can shorten a clock, never lengthen one.
- The maximum life of an acceptance is one renewal. A second renewal escalates instead.
- An ignore file entry with no expiry and no linked disposition is a policy violation, and that check belongs in continuous integration.
- Re-triage runs when the count of open acceptances passes a number written here, not on a calendar.

Pick your own numbers. Writing them down matters far more than which ones you pick, because a policy without numbers is a preference, and a preference is not auditable.

## The risks that produce no finding

Everything above concerns entries in a scanner's output. The larger real risk in most dependency trees produces no entry at all, and this section exists so the worklist does not become the whole picture.

- **An unmaintained package.** No release in three years, a maintainer who has moved on. No finding today, and it guarantees the next finding in it has no fix.
- **A single-maintainer package whose publishing rights sit on one account.** One compromised account publishes to everyone downstream. Nothing to report until the day there is.
- **A package added by a typo.** Typosquats resolve, install and execute, and they look correct in a diff.
- **An install script.** Lifecycle scripts in npm, `setup.py` in Python, build scripts in Gradle: arbitrary code at install time, on developer machines and build agents, before any test runs.
- **A dependency that pulls in a hundred more.** Each is another publishing account with a path into your build.

Say this plainly in the document you produce, because it is true: these are a larger real risk than most of the findings you just triaged. The countermeasures differ in kind. Review lockfile changes as carefully as source changes, disable install scripts by default and allow them by name, and require a recorded human decision for every new direct dependency, carrying the maintainer count and the last release date. None of that is triage, and all of it belongs in the same policy document.

## Worked example

A billing service at a mid-size logistics company, everything invented. The scanner returns 412 findings against the lockfile: 18 critical, 96 high.

**Finding A. Severity 9.8, critical.** A deserialisation flaw in an XML parsing library. EPSS 0.02, not on KEV. The advisory names the affected entry function; a search of the service's source finds no import of the library anywhere, and it arrives transitively through a report-rendering package that uses a different parser path and disables the feature by default. **Disposition: not applicable**, recording two search strings, the date, zero hits and the parser configuration. Trigger: any change to the report-rendering package's major version.

**Finding B. Severity 6.5, medium.** An authentication bypass in a small HTTP middleware. On the KEV catalogue, added this year, carrying the ransomware flag. EPSS 0.71. **Disposition: fix now**, three working days, with a log review of the service's authentication events for the last ninety days. The medium band describes the defect; the catalogue entry describes what is happening to other people, and the catalogue wins.

**Finding C. Severity 7.5, high.** A compression library four levels deep, one resolved copy in the lockfile. The direct parent's latest release is fourteen months older than the advisory, so waiting upstream is not a plan. **Disposition: fix now via a lockfile override** pinning the fixed patch version, with a regression test covering the two call sites, plus a ticket to replace the unmaintained parent within two quarters.

**Finding D. Severity 9.1, critical.** A build-time formatter that never enters the runtime image. EPSS 0.004, not on KEV, a development dependency reachable only from a build agent. **Disposition: accept with a 180-day expiry**, owner named, reopened early if the tool moves into a shipped image.

**Verdict.** Two changes ship this week: the KEV middleware fix and the compression override, the second carrying a test. One finding is closed permanently with evidence, one accepted with a date and a name against it. The highest-severity finding of the four ships nothing, and the medium one is the emergency. The other 408 are dispositioned by the policy table rather than by a person, and that policy is the artefact committed alongside the worklist.

## Failure modes

**Severity Sort.** The queue ordered by CVSS descending and worked from the top. The tell: the top rows are all base scores from advisories, none carrying an EPSS score or a KEV check. The work is real and uncorrelated with the risk.

**Upgrade Avalanche.** One pull request upgrading forty packages to clear the alert count. A large behavioural change to production under a label that discourages careful review, and when it breaks something the incident is self-inflicted.

**Acceptance Without Expiry.** Rows accepted with no owner and no date, or an ignore file that has grown for two years. Indistinguishable from a queue nobody read, and it reads that way in an audit.

**Reachability Assumed.** A finding closed as unreachable with no recorded search. The tell is a reason field reading "not used" and nothing else. It reopens next scan, gets re-argued, and is closed again by somebody who knows less.

**Lockfile Blindness.** Triage against the manifest, so the analysis covers declared ranges rather than installed versions. Symptoms: transitive findings assigned upgrades to packages you do not declare, and duplicate resolved copies where only one got checked.

**Scanner Monoculture.** One tool's database treated as the complete picture. Advisory databases differ in ecosystem coverage, in how they express affected ranges and in how fast they publish, so a finding absent from your scanner is not a finding that does not exist.

**Alert Ageing.** A queue so long the genuinely urgent entry is invisible inside it. The tell is the count rising every month regardless of work done, and the first reaction to a red check being to merge anyway.

**Fix Without Test.** An override or version bump merged with no test exercising the call sites it affects. The vulnerability is gone and the behaviour is untested, which trades a hypothetical problem for a real one.

**Stale Policy Citation.** A remediation policy citing BOD 22-01's fourteen-day KEV deadline as current, when it was revoked on 10 June 2026 and superseded by BOD 26-04. The tell is a policy with no version line and no review date, the same defect the acceptance rule is about.

## What this skill does not do

- **It does not scan.** It starts from a finding list, so you need a scanner. OSV-Scanner and Dependabot alerts are both free and cover most ecosystems, and either is a better place to spend your first hour than this file.
- **It cannot compute reachability rigorously.** Level three needs static call-graph analysis from your real entry points, and a recorded search is weak evidence rather than proof. Commercial reachability tools do this properly and are worth their price at scale.
- **It goes no deeper on container base images and operating system packages.** The same three signals apply, but distribution backports mean a version string in a base image often does not mean what a scanner reading it assumes.
- **It has no view on licence compliance.** That report shares a scanner with this one and nothing else, including its reader.
- **It cannot judge a maintainer.** Whether a publishing account is trustworthy, whether a change of ownership is benign, whether a new maintainer is who they say they are: none of that is in any feed, and the section on risks that produce no finding is the most this file honestly offers.
- **It does not perform or test the upgrade.** A fix-now disposition hands you a change that can break production by itself, and doing that safely is a separate job.
