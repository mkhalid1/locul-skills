---
name: release-announcement-set
description: Writes the four pieces of copy that announce one product release: the customer email, the in-app note, the changelog entry and the short social post. Covers a significance rating that decides which surfaces get used at all, three email archetypes with a selection rule, a fixed slot order in which benefit reinforcement comes after the call to action, a banned-opener list, and the four constraints that make each surface different from the other three. This skill should be used when a feature, fix, integration, partnership or version is about to ship and the announcement copy has to be written.
---

# Release announcement set

## The claim this skill is built on

One release, four surfaces, four different readers. The obvious approach is to write the email properly and then cut it down three times, and it fails in a specific way: the shortened versions inherit the email's assumptions, and every one of those assumptions is false somewhere else. The email assumes the reader is a customer. The changelog reader may not be. The email assumes the reader remembers how the thing worked last month. The social reader has never seen it work at all. The email assumes the reader can reply. On three of the four surfaces, replying is not a thing that exists.

So these are not four lengths of one message. They are four messages that share a name and a claim and differ on everything that depends on who is reading and why. The four cross-surface constraints below are the whole reason this is one skill rather than four.

There is a second claim, smaller and used first: rate the release before writing anything. Significance chooses the email archetype, and it chooses which surfaces are used at all. A minor fix does not earn an email.

## Step 1. Rate the significance, before writing a word

Three tiers, rated against what the release does to a reader, never against the size of the change.

**Tier 1, minor.** A bug fix, a small improvement, a new setting, a performance change nobody asked about. Test: a customer who never hears about this will not behave differently. Surfaces: the changelog, and nothing else. Add an in-app note only if the change alters something visible that a customer might otherwise report as a fault.

**Tier 2, notable.** A new feature, an integration, a partnership, a certification, a raised limit. Test: some customers will do something they could not do before, or will stop asking for something. Surfaces: changelog, in-app note, email, plus a social post if the news means anything to someone who is not a customer.

**Tier 3, significant.** A release that changes what the product is for, replaces a default behaviour on every existing account, or changes what customers pay and how they are billed. Test: it changes the one-sentence answer to what this product does. Surfaces: all four, and the email is the founder letter.

**The version number does not rate the release.** Under semantic versioning, a major bump means an incompatible interface change and a patch means a backwards-compatible fix. Neither statement is about whether a customer cares. A major version can be tier 1 to everybody who is not an integrator, and a patch that stops silent data loss is tier 3. Rate the release, then number it, and never let the number choose the announcement.

**The decision rule, and its cannot-tell branch.** Ask the three tier-3 questions. Does it replace a default behaviour on existing accounts? Does it change money? Does it change the one-sentence description of the product? Three yeses is tier 3. Zero or one yes is tier 2, or tier 1 if nobody has to do anything. **Two yeses is the case where you cannot tell, and the rule is to announce it as tier 2 with the plain announcement.** The costs are asymmetric. Announcing a tier-2 release as tier 3 spends a founder letter on something that did not need one, and every founder letter after it is read as marketing, which is a price you pay later and never see itemised. Announcing a tier-3 release as tier 2 costs attention once and is recoverable: a letter a week later reads as considered rather than as noise.

**Does it earn an email at all.** Send only if one of these is true: the reader has to do something, the reader will be confused when they discover it alone, or the reader asked for it. Otherwise the changelog is the entire announcement. A list does not regenerate, and every send spends a little of it.

## Step 2. Pick the email archetype

Three archetypes. The selection rule is keyed to the significance tier and to where the news came from.

**The plain announcement, for a minor feature or an integration.** Short, direct, no story. 120 to 180 words. One thing changed, here is where it is, here is who it helps. Nothing is being built up to. The failure to avoid is decorating a small piece of news until it looks like it is pretending to be bigger, which readers notice immediately.

