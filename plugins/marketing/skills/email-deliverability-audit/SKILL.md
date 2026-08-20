---
name: email-deliverability-audit
description: Audits an email sending setup in descending order of how fatally each defect blocks inbox placement. Covers SPF including the ten lookup evaluation limit, DKIM selectors, key length and what the signature actually covers, DMARC policy and the relaxed versus strict alignment rule that decides whether a technically valid message passes, the bulk sender requirements published in February 2024, spam traps and bounce handling, domain and IP warm-up, shared versus dedicated IP selection, subdomain separation, and the content factors that genuinely matter. This skill should be used before the first send from a new domain or platform, when mail that used to arrive starts going to spam, or when migrating a list between sending platforms.
---

# Email deliverability audit

## The claim this skill is built on

Deliverability advice is usually written in the wrong order, and the wrong order is expensive here in a way it is not elsewhere.

The typical article opens with subject line words to avoid, spends a paragraph on how many images is too many, and mentions authentication near the end as something your provider probably handles. That ordering is close to exactly inverted. A message from a domain that fails DMARC alignment, sent to a list with a twelve percent hard bounce rate, will not reach the inbox no matter what the subject line says. A message from an authenticated domain with a clean, engaged list reaches the inbox while containing every word on every trigger list ever published.

So this audit runs in descending order of fatality and stops to raise the alarm at the first tier that fails. Six tiers:

0. The mail is not authenticated, or it is authenticated and not aligned.
1. The list is the defect.
2. The sending pattern is the defect.
3. The reputation model or the infrastructure is wrong.
4. The message construction is the defect.
5. Nobody is measuring, so nobody will notice when any of the above changes.

Report the highest failing tier first, alone, and loudly. Everything below a Tier 0 failure is an appendix.

## Tier 0. Authentication, and the part nobody gets right

### SPF, and the lookup budget

SPF is published as a DNS TXT record beginning `v=spf1` and it authorises sending hosts for the **envelope sender domain**, the address in the SMTP `MAIL FROM` command, which is also what appears in `Return-Path`. It says nothing at all about the domain a human sees in the From header. This is the first thing to understand and the thing most explanations skip. A message can pass SPF perfectly while displaying any From address in the world.

**The ten lookup limit is the defect you will actually find.** RFC 7208 caps the number of mechanisms and modifiers that cause a DNS query at ten per evaluation: `include`, `a`, `mx`, `ptr`, `exists` and `redirect` all count. Exceed it and the evaluation returns `permerror`, which is a permanent failure, not a retry, and which DMARC treats as an SPF fail. There is a second, separate limit of two void lookups, meaning queries that return no answer or a non-existent domain.

The reason this breaks silently is that `include:` chains are recursive and the lookups inside them count against your budget. A record with four vendor includes looks like four lookups and is frequently eleven, because one of those vendors includes two of their own regional records, each of which includes another. Nobody sees this happen. The record was fine on Tuesday, marketing added a webinar platform on Wednesday, and by Thursday every message from the domain fails SPF. Count the fully resolved chain, not the record you can see.

The remedies, in order of preference: remove vendors that no longer send for you, which is usually two of them; replace an `include` of a vendor who publishes a stable IP range with the `ip4` and `ip6` mechanisms directly, since those cost no lookups; and only then consider an SPF flattening service, which resolves the chain into literal addresses at the cost of breaking whenever a vendor renumbers without telling you.

**The all mechanism.** `-all` is a hard fail, telling receivers that anything not listed is not you. `~all` is a soft fail, meaning treat it as suspicious but accept. `?all` is neutral, which is the same as publishing nothing and worse than publishing nothing because it looks deliberate. Use `~all` while you are still discovering which systems send as you, which is the whole point of the monitoring phase below. Move to `-all` once your aggregate reports have shown you every legitimate source for several weeks. On a domain that sends no mail at all, publish `v=spf1 -all` today, because an unused domain with no record is a free identity for someone else.

