---
name: product-truth-pack
description: Produces the single source of product truth that every article, landing page and caption must draw from, plus the register of claims that may never be made. Covers the numbered per-feature file layout with its required honest-limits section, the live versus roadmap ledger with dated override headers, the rule that a how-to for an unshipped integration is not publishable and the only writable form is coming soon plus today's workaround, the ban on limitations deduced from silence, and a forbidden-claims register of exact strings scoped to body copy, search engine fields, alt text and any words rendered inside an image, each ban carrying its honest replacement. This skill should be used when briefing writers or an agency on a product they do not use, when a draft describes something the product does not do, when a feature ships or a price changes, or when a content programme has no agreed set of product facts to write from.
---

# Product truth pack

## The claim this skill is built on

Writers do not invent product facts out of mischief. They invent them because they hit a gap mid-sentence and the sentence has to end.

That is the whole mechanism. Somebody is 900 words into an article, they need one clause about how the export works, nobody is online to ask, and the surrounding documentation implies an answer. They write the implied answer. It is fluent, it is plausible, and about one time in five it is wrong. Telling people not to make things up does not address this, because they did not experience it as making something up. They experienced it as reasoning.

So the pack is built to remove the gap rather than to scold the writer. It is a small set of files with a fixed read order, every one of which ends in a section listing what the feature does not do, plus a register of exact strings that may never appear anywhere. The register is the half that people underbuild and it is the half that gets you into trouble, because a claim does not become safe by being short, and alt text is a claim.

## Step 1: the file layout, numbered for read order

Create a `feature-descriptions` folder holding one file per feature, numbered so the order is not a matter of taste:

```
feature-descriptions/
  README.md               the accuracy rules, the register, the ledger
  00-product-overview.md
  01-<core feature>.md
  02-<core feature>.md
  ...
  08-pricing.md
```

Numbering exists because the read order changes what a writer believes. Somebody who reads a feature file before the overview builds their mental model from a detail and then fits everything else around it. The overview is 00 and it is read first, every time. Pricing is last, because pricing is the file most likely to be stale and you want it read with the rest of the picture already in place.

The README carries the accuracy rules, the register and the ledger. Keep them in one file so a writer opening the pack for the first time cannot miss them, and so a single review covers all the rules at once.

## Step 2: the shape of every feature file

Four sections, in this order, and the order is the point.

1. **What it is.** Two or three sentences, and one of them is a quotable reader-facing line: a sentence a writer may paste into an article unedited. Providing it is how you stop nine writers each inventing a slightly different description.
2. **Who it is for.** The situations where this feature is the answer, stated as circumstances rather than adjectives.
3. **What it does.** Documented behaviour only. Every bullet corresponds to something a reader could verify in the product or on the live site.
4. **Honest limits.** What it does not do, what it does badly, what it needs from the user. This section is required. A feature file without it does not get merged.

The honest-limits section is required for one specific reason: it is the section that prevents overclaiming. A writer working from a file that lists only capabilities will describe the feature as unbounded, because nothing in front of them suggests an edge. A writer who can see the edges writes around them without ever needing to ask.

Add a fifth section where it applies. **Open questions**, recording unresolved internal inconsistencies rather than papering over them. If two people in the company describe the same behaviour differently, write both down with their names removed and mark it open. A visible gap is written around. A hidden gap is guessed at.

## Step 3: the five hard rules

These belong in the README, in these terms.

**Rule one, the scope of what may be claimed.** Only claim what these files or the live site confirm. If a feature, integration, limit or price is not documented here or verifiable on the site, do not state it as fact. Flag it and write around it.

**Rule two, the live versus roadmap binary.** Maintain an explicit ledger of what ships today. Every capability is in exactly one of two states, and there is no third state for things that mostly work or are nearly ready.

> Live items may be described as shipping. Roadmap items must never be described as shipping. When in doubt, do not claim it.

Carry a **dated override header** at the top of any file where the owner has corrected a fact, in the form `Override 2026-08-20: <corrected fact>`. The newest dated confirmation wins over older prose further down the same file. This exists because the realistic alternative is rewriting the whole file every time one detail changes, which nobody does, so the file quietly contains two contradictory statements and the writer picks whichever they read first.

**Rule three, integrations split into two lists.** An integration list has an Available now section and a Coming soon section, and the second one carries a target quarter and the instruction not to present its contents as available. Then the consequence that people resist:

A how-to for an integration that has not shipped is not publishable. Not in a hedged form, not with a note at the bottom. If the keyword must be targeted for search reasons, the only writable form is **coming soon plus today's workaround**: state the status and the quarter plainly, then document in full the path that works today. Re-verify the status at write time, because the gap between commissioning and publishing is exactly where a quarter turns over.

**Rule four, internal figures.** Any internal marketing statistic with no methodology behind it must be attributed as the company's own user-reported figure, never as externally verified. Re-verify before relying on a number at all. Most inherited marketing numbers turn out to have no traceable origin, which is a finding in itself and belongs in Open questions.

