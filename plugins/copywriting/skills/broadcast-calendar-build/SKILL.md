---
name: broadcast-calendar-build
description: Builds an email send calendar and a per-campaign cadence for a list you own, on the explicit thesis that a small number of well-aimed broadcasts carries more revenue than an automated behavioural drip sequence. Produces two behavioural segment definitions with 90 and 180 day boundaries, a two-send campaign structure with a last-chance send at close minus 24 hours, a 7 to 10 day offer window, and a twelve-week discovery protocol that tests twelve structurally different offer mechanics ranked on revenue per recipient. This skill should be used when planning email sends for a list you already own, when deciding between broadcasts and an automated sequence, or when a calendar of campaigns needs a cadence, a close date and a way of telling which offers actually earned.
---

# Broadcast calendar build

## The claim this skill is built on, and the thing that would disprove it

The default advice for email is to build a behavioural drip: a welcome series on signup, a re-engagement flow on dormancy, a cart flow on abandonment, all firing automatically. The claim here is that for most lists, most of the time, that machinery underperforms a small number of well-aimed broadcasts, and the underperformance is misdiagnosed as a copy problem.

The mechanism is simple. A message that reads as a welcome series gets treated as one. Nobody is waiting for message three of five. The numbers come back low, the team rewrites message three, the numbers stay low, and the conclusion drawn is that email is a slow channel. The remedy is not better automation. It is sending the broadcast that already works more often, to a list that recognises the sender and expects the offer.

**Because this contradicts the consensus, the falsifier is a step and not a footnote.** State it in writing before you build anything:

> This calendar is built on the claim that broadcasts beat an automated drip on revenue per recipient. It is wrong if a drip arm beats the broadcast arm on revenue per recipient over one full cycle. If that happens, keep the drip and discard this.

Then make it testable. Hold out a random 10 percent of the list at the start, run the drip against the broadcast calendar for one full cycle, defined as 90 days or three campaigns, whichever is longer, and compare revenue per recipient at the end. The holdout is small enough to cost little and large enough to be visible if the effect is real. If you will not run the holdout, say so, and treat everything below as a preference rather than a finding.

**One scope condition, stated so the claim stays honest.** None of this argues against product-triggered transactional mail: receipts, password resets, expiry warnings, failed payment notices. Those are product events, they are expected, they are opened, and they are not marketing sequences. The argument is about automated marketing narratives, not about the plumbing that tells someone their card was declined.

## Step 1. Resist segmentation, then define exactly two segments

The instinct after building a list is to slice it: by role, by industry, by company size, by which lead magnet they took. Resist it, deliberately and on the record.

The reason is that buyers wear several hats. A person who signed up as a freelance designer runs an agency two years later, and a segment defined by the title they typed in a form removes them from the send that would have converted. Every attribute segment is a bet that the attribute you captured still describes them and still predicts what they want. That bet loses quietly, because you never see the sale that did not happen.

Let the offer do the qualifying. A clearly stated offer with a price and a boundary is a better filter than any field on a form, because the reader applies it to themselves in the present tense.

Where segmentation is genuinely needed, use exactly two behavioural segments and no more:

- **ACTIVE:** a site visit, a click, or a purchase within the last 90 days.
- **INACTIVE:** has opened an email at some point, but no visit, click or purchase within 180 days.

Both are behavioural, both are computable, and both should be written as predicates against fields you actually have. Write them out with the field names before you build a row of the calendar, for example `last_click_at`, `last_order_at` and `last_session_at`, and confirm all three exist and are populated. A segment defined against a field your platform does not fill is a segment that silently resolves to everyone or to nobody.

Everyone who is in neither segment is the default audience, and the default audience is the whole list. That is the normal case and it should stay the normal case.

The 180 day boundary has a second life outside this file. It is also roughly where the conversation about suppressing dormant addresses begins, and that is a deliverability question rather than a calendar question. Continuing to mail people who never respond teaches filters that your mail is unwanted by your own audience, and no calendar survives that.

## Step 2. Two sends per campaign, and only two

Every campaign gets exactly two sends.