**SPF does not survive forwarding.** When a recipient forwards to another address, the forwarding server relays the message from its own IP while preserving the envelope sender, so SPF is evaluated against a host that is not in your record and fails. Sender Rewriting Scheme rewrites the envelope sender to fix this and not every forwarder implements it. This is the structural reason you cannot rely on SPF alone, and it is the structural reason DKIM exists.

Also worth checking: the `ptr` mechanism is deprecated by the RFC and should not appear in a modern record; there must be exactly one SPF record per domain, since two is an error rather than a merge; and subdomains do not inherit, so every sending subdomain needs its own.

### DKIM, and what the signature covers

DKIM signs the message with a private key held by the sending system. The matching public key is published in DNS at `selector._domainkey.yourdomain.com`, where the selector is an arbitrary label chosen by the sender, which is what makes multiple simultaneous keys possible.

**Key length.** 1024 bits is the historical floor and is still widely deployed. 2048 bits is the current recommendation and what you should publish for anything new. The practical obstacle is that a DNS TXT string is capped at 255 characters, so a 2048 bit key must be split across multiple quoted strings in the record, and a DNS control panel that does not handle that correctly will produce a key that silently fails to verify. After publishing, always verify the selector resolves and the key parses.

**What the signature covers.** The `h=` tag lists which headers are signed, typically From, To, Subject, Date and a handful more. The `bh=` tag holds a hash of the message body. If any signed header or any byte of the body changes in transit, the signature no longer verifies. Two consequences follow. First, a mailing list that appends a footer, rewrites the subject with a `[list-name]` prefix, or converts the body encoding breaks DKIM by definition, which is why such lists rewrite the From header or implement ARC. Second, the `l=` tag, which limits how much of the body is hashed, exists and should not be used, because it lets anyone append arbitrary content below the signed portion.

Set canonicalisation to `relaxed/relaxed`. The `simple` canonicalisation tolerates no change at all, including the whitespace normalisation that ordinary mail servers perform routinely.

**Rotation.** Keys should be rotated on a schedule, commonly every six to twelve months, and immediately if a key is ever exposed. The sequence matters: publish the new public key under a new selector, switch signing to the new selector, wait long enough for mail signed with the old key to be delivered and verified, and only then remove the old record. Removing the old selector first invalidates every message still in flight.

The domain in the `d=` tag is the domain DKIM authenticates, and it is the one alignment will care about.

### DMARC, and alignment, which is the whole game

DMARC is a TXT record at `_dmarc.yourdomain.com`. The minimum useful form is `v=DMARC1; p=none; rua=mailto:reports@yourdomain.com`.

**The policy values.** `p=none` asks receivers to do nothing differently and to send you reports. `p=quarantine` asks them to treat failing mail as suspicious, in practice the spam folder. `p=reject` asks them to refuse it at the SMTP conversation, so it does not arrive anywhere.

**Alignment is the part almost nobody gets right.** DMARC passes if **either** SPF passes and its domain aligns with the visible From domain, **or** DKIM passes and its `d=` domain aligns with the visible From domain. Only one has to hold. But raw pass is not enough, and this is where working setups fail.

Concretely. Suppose the visible From is `hello@yourcompany.com`. Your sending platform uses its own bounce domain, so the envelope sender is `bounce-92a@mail.sendingplatform.net`. SPF is evaluated against `mail.sendingplatform.net`, and it passes, because the platform's record is correct. It is not aligned, because that domain is not yours. Meanwhile the platform signs with `d=sendingplatform.net`. DKIM passes and is also not aligned. Both raw checks are green in every header inspector, and DMARC fails, and at `p=reject` the message is refused. Every checker that reads only records will tell you your setup is perfect.

**Relaxed versus strict.** Relaxed alignment, the default for both `aspf` and `adkim`, requires only that the organisational domains match, meaning the registrable domain determined from the public suffix list. So `mail.yourcompany.com` aligns with `yourcompany.com` under relaxed alignment, and a subdomain sending arrangement works. Strict alignment, `aspf=s` or `adkim=s`, requires an exact string match of the full domain, so `mail.yourcompany.com` does not align with `yourcompany.com`. Strict is for organisations that have finished the work and want to close the subdomain door. Relaxed is correct for almost everyone, and it is what makes the standard fix possible: point a subdomain of yours at the platform as the custom return path, and have the platform sign with `d=` in your domain.