**The credibility-led announcement, for a partnership, a certification or any third-party approval.** It opens on the external validation, not on your feelings about it, and it attaches a ready-made resource. This one has a different job from the other two: the reader's actual task with this news is to forward it to somebody internally who wanted the assurance, usually in security, legal or procurement. So give them the object to forward. A report, a certificate, a one-page summary, a documentation page written for a reader who is not your user. An audit result with no attached artefact makes the reader do the packaging, and they will not.

**The founder letter, for a significant release.** 250 to 400 words. It opens on a customer conversation rendered as dialogue and it closes with a named human. It is the only archetype that may contain a forward-looking sentence, and that sentence may not contain a date.

**If you cannot tell between plain and credibility-led,** ask who the reader forwards it to. If the answer is a colleague who has to approve something, it is credibility-led. If the answer is nobody, it is plain.

## Step 3. The dialogue device inside the founder letter

This is the strongest single device here and it is easy to get almost right. A customer asks for something. The founder answers with a larger version of it. The release is that answer.

Two speakers, no narration. The construction that gets replaced is the wrapper: "one of our customers reached out last month and said". That sentence turns a story into a report about a story. Cut it and let the two lines stand.

    "Can you make the reminder go out the night before instead of the morning of?"

    "What if it went out whenever that person is most likely to read it?"

Four rules that decide whether the device works.

1. **The ask must be smaller than the answer.** If the two are the same size, you have a completed feature request, not a story, and the dialogue adds nothing.
2. **One exchange.** Two exchanges is a transcript, and a transcript needs narration, which is what you were removing.
3. **The customer is described by role, never named,** and no quotation is attributed to a real identifiable person unless they wrote it and agreed to it in that form. An invented quote with a real name attached is the one mistake here with legal weight.
4. **The answer is the release.** If the paragraph after the dialogue introduces something other than the thing the second line describes, the device has decorated the letter instead of structuring it.

The close is the other half: a first name and a role, not "The Team". The letter's entire premise is that a person is talking, and an anonymous sign-off retracts it in the last two words.

## Step 4. Banned openers

Four phrases, banned outright: **"thrilled to announce", "excited to share", "introducing", "big news".**

The ban carries information precisely because these are the four that get reached for by default, and the default is what makes every announcement email sound like every other one. They also share a structural defect: each spends the first sentence on the sender's emotional state, which is the one piece of information the reader did not need.

The near misses fail the same test: "delighted to announce", "we are pleased to", "say hello to", "meet the new", "the wait is over", "we have been busy", "we heard you". Swapping one banned phrase for its synonym is not a rewrite.

**The replacement rule.** The first sentence names one of two things: who asked for this, or what is now possible. Then the test: could this first sentence appear at the top of any other company's announcement? If yes, it is not carrying news and it goes.

## Step 5. The fixed slot order

Seven slots, in this order, for all three email archetypes.

1. **Greeting.**
2. **Context hook.** Why this matters, or who asked for it. In the founder letter this slot is the dialogue.
3. **The announcement, stated plainly.** What it is, in one or two sentences, with no build-up left in front of it.
4. **The call to action.** One action, one destination, one link. A second link competing with it converts the choice from do-or-not into which-one, and which-one loses.
5. **The support offer.** A sentence inviting a reply.
6. **Benefit reinforcement.** What it means for them, after the call to action.
7. **Sign-off.**

**Why slot 6 sits after slot 4, which is the counterintuitive part.** The call to action is the exit. Everyone who was going to click has left the page by then. That means benefits placed before the call to action are read almost entirely by people who had already decided to click, where they change nothing, while pushing the news itself further down. Placed after, they are aimed at exactly the reader who reached the link and did not take it, and that reader is the only one still available to persuade. It also produces a better email mechanically, because the news lands in the first third instead of the middle.

**The support offer is not a second call to action.** It is a lower-cost alternative to leaving. "Reply to this email and I will send you the steps" beats "contact support" because it names the cheapest possible next action, and the confused reader either replies or churns quietly. It only works if the sending address accepts replies and a person reads them within a day. On a no-reply address, the offer is a lie and readers find out.