- **Send 1, the launch.** Goes at the moment the window opens, to the full eligible audience, with nobody suppressed.
- **Send 2, the last chance.** Goes at **close minus 24 hours**, with people who have already bought suppressed.

Three details carry the weight.

**Suppression applies to the second send only, never the first.** A buyer suppressed from the launch is a person you decided in advance would not want the new thing, and the most reliable predictor of a purchase is a previous purchase. Suppress them from the last-chance send, because a scarcity reminder addressed to someone who already acted reads as carelessness, and carelessness is what unsubscribes are made of.

**The second send is timed against the close, not against the launch.** Write it as an offset in the calendar, close minus 24 hours, not as a fixed weekday. If the window moves, the last-chance send moves with it automatically and the mechanic stays intact. Calendars that hardcode both dates produce a last-chance send that arrives three days before the close, which is not a last chance.

**There is no third send.** A middle reminder feels helpful and is the most expensive habit on this list, because it teaches the audience that the last-chance send is one of several. Once a list learns that, the 24 hour message stops working and there is nothing to replace it with. Two sends is a cap, not a starting point.

## Step 3. Hold the offer window at 7 to 10 days

Shorter than 7 days and the structure has nowhere to live. Somebody is away for three days, the launch send lands during a busy afternoon, and the last-chance send arrives before they have read the first one. Longer than 10 days and the deadline stops being felt: the middle of a fourteen-day window is dead air, which is precisely when somebody suggests a third send.

Pick a close time, state one time zone, and put both in the copy of both sends. A close date without a time creates an argument on the day.

**Never extend.** An extended deadline retires the mechanic permanently, and it retires it for every future campaign rather than only for this one. The list learns in one campaign and does not unlearn. If a window is genuinely mistimed, close it on schedule and run the offer again later as a new campaign with a new reason.

## Step 4. Cap the body at roughly 500 words

The cap is not a style preference. It is the constraint that forces the offer to be legible, because at 500 words there is no room for a story that arrives at the point on the fourth screen.

What fits, in order: one line of context, the offer in a single sentence including the price and the boundary, the reason it exists now, the proof or the artefact itself, the close date with the time zone, and one call to action which may appear twice at most.

The last-chance send is shorter still, 120 to 200 words. It restates the offer in one sentence, names the close time, and stops. A last-chance send that re-argues the case is a third send wearing the second send's timestamp.

**Show the thing.** Where the product produces a visible artefact, put real output from it in the send rather than a description of it. A file converter shows a converted file, a reporting tool shows a real report with the numbers changed, a template pack shows a page from the template. A description of an artefact competes with every other description in the inbox. The artefact competes with nothing.

## Step 5. Run the twelve-week discovery protocol

You do not know which offers your list responds to, and neither does anyone else, because it is a property of that list. Find out with a structured experiment rather than by taste.

One send a week for twelve weeks. Every week is a structurally **different offer mechanic**, not a different product. This is the distinction that makes the protocol worth running: twelve weeks of new products tells you which product is popular, and twelve weeks of mechanics tells you how this list likes to be sold to, which generalises to everything you sell afterwards.

The twelve mechanics:

1. A new item in a category
2. Buy one, get one
3. A discount on the worst seller
4. A bonus attached to the best seller
5. A bulk package
6. A bundle at one price
7. A service tier granted above a spend threshold
8. A free item from a different product line
9. A novel package that has not existed before
10. A shipping or fee waiver
11. An entry to win, above a spend threshold
12. A prize for the top spender

Hold everything else constant: same audience, same send time and weekday, same sender name, same rough body length. You are varying one thing, so vary one thing.

Two honest weaknesses in the design, stated rather than hidden. There is no control arm, so the protocol ranks mechanics against each other on your list and tells you nothing about how your list compares to anyone else's. And the same people see all twelve, so novelty decays across the run and week 12 sits at a disadvantage against week 1. Mitigate by treating differences under a stated margin as ties, and by rotating the order if you ever run it again.

**The stop rule is absolute: the protocol ends at twelve weeks whether or not a winner emerged.** A run that produced no clear winner is a finding about the list, not a reason to run a thirteenth week. It usually means the list is too small to separate effects, or too cold to respond to any offer, and both of those are answered somewhere other than the calendar.