**The aggregate report address.** The `rua=` tag receives daily XML reports from participating receivers listing every IP that sent mail claiming to be your domain, with volumes and pass or fail results per authentication method. This is the only visibility you will ever have into who is sending as you. Set it before you set anything else. Raw DMARC XML is unpleasant to read, so route it to a parsing service, of which several offer a free tier at low volume. The `ruf=` tag requests per-message forensic reports and is largely unsupported now for privacy reasons.

**The sequence, which is not optional.** Start at `p=none` with `rua` set. Read reports for at least two to four weeks, longer if you have quarterly sending systems. You will discover senders you forgot: the ticketing system, the payroll provider, an old CRM, someone's script. Fix alignment for each legitimate one. Only then move to `p=quarantine`, and only then to `p=reject`. Publishing `p=reject` first is how an organisation discovers on a Monday morning that its invoices have stopped arriving. Note that `pct=` exists in RFC 7489 for ramping a policy across a fraction of mail, and that the revision of DMARC working through the standards process changes how ramping is expressed, so check the current specification before building a plan around that tag.

**The subdomain policy.** `sp=` sets policy for subdomains. In its absence, subdomains inherit `p`. This matters in both directions: a parked or non-sending domain should carry `v=DMARC1; p=reject; sp=reject;` so nobody can send from `invoices.yourcompany.com`, while a domain in the middle of a rollout may want `p=quarantine; sp=none` so an unfinished subdomain is not caught by the parent policy. A subdomain with its own DMARC record overrides both.

### The February 2024 bulk sender requirements

In October 2023 the two largest consumer mailbox providers, Gmail and Yahoo Mail, published requirements that apply to senders above roughly 5,000 messages a day to their users. They did not all start on the same day: authentication and the complaint-rate ceiling took effect on 1 February 2024, and one-click unsubscribe was given a later date of its own, set out below. **These are stated here as published in 2024 and must be re-checked against current documentation, because they have been tightened once already and the thresholds are the sort of thing that moves.**

- **Authentication.** SPF and DKIM both, plus a DMARC record at minimum `p=none`, plus alignment of the From domain with either the SPF domain or the DKIM domain. Alignment is named explicitly, which is why the section above is the longest in this file.
- **One-click unsubscribe** for commercial and subscribed mail, implemented as the `List-Unsubscribe` header together with `List-Unsubscribe-Post` per RFC 8058, honoured within two days, and in addition to a visible unsubscribe link in the message body. This one is dated separately: senders that already carried an unsubscribe link had until 1 June 2024 to support one-click, rather than the 1 February 2024 date that applies to authentication and the complaint rate.
- **A user-reported spam rate below 0.3 percent**, as measured in the provider's own postmaster tooling, with a stated target of staying under 0.1 percent. The gap between those two numbers is the working margin, and a sender sitting at 0.25 percent is not compliant with a margin, it is one bad campaign from filtering.
- Valid forward and reverse DNS for sending IPs, TLS for transmission, and messages that conform to RFC 5322 formatting.

A third major consumer provider announced comparable requirements taking effect during 2025. Treat the direction of travel as settled: authentication plus alignment plus a low complaint rate is now the entry ticket rather than an optimisation.

## Tier 1. The list is the defect

**Spam traps, and why the two kinds behave differently.** A **pristine trap** is an address that has never belonged to a person and has never opted in to anything. It is published where only an automated harvester would find it. Hitting one is close to proof that you scraped or bought addresses, and it can produce an immediate blocklisting rather than a gradual reputation decline. A **recycled trap** is an address that did belong to a person, was abandoned, bounced for a defined period, and was then reactivated by the provider specifically to catch senders who never clean their lists. Hitting one implies neglect rather than theft, and is treated more leniently, but it is still a direct statement to the receiver that you do not process bounces. The practical difference: recycled traps are cured by hygiene, pristine traps are cured only by never acquiring the address in the first place.

