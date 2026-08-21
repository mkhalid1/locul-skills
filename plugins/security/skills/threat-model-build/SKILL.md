---
name: threat-model-build
description: Builds a threat model document: a scope statement tied to a release, an element inventory, a numbered trust boundary list, a threat table with one of four dispositions on every row, and the deferred decisions with their reasons. Carries the STRIDE per element mapping, the published definition of a trust boundary, a boundary decision rule with an unknown branch, and an ordering method that uses no invented risk scores. It works at design level and finds no implementation bugs. This skill should be used when a feature, service or integration needs its security design reasoned about and recorded before it ships, or when an existing model must be rebuilt after an architecture change.
---

# Threat model build

## The claim this skill is built on

A threat model is a decision record, not a brainstorm. You finish with a document in five parts: a scope statement naming the release, an element inventory, a numbered list of trust boundaries, a threat table where every row carries a disposition, and a deferred list where every entry carries a reason and an owner.

The obvious approach is to name the feature and list attacks against it. That list is plausible, often quite good, and carries two defects that more attacks do not repair.

It cannot be prioritised, because priority comes not from how clever an attack is but from which boundary it crosses and what sits on the other side. A list with no boundaries attached threw that away before anyone read it, so two reasonable people rank it differently and nothing settles it.

It cannot be closed, because a record must show what was considered and dismissed, or nobody can distinguish a threat that does not apply from a threat nobody had. When a new person asks in six months whether anyone considered the queue, the answer has to be in the document.

Everything below is ordered so the information needed to rank a threat exists before any threat is written down.

## The four questions, and the one that gets skipped

The Threat Modeling Manifesto, published at threatmodelingmanifesto.org by a working group of practitioners in 2020, states the framing as four questions: what are we working on, what can go wrong, what are we going to do about it, and did we do a good enough job. The same four are published as Shostack's Four Question Framework, which phrases the last as "did we do a good job".

Each question has an output, and naming the output stops the exercise becoming a conversation.

- **What are we working on**: the scope statement, element inventory and boundary list. A written artefact with a version on it, not a discussion.
- **What can go wrong**: the threat table, one row per applicable element and category pair.
- **What are we going to do about it**: a disposition on every row, from a closed set of four.
- **Did we do a good enough job**: a completeness check, the deferred list and a review trigger.

The fourth is the one everybody skips and the only one that makes the exercise repeatable. Without it nothing states what was not covered, the next model starts from zero, and nobody can tell whether this one is stale. The manifesto's values put continuous refinement over a single delivery, and the review trigger is what makes that true rather than aspirational.

## Step 1. Scope, written before anything is drawn

An unscoped model is unfinishable, because every system connects to another system and every threat opens a new one. Write four things first.

1. **The release or commit this model describes, with a date.** A model with no version line cannot be known to be stale.
2. **In scope**, listed as the components you can actually change.
3. **Out of scope, each with a reason.** The move that matters: an out-of-scope system does not vanish, it becomes an external interactor. The identity provider you do not own is still a box, still a data source, still on the far side of a boundary. Treating out of scope as absent is how a model quietly assumes an integration is safe.
4. **Assumed trusted**, each entry with an owner and one line saying how you would find out it was false. "The object store enforces per-tenant prefixes" is an assumption. "Owned by the platform team, falsified by a test that writes with tenant A's credentials into tenant B's prefix and expects a denial" is that assumption made checkable.

One further rule, from the Microsoft SDL material and easy to get wrong: do not model both sides of a boundary at once. Pick a side, represent the other as external interactors, and build a second model from that side if you own it too. A diagram detailing client and server simultaneously implicitly trusts the client, which is the one thing you cannot do.

## Step 2. The diagram, in text

Use the standard data flow diagram element set: **data flow**, **data store**, **process** and **interactor**, also called an external entity, plus one symbol added specifically for threat modelling, the **trust boundary**. That five-symbol set and its use for STRIDE are set out in "Uncover Security Design Flaws Using The STRIDE Approach" by Hernan, Lambert, Ostwald and Shostack, MSDN Magazine, November 2006.

No drawing tool is required, and text reviews better in a pull request. One workable notation:

```
EXTERNAL  browser        anonymous until authenticated
PROCESS   upload-api     identity api-sa, tenant scoped
PROCESS   scan-worker    identity worker-sa, writes all prefixes
STORE     raw-uploads    object store, per-tenant prefixes
STORE     jobs-queue     one queue, all tenants
FLOW      browser -> upload-api      crosses B1
FLOW      upload-api -> jobs-queue   crosses B2
BOUNDARY  B1  network edge     outside is the public internet
BOUNDARY  B2  identity change  api-sa one side, worker-sa the other
BOUNDARY  B3  tenancy          inside raw-uploads, prefix per tenant
```