## Step 6. Rank on revenue per recipient

The ranking column is **revenue per recipient**: revenue attributed to the campaign, divided by the number of people it was sent to. Not per opener. Not per click. Per recipient.

**Why not open rate.** Ranking by opens selects for subject-line novelty, which is not the thing you are testing, and the number is unreliable in the first place. Image prefetching by privacy-protecting mail clients has inflated open counts since Apple's Mail Privacy Protection shipped in September 2021, and the share of your list affected depends on the mail clients your audience happens to use, which you did not choose and cannot see.

**Why not per opener.** Dividing by openers flatters a mechanic that nobody opened. If two people open and one buys, that is a 50 percent conversion rate and a failure.

**Fix the attribution window before week one and never move it.** A single stated rule, for example revenue from an order placed within 72 hours of the send, or revenue placed at any point before the close for a windowed campaign. Whichever you pick, apply it identically to all twelve. A window that changes mid-protocol ranks the window.

Two more columns, and neither is a ranking column:

- **Unsubscribe rate and complaint rate per send, used as a veto.** Any mechanic whose complaint rate crosses your sending platform's line is disqualified regardless of what it earned. Entry-to-win and top-spender mechanics are the usual offenders, because they attract people who wanted the prize.
- **Contribution per recipient, where the mechanics differ in cost.** A shipping waiver and a bulk package can rank identically on revenue per recipient and land in completely different places once the cost of the waiver is subtracted.

## Step 7. Prefer repeatable offers, then build the standing rotation

A repeatable offer is one the same person can take more than once, whose value does not expire. A one-shot offer wins once and then dies, and a calendar built entirely from one-shot offers decays visibly: strong for a quarter, then flat, and the flatness gets blamed on the writing.

When the twelve weeks are ranked, keep the top three or four mechanics as a standing rotation and retire the rest. Prefer a repeatable mechanic over a one-shot mechanic when the two are within the margin you declared. Then re-run the protocol only when the list composition changes materially, which means one of: the list has grown by more than half, the dominant acquisition source has changed, or the product line has changed. Re-running it because the numbers dipped is how a rotation becomes another twelve weeks of experiments.

## What the finished calendar contains

One row per campaign, and these columns: week and date, campaign name, offer mechanic from the twelve, audience (all, ACTIVE, or INACTIVE), send 1 date and time, close date and time with time zone, send 2 date and time expressed as close minus 24 hours, the suppression rule, the body cap, revenue per recipient, complaint rate, and a verdict of keep or retire.

Keep it wherever the team already looks: a spreadsheet, a shared document, a table checked into the repository. Nothing here depends on a particular application or operating system, and a calendar nobody opens is worse than no calendar, because it is quoted from memory.

## The decision rule

- **You can attribute revenue to a send.** Run the twelve-week protocol as written and rank on revenue per recipient.
- **You can attribute orders but not revenue.** Rank on orders per thousand recipients multiplied by the average order value for that mechanic, and write the substitution at the top of the table so nobody later reads it as revenue.
- **The list is under roughly 500 engaged recipients.** Run the protocol for the qualitative signal, and do not retire a mechanic on one week's number, because at that size a single order moves the ranking. Instead carry the top four forward and rank after they have each run twice.
- **The list has not been mailed in more than six months.** Do not open with a twelve-week protocol. Send one reconnection message that says what is coming and offers the exit, and start the protocol on whoever remains. Starting cold produces a complaint spike that ends the calendar and can end the domain.
- **You cannot tell whether the revenue you are seeing came from the send.** Stop building the calendar. Instrument first: one tagged link per campaign, one written attribution window, one place the number lands. Without it the protocol produces twelve anecdotes, and you will rank them on the only number you do have, which is opens, which is the failure this file exists to prevent.

## Worked example, compressed

A two-person company sells a desktop file-conversion utility at a one-off price, plus a paid template pack. The list is 9,400 addresses. Current setup: a five-message automated welcome series and one newsletter a month. Over the last full cycle the welcome series returned 0.04 in revenue per recipient, in whatever currency they sell in, and nobody had computed that figure before being asked for it.