## Step 6. Format constraints for the email

**One sentence per line, with a blank line between.** It survives a phone screen, and while drafting it makes the slot order visible: you can see at a glance whether the benefits crept above the link.

**No bullets and no numbered lists in the body.** A bulleted feature list changes the reading posture from message-from-a-person to specification, and the reader starts skimming for the row that concerns them. The bullets belong in the changelog entry, which is the surface built for scanning.

**The subject line states the feature plainly.** Not curiosity, not a question, not a number. Aim under 45 characters, because mobile clients truncate subjects at varying widths and roughly 35 to 40 characters is what reliably survives in portrait. If the feature has a name, the name goes in the subject.

**Preheader text does not repeat the subject.** It is the second half of the sentence, not an echo of the first.

**Length caps:** plain 120 to 180 words, credibility-led 150 to 220, founder letter 250 to 400. Past roughly 450 words the reader is scrolling to find the link, which means the slot order has stopped operating.

## Step 7. The anti-polish rule

Slightly informal grammar is acceptable and often better. A sentence that starts with And. A contraction. A fragment. A word that a style guide would flag and a person would say.

Over-editing is a real failure mode and not a small one: the more passes a founder letter takes, the more it converges on the same neutral corporate register as every other announcement, at which point the archetype has been undone. The tell is that no sentence could only have been written by the person signing it.

The rule permits looseness of register. It does not permit sloppiness of fact. Not covered by anti-polish: a wrong link, a wrong price, a wrong name, an unclear instruction, a broken claim, or a typo in the subject line, which is the one place a mistake is visible before the reader has decided to open.

The practical check is to read it aloud. Anything you would not say to a customer standing in front of you is either over-polished or over-written, and both are fixed by cutting.

## Step 8. The four cross-surface constraints

The reader's posture on each surface, which is where every constraint below comes from.

| Surface | Who is reading | Posture | How they arrived | Reply path |
| --- | --- | --- | --- | --- |
| Email | An existing customer | Receiving, uninvited | You sent it | Yes |
| In-app note | An existing customer | Mid-task, product on screen | You interrupted them | No |
| Changelog | Anyone, including future readers | Searching | They came looking, often months later | No |
| Social post | Mostly non-customers | Scrolling | An algorithm | No, not usefully |

**Constraint 1. The changelog states the old behaviour as well as the new.**

It is the only one of the four written for somebody searching rather than receiving, and what they search for is the old behaviour, under its old name, described as a symptom. "Exports now run in the background" is invisible to the person typing "export times out on large date ranges". So each entry carries four things: the area, what it used to do, what it does now, and what the reader must do, including the words "no action needed" when that is the answer.

Two consequences. The entry must contain the vocabulary of the problem, not only the vocabulary of the feature, because the problem words are the search terms. And the changelog is the only surface read out of order and long after publication, so anything whose meaning depends on the reading moment breaks: no "recently", no "as of today", no "in this week's release", no "as mentioned above". Date and version every entry, and keep "now" only in a sentence sitting under a printed date.

**Constraint 2. The in-app note must not describe what the reader can already see.**

It is the only surface with the product live behind it. If the note says there is a new Export button in the toolbar while the button sits three centimetres away, the note has spent an interruption narrating the screen.

Its job is the one thing that is not on screen, and there are usually only three candidates: what this replaces, what it costs, and where the setting lives if the setting is not visible from here. One to three sentences, one action, dismissible, never blocking, and it must not fire on a screen where the thing it describes does not exist.

If the note names a keyboard shortcut, name both, since the modifier differs on Windows and Mac, and a note that names only one is wrong for half the readers who see it.

**Constraint 3. The social post cannot assume the prior state.**

It is the only surface read by people who have never used the product, and every reference to what it used to do is noise to them. Either explain the prior state inside the same sentence, framed as the general problem rather than as your product's old behaviour, or drop the comparison and state the capability flat.

