---
name: persona-set-build
description: Builds a small persona set that downstream work can act on, using constraint blocks with units, a named incumbent set including non-product incumbents, verbatim vocabulary capture, jobs written as situations, a behaviour-at-scale triple, and a cross-persona analysis whose conflicts bucket is treated as the deliverable rather than as a defect. Includes an anti-decoration gate, provenance tagging for unevidenced fields, and a rule for when a set cannot be trusted from the inside. This skill should be used when creating or rebuilding personas, user profiles or ideal customer profiles, when an existing set is being reviewed for whether it can support a decision, or when a roadmap argument needs the disagreement between user types made explicit.
---

# Persona set build

## The claim this skill is built on

A persona set is not a description of your audience. It is a device for making disagreements about a product decision visible while they are still cheap to resolve. That is why the deliverable is not the cards. It is the list of things the cards disagree about.

The obvious approach fails in a specific and repeatable way. Asked for personas, most teams write one card per audience segment, fill each with a name, an age, a job title, a stock photograph and a paragraph of aspiration, and finish with three documents that want the same thing in three different fonts. Nothing in those cards can produce a no. Every proposal is approved by all of them, which means the set has no discriminating power, which means it gets quoted in one deck and never opened again.

The repair is in the field list, the count rule and the analysis step. It is not in the writing. A beautifully written decorative persona is still decorative.

## Step 1: decide the count before you decide the audiences

A set is three to five cards.

**The test for adding a card is not that another audience exists.** It is that the new card would disagree with an existing card about a decision you actually face. Before opening a new card, finish this sentence in writing: "this card will disagree with card X about Y, and Y is a decision on our roadmap this quarter." If you cannot finish it, you do not have a card. You have a job title.

Below three cards there is no room for the conflicts bucket, so the set cannot produce its main output. Above five, the marginal card is nearly always a duplicate wearing a different job title, and nobody reads past the third one anyway. Cards get cut at this step, before any writing, which is the only point at which cutting one is free.

## Step 2: the identity card, and the fields left out on purpose

Nine fields, and no decorative ones:

- An invented name plus a short role handle, so people say the handle out loud in meetings
- Role and what they are accountable for
- Organisation size
- Experience level in this role
- Primary goal, stated as an outcome someone else can observe
- Daily reality, meaning what the day actually contains
- Top three frustrations
- Technology comfort level
- What success looks like, in their own terms rather than yours

Deliberately absent: age, gender, location, income bracket, a photograph, a personality adjective, a favourite brand. Not because these are objectionable but because none of them can flip a decision. Apply this test to any field you want to add: name one product decision that changes if this field changes. Age almost never passes it. Budget authority always does.

## Step 3: the constraint block, which is what separates usable from decorative

Four fields, each with a unit attached:

- **Time available.** Minutes per week for this specific task, not "busy". Fifteen minutes a week and three hours a week are different products.
- **Budget authority and its ceiling.** Can this person spend without approval, up to what number, and who signs above it. This single field decides more packaging arguments than all the demographics combined.
- **Team size.** How many people touch the artifact, and how many of them are not like this persona. The second number predicts every hand-off problem you will have.
- **Tools in use today.** The actual stack for this task, including the parts that are not software: a printed checklist, a weekly call, a person who just knows.

Demographics predict nothing about product decisions. Constraints predict nearly everything, because a constraint is what produces a refusal, and a refusal is what a decision is made of.

**Tag every field with its provenance: observed, reported, or assumed.** Observed means you watched it. Reported means someone told you. Assumed means you decided it. This tag costs one word and it is what makes the cannot-tell branch below workable, because an assumed constraint block reads exactly like an observed one once it is typed up.

## Step 4: the incumbent set

Name two or three things this person would compare you against, **including the unglamorous ones**. Generic classes, since the point is the shape rather than the brand: a spreadsheet with a shared tab, a shared inbox plus a rota, a recurring meeting and a document, a script somebody wrote and left behind, a general purpose tool used sideways, a person who does it manually, or absorbing the cost and doing nothing at all.

**Rule: at least one incumbent per card must not be a product.** If every named incumbent is a competing product, you have described the smaller of the two available markets, the one that already knows it has this problem and is already paying to solve it.

For each incumbent add the switching cost in the persona's own units: an afternoon, a quarter, a change control ticket, an argument with a colleague who built the spreadsheet.

