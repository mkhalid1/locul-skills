---
name: meta-ads-copy-generator
description: Produces a pastable Meta ad copy set from a product, an offer, a funnel stage and an audience: primary text and headline per variant, three variants pinned to three awareness levels, and a creative direction brief. It writes only to the fields each placement actually renders, builds the first line to stand alone because Meta publishes no truncation point, and runs a policy pre-flight against Meta's personal attributes and health claim standards. This skill should be used when Meta ad copy is being written or refitted to a changed placement set, or when a draft carries an unsourced claim or addresses the reader as a member of a category the policy protects.
---

# Meta ads copy generator

## The claim this skill is built on

A Meta copy set is a placement problem before it is a writing problem. The fields you write into are not fixed: they differ by placement, one of the most used placements renders only one of them, and Meta's recommended primary text lengths vary by more than a factor of three across the set. Meta publishes no maximum and no truncation point for any of them.

So the familiar brief, "write three Facebook ads, 125 characters of primary text, a 40 character headline and a description", describes a spec sheet that does not exist. It produces a description for a placement that no longer lists one, a headline for a placement that renders none, and a first line written to a truncation figure Meta has never published. The second failure is quieter: where the audience is defined by a health condition, a financial situation, an age band or a record, the most natural register in direct response is exactly what Meta's personal attributes policy prohibits.

Order matters, for a mechanical reason. Meta treats an edited ad as a new ad and reviews it again, and a creative change is a significant edit that also resets the learning phase. A policy failure caught before launch costs a rewrite; the same failure caught after launch costs the read window the budget was sized for.

## Part one. Inventory the fields the placements actually render

Every figure below is Meta's own Text Recommendations, read on 31 August 2026. Meta's help pages carry no last-updated stamp, so that date is the whole warranty.

| Placement and format | Primary text | Headline | Description |
| --- | --- | --- | --- |
| Facebook Feed, image | 50-150 characters | 27 characters | not listed |
| Instagram Feed, image | 125 characters | 40 characters | not listed |
| Facebook Stories, image | 125 characters | 40 characters | not listed |
| Facebook Reels, video | 40 characters | 55 characters | not listed |
| Instagram Reels, video | 44 characters | not listed | not listed |

**These are recommendations, not caps.** Meta files every character count under "Text Recommendations", while hard constraints such as file size and minimum width sit separately under "Technical Requirements". On the same page "maximum" appears for exactly one text item: "Maximum Number of Hashtags: 30". The Marketing API agrees, typing message, name and description as plain strings with no documented limit.

**The description has quietly left the Feed spec.** It still exists as an ad object field in the Marketing API, but it is no longer listed in Text Recommendations on the Feed spec pages.

**Instagram Reels lists primary text only.** No headline, no description, so if it is in the set at least one variant must carry its whole proposition in primary text.

Do the inventory before writing: afterwards is not an edit, it is a rewrite. Advantage+ placements is the default, and Meta describes its reach as Facebook, Messenger, Instagram, WhatsApp, Audience Network and Threads, so the whole table applies and surfaces beyond it carry their own pages. Writing a short form of the first line to the shortest recommendation in your set is this file's derivation from the table rather than a Meta instruction, and it makes the short version deliberate instead of whatever the interface cuts.

## Part two. The first line, and the number nobody can look up

"Primary text truncates at 125 characters" is wrong twice. It is wrong about what 125 is: a per-placement recommendation for Instagram Feed and Facebook Stories, not a global rule and not a limit. And no truncation point exists in the documentation at all. Across the five spec pages above, plus the image best practices and safe zone articles, no page states one and none contains the phrase "See more". Where the break falls depends on placement, device width, font scaling and line breaks, likely why there is no number to publish.

**The standalone first line rule.** With no figure to write to, this file's own answer is to make the break irrelevant: the first sentence carries the whole proposition, who it is for, what changes, one concrete specific, and reads as complete if everything after it is deleted. That rule is the author's, not Meta's.

**How to find your own break.** Open the placement preview for each placement on a phone, not a desktop, and write to the shortest visible span. Re-check when the placement set changes, when line breaks are added and when the copy is translated. Record the span with the date, and never pass it on as a truncation figure: it is an observation of an interface, not a documented value, and it drifts.

## Part three. The hook taxonomy

Six types, numbered so the later parts can refer to them. This is ordinary direct-response craft, not anything Meta documents; what matters is the tag, because **claim-bearing** hooks are what the Part five pre-flight blocks most often.

1. **Named cost**, claim-bearing. "Four unbilled hours a week is a full day of margin."
2. **Mechanism.** "The blade spins in both directions, so wet grass gets cut instead of flattened."
3. **False belief.** "Renewal quotes are priced for people who never switch." Highest policy exposure: it slides into second person by itself.
4. **Specimen**, claim-bearing. "One shared board replaced eleven status meetings at a forty-person agency."
5. **Comparison**, against the behaviour the product replaces, never a named competitor. "Same twenty minutes as ordering a takeaway, minus the delivery fee."
6. **Direct offer.** "Two months free on annual, cancel any time, no card for the trial."