**Thesis and falsifier written first.** A 10 percent holdout, 940 addresses, stays on the welcome series for the full cycle.

**Segments computed.** ACTIVE resolves to 1,830 addresses on a 90 day click or order. INACTIVE resolves to 3,100 on an open ever, nothing in 180 days. The remaining 4,470 are the default audience and get every launch send.

**Campaign one.** Mechanic: a bundle at one price, the utility and the template pack together. Window opens Monday 09:00 and closes the following Wednesday 17:00, nine days. Send 1 Monday 09:00 to all 9,400. Send 2 Tuesday 17:00, close minus 24 hours, with the 212 buyers suppressed. Bodies at 460 and 170 words. The launch send contains a real converted file rather than a description of one.

**Twelve weeks run.** Ranked on revenue per recipient inside a fixed 72 hour window: the bundle at one price returns 0.41, the bulk package 0.33, the bonus on the best seller 0.29, and the remaining nine sit under 0.20. The entry-to-win mechanic produced the highest open rate of the twelve by a wide margin and ranked seventh on revenue per recipient, with the second-highest unsubscribe rate in the run. The holdout arm on the welcome series returned 0.05.

**Verdict: retire the welcome series and ship a four-mechanic standing rotation on a six-week cycle.** Bundle at one price, bulk package, bonus on the best seller, and one repeatable mechanic held over from the middle of the table because the two above it are one-shot. Re-run the protocol only when the list has grown past roughly 14,000 or the product line changes. The thesis survived this once, on one list, which is not proof and is the reason the holdout stays in place for the next cycle.

## Failure modes

**Welcome-series drift.** The calendar acquires an automated onboarding sequence, its numbers are poor, and the poor numbers get blamed on the copy of message three. From the outside it looks like a team on their fourth rewrite of a message that nobody was ever waiting for.

**Over-segmentation.** The list is sliced by role or industry, revenue per send rises slightly on each slice, and total revenue falls, because the multi-hat buyer who would have bought was filtered out by the title they typed into a form two years ago. It looks like precision and it is subtraction.

**The third send.** Somebody adds a helpful mid-window reminder. It performs adequately once. The next campaign's last-chance send underperforms, and the campaign after that underperforms more, and nobody connects the two.

**Open-rate optimisation.** The twelve-week table is ranked by opens because that is the column the dashboard sorted by. The winner is the mechanic with the most novel subject line, the rotation gets built on it, and revenue does not move.

**One-shot rotation.** Every mechanic in the standing rotation is something a person can take exactly once. The first cycle is strong, the second is soft, the third is flat, and the diagnosis offered is list fatigue rather than offer structure.

**Untrue scarcity.** A deadline is extended once, because the numbers on the day were disappointing. Every subsequent close date is disbelieved, last-chance sends stop working, and the mechanic cannot be recovered.

**Twelve products dressed as twelve mechanics.** The protocol runs twelve weeks of different items at the same discount. It produces a popularity ranking of the catalogue, which is worth knowing and is not what the protocol was for, and it generalises to nothing.

**Suppression on the launch send.** Previous buyers are excluded from send 1 to avoid annoying them. The single most responsive group on the list never sees the offer, and the campaign's revenue per recipient falls for a reason nobody looks for, because the suppression was recorded as a courtesy.

## What this skill does not do

- It does not get your mail delivered. Authentication, alignment, list hygiene and sending pattern decide whether any of this arrives, and a calendar cannot see the inbox.
- It does not tell you whether the offer is any good. It structures, schedules and ranks offers. An offer nobody wants ranks last in a well-designed protocol.
- It cannot compute your revenue per recipient. Every ranking depends on figures you supply, from a system it cannot read.
- It does not settle consent or marketing law, which vary by jurisdiction and apply before the first send rather than after it.
- The central thesis is untested here. The file states its own falsifier and tells you to run the holdout, and a drip arm that wins is a real outcome rather than a rhetorical one.
- It says nothing about transactional or product-triggered mail, which follows different rules and is not what the argument is about.