**Rule five, the ban on deduced limitations.** A writer may not infer a limitation from silence. If the pack does not say whether the product handles a case, that is a gap in the pack, not a fact about the product. Limits come from the file or they do not get written. This rule exists because the failure is invisible: an inferred limitation reads as commendable honesty, sails through review, and can be quoted back at you by a competitor as your own admission.

## Step 4: the forbidden-claims register

Scope it explicitly, in these words, at the top of the register:

> This register applies to the body, every search engine field, every alt text, and any text rendered inside an image.

The image clause is the one that gets missed, and the reason is structural rather than careless. Images are produced last, often by a different tool or a different person, usually from the article's own headline, and they are the single surface nobody rereads. A pricing graphic rendering a retired price is a claim. It gets screenshotted, it circulates, and it outlives the correction to the body by months.

Enumerate exact strings, not themes. A theme is unenforceable and ungreppable. And give every banned string its honest replacement in the same row, because the writer still needs a sentence in that slot: ban the phrase without supplying the alternative and they will improvise something vaguer, less checkable and usually worse.

A register for an invented self-hosted document signing service, showing the shapes worth copying:

| Banned string | Why | Honest replacement |
| --- | --- | --- |
| Any sentence containing both a price and the word "unlimited", in any of its phrasings: "unlimited signing on the 12 pound plan", "12 pounds for unlimited signing", "unlimited requests from 12 pounds", "12 pounds a month, unlimited", "unlimited on the starter plan at 12 pounds", "for 12 pounds you get unlimited" | The two facts are true separately and false together, because the plan carries a documented cap | "The starter plan includes 200 signature requests a month. Above that, the documented fair-use terms apply." |
| "unlimited" anywhere without the numeric caveat in the same sentence | Same claim, wearing a different sentence | "No per-seat charge, with a documented fair-use limit of 200 requests a month." |
| "an order of magnitude cheaper than <incumbent>" | An invented ratio, and a comparative claim you would have to substantiate | Anchor to the incumbent's own published figure with the date captured: "their published list price for the equivalent tier was X per user per month on <date>. Ours is Y." |
| Any specific speed number for an operation that has none: "signs in under two seconds", "syncs instantly" | There is no measurement behind the number | Name the mechanism with no figure: "the request is queued on the device and delivered when the connection returns." |
| "set up in five minutes" | The only measured figure is 40 minutes for a self-hosted install | "A self-hosted install takes about 40 minutes on the documented reference setup." |
| Any beta, waitlist or scarcity framing on a generally available product: "join the waitlist", "12 founder places left" | The product is generally available, so the framing is false urgency | "Available now", plus the actual documented trial or refund terms. |
| Invented incumbent pricing of any kind | You cannot substantiate a competitor's price from memory | Cite the competitor's own published page with the date captured, or drop the comparison. |
| "money back, no questions asked" | The documented remedy is conditional | The exact documented remedy, its conditions, and who decides. |
| "14-day free trial" (retired) | Retired when the model changed | "30 days to ask for a refund, on the terms set out on the pricing page." |
| The previous tagline, banned from reuse verbatim | It names a capability that was removed in 2026 | Use the current approved positioning line, from whichever document owns it. |

Two more rules that catch true-but-overbroad claims:

- **Scope a locality claim to what is actually local.** Say documents are stored on your own server. Never say the blanket "nothing ever leaves your network", because licence checks, update requests and error reports do, and the blanket version is the one a security reviewer will test.
- **Scope a source or format claim to the named, verified list.** Say it imports the four formats listed in the integrations file. Never "imports from any accounting package", which is a promise to support software nobody has tested.

**The internal-mechanics ban.** Never publish architecture, endpoint names, queue names, model names or pipeline internals. For each sensitive area, define the one approved public-facing sentence and permit no other description. For the signing service the approved sentence is: "signing keys are generated on your own server and are never transmitted to us." No article may elaborate on that, and a writer who wants to say more is asking a security question, not a copy question.

## Step 5: hook the pack into the run, in three places

A pack nobody opens is a wiki page. Three hooks, each at a different moment:

1. **The ordered read list**, at step one of any writing run: `00-product-overview.md`, then the feature file matching the row's angle, then `08-pricing.md` if the piece mentions price at all.
2. **The per-row product-accuracy check**, before writing begins: does this row's keyword name an integration, a price, a guarantee or a number? If yes, resolve its state in the ledger now, not at draft review.
3. **The pre-publish check**: grep every banned string across the body, every search engine field, the alt text and the image-text manifest. This is the hook that catches the image copy, and it is the only one that can.

## Step 6: when the pack does not exist yet, do not abort

You will be asked to run a content programme before the pack exists. Refusing is the wrong answer, because the programme then produces nothing at all while somebody schedules a workshop that keeps slipping.

The fallback has two parts and both are required. First, a **locked facts** block inside the run playbook itself, headed "locked facts, ground truth, cite exactly", holding eight to fifteen short factual lines: what the product is, the plan names and prices, the live integrations, the two or three numbers that may be used, and the current positioning line. Second, a line in **every** run record reading `Governance files missing: <list>`, repeated on every run until the files land.