Cap a set of three at one claim-bearing hook, so a policy block costs one variant rather than the whole set. That cap is this file's own rule, not a Meta one.

## Part four. Pin each variant to an awareness level, and the branch when you cannot tell

Three variants is this file's default, not a Meta rule: the ad set has one budget that has to produce a readable result, and that arithmetic is the ad set's job. The five levels of customer awareness, unaware, problem aware, solution aware, product aware and most aware, are a framework published by the copywriter Eugene Schwartz: well known in the trade, but general marketing knowledge rather than anything checked against a source here.

Meta publishes nothing about awareness levels or hook types, so the banding below is this file's editorial convention. It exists to stop three variants arguing the same way, and it claims nothing about performance.

- **Cold prospecting.** Unaware, problem aware, solution aware. Hooks 3, 1 and 2.
- **Mid funnel.** Problem aware, solution aware, product aware. Hooks 1, 4 and 5.
- **Retargeting.** Solution aware, product aware, most aware. Hooks 4, 5 and 6.

**Which variant leads.** The coldest level in the band. The reasoning is the author's: on the default placement set the ad reaches people colder than the brief describes, and a most aware line shown to a stranger has nothing to attach to.

**The decision rule for the claim-led variant.**

- **Substantiation is supplied**, a figure with a source, a named case or a documented result that can go on file before launch: write the claim-led variant and attach the source.
- **The offer is explicitly unsubstantiated**, the requester says there is no evidence yet: use the mechanism hook, which claims process rather than outcome.
- **You cannot tell**, the common case, because the brief supplies a number with no provenance, "3x faster", "saves 10 hours a month": treat it as unsubstantiated, which is this file's own default rather than a Meta requirement. Write the mechanism variant and return the claim slot as a blocked item naming exactly what would unblock it. Do not soften it to "up to", which keeps the claim and removes your ability to defend it.

The branch earns its keep in health, beauty and finance, where Meta prohibits "promises of specific outcomes within a set timeframe without disclaimers or qualifiers" and the permitted route in the same policy is to "clearly indicate the time taken to achieve noticeable results" (page dated 22 July 2026). One constraint not to invent: Meta publishes no general rule about superlatives, and advice that it bans "best" or "#1" is not traceable to a Meta page.

## Part five. The policy pre-flight, over every variant, before anything ships

Meta's personal attributes policy, verbatim: "Ads must not contain content that asserts or implies personal attributes. This includes direct or indirect assertions or implications about a person's race, ethnicity, religion, beliefs, age, sexual orientation or practices, gender identity, disability, physical or mental health (including medical conditions), vulnerable financial status, voting status, membership in a trade union, criminal record, or name." The page is dated 26 June 2024.

The test is narrower than "avoid second person". Run every line against one question: **does it assert or imply that this reader has one of the attributes in that list?** If not, second person is fine and "Are you a plumber?" breaks nothing. If so, the line fails however well it reads, and Meta's paired examples show the fix: describing an audience in the third person is allowed, addressing the reader as a member of the category is not. The violation is usually one word or a question mark.

| Surface pattern | Meta's published rejection | Meta's published equivalent that is allowed |
| --- | --- | --- |
| Second-person question about a listed attribute | "Do you have diabetes?", "Are you a convicted felon?" | "Depression counseling", "Services to clean up any previous offenses" |
| The word "other" in front of a category | "Meet other black singles near you!", "Meet other seniors" | "Find black singles today.", "Meet seniors" |
| Implied knowledge of a record | "Records show that your voter registration is incomplete" | "Learn about voter registration" |
| The reader's name or ID | "What is your driver's license number?" | "We print customizable t-shirts with your name." |

Note the third row: no question and no "other", so a pattern match is a first sweep, not the test.

Two further passes over the same drafts. **Body image**: prohibited content includes "statements of inferiority about physical appearance" and content "implying or attempting to generate negative self-perception in order to promote diet, weight loss or other health related products", which is what a pain-and-tension hook breaks. **Claims**: before-and-after imagery is not banned outright, since cosmetic transformations are permitted when targeting adults 18 and over. What is policed is the claim structure around them.

Review is broader than the words, covering "images, video, text and targeting information, as well as an ad's associated landing page or other destinations", and it is not one-shot: "ads may be reviewed again, including after they are live".

## Part six. Declare what the automation will do to your words

Advantage+ creative is not a formatting layer. In Meta's words: "The media and text you upload may be adjusted to help improve ad performance while maintaining the core message of your campaign." Three enhancements are documented in the Marketing API as opt-in by default, so anyone who has never opened the Enhancements panel is running description automation, adapt to placement and relevant comments. Three more rewrite the words:

- **Text generation** can produce "up to 5 variations of primary text and headline".
- **Text improvements** takes "keywords and phrases from your original ad copy" and displays them "directly or adjusted" as overlays, footers and headlines, so body copy can end up on the image.
- **Dynamic creative**, where still offered, documents "Swapping text between fields, such as primary text and headline", so a headline can deliver as body copy.