Four sanity rules, all from the same source, all of which catch real omissions:

1. **No magic sources or sinks.** Every store needs a named reader and a named writer. A store with no reader is either dead data you should delete or a reader you have not modelled, and the second is far more common.
2. **No psychokinesis as transport.** Data never travels from a person to a store without a process in between. A diagram showing one has hidden the component where the interesting threats live.
3. **Collapse similar elements inside one boundary.** Three services in the same runtime, at the same trust level, inside the same boundary are one process here. Think major function, not deployment unit.
4. **Detail one side of a boundary at a time**, as in step 1.

**The right depth.** Deep enough when every boundary-crossing flow and every element touching attacker-influenceable data is named. Too deep when adding an element neither adds a crossing nor changes a disposition. The tell: a region of the diagram with no boundary line on it is past the level where the method pays.

## Step 3. Trust boundaries, defined rather than paraphrased

The published definition, from CWE-501, Trust Boundary Violation: "A trust boundary can be thought of as line drawn through a program. On one side of the line, data is untrusted. On the other side of the line, data is assumed to be trustworthy." The MSDN treatment puts it from the other direction: the border between trusted and untrusted elements, where being on the far side means you do not trust what is there.

Note what the definition does not say. Nothing about networks, firewalls or machines. A boundary is about the trust status of data, which is why several important ones are invisible on an infrastructure diagram.

Enumerate these seven kinds explicitly, because the list is the part nobody produces from memory under time pressure:

- **The network edge.** The easy one, and the only one most diagrams already have.
- **A process or machine boundary.** Components already separated, or separable later.
- **A privilege change.** An administrative console, a role escalation, a job running under a broader service identity than whatever enqueued it.
- **A tenancy boundary.** Tenant A and tenant B inside one database, bucket, queue or cache. In most multi-tenant products this is the highest-value boundary in the model and the least often drawn.
- **Your code and a third-party service.** A payment provider, an email sender, a content delivery network. Their response is data you did not produce.
- **Deterministic code and a model that reads untrusted text.** The output of a model that consumed a web page, an email or a user document is attacker-influenceable, even though your own code produced it inside your own process. Draw the line here. Defending the crossing is a separate job with its own artefact.
- **A human boundary.** A support agent who can act on a customer's account is a privilege change wearing a person.

**The decision rule.** For any data flow, ask whether it crosses a boundary.

- **Different privileges, tenancies or organisations** at the two ends: yes.
- Data **attacker-influenceable at one end and treated as trusted at the other**: yes, even inside a single process, even between two functions in one file.
- Same privilege, tenancy and organisation, data trusted at both ends: no.
- **Cannot tell**: treat it as a boundary and model it. The asymmetry decides it. A false boundary costs one table row and a sentence saying why it was dropped. A missed one costs the finding, and that is almost always the finding nobody reaches any other way.

Number every boundary and never reuse a number, including after one is removed. Threat rows reference boundaries by number, and reused numbers make old models unreadable.

## Step 4. STRIDE per element, not per feature

STRIDE was developed at Microsoft by Loren Kohnfelder and Praerit Garg, in a document titled "The threats to our products" dated April 1999. Six categories, each the negation of a security property:

| Category | What it is | Property violated |
| --- | --- | --- |
| Spoofing | Using another party's identity or credentials | Authentication |
| Tampering | Malicious modification of data, at rest or in transit | Integrity |
| Repudiation | Acting and plausibly denying it, because nothing proves otherwise | Non-repudiation |
| Information disclosure | Exposure to parties not meant to have it | Confidentiality |
| Denial of service | Denying service to valid users | Availability |
| Elevation of privilege | An unprivileged party gaining privileged access | Authorisation |

The part that turns the mnemonic into a procedure is the mapping of categories to element types, published as Figure 5 of the MSDN Magazine article above and named STRIDE per Element by Microsoft's own Threat Modeling Tool:

| Element | S | T | R | I | D | E |
| --- | --- | --- | --- | --- | --- | --- |
| Interactor (external entity) | yes | | yes | | | |
| Process | yes | yes | yes | yes | yes | yes |
| Data flow | | yes | | yes | yes | |
| Data store | | yes | | yes | yes | |

Two amendments worth carrying. A data store that is a **log** takes repudiation too, because tampering with the log is precisely how an action becomes deniable. And a flow sitting **entirely inside one boundary** still gets a row: the MSDN article works that argument through and lands on priority rather than omission, on the grounds that the assumed boundary may be wrong and anything can fail.