The second part is the one that matters. A temporary fallback with no visible reminder becomes permanent within about six weeks, and the locked-facts block slowly grows into a worse version of the pack, undated and owned by nobody.

## The decision rule, at the point a writer needs a fact

Take exactly one branch.

- **The fact is in the pack.** Use it. Where a quotable reader-facing line exists, paste it unedited.
- **Not in the pack, but verifiable on the live site today.** Use it, and add it to the pack with the date captured, because the next writer will need the same fact and should not have to look it up twice.
- **The pack and the live site disagree.** A dated override newer than both wins. Failing that, the live site wins for anything a reader can see with their own eyes, and the discrepancy goes to Open questions with a named owner the same day.
- **The fact is a limitation you inferred rather than read.** You may not write it. Silence is not evidence of absence. Write around the gap and log it.
- **You cannot tell whether the capability is live or roadmap.** Treat it as roadmap, which is the branch that fails safely. Write around it. If the article cannot exist without it, the only writable form is coming soon plus today's workaround. If even that will not stand up, defer the row, log the missing fact with an owner, and pick the next row. Deferring one row is cheap. A published how-to for something that does not exist generates support tickets, sales conversations and a correction.

## Worked example

The invented signing service. Row: "how to send a signature request from your accounting package", commissioned because the phrase has real search demand.

**Accuracy check, before writing.** The keyword names an integration, so the ledger is consulted first. The accounting integration sits under Coming soon with a target of the fourth quarter. The how-to is therefore not publishable in its commissioned form.

**Reframe.** The row is rewritten as coming soon plus today's workaround. The article states the status and the quarter in the second section, then documents the path that works today in full: export the invoice as a PDF, name it with the invoice number so the audit trail stays matched, upload, choose the template. A re-verify note is attached to the row, because the quarter may turn over before it publishes.

**Draft review, against the register.**

- "unlimited signature requests on the starter plan at 12 pounds a month". Banned, being a price and the word in one sentence. Replaced with the documented 200 a month plus the fair-use line.
- The pricing graphic rendered the word "unlimited" in its second panel, and its alt text read "pricing table showing unlimited signing from 12 pounds". Both caught only because the register covers image text and alt text. Graphic regenerated, alt text rewritten to describe the image factually.
- The draft asserted that the service "cannot handle documents with more than one signer". Deduced from the fact that the signing-flow file shows only single-signer examples. It is false: multi-signer is documented in the templates file. Deleted under rule five, and a cross-reference added to the signing-flow file so the next writer does not repeat it.
- One genuine unknown: whether the self-hosted install supports a particular database version. Not in the pack, not on the site. Written around, logged in Open questions with an owner and a date.

**Verdict: publishable in the reframed form.** Three claim edits, two image assets regenerated, one open question logged, one inferred limitation removed, and no row deferred. The pack changed by two lines, which is the part most teams skip and the reason the same error recurs.

## Failure modes

**A roadmap feature described as shipping.** The tell arrives from outside the marketing team: support tickets asking how to switch on something that does not exist, and a salesperson being asked about it on a call they were not prepared for.

**An internal figure quoted as externally verified.** Recognisable the day a prospect or a journalist asks for the study, and there is no study, only a spreadsheet from a previous team with no methodology tab.

**A false limitation deduced from silence.** The worst version is when a competitor's comparison page cites your own article as evidence that you cannot do something you can. You wrote their proof for them.

**A banned claim reappearing inside image text.** The body is clean, the review passed, and a screenshot circulating on social shows the retired price in 48 point type. This happens whenever the register is scoped to body copy alone.

**The pack drifting from the product with no dated override.** Two files in the same pack contradict each other, neither is dated, and every writer resolves the contradiction differently and privately. Symptom: two published articles stating opposite things, both traceable to the pack.

**A ban with no honest replacement.** The banned phrase does disappear. In its place: "as much as you need". Same claim, less precision, nothing checkable, and it passes the grep.

**The pack turning into a second marketing site.** Files full of adjectives, no numbers, and every honest-limits section reading "none known". At that point it has stopped being a source of truth and become a place where claims are laundered into apparent documentation.

**The fallback becoming permanent.** The locked-facts block has grown to 40 lines, the missing-files line has appeared in every run record for four months, and nobody reads it any more. The line is only load-bearing while somebody still notices it.

## What this skill does not do

- It does not verify anything. Every fact comes from what a person supplies, and a pack built from an out-of-date help centre will be confidently wrong in exactly the places that help centre is wrong.
- It does not enforce the register. Grepping strings across a repository is a linter's job and a linter does it better, and this file only decides what the strings are.
- It does not clear a claim legally. Substantiation standards for comparisons, guarantees and regulated claims are set by advertising rules in your market, and this is a working discipline rather than compliance.
- It does not write the article. It supplies the facts, the limits and the bans, and the drafting happens somewhere else.
- It does not maintain itself. Without an owner and the dated override habit, it decays into a document quoted with full confidence about a product that changed a year ago.