Dynamic creative was withdrawn from the sales and app promotion objectives as of June 2024, and its documented replacement, flexible format, is itself going: "Starting in March 2026, the flexible format will no longer be available in Ad setup." Meta's own migration path points at a format it is retiring, so check the interface, not the help page.

The consequence is one rule: **every line must survive being moved.** Write the headline so it reads as a first line and the first line so it reads as a headline, and never split a sentence across the two fields. If a set depends on field order, say so in the brief and name the enhancements to switch off.

## Part seven. The creative direction brief

One per variant, and it is a direction, not an asset.

**Safe zone.** For 9:16 creative in Stories, Reels, Feed and in-stream reels, Meta's placement pages say leave roughly 14 per cent of the top, 35 per cent of the bottom and 6 per cent of each side free of key elements, text and logos. For Reels ads carrying disclaimers the bottom band is 40 per cent, not 35. This is not cosmetic: on screens taller than 9:16 Meta "may either zoom the creative to better fill the whole screen (which can crop areas outside the safe zone), or show the creative in its original 9:16 aspect ratio and use a black background". Ads Manager ships a Safe zone guardrail toggle that draws a yellow overlay: use it rather than judge by eye.

**Text on the image.** The 20 per cent rule is gone and Meta says so: "There is no longer a limit on the amount of text that can exist in your ad image. The text overlay tool is no longer available." No primary source establishes a withdrawal date, so do not quote one. The live constraint is cropping, not proportion.

**Fields to fill per variant.** The frame at second zero. The burned-in caption carrying the first line: Meta's Facebook Reels spec files captions as "Optional, but recommended" and says auto-captioning is not supported there, so a caption nobody burned in may not exist at all. One message only, per Meta's guidance: "Don't communicate too many messages because ads usually only have one call to action." The aspect ratio, and the element that must stay inside the safe zone.

## Worked example, compressed

A subscription bookkeeping service for self-employed tradespeople, cold prospecting, placements on the Advantage+ default. The brief supplies one claim, "saves 10 hours a month", with no source.

**Field inventory.** The default set includes Instagram Reels, so one variant carries everything in primary text, and the shortest recommendation in the set is Facebook Reels at 40 characters. Each variant gets a long first line for Feed and a short form written to 40. No description, because no Feed page lists one.

**Hooks and pins.** Cold, led by the coldest: A is the false belief, B the named cost, which needs the claim, and C the mechanism.

**The claim branch fires.** The 10 hours figure has no provenance, so B becomes a second mechanism hook and the claim slot goes back blocked, naming what would unblock it: client time logs before and after, with the sample size stated.

**Policy pre-flight.** The first draft of A read "Behind on your tax bill and still doing the books at midnight?" The first clause implies vulnerable financial status, which is on the policy list and sits beside Meta's own not-allowed example, "Are you bankrupt? Check out our services." It is rewritten as a third-person statement about how the category works.

**Variant A, as it is handed over.**

- Primary text, long form: "Monthly reconciliation is why your numbers are always six weeks old. Receipts get coded the day they are photographed, so the March figure is a March figure."
- Primary text, short form, 38 characters, for the Reels placements: "Your numbers are always six weeks old."
- Headline, 23 characters, inside the Facebook Feed recommendation of 27: "Books that close weekly"
- Description: none.
- Creative direction: a phone photographing a receipt on a van dashboard at second zero, the burned-in caption carrying the short form, one message, 9:16, caption clear of the bottom 35 per cent.

**Verdict.** Ship false belief, mechanism, mechanism, none of them depending on a headline Instagram Reels will not render, with text generation switched off. The fourth variant everyone wanted, the "10 hours a month" one, is returned blocked.

## Failure modes

**The phantom description.** A description line is written and signed off for a Facebook Feed ad, then never appears, because that placement's Text Recommendations no longer list the field.

**The headline that never renders.** A variant's whole proposition sits in a 40 character headline, and on Instagram Reels what delivers is body copy with the point removed.

**The second-person rejection.** A variant opens "Do you have X?" where X is a condition, a debt or a record, reads well internally, and is rejected.

**The word "other".** One word crosses from describing an audience to asserting this reader belongs to it, invisibly at proofreading speed.

**The unmoored claim.** A figure with no provenance becomes a timeframe promise, the ad stops mid-flight, and the read is spoiled for every variant sharing the budget.

**The unread automation panel.** Nobody opens Enhancements, so the documented defaults run unseen and the set that delivers is not the set approved.

## What this skill does not do

- It cannot see an ad account, so it does not know which placements received delivery, what previous creative earned, or whether anything was rejected.
- It does not choose the objective, the audience or the budget, does not decide when a variant is killed, and does not determine whether an offer falls into a Special Ad Category.
- It does not observe where the "See more" break falls, and it does not produce the image or video. Both are human work: a live preview on a real phone, and a designer or editor shooting, cutting and cropping inside the safe zone.
- It cannot confirm a character recommendation still says what it said on 31 August 2026, because Meta's help pages carry no last-updated stamp. Re-read the page first.