**Bounce handling.** A hard bounce is a permanent 5xx failure, meaning the mailbox or the domain does not exist. Suppress the address immediately, permanently, and never retry it. A soft bounce is a temporary 4xx, meaning full mailbox, greylisting or rate limiting. Retry with backoff and suppress after a defined run of consecutive failures, commonly three to five over several days. Keep the hard bounce rate under 2 percent, and treat anything above that as a list problem rather than a sending problem. Many sending platforms suspend accounts above that line automatically.

**A purchased list is a technical problem, not only a legal one.** The addresses were harvested, so pristine traps are close to certain. There is no engagement history, so the first send is a large volume of mail to strangers with no prior positive signal. Complaint rates on such sends routinely exceed the 0.3 percent threshold by an order of magnitude. And the fingerprint of the send, a new sending identity, a sudden volume, an unengaged audience, is precisely the pattern the filters are trained on. One send can move a domain from good standing to filtered, and the recovery is measured in weeks of clean sending, not in an appeal.

**Re-engagement before suppression.** Engagement is the strongest single input to modern filtering, and continuing to mail people who never open teaches the filter that your mail is unwanted by your own audience. Define a dormancy window appropriate to your sending frequency, commonly 90 to 180 days with no open or click. Send a short re-engagement series of two or three messages that asks a direct question and makes staying subscribed an active choice. Then suppress everyone who did not respond. Suppressing them is not a loss. They were already a cost.

## Tier 2. The sending pattern is the defect

**A new domain and a new IP are different problems.** A new IP has no reputation, and receivers rate limit unknown IPs by default, so the fix is volume applied gradually. A new domain is worse than neutral, because the overwhelming majority of newly registered domains that start sending are sending something nobody asked for, and some filters apply additional scrutiny to domains registered in the last month. Let a domain exist, resolve, and send small amounts of real mail for a few weeks before you point a campaign at it.

**Ramp shape.** Start low, in the tens per day per IP, and roughly double every day or two as long as bounces and complaints stay clean. Four to eight weeks to full volume is a normal shape. Send to your most engaged recipients first, because their opens and replies are the positive signal the ramp exists to generate. If complaints rise, hold volume flat rather than continuing to climb.

**The spike is the signal that hurts.** Filters model an expected volume for a sending identity. Zero for three weeks followed by fifty thousand in an hour is anomalous regardless of how good the content is, and the response is throttling or bulk foldering. Consistency beats volume. A sender who mails ten thousand a week every week is in a better position than one who mails forty thousand once a month, with identical annual totals.

Avoid reciprocal warm-up networks, where accounts open and reply to each other's mail to manufacture engagement. The engagement comes from an audience that looks nothing like yours, it is a recognisable pattern, and it teaches you nothing about whether real people want your mail.

## Tier 3. Reputation and infrastructure

**Domain reputation versus IP reputation.** IP reputation attaches to the sending host and can be escaped by moving. Domain reputation attaches to the domain in the From header and the DKIM `d=` domain, travels with you wherever you send from, and is the dominant signal at the large consumer providers. You cannot outrun it by changing platforms.

**Shared versus dedicated IP, with an honest rule.** A dedicated IP gives you a reputation nobody else can damage, and requires enough consistent volume to establish and maintain one. A shared IP pool means inheriting an established warm reputation, at the price of sharing it with the pool's worst tenant.

- Under roughly 5,000 messages a month, or sending less often than weekly: **shared**. A dedicated IP at that volume never accumulates enough signal to be trusted and looks dormant between sends.
- Above roughly 100,000 messages a month on a steady weekly or better cadence: **dedicated**, and warm it properly.
- In between, or if your volume is seasonal: **shared**, and revisit when the volume is steady rather than when it is large.
- If you cannot get a reliable monthly figure: **stay shared**. The failure mode of a premature dedicated IP is worse than the failure mode of a slightly late one, because it is invisible for months.

