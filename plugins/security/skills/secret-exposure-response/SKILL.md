---
name: secret-exposure-response
description: Produces a written response plan for a credential that has been exposed, covering the classified inventory of what leaked and what it grants, the dependency-ordered rotation sequence that avoids taking production down while revoking, the per-provider audit steps with their log retention limits, and the notification question handed to whoever is entitled to decide it. It is built on two facts: the exposure window opens at publication rather than at discovery, and rewriting git history does not close it. This skill should be used when a key, token, password or private key has appeared anywhere public or semi-public, when a provider reports that a secret of yours was found, or when someone proposes to fix a leak by force pushing.
---

# Secret exposure response

## The claim this is built on

A credential is compromised from the moment it is published, not from the moment you notice. Everything you do to the repository afterwards is cleanup, and only invalidating the credential reduces risk.

This matters because the instinct is the opposite. The first move most teams make is to delete the offending line, rewrite history and force push, which feels like remediation and achieves the least of any available action. GitHub says so in its own documentation, verified on 20 August 2026: "If you only rewrite your history and force push it, the commits with sensitive data may still be accessible elsewhere: In any clones or forks of your repository, Directly via their SHA-1 hashes in cached views on GitHub, Through any pull requests that reference them." It adds the part people never think about: "You cannot remove sensitive data from other users' clones of your repository." Its own remediation guidance leads with rotation, not deletion: "if the sensitive data you need to remove is a secret (e.g. password/token/credential)... then as a first step you need to revoke and/or rotate that secret."

Deletion is weaker still on a repository that has been forked. Truffle Security published research by Joe Leon on 24 July 2024 naming the class Cross Fork Object Reference, showing that commits pushed to a fork that is later deleted stay retrievable, and that when a public repository with forks is deleted, the upstream commits remain accessible through any fork. GitHub's position is that this is intended, documented architecture rather than a defect, which is exactly why the remediation is always invalidation and never takedown.

The published order is not a matter of taste. The OWASP Secrets Management Cheat Sheet, section 9.2 Remediation, sequences it as revocation, rotation, deletion, logging. Deletion is third.

## What you produce

The output is a document, not a completed checklist. Write it as you go, because most of it has to be written while the facts are still available.

1. **Exposure record.** What the value is, where it was published, the timestamp of publication rather than of discovery, and every other place it propagated to.
2. **Classified inventory.** One row per exposed secret, with what it grants and how it has to be replaced.
3. **Rotation sequence.** Ordered steps with the dependency graph made explicit and an owner against each step.
4. **Revocation confirmations.** For each old credential, the evidence that it is now rejected.
5. **Audit findings.** Per secret, the log source consulted, the window it covers, the queries run and what was found.
6. **Notification decision.** The facts relevant to it, the question asked, who answered it and when.
7. **Prevention actions.** Owners and dates, ending with the structural change rather than a reminder to be careful.

## The clock, and what is honestly known about it

The strongest peer-reviewed measurement here is Meli, McNiece and Reaves, "How Bad Can It Git? Characterizing Secret Leakage in Public GitHub Repositories", NDSS 2019, from North Carolina State University and Cisco. Their nearly six-month real-time scan of public commits found secrets in over 100,000 repositories, with thousands of new unique secrets leaking every day. The number that should change your ordering is the discovery latency they measured: they pushed a known random string to a known repository, started a timer and queried the GitHub Search API until it appeared, repeating once a minute over 24 hours. The median time to discovery was 20 seconds, with times ranging from half a second to over four minutes. Any stranger with a script and a search API key sees your commit inside a minute.

Be precise about what that measures: how fast a published secret becomes findable, not how fast it gets used. The widely repeated claim that a leaked cloud key is exercised within a minute of publication is not carried here, because it could not be traced to a primary source. Treat it as folklore until someone shows you the experiment, and argue the urgency from the documented discovery latency instead.

The same paper measured what happens next, which is the argument for treating your own cleanup as low value: about 6% of detected secrets were removed in the first hour, over 12% were gone by the end of the first day, and only 19% were gone after 16 days, so 81% were never removed at all. Removals were overwhelmingly new commits deleting the file rather than repository deletions, so the secrets stayed reachable anyway.