A persona with no named incumbent cannot evaluate anything, because every judgement it makes is against nothing. That is the mechanical reason decorative persona sets are always enthusiastic about the roadmap.

## Step 5: capture the vocabulary verbatim

Record the words this role uses for the problem, which are usually not the words your product uses. The same failure sounds different by register. An executive says "we keep getting surprised". An operations lead says "we find out from the customer". An engineer says "the check runs after the deploy, not before". One failure, three registers, and the register is exactly what makes downstream copy sound native or foreign.

Capture four things: the words for the problem, the words for the current workaround, the words for the moment it hurts, and the words for the outcome. If a phrase in the block also appears in your own marketing, suspect it. It is probably yours and not theirs.

## Step 6: jobs written as situations, not as features

Use the Jobs To Be Done form, and apply it rather than citing it: when [situation], I want to [motivation], so I can [outcome].

**The substitution test.** If your product's name fits into the "I want to" slot, you have written a feature request and called it a job. "I want to run the scanner" is a feature. "I want to know whether this release breaks anything a screen reader depends on, before a customer tells me" is a job, and it can be satisfied several ways, only one of which is your product.

Three to five jobs per card, ranked by frequency multiplied by pain, both scored in the persona's units rather than yours.

## Step 7: the behaviour-at-scale triple

This is the field almost every persona set omits and the one that catches the most design errors. Three questions per card, each answered in a sentence rather than a yes or a no.

1. **What changes at 500 items instead of 5.** A list becomes a queue, a queue needs state, review becomes sampling, and the person starts trusting a summary they cannot verify. Most interfaces are designed against the five-item case and shipped to the five-hundred-item one.
2. **What happens on a slow or intermittent connection.** Whether unsaved work survives, whether this person retries or abandons, and whether they trust the result that eventually arrives. Trains, hotel networks, tethered phones, sites with restricted egress.
3. **What happens when they hand the artifact to someone with no context.** Whether it explains itself, whether the recipient can open it at all, and on what machine. A file path, a keyboard shortcut and the default application for a file type all differ between Windows and macOS, and the hand-off is exactly where that difference bites.

These three questions are where the persona stops agreeing with the happy path, which is the first useful thing most cards ever do.

## Step 8: the ground-truth rule, scenarios not adjectives

Every insight a card produces must be a scenario, not an adjective.

**The parse test.** The line has to contain a trigger or a time, an action the person took, and what happened instead. "The dashboard is confusing" fails: it names a feeling with no moment attached. "On the Monday report she opens the dashboard, cannot find last week's number in under a minute, exports to a spreadsheet and rebuilds the chart by hand, which now happens every week" passes, and it contains three separate design leads that the adjective version does not.

A card that cannot produce one scenario is not finished. Either research it or cut it, but do not ship it as a card that only produces adjectives, because adjectives are unfalsifiable and therefore unarguable.

## Step 9: the cross-persona analysis, which is the actual output

Four buckets:

- **Universal pain points.** The weakest bucket and the most quoted, because nearly everything in it was already known before the set existed.
- **Conflicting needs.** The finding.
- **Highest impact opportunities.** Ranked by how many cards they serve without hurting another card.
- **Dealbreakers per segment.** At least one per card, phrased as a condition under which this persona will not adopt regardless of everything else you build.

**Treat the conflicts bucket as the deliverable.** Where two personas want opposite things, a product decision has been surfaced early and cheaply. Resolving it inside the document by picking a favourite destroys the exact information the set was built to produce, and it hides the decision inside a research artefact where nobody will look for it.

Write each conflict in five lines: the decision at stake; position A and what A loses if B wins; position B and what B loses if A wins; the observable that would settle it; and who decides. The fourth line is what converts an argument into a research question with a due date.

## The decision rule: is this set finished?

- **All four gates pass** (every card names an incumbent including one non-product incumbent, every constraint block is non-empty, every card can produce one scenario, the conflicts bucket is non-empty). Ship it, with the provenance tags left visible.
- **A card names no incumbent, or its constraint block is empty.** That card is decorative. Fix it or cut it, and cutting is usually correct, because the missing incumbent means nobody has thought about what this person does today.
- **The conflicts bucket is empty.** You have one persona and several illustrations of it. Merge them into one card and rebuild the set around a genuine disagreement, or accept that you have a single-persona product and stop.
- **You cannot tell.** The set is well formed and every field is tagged assumed. This is the common case and it is invisible from inside the document, because assumed and observed fields read identically once typed. The tell is that no card contains a number you did not choose yourself. The correct action is not to bin the set. It is to label it as a hypothesis and run five conversations against the two fields that most change the roadmap, which are the budget ceiling and the incumbent. Everything else can stay assumed for another month without much harm.