**Subdomain separation.** Send transactional mail, meaning receipts, password resets and alerts, from one subdomain, and marketing mail from another, each with its own DKIM selector. The reason is blast radius: when a campaign draws complaints, the damage lands on the marketing subdomain rather than on the stream that has to work. Be honest about the limit of this, though. Reputation aggregates at the organisational domain too, so separation reduces contagion rather than eliminating it. Do not send bulk mail from the root domain, because that is the one you cannot afford to replace.

**Why one bad campaign moves everything.** Reputation systems weight recent behaviour heavily and decay slowly. A single send to a stale segment produces a complaint spike, the spike moves the domain's grade, and the next campaign, which is fine, is filtered because of the last one. There is no reset. The only recovery is a period of low volume mail to your most engaged recipients until the average recovers.

## Tier 4. Content, ranked honestly

Content-level factors matter, and they matter far less than everything above. A skill that opens with a list of forbidden words has its priorities inverted, and the words themselves are close to irrelevant at the large providers, which have not relied on naive keyword matching for a very long time. The content factors that are real are structural rather than lexical:

- **Image-only messages.** A message that is one large image has no text to classify, and images are frequently not loaded for an unknown sender, so what arrives is a blank rectangle. This is also the classic construction for evading text filters, which is why it is treated as suspicious.
- **Link shorteners.** A shortened link carries the shared reputation of everyone else using that shortener, including the abusive ones, and hides the destination from filters that would otherwise assess it. Use your own domain if you need short links.
- **Mismatched link domains.** Display text saying one domain with an `href` pointing at another is the defining fingerprint of phishing. Tracking redirects through a platform domain are a mild version of the same thing and are tolerated because they are ubiquitous, but a redirect chain through three unrelated hosts is not.
- **No plain text alternative.** Send `multipart/alternative` with a real text part, not an empty one and not a machine-stripped one. An HTML-only message is a small negative signal and an accessibility failure.
- **Link destination reputation.** A link to a domain that is itself blocklisted will bulk an otherwise perfect message. Check where you are pointing, particularly for user-supplied or partner links.
- **Size.** Very large HTML gets clipped by some clients, which hides the unsubscribe link at the bottom, which raises complaints. Keep the body small.

**BIMI belongs here rather than at the top.** BIMI displays your logo beside the message in supporting clients. It requires DMARC at enforcement, meaning quarantine or reject rather than none, an SVG logo hosted over HTTPS in the required profile, and for the major supporting inboxes a Verified Mark Certificate or Common Mark Certificate from an approved authority, which is a recurring annual cost in the high hundreds to low thousands of US dollars. It is a reward for having finished the work below it, not a lever that improves placement. Anyone selling BIMI as a deliverability fix has the causation backwards.

## Tier 5. Measurement

If you are not reading aggregate DMARC reports and the provider postmaster tools, you will discover your next problem from a colleague asking why they stopped getting the newsletter. Set up the `rua` destination, verify your domain in the provider postmaster consoles, and check the complaint rate against the 0.3 percent threshold weekly rather than after a campaign. Keep a seed set of real accounts at the major consumer providers and look at where your own mail lands, since placement is not visible in any sending platform's dashboard.

## The decision rule for a suspected placement problem

- **Authentication or alignment fails.** Fix it, change nothing else, wait a full sending cycle. Everything downstream is unmeasurable until this is clean.
- **Authentication is clean and the complaint rate is above 0.3 percent.** The list is the defect. Suppress dormant segments and cut volume before touching creative.
- **Authentication is clean, complaints are low, and one provider is filtering while others are not.** It is a reputation problem specific to that provider. Read their postmaster tooling and reduce volume to that provider while sending only to engaged recipients there.
- **Authentication is clean, complaints are low, and everything is filtered everywhere.** Check blocklist status and check link destinations, then look at the sending pattern for a recent spike.
- **You cannot tell, because you have no complaint data and no aggregate reports.** Stop diagnosing. Instrument first: `rua` destination, postmaster verification, seed accounts. Guessing at deliverability without data produces confident changes that cannot be evaluated, and the most common outcome is three simultaneous changes and no idea which one helped.