Removal is also not revocation, and people conflate them. Truffle Security tracked 10,000 secrets across 9,399 files from GitHub's public event feed between 28 September and 17 October 2023, then followed them for 31 days: 74% were still valid at the end, 63% of the exposed files were still public a month later, and in more than half the cases where the file was taken down the key stayed live. Where a key was revoked at all, it usually happened in the first three days.

One scheduling rule follows. **Triage and rotation run in parallel, not in sequence.** One person mints replacement credentials while another finishes classifying, and nobody waits for a complete inventory before invalidating the highest-privilege item in it.

## Minutes 0 to 10, classify before you touch anything

For every exposed value, answer five questions. They take a minute each and they determine the entire response shape.

1. What does it grant, in permissions rather than in product names?
2. Is it long-lived, or does it expire without help?
3. Can it be scoped down instead of replaced, and does scoping down actually revoke the exposed capability?
4. Is replacement atomic, or is there a propagation delay?
5. What else authenticates with this same value?

The type of secret decides most of the answers.

| Type | Response shape |
| --- | --- |
| Cloud provider access key | High blast radius, often includes the ability to mint further credentials. Revoke first. Expect the provider to act before you do. |
| Payment provider key | Money and customer data. Revoke first. Check for refunds, payouts and new webhook endpoints, not just charges. |
| Database credential | Usually many consumers and no expiry. The classic case for creating a second role rather than changing one password. |
| Signing key | Anything signed with it stays trusted until verifiers stop accepting it, so revocation means a verifier-side change too. |
| OAuth client secret | Rotating it invalidates the client's ability to obtain new tokens. Existing access tokens usually survive until expiry, so check the token lifetime. |
| Webhook signing secret | The failure is silent and inbound: an attacker can forge events you accept. Most providers allow two active secrets during a transition. Use that. |
| Personal access token | Scope is frequently wider than the holder remembers, and it often carries write access to source. Revoke, then enumerate what it could reach. |
| Encryption key | Rotation alone does not help, because existing data stays encrypted under the old key. You need re-encryption, and the old key stays alive until it completes. |

The encryption key row is the one that breaks the pattern, and it is worth stating twice. If you revoke or destroy an exposed data-encryption key the way you would revoke an API key, you lose the data. The response is to introduce a new key, re-encrypt under it, verify, and only then retire the old key, while treating everything encrypted under the exposed key as readable by whoever holds it in the meantime.

One general rule for classification: prefer the provider's own permission listing over anyone's memory of what the key was for. Keys acquire scope over years and nobody updates the ticket.

## The rotation order, which is the real content

The default sequence is **mint, deploy, verify, revoke**. Create the replacement credential, distribute it to every consumer, confirm each consumer is authenticating with the new value, and only then invalidate the old one. Reversing those last two steps is the most common self-inflicted outage in this whole procedure, because the gap between revoking and finishing the deployment is a gap in which nothing works.

That default assumes a provider that lets two credentials be live at once. Before you start, write down the consumer list, because this is where plans fail. Consumers are not just running services: enumerate application processes per environment, CI and deployment pipelines, scheduled and batch jobs including the quarterly ones, developer machines, infrastructure-as-code state, backup and restore scripts, third-party integrations configured in someone else's dashboard, monitoring and error reporting agents, and any client you do not control.

That last case is why a credential should never be embedded in a distributed client. If it is, the propagation delay is an app store review plus user upgrade behaviour, measured in weeks, so you revoke, break old versions, and ship a build that fetches the value at runtime.

**When the provider allows only one live credential per principal, do not fight it. Duplicate the principal instead.** Create a second database role, a second service account, a second IAM user, a second API client, issue a credential to that, migrate consumers to it, then delete the exposed principal outright. This converts an atomic swap you cannot perform into an ordinary migration, and it has the extra benefit that deleting the principal revokes every credential it ever held. Where even that is impossible, the fallback is a planned interruption: pre-stage the new value everywhere behind a configuration switch that reads either variable, take the short outage deliberately at a time you choose, and flip.