## Worked example, compressed

Invented product: a tool that drafts customer-facing release notes from a repository's commit history.

**Count.** Four candidate audiences: a solo maintainer of an open source library, a product marketing manager at a company of about 200 people, a support lead, and an engineering manager. The engineering manager card is cut at step 1, because the disagreement sentence could not be finished: every position it held was already held by the support lead. Three cards.

**Constraints.** The maintainer has about twenty minutes per release, a personal card with a ceiling in the low tens per month, a team of one. The marketing manager has a departmental card with a stated ceiling and a signature required above it, a team of six of whom four are not like her. The support lead has no budget at all and must route the request through someone else, which is itself a dealbreaker.

**Incumbents.** Maintainer: a hand-edited markdown file plus a script, switching cost one afternoon. Marketing manager: a document template plus a weekly review meeting, switching cost one quarter because the meeting is a habit and not a tool. Support lead: a shared inbox and the ticket tool, switching cost one change-control ticket.

**Behaviour at scale.** At 500 commits the maintainer needs summarisation rather than a list. At 500 notes a quarter the marketing manager's flow becomes an approvals queue with state. Offline: the maintainer edits on a train and will not tolerate losing a draft. Hand-off: a contributor with no context must be able to regenerate the notes on Windows or macOS, without a licence and without being added to a workspace.

**Conflicts bucket, one entry.** The decision: does publishing happen automatically or behind an approval gate. The maintainer loses the entire value if there is a gate, because a gate needs a second person and there is no second person. The marketing manager loses brand and legal control if there is no gate, and she is the one who gets asked about it. The observable that would settle it: what share of trial workspaces have more than one member inside the first fourteen days. The decider: the roadmap, not this document.

**Verdict: three cards, one cut, one conflict, and the conflict is the product decision.** The set is accepted with every budget ceiling tagged assumed, and the first research round is five conversations about spending authority and about what each person does today, not about features.

## Failure modes

**The demographic card.** Age, location and a photograph, with nothing in it that could change a decision. Recognisable because the card can be read aloud in full without anybody objecting to anything.

**The agreeable set.** Three personas who want the same thing. Recognisable because the conflicts bucket is empty and the opportunities bucket is a feature list everyone already agreed on.

**Conflict laundering.** Divergent needs smoothed into one synthesised user, usually called something like "our core customer". Recognisable because the composite has a budget ceiling nobody has and a time budget nobody has, and because the hardest roadmap question quietly disappeared during the writing.

**Adjective feedback.** Cards that emit "confusing", "clunky" and "cluttered" instead of the moment and the workaround. Recognisable because nobody can act on the output without going back and asking what was meant.

**The vacuum persona.** No named incumbent, so the card evaluates against nothing and approves everything. Recognisable because the persona is enthusiastic about a feature that would take a quarter and save that person nine minutes.

**Happy-path only.** No behaviour-at-scale answers, so the set never predicts the failure that actually arrives. Recognisable in hindsight: the first support escalation after launch is about volume, offline behaviour or a hand-off, and no card mentioned any of the three.

**Generic positivity.** Cards written, consciously or not, to validate a roadmap that already exists. Recognisable because every card's primary goal is a restatement of a feature already on the plan, and this is the failure mode that makes teams stop trusting persona work altogether.

**The team in disguise.** Cards that describe the people who built the product. Recognisable because technology comfort level is high on every card, every incumbent is a developer tool, and no card would ever need documentation.

## What this skill does not do

- It does not do research. It cannot recruit, interview or observe anyone, and every field tagged assumed stays assumed until a person is spoken to.
- It cannot size or rank segments. It will not tell you which persona is the largest, the most valuable or the cheapest to reach, and those are commercial questions the document has no access to.
- It does not validate that the segments are real. Distinct cards on paper can turn out to be one behaviour with three job titles, and only behavioural data settles that.
- It does not resolve the conflicts it surfaces. That is deliberate, and it means the output arrives as an unresolved decision that somebody senior has to make.
- It does not maintain itself. Constraint blocks rot as budgets, tools and team structures change, and a card more than about a year old should be treated as assumed everywhere until re-checked.