## Worked example, compressed

A software company with about 40,000 recipients moved from one sending platform to another six weeks ago. Open rates halved at one consumer provider and are unchanged elsewhere. The team has rewritten the subject lines twice.

**Tier 0, fatal.** The From header is `updates@examplecompany.com`. The envelope sender, read from a received message rather than from a record, is `bounces@mail-out.platformdomain.example`. SPF passes for that domain and is not aligned. DKIM is signed with `d=platformdomain.example`, passes, and is not aligned. The DMARC record reads `v=DMARC1; p=quarantine; rua=` with an address at a domain that no longer exists, so no reports have been received or noticed for a year. The result is that every message fails DMARC and is quarantined by the provider that enforces the policy most strictly. Separately, the SPF record now resolves to eleven lookups after the new platform's include was added alongside the old platform's, which was never removed, so SPF returns permerror on top of everything else.

**The fix, in order.** Delete the old platform's include, taking the chain to eight. Configure a custom return path on a subdomain, so the envelope sender becomes a subdomain of the company domain and SPF aligns under relaxed alignment. Configure DKIM signing with `d=examplecompany.com` on a new selector, with a 2048 bit key, so DKIM aligns as well and survives forwarding. Repair the `rua` address and read reports for two weeks before touching the policy.

**Tier 1.** 6 percent of the list has not opened in two years and has never been suppressed. Nothing about that is fatal on its own, and it is the reason the complaint rate is 0.24 percent, uncomfortably close to the threshold.

**Tier 2.** The migration sent full volume from the new platform on day one with no ramp, which is the spike shape described above.

**Tier 3.** Content is fine. The subject line rewrites addressed nothing.

**Verdict: hold.** Do not send another campaign until alignment is fixed, because every message sent in the meantime adds a failing authentication result to the domain's record. The subject lines were never the problem, and the two rewrites cost six weeks.

## Failure modes

**Reading records and calling it an audit.** Every record can be individually valid while the system fails. Alignment is only visible in a real message, so fetch one and read its `Authentication-Results` and `Return-Path` headers.

**Counting the SPF lookups you can see.** The record shows four includes. The evaluation performs eleven queries. Resolve the whole chain.

**Publishing p=reject on day one.** It works immediately, in the sense that legitimate mail from systems you had forgotten stops being delivered immediately.

**Treating a soft bounce like a hard bounce, or the reverse.** Suppressing on a full mailbox loses real subscribers. Retrying a nonexistent mailbox for weeks is a direct signal to the receiver that you do not process failures.

**Blaming content.** The subject line is where teams look because it is the part they control without asking anyone. It is very rarely the cause, and the time spent rewriting it is time the domain spends still broken.

**Assuming the sending platform handles it.** Platforms handle their own authentication. Aligning that authentication with your domain is a configuration step somebody has to perform, and the default on most platforms is not aligned.

**Buying a dedicated IP because it sounds professional.** Below the volume needed to sustain a reputation, a dedicated IP is worse than a shared pool, and the damage is invisible for months.

**Fixing several things at once during an incident.** Alignment, list suppression and a volume cut in the same week means you learn nothing about which of them mattered, and you will face this again.

## What this skill does not do

- It does not check where your mail landed. Inbox placement requires seed accounts or a placement testing service, and no amount of record reading substitutes for looking.
- It cannot read your aggregate DMARC reports, your postmaster console or your bounce logs. It will tell you what to look for in them and cannot look for you.
- It does not assess consent or legality. Whether you were permitted to contact these people is a separate question with its own jurisdictional answers, and a technically perfect setup does not make an unlawful send lawful.
- It does not know your volume, your provider mix or your complaint rate, so every volume-dependent branch needs a number you supply.
- Provider requirements and thresholds move. The figures here are dated to February 2024 and the specifications to their published versions, and both should be checked against current documentation before you rely on them.
- It will not recover a burned domain. Reputation recovery is a slow process of consistent, engaged sending, and no audit shortens it.