Why the mapping matters. Without it, "apply STRIDE to this feature" is a brainstorm with an acronym attached and its completeness cannot be assessed. With it the model has a fixed cell count. Two interactors, three processes, three stores and six flows gives 4 plus 18 plus 9 plus 18, which is 49 cells, and the model is finished when all 49 have a verdict. Most read "not applicable" with one sentence saying why, and those sentences are the half that makes it a record.

**Work in this order:** boundary-crossing flows first, then the processes on the trusted side of each crossing, then stores, then interactors. A threat on a crossing flow arrives with its priority attached.

## Step 5. Disposition, which is what makes it a record

Every row carrying a real threat gets exactly one of four dispositions, each with a mandatory field. A row without its field is not finished.

1. **Mitigated.** Name the control, its **location**, and how it would be **verified**. "We validate input" is a category, not a control. "The upload handler rejects any content type outside a four-item allowlist, enforced in the request middleware, verified by the traversal-filename test" is a control, because somebody can go and look.
2. **Transferred.** Name the **party** and the **mechanism**: a provider's published service terms, a contract clause, an insurance policy, a configuration the customer owns. Transferred means somebody else's job, so there has to be a somebody.
3. **Accepted.** Name the **person**, the **date** and the **review trigger**, the trigger being a date, a scale threshold or a named architecture change. An acceptance with no name is indistinguishable from a row nobody looked at, and in an incident review it reads as one.
4. **Eliminated.** The feature, flow, store or capability is removed. The only disposition that shrinks the model rather than growing it, and the most under-used.

"We should look at this", "TODO" and "needs investigation" are not dispositions. If the work is genuinely not done, the honest row is Accepted with an owner and a dated review, which somebody can be held to.

Every threat row also carries two more columns: **the boundary crossed**, by number or none, and **the asset on the far side**. Those two are the entire input to the next step.

## Step 6. Ordering without invented numbers

Numeric risk scoring fails predictably. A score multiplied out of a likelihood nobody can estimate yields a figure with a decimal place and no information, and it then defeats discussion, because disagreeing with 7.4 means proposing a different invented number rather than a reason.

Sort on four keys instead, in order, and write the deciding key on the row:

1. **Boundary crossed and the asset behind it.** A threat crossing from the public internet into a store holding every tenant's documents outranks one crossing from an authenticated administrator into a store of feature flags.
2. **What the attacker needs.** No credentials, then any authenticated user, then a specific role, then an operator with production access. Each step down sharply cuts the population who can attempt it.
3. **Blast radius.** One record, one tenant, or every tenant. Anything crossing a tenancy boundary sorts near the top in a multi-tenant product, almost regardless of the other keys.
4. **Existing control elsewhere on the path.** A threat already caught by something else drops, and the row names that control and its location, which makes the claim checkable rather than reassuring.

The output is an ordered list where every position carries its reason. A reader can argue with a reason, and the argument improves the model. Nobody can argue with a number that was never measured.

## When STRIDE is the wrong method

**Privacy.** LINDDUN, from the DistriNet unit at KU Leuven and first published in 2010, covers seven privacy threat types: Linking, Identifying, Non-repudiation, Detecting, Data Disclosure, Unawareness and Non-compliance. Note the inversion. Non-repudiation is a security goal in STRIDE and a threat in LINDDUN, because a system that makes an action undeniable also makes a person's participation undeniable. Where the asset is personal data rather than system integrity, STRIDE has no vocabulary for the threats that matter.

**One catastrophic outcome.** Attack trees, popularised by Bruce Schneier in Dr Dobb's Journal in December 1999, put the attacker's goal at the root, child nodes as conditions that must hold for the parent to be true, joined by AND and OR gates, and concrete attacks at the leaves. They are goal-first and depth-first where STRIDE is system-first and breadth-first. Reach for a tree when you already know the one outcome that would end the company and want every path to it, never to enumerate.

**Business risk.** PASTA, the Process for Attack Simulation and Threat Analysis, set out by Tony UcedaVélez and Marco Morana in Risk Centric Threat Modeling in 2015, runs seven stages from business objectives through technical scope, decomposition, threat analysis correlated against real intelligence, weakness analysis and attack modelling to risk and impact. It answers which risks the business cannot afford, and it assumes you have threat intelligence and weeks.

## Worked example

A multi-tenant document tool adds file upload with background processing. Everything here is invented.

**Scope.** Release 4.2, dated. In: the upload endpoint, raw-uploads store, jobs queue, scan worker, thumbnail store. Out, and therefore modelled as external interactors: the identity provider and a third-party scanning API. Assumed trusted: the object store enforces per-tenant prefixes, owned by the platform team, falsified by a cross-prefix write test.