Two propagation delays catch people. Container images bake configuration at build time in many pipelines, so "deployed" means rebuilt and rolled out rather than a variable changed in a dashboard. And cached configuration at a CDN or edge can keep serving the old value after the origin has changed, so verification means observing a successful authentication with the new credential from each consumer, not reading a settings page.

**The quarantine trap.** If the exposed key is an AWS access key found publicly, AWS may act before you do. It maintains a managed policy, `AWSCompromisedKeyQuarantineV3`, described in its own documentation as "applied by AWS in the event that an IAM user's credentials have been compromised or exposed publicly", created 21 August 2024 and last edited 16 March 2026. Read its statement list before you plan, because among many other actions it denies `iam:CreateAccessKey`, `iam:DeleteAccessKey`, `iam:UpdateAccessKey`, `sts:GetSessionToken` and `cloudtrail:LookupEvents`. The quarantined identity therefore cannot rotate its own keys and cannot read its own audit trail. Run the rotation from a different principal with its own administrative access, and do not detach the policy to make your life easier, because AWS's own note says not to and the support case it opened is where the instructions live.

This behaviour is not unique to one provider. GitHub's secret scanning partner programme sends a payload to the provider's endpoint when a match is found in a public source, and GitHub tells partners to treat those secrets as "public and compromised". Assume the provider will disable the credential on its own schedule, which is another reason to mint the replacement first.

## Decision rule, revoke now or roll forward

- **If the credential grants write access to production data, the ability to spend money, or the ability to create further credentials or identities: revoke first and accept the outage.** Containment beats availability here, and this is why OWASP puts revocation ahead of rotation. Tell the on-call channel what you are about to break before you break it.
- **If it is read-only against non-sensitive data, cannot spend and cannot escalate: roll forward gracefully.** Mint, deploy, verify, revoke, inside the hour, no outage.
- **If you cannot tell what it grants: revoke first.** An unknown scope is a maximum scope until proven otherwise. Make this concrete with a timer: if you cannot enumerate the permissions from the provider's own console or API within ten minutes, you are in this branch, and further archaeology happens after the credential is dead rather than before.

One qualification worth carrying, from OWASP's rotation guidance: "User credentials are excluded from regular rotation. These should only be rotated if there is suspicion or evidence that they have been compromised, according to NIST recommendations." An exposure is exactly that suspicion, so rotate them here. The point is that a leak is not a reason to start a blanket password expiry policy afterwards.

## The audit, and the sentence you are allowed to write

Revocation ends the future risk. It says nothing about what happened during the exposure window, and someone senior will ask that within a day.

Open the provider's own logs and look for type-appropriate signals. For a cloud key: new identities, new access keys, policy attachments, compute started in regions you do not use, and outbound data transfer. For a payment key: refunds, payouts, new API keys, and above all new or changed webhook endpoints, which is how an attacker keeps a foothold after your key dies. For a source-control token: repository reads, new deploy keys, changes to workflow files, and new collaborators. For a database credential: connections from unfamiliar addresses and any large read.

Then apply the retention rule, which is the part people get wrong. **You may only write "no evidence of use" if you also write which log you looked at and how far back it goes.** Defaults are shorter than assumed. AWS CloudTrail Event history, the free view available without configuration, is "limited to the past 90 days of events" and, in AWS's own words, "only shows management events. It does not show data events, Insights events, or network activity events." An object read performed with the stolen key is therefore absent unless data events were already recorded on a trail, and a search there covers one account and one region with a single attribute filter.

Write the finding in this shape: "Reviewed CloudTrail management events for this account in the two regions in use, from the publication timestamp to now, a window fully covered by the 90 day history. No API calls from the exposed key. Object-level reads are not covered by this source and no data event trail was configured." That is honest and checkable. "No evidence of misuse" on its own is a claim nobody can verify and everyone will later treat as an assurance.

## Notification, which is a legal question this file does not answer

Whether an exposure triggers a notification obligation depends on what the credential could reach, which people are affected, where they live, and what kind of organisation you are. Counsel decides. What follows exists so you can ask a precise question quickly, and it is not legal advice.