The corollary: the social post is the only surface that has to name the product and say what kind of thing it is in the same breath, because the other three are read by people who already know. The first line has to work before the truncation point, which on most feeds arrives somewhere between 140 and 220 characters, and exact platform limits belong to a platform-specific check rather than to this one.

**Constraint 4. The email is the only surface that can carry a support offer.**

Because it is the only one with a reply path. A support offer is an invitation to reply, and an invitation to reply on a surface with no reply is worse than nothing: it tells the reader you have not thought about where they are standing. On the changelog and the in-app note, the equivalent is a link to the actual support channel. On social, there is no equivalent, and asking people to raise support issues in public replies is a decision about your support model, not about copy.

## Step 9. The consistency check across all four

Run this after all four exist and before anything ships.

- **One name for the thing.** Same words, same capitalisation, on all four surfaces. The classic break is the changelog using the internal name and the email using the marketing name, which makes them unlinkable for the reader who sees both.
- **One claim.** If the email says it is faster and the changelog says it is more reliable, the reader learns nothing except that two people wrote them.
- **No surface promises what another withholds.** The specific case to check: the social post describing a capability that the in-app note gates behind a plan.
- **One destination** for the learn-more path, so all four point at the same page.
- **Publication order: changelog, in-app note, email, social.** This ordering is operational rather than editorial. The changelog goes first because support will be asked before the email lands and needs something to link to. The email goes after the in-app note so that a reader who clicks through finds the product already saying the same thing. Social goes last because it draws in strangers, and a stranger who arrives before the page is right is the most expensive reader in the set.

## Worked example, compressed

A subscription billing service. The release: failed card payments used to be retried on a fixed schedule of day 1, day 3 and day 5, then the subscription cancelled. They are now retried when the card is most likely to authorise, and the customer's failure notice names the specific card. Everything in this example is invented.

**Rating.** Does it replace a default behaviour on existing accounts? Yes, every account. Does it change money? Yes, directly. Does it change the one-sentence description of the product? Yes: it is now a billing service that recovers payments, not one that reports failures. Three yeses, so tier 3, so all four surfaces and a founder letter.

The counterfactual, for the branch. Had the same work shipped as a merchant-facing dashboard showing recovery rates, with retry timing unchanged, it would answer yes once. That is one yes, tier 2, plain announcement, no social post, and no letter.

**Naming.** Engineering calls it smart retries. Marketing has written Payment Recovery. Pick one before writing: smart retries, lowercase, on all four.

**Email, founder letter, slots in brackets.**

> Subject: Smart retries for failed payments
>
> Hi [first name], [greeting]
>
> "Can you retry failed payments on day 7 as well as day 5?" [context hook, dialogue]
>
> "What if we stopped guessing the day?"
>
> That was a finance lead at a subscription business in March, and it is the reason for this release. Failed payments are now retried when the card is most likely to authorise, instead of on a fixed three-attempt schedule, and the notice your customer receives names the card that failed. [announcement]
>
> Your existing retry settings have been migrated. There is nothing to switch on. [announcement, second half]
>
> Open the billing settings to see the new schedule for your account. [call to action]
>
> If anything looks wrong there, reply to this message and I will look at your account myself. [support offer]
>
> The reason we did it this way: a card that fails on a Tuesday morning is often a card that works on a Friday, and a fixed schedule cannot see the difference. Fewer of your customers should now lose a subscription they meant to keep. [benefit reinforcement, after the call to action]
>
> [first name], [role] [sign-off]

**In-app note.** "Retries no longer run on the fixed day 1, 3 and 5 schedule. Your settings have been migrated and the new schedule is in billing settings." Two sentences, and neither describes the banner, the button or the screen the reader is looking at.

**Changelog entry.**

> **2026-08-19, version 4.7.0. Billing, payment retries.**
> Previously, a failed card payment was retried on days 1, 3 and 5, and the subscription cancelled after the third failure. Retries are now scheduled per card, based on when that card is most likely to authorise, within the same window. The customer notice names the failing card rather than saying "your payment method". No action required, and existing retry settings were migrated.