**Elements and boundaries.** Two interactors, three processes, three stores, six flows: fourteen elements, 49 cells. B1, the network edge. B2, an identity change at the queue, because upload-api runs tenant-scoped while the scan worker can write every tenant's prefix, and the message carries a browser-supplied filename. B3, tenancy inside raw-uploads. B2 is the one usually missed, because it sits inside one network and the queue is drawn as an ordinary box.

**STRIDE pass on the queue-to-worker flow, which crosses B2.** A data flow takes tampering, information disclosure and denial of service.

- **Tampering.** The message carries an object key derived from a browser-supplied filename, and the worker can write every prefix, so a traversal sequence in the key puts the write wherever the attacker chose. *Mitigated:* the API generates the key as a server-side identifier and the filename travels in a separate field the worker never uses in a path. Verified by a traversal-filename test asserting the stored key.
- **Information disclosure.** The message carries a signed download URL, so anything with queue read access has file access for its lifetime. *Accepted:* platform lead named, dated, review trigger "the first time anything outside the worker fleet gets read access to this queue". Recorded reason: at 4.2 only the worker fleet can read it and the URL lives sixty seconds.
- **Denial of service.** One tenant can enqueue faster than the fleet drains, delaying every other tenant, which makes it a B3 crossing in effect. *Mitigated* by a per-tenant in-flight cap, and *transferred* in part, since the object store's request limits are the provider's obligation under its published service terms.

**One process row, since a process takes all six.** Elevation of privilege on the scan worker: it holds credentials that write every prefix, so any file-parser defect is a cross-tenant write. *Eliminated:* the API mints a per-job credential scoped to one prefix. Elimination rather than mitigation, because the capability is gone rather than guarded.

**Verdict.** Seven of the 49 cells carry a real threat: three mitigated, two accepted with owners and dates, one transferred, one eliminated. Two of the seven were visible only because B2 was drawn. The feature ships at 4.2, with the accepted row's review trigger written into the queue's own documentation so the next person to add a reader must reopen this model. The other 42 cells read "not applicable" with a sentence each, and those sentences are what a reviewer needs in eight months.

## Failure modes

**Threat Soup.** An unordered list of plausible attacks, no elements, no boundaries. The tell: two people rank it differently and nothing in the document settles it.

**Diagram Theatre.** A clean, colour-coded architecture diagram, every box drawn, not one dotted line. The threats it produces are the ones somebody already knew.

**Mitigation Assumption.** A row says mitigated and names a control nobody checked. The tell: the control is a category rather than a location. A refactor removes it and the row still says mitigated.

**Scope Creep.** The model starts on an upload feature and ends up modelling the identity provider. The boundary list keeps growing and no row has a disposition. The cause is always an out-of-scope system treated as absent rather than as an interactor.

**Acceptance Without An Owner.** Rows reading "accepted, low risk", no name, no date, no trigger. Indistinguishable from a row nobody read.

**Score Invention.** Likelihood times impact to one decimal place, from guessed numbers. The tell: the ranking survives every challenge, because arguing requires a competing invented number.

**Model Rot.** Accurate for a release eighteen months gone. A queue added, a provider swapped, a service split. The tell: no version line, or one naming a release nobody deploys.

**Attacker Fantasy.** A page on nation-state capability against a store of feature flags, and no row for the authenticated customer who can read another customer's invoice. The symptom: an interactor list with one entry called "attacker" instead of the real roles.

**The Second Side Never Modelled.** The client collapsed into an interactor and no companion model, even though you ship the client. Everything past B1 is unexamined by design and nobody recorded that the omission was deliberate.

## What this skill does not do

- **It finds design problems, not implementation bugs.** The injection sitting in the code is invisible to it. Claude Code's automated security review, announced in August 2025, covers that half: a /security-review command and a GitHub Action that read a diff for SQL injection, cross-site scripting, authentication and authorisation flaws, insecure data handling and dependency vulnerabilities, posting findings inline on a pull request. First party, free with the tool, better than this at that job. Run both, at different moments.
- **It tests nothing.** Every control in a mitigated row is a claim until somebody runs the check beside it. The verification column is a plan, not a result.
- **It has no view on compliance frameworks.** Mapping controls onto a published standard is a different document with a different reader.
- **It cannot model what you do not tell it.** A forgotten integration is a boundary that never gets drawn, and nothing signals the gap.
- **It does not cover the neighbouring security jobs.** Triaging dependency findings, responding to a leaked credential, writing a content security policy that deploys, and defending an agent that reads untrusted text are four separate jobs with four separate artefacts. This one marks where each becomes relevant and stops.
- **It produces no attack code**, no exploit, and no test harness for the threats it names.