**GDPR**, applicable across the EU since 25 May 2018, Article 33(1): the controller shall notify the supervisory authority "without undue delay and, where feasible, not later than 72 hours after having become aware of it", unless the breach "is unlikely to result in a risk to the rights and freedoms of natural persons". Where 72 hours is missed, the notification "shall be accompanied by reasons for the delay", so the deadline is not a cliff edge if you can explain yourself. Article 33(5) requires documenting every personal data breach whether or not it was notified, which is another reason the response plan is a document. Article 34 sets a higher bar, high risk to individuals, for telling the individuals themselves. Breach notification failures sit in the lower fine tier under Article 83(4), up to 10 million euro or 2% of total worldwide annual turnover, whichever is higher, rather than the 4% tier.

**SEC Form 8-K Item 1.05**, adopted 26 July 2023, applies to registrants with the Commission rather than to companies generally. In the SEC's own words, it "will generally be due four business days after a registrant determines that a cybersecurity incident is material". The clock runs from the materiality determination, not from discovery, and disclosure may be delayed where the United States Attorney General determines immediate disclosure would pose a substantial risk to national security or public safety. Compliance for the 8-K disclosure began the later of 90 days after Federal Register publication or 18 December 2023, with smaller reporting companies given an additional 180 days.

**State law moves, and stale deadlines circulate.** California Civil Code 1798.82(a)(2)(A) now requires that disclosure "shall be made within 30 calendar days of discovery or notification of the data breach", added by SB 446 and effective 1 January 2026. Anyone quoting only the older "most expedient time possible" standard is working from an out of date note, and the same is likely true of other jurisdictions you have not rechecked this year.

## Prevention, written into the plan rather than promised

Platform scanning is worth turning on and worth understanding precisely. GitHub secret scanning runs automatically and free on public repositories, and covers more surface than people expect: the entire Git history on all branches, issue titles, descriptions and comments including historical ones, pull request content, Discussions, wikis and secret gists. Private and internal repositories need GitHub Secret Protection on Team or Enterprise Cloud.

Its limits matter more than its coverage. Partner patterns are matched against known provider formats, and, in GitHub's words, "Partner secrets are reported directly to the provider and aren't displayed in your repository alerts", so an empty alert list is not evidence that nothing leaked. Detection of generic credentials such as private keys and connection strings, organisation-specific custom patterns, and AI-detected unstructured secrets such as passwords are separate capabilities rather than the free baseline, which leaves a high-entropy string with no recognisable shape, an internal signing secret, or a password in a build log with no pattern to fire against.

Push protection blocks pushes from the command line, commits made in the web interface and file uploads, but a developer with write access can bypass it by choosing a reason, and the options include "I'll fix it later". Treat every bypass as an event worth reviewing rather than a setting that solved the problem.

The structural fixes, in the order they pay off:

- **Remove the long-lived credential entirely where you can.** Workload identity federation lets CI exchange a short-lived identity token for temporary cloud credentials, so there is no static key in the CI provider to leak.
- **Shorten lifetimes.** A one-hour credential turns a leak into an inconvenience.
- **Scope down.** Most keys hold permissions they have never used, and the provider can usually tell you which.
- **Keep values out of the process environment where an error reporter can serialise them.** Crash reports and observability payloads routinely capture the whole environment, and that copy sits outside every scanner you run.
- **Add pre-commit scanning locally**, the only layer that stops publication rather than reporting it.

## Worked example

A mid-size logistics company. A contractor commits a sample integration script to a public repository, including a live payment provider secret key. A community scanner reports it 40 minutes later.

**Classification.** One secret, payment provider, live key. Grants charges, refunds, payout configuration and webhook management. Long-lived, no expiry. Cannot be scoped down meaningfully. Decision rule branch one: it can move money, so revoke first is on the table.

**Parallel triage.** One engineer opens the provider dashboard and mints a replacement key immediately. A second engineer builds the consumer list and finds four, not the three everyone assumed: the checkout service, a nightly reconciliation job, an internal refunds tool, and a spreadsheet-driven finance export running from a workstation.