It names the old behaviour, in the old numbers, so a search for "payment retried on day 3" reaches it.

**Social post.** "Most failed subscription payments are not declines. They are a card that would have worked on a different day. Our billing service now retries each failed payment when that card is most likely to authorise, rather than on a fixed schedule." No prior-state assumption, the category is named, and the problem is stated before the product.

**Consistency check.** One name, smart retries, on all four. One claim, recovery rather than speed. Nothing promised on social that the product gates. One destination, billing settings. Order: changelog, in-app note, email, social.

**Verdict.** The first draft of this set failed four checks. It opened "We are excited to share smart retries", which is a banned opener. It put the whole benefit paragraph above the link, where only the people already clicking would read it. Its changelog entry read "improved payment retry logic", naming neither the old schedule nor the new one, which makes it unfindable by anyone searching the symptom. And the social post said "retries are no longer fixed", which means nothing to somebody who has never used the product. The version above is what passes.

## Failure modes

**The corporate opener.** The first sentence reports the sender's feelings and the news arrives in sentence three. From the outside it is indistinguishable from every other announcement in that inbox, which is the actual damage.

**Benefits above the link.** Everything persuasive sits in the paragraphs the persuadable reader never reaches, and the news is pushed halfway down. The tell is a link that appears after the 200-word mark.

**The spec sheet.** Bullets in the email body. The reader stops reading it as a message and starts scanning it as documentation, and the founder letter in particular collapses on contact with a bulleted list.

**Archetype mismatch.** A founder letter for a two-week integration. It spends credibility on something that did not need it, and the next letter, for the release that did need it, is read as marketing. Nobody reports this and it does not show in the numbers for that send.

**Over-polished into brand voice.** Every sentence is correct, no sentence could only have been written by the person signing it, and the anonymous register makes the named sign-off read as a device.

**No support offer.** The confused reader has two options, work it out or leave, and leaving is cheaper. The symptom appears later as quiet non-renewal rather than as a support ticket, which is why nobody attributes it to the email.

**Changelog amnesia.** The entry names only the new behaviour. Someone searching the old behaviour by its old name never finds it, concludes the change is undocumented, and opens a ticket describing a bug that is actually a documented change.

**Prior-state assumption on social.** "No more waiting for the day 3 retry" is meaningless to a reader who has never seen day 3, and it reads as an in-joke for people already inside the product, which is the opposite of what the surface is for.

**The in-app note that narrates the screen.** It describes the button the reader can see, uses up the one interruption you get, and teaches people to dismiss your notes unread.

**Name drift.** The internal name in the changelog, the marketing name in the email, a third phrasing on social. The reader who sees two of them cannot tell whether they are looking at one release or two.

**Simultaneous publication.** All four go out at once, the email arrives before the changelog entry is live, and support spends the first morning answering questions with no page to link to.

## What this skill does not do

- It does not decide whether the release should ship or when. Sequencing, readiness and dependencies belong to launch planning, and a well-written announcement for a release that is not ready is a faster route to the same problem.
- It cannot see your list, your segments or your feature flags. It does not know who already has the thing, who is on a plan that excludes it, or who unsubscribed, and it will happily write an announcement to people who cannot use it.
- It is not a legal review. Whether the email is commercial, and therefore needs an unsubscribe mechanism, an accurate sender identity and a postal address, depends on where your recipients are, and the answer differs between the United States and the United Kingdom and the European Union.
- It does not handle breaking changes. Where customers must migrate, the announcement is the smallest part: the deprecation window, the dated timeline and the migration path carry the weight, and none of those are copy decisions.
- It does not localise. The anti-polish rule, the dialogue device and the informal register all behave differently once translated, and a translator will find problems no rule here anticipates.
- It measures nothing. Whether the email was opened, whether the note was dismissed, whether anyone reached the changelog entry: all four surfaces have their own analytics and this file has no access to any of them.