**Sequencing.** The provider supports multiple live keys, so the swap is graceful and takes eleven minutes: deploy to checkout, verify a successful authenticated call, deploy to the reconciliation job and force one run, update the refunds tool, then the finance export. The carrier SFTP account discovered alongside it allows only one credential, so it gets the other procedure: a second account is created with the carrier, jobs are pointed at it, and the original account is closed rather than having its password changed.

**Revocation.** The original key is deleted in the provider dashboard, and its deletion is confirmed by an authenticated request that returns an authentication error rather than by trusting the interface.

**Audit.** Provider event log reviewed from the commit timestamp forward. No charges, no refunds. One detail justifies the whole step: a webhook endpoint had been added six minutes before revocation, pointing at an unfamiliar host. It is deleted, and the case moves from "exposed, no use" to "used, foothold attempted, removed".

**History.** The commit is rewritten and GitHub Support is asked to clear cached views and pull request references. This happens last and is recorded as cleanup rather than as remediation.

**Verdict.** Contained. The plan's prevention section carries three items with owners: contractors get scoped restricted keys rather than the live key, pre-commit scanning is added to the shared template, and the finance export is moved off a workstation. The unbudgeted item is the webhook check, which nobody had in a runbook and which was the only sign of an attacker.

## Failure modes

- **History Theatre.** The team spends the first hour rewriting history and the key is still live at the end of it. Recognisable by a timeline where the first revocation happens after the first force push.
- **Revoke Before Deploy.** The old credential dies before the new one is everywhere. Looks like a self-inflicted outage that starts precisely when the remediation is announced as finished.
- **Partial Inventory.** Four of five consumers get the new value. The fifth is a quarterly job, and it fails at three in the morning eleven weeks later, by which time nobody connects it to the leak.
- **Scope Assumption.** The key is described from memory as read-only, and the audit later shows it could write. The tell is that nobody opened the provider's permission listing during the incident.
- **Log Window Expired.** The plan says no evidence of use, and the log consulted covers less time than the exposure. Recognisable by a finding with no window stated next to it.
- **Encryption Key Confusion.** An exposed data-encryption key is treated like an API key and rotated or destroyed, and data becomes unreadable. Shows up as a restore that fails after the incident is closed.
- **Rotation Without Audit.** The credential is replaced quickly and cleanly, and nobody looks at what it did while it was public. The attacker's persistence, usually a new webhook, deploy key or IAM user, survives the rotation untouched.
- **Same Secret Reissued.** The replacement is placed in exactly the same file, image layer or shared document as the original, and the second leak follows the first within the month.
- **Quarantine Deadlock.** The provider disables or quarantines the identity first, and the rotation is attempted from that same identity, which no longer holds the permissions to rotate anything or read its own trail.
- **Alert Absence Mistaken For Safety.** Nobody checks the provider mailbox because the repository shows no scanning alerts, missing that partner matches are reported to the provider rather than surfaced as repository alerts.

## What this skill does not do

- **It is not legal advice.** It names GDPR Article 33, SEC Item 1.05 and one state statute with their dates so you can ask a precise question. Whether any of them applies to your situation is a decision for counsel, and the answer varies by jurisdiction, by the data involved and by what kind of organisation you are.
- **It cannot tell you whether the credential was used.** It tells you which log to open, what it covers and what sentence the evidence supports. Where the retention window has closed, the honest output is that the question cannot be answered.
- **It does not scan.** Detection belongs to platform secret scanning and to dedicated scanners, all of which are better at finding secrets than any procedure written in prose. This file begins after a finding.
- **It does not design a secrets management system.** Vault choice, key hierarchies, envelope encryption and automated rotation schedules are a separate project, and the OWASP cheat sheet is the free entry point to it.
- **It stops where exfiltration begins.** Once data is established as taken, you are running an incident with forensics, evidence preservation and external communications, and none of that is here.
- **Provider behaviour changes.** Every platform fact above was checked against published documentation on 20 August 2026. Quarantine policies, retention windows and scanning coverage all move, so verify the two that your plan depends on before you rely on them.
