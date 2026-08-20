---
name: dormant-list-revival-plan
description: Produces a dated re-engagement campaign for a lapsed self-serve email list, rather than one broadcast to everybody who went quiet. Covers the three data tables to pull before segmenting, seven behavioural segments whose conversion bands span two orders of magnitude, a value-per-hour calculation that decides the work order and often puts the smallest segment first, the send order that doubles as the sending ramp, one distinct offer per segment keyed to why each group went cold, suppression logic and the difference between dormant and unsubscribed, per-send numeric stop rules with a three-branch mid-campaign gate, and the consent questions that need a qualified check. This skill should be used when a win-back, reactivation or re-engagement campaign is being planned for lapsed users or churned customers, when someone proposes emailing everyone who ever signed up, or when a previous re-engagement send produced complaints instead of revenue.
---

# Dormant list revival plan

## The claim this skill is built on

A lapsed self-serve list is not one audience that went quiet. It is seven behaviourally distinct populations whose conversion rates differ by two orders of magnitude, sharing one property that makes them dangerous: mailing them is the fastest available way to damage the domain you need for everything else you send.

The obvious approach, one warm message with a discount to everyone who has not opened in six months, fails in a repeatable order. The largest segment on almost any self-serve list is people who signed up and never once used the product: least likely to open, most likely to hold a dead address, most likely to report the message. Mail them first because they are the biggest, or alongside everyone else, and the reputation damage lands before the segments that would have paid you get their message delivered.

So the knowledge is not the copy. It is what the segments are, what order you work them in and why that is not the size order, which offer belongs to each, and which numbers decide, between sends rather than at the end, whether to carry on.

## Part one. Pull three tables, and know the shape to expect

**Accounts:** signup date, last login, login count, plan, deleted or deactivated flag.

**Usage:** lifetime actions, sessions or minutes, whichever your product counts, aggregated to one number per account.

**Billing:** subscriptions started and ended, ending reason, stated cancellation reasons, failed payment events, and customer records that exist at the payment processor with no subscription ever attached.

**The billing table is the one people skip and it holds the two highest-yielding segments.** A customer object with no subscription is someone who reached the payment form and stopped, so they had already decided and were blocked by mechanics. A subscription that ended on a failed payment is not a decision at all. Neither is visible in an email tool, and neither is visible in the accounts table either.

**Expect this shape.** Observed on one anonymous three-thousand-address list belonging to one consumer tool, on a single account. Treat the shape as calibration and the numbers as a hypothesis to test against your own export.

| Observation on that one list | Figure |
|---|---|
| Recorded zero usage, ever | about 63% |
| Logged in exactly once and never returned | about 55% |
| Had any meaningful usage at all | under 10% |
| Of everyone who reached the payment provider, ever subscribed | about 22% |
| Of churned subscriptions, ended on a failed payment | about 30% |
| Of cancellations, left no reason at all | about 53% |

If your export looks like that, nothing unusual has happened to you.

**Never segment on email engagement alone.** Since image-prefetching privacy features became common in mail clients from 2021 onward, a recorded open can happen with no human involved, so an open-based cut can put someone in a warm bucket who has not read you in two years. Clicks are the honest signal, and product data is better than either.

## Part two. Seven segments, cut by signal and never by recency

**Define dormancy in your product's own terms.** A workable rule is three times the typical gap between sessions for an active account, floored at 90 days. If active users open the product twice a week, someone silent for six weeks is a churn risk rather than a revival target. A recency threshold copied from another company encodes that company's usage cycle.

| # | Segment | Definition | Planning band |
|---|---|---|---|
| 1 | Never-activated | Signed up, zero meaningful usage | 0.3% to 0.5% |
| 2 | Trialists | Trivial usage, one or two sessions, then gone | 1.5% to 3% |
| 3 | Engaged-but-free | Real, repeated usage, never paid | 5% to 15% |
| 4 | Checkout abandoners | Reached the payment provider, never subscribed | 8% to 12% |
| 5 | Failed-payment churn | Subscription ended on a payment failure | 35% to 60% |
| 6 | Cancellation churn | Subscription deliberately cancelled | 15% to 20% |
| 7 | Currently paying | Active subscription, retention rather than revival | about 60% |

**Every band in that column was observed on one operator's own campaign against one self-serve product.** They are planning defaults so the arithmetic in part three can be run at all, not industry benchmarks, and nobody publishes industry benchmarks for this. Replace them after send one. The ratio between segments matters more than the absolute levels, and the ratio is the part that tends to survive.

**The spread is why this is worth doing.** From 0.3% to 60% is a factor of two hundred, so two people on the same list, both silent for a year, deserve wildly different amounts of your attention. A broadcast prices them identically.

## Part three. Rank by expected value per hour, not by size

```
value_per_hour = (N x c x V) / H

N = addresses in the segment after suppression
c = planning conversion band, use the midpoint until you have your own number
V = expected first-year value of one conversion, after that segment's own
    discount and after the share who lapse again inside the free window
H = hours to produce and run it: copy, offer setup, list build, address
    verification where needed, and reply handling
```

Two structural facts make the result counterintuitive.

**`H` is roughly flat across segments.** A segment of two hundred and a segment of twenty thousand each need one set of three emails, one offer configured and one code minted. Effort scales with the number of segments, not the number of addresses. The exception is the cold tier, where verification and complaint monitoring add hours that do scale with `N`.

**`c` and `N` span comparable ranges, so neither dominates.** Conversion spans two orders of magnitude and segment sizes usually span about the same, so the product can land either way. That is why you compute it rather than intuit it, and the answer is frequently that a segment which is a rounding error on the list is worked first while the largest segment is worked last or dropped.

**Write the ranking down as a table with `H` estimated honestly**, because the argument at week three will be about the biggest segment and this table is what settles it.

## Part four. The send order, which is also the warm-up ramp

Ranked segments group into sending tiers. **Tier A:** currently paying, tens or low hundreds of recipients, highest affinity. **Tier B:** the intent signals, meaning failed-payment churn, checkout abandoners, engaged-but-free and cancellation churn. **Tier C:** trialists. **Tier D:** never-activated, and only after address verification.

**Reversing that order is how a revival campaign destroys the sending reputation it needed.** Mailbox providers score a sender on recent behaviour, per domain and per sending address, on a rolling window. No provider publishes the weighting, so treat the magnitude as unknown, but the direction is stated plainly in every provider's own sender guidance: recent engagement raises placement, and recent complaints, bounces and silence lower it. A revival campaign inverts the ratio a normal campaign relies on, where an engaged majority carries an unengaged minority. Here almost every recipient is unengaged by construction, so the ordinary safety margin does not exist.

Send the cold tier first and three things go wrong within days. Hard bounces spike, because dead addresses concentrate in the oldest segment and bounce rate is among the fastest-acting negative signals there is. The share of recent volume that nobody opened collapses in the same window. And complaints run highest from the people with the weakest memory of signing up.

**Then the damage lands on someone else.** Placement degrades over days, so by the time the failed-payment segment gets its message, an audience that would have reactivated at high rates is being filtered. There is no second attempt: the offer, the copy and the timing were all correct and the message was not seen. That revenue is gone rather than deferred, and the post-mortem blames the offer.

**You do not need a separate warm-up plan, because this order is one.** A warm-up schedule prescribes starting at low volume with your best-engaging recipients and raising volume as you go, and the value-per-hour ranking produces that shape by itself, because engagement quality and expected conversion fall together as segment size rises. One ordering satisfies two independent constraints, so you are not trading revenue against deliverability.

**Where the two constraints disagree, risk wins the ordering.** A very large cold segment can post high value per hour, and all of that value is contingent on delivery. Move it later, or drop it.

**It is also an information order:** each tier gives you real bounce, complaint and click rates on a small, safe population before you commit the larger one behind it. And because bounces are cleaned after each send rather than at the end, every later tier mails a cleaner list than the last.

**Volume discipline.** A starting default from one small-list operator's practice is under 500 messages a day in week one, then ramp. The number that matters is relative: do not send a multiple of your recent normal daily volume. If you already send tens of thousands a day, 500 is meaningless and your ramp is a different shape.

**A six-week shape**, a default rather than a law: week 1 infrastructure and personal outreach to current payers, week 2 failed-payment recovery, week 3 checkout abandoners, week 4 engaged-but-free, week 5 cancellation churn, week 6 trialists, analysis, and the decision about whether Tier D happens at all.

## Part five. One offer per segment, keyed to why they went cold

A single discount across the whole list trains current customers to wait for the next one, insults people who already reached your payment form and were stopped by something else, and tells someone whose card expired that you think they left on purpose.

| Segment | Offer | Why this and not a discount |
|---|---|---|
| Currently paying | Grandfather their rate for a stated period, give the cohort a status marker, ask for calls and testimonials | A discount here is pure margin destruction, and you need the testimonials later. |
| Failed-payment churn | Friction removal: one month free, no card required to reactivate, framed as an apology | It was a dead card, not a decision. A discount answers a question they never asked. |
| Checkout abandoners | Free trial, no card, dated expiry | Remove the exact friction that stopped them, which was the card form. |
| Engaged-but-free | Time-boxed steep discount, around 50% off the first three months | They already use it and believe in it. The gap is an impulse price point, not conviction. |
| Cancellation churn | A long free window, 60 days, plus one question and a promise to stop emailing if it is still not right | A decision is not reversed by 20% off, and the answers are worth more than the reactivations. |
| Trialists | A demo or short walkthrough with no ask at all, then a trial offer only to those who watched | They have no formed opinion, so any decision you request is the one they will not make. |
| Never-activated | One curiosity email, no offer, used purely to qualify by click | They have never seen the product work, so an offer on an experience they never had is noise. |

**One hard gate on the cancellation segment: if the product has not genuinely changed since they left, do not send it at all.** That message only works when it leads with what is different. Without a real change it is a discount aimed at people who already evaluated you and said no, and it draws complaints from the best-informed audience on the list.

**Mint a distinct promotional code per segment**, even where two segments get economically identical offers. Redemption then attributes without reconstructing anything from click data, and the code is the only attribution that survives a forward to a colleague.

## Part six. Cadence and assets

**Three emails per warm segment. Cold segments get exactly one.** Day 1, the main message and the offer. Day 3, to people who opened and did not click and only to them, one question asked literally: what stopped you? That send produces the campaign's most valuable output, which is prose from real people about why they left. Day 7, the urgency close against the offer's stated expiry.

Where the buying decision involves other people, stretch the same three roles to day 0, day 5 to 7 and day 12 to 21. Sending three to a cold segment triples the complaint risk of your riskiest population for a yield improvement that will not appear.

Each email needs three subject-line variants and **a preview text line written as its own field**, not inherited as a truncation of the first sentence. That default is why so many revival emails preview as "View this email in your browser".

**The reactivation landing page must exist before the abandoner wave**, so build it in week one or two. It is not the pricing page: it addresses someone who already knows the product, states what changed since they left, and has the offer already applied rather than a code to type.

**Measure trial-to-paid at 30 days and not before.** Read earlier and you are measuring the trial length rather than the offer.

## Part seven. Suppression, which happens before segmentation and is a boolean

Never contact, at any age, under any segment:

- Anyone who unsubscribed. Ever, from anything.
- Anyone who marked a previous message as spam, including complaints arriving through feedback loops.
- Any address that hard bounced on a previous send.
- Anyone who requested account or data deletion. Contacting them may itself be the breach, and in a properly implemented deletion their record should not be there to segment.
- Anyone in an open refund, chargeback or abuse case.
- Role addresses on the cold tiers, meaning generic shared inboxes rather than named people. They complain at higher rates and nobody there remembers signing up.
- Any address whose consent origin you cannot establish, particularly lists inherited through an acquisition, an import or a tool migration, until that origin has been checked.

**Dormant and unsubscribed are not two points on one scale.** Dormancy is a behaviour you measured. Unsubscription is an instruction you were given. The practical trap is structural: many platforms scope an unsubscribe to a list or a stream rather than globally, so building the revival as a fresh list in a new tool is the standard mechanism for mailing people who opted out three years ago. Verify that your suppression file is global and that it survives the import, before the import.

## Part eight. The legal layer, which this file does not answer

**This is the part a skill cannot decide.** Consent rules, the age past which a lapsed address can no longer lawfully be mailed, and what re-permission requires vary by jurisdiction and change. These are the questions to take to a qualified check, stated as of August 2026, and the list is not exhaustive.

- **Which lawful basis covers each segment, because it may not be the same one.** In consent-based regimes there is often a narrower route for existing customers of similar products, so a paid-then-lapsed segment can be mailable while a never-activated free signup from the same week is not. Your segments and your consent position may split along different lines, and that is the case that catches people.
- **Whether implied consent has an expiry, and whether yours has run out.** Canada's anti-spam legislation, in force since 2014, has treated implied consent from an existing business relationship as lasting two years from the relevant transaction and six months from an enquiry, with express consent not expiring. A dormant list is exactly the population where that clock has run.
- **Whether consent degrades even where no statutory expiry is written down.** Regulators in consent-based jurisdictions have generally held that consent is not permanent and should be refreshed, without setting one uniform period. "It has been four years" is a real question.
- **Whether a re-permission email is itself a marketing message.** This is the trap specific to this campaign, and enforcement has been taken on exactly this point: an email asking whether someone still wants to hear from you can itself be direct marketing, and so require the consent it was sent to obtain. "We will just ask them to opt in again" is not automatically the safe option.
- **What the unsubscribe has to do, and how fast.** United States federal law has required opt-outs honoured within ten business days and a valid physical postal address in the message. Large mailbox providers have separately required one-click unsubscribe and processing within two days for bulk senders since 2024. Build to the stricter applicable rule.
- **Whether the addresses transferred lawfully** if they arrived with an acquisition, a merger or a bought list. Consent frequently does not travel.
- **Geographic scoping as the cheap control.** Limiting the campaign to jurisdictions you have actually checked is far cheaper than the compliance work for the ones you have not, and it is one predicate in the query.

## Part nine. Stop rules, written down before send one

| Signal | Threshold | Read it | Action if breached |
|---|---|---|---|
| Spam complaints | 0.1% | After every send, before the next | Stop. No further tier until the cause is found. |
| Hard bounces | 2% | After every send | Pause, clean, verify the next segment before it goes. |
| Unsubscribes | 1% | After every send | Investigate, do not stop. Above 2% on a warm segment the offer is wrong. |
| Clicks | 3% to 5% on warm segments | After send one of each segment | Below the floor, rewrite before send two rather than sending it as planned. |
| Opens | 20% on warm segments | Advisory only | Never the deciding number. Prefetching inflates it. |
| Trial or reactivation start | 5% to 10% warm, 1% to 2% cold | End of each segment's sequence | Below half the floor, do not extend into the next colder tier. |

**On the complaint threshold.** Bulk sender requirements at the major consumer mailbox providers, introduced in February 2024, set a ceiling around 0.3% and recommend staying under 0.1%. Stop at 0.1% anyway, because the published ceiling is where enforcement begins rather than where harm begins. Re-check the current published figures before relying on either. The rest of the table is one operator's default, so replace it after two sends.

**Bounces get cleaned after the first send, not at the end.** Every later tier is larger than the one before it and inherits whatever hygiene the earlier tier established.

## Part ten. The mid-campaign gate, three branches

Run this once the three highest-value segments have completed, typically end of week three or four. Commit the numbers in writing before send one, and measure only new recurring revenue from those segments, counting subscriptions that actually started rather than trials in flight, and counting discounted revenue at the price it is actually billed.

- **At or above the upper end of your combined target:** proceed at full speed into the larger segments.
- **Below roughly 40% of target:** stop and diagnose. Do not answer a weak result by mailing the biggest and coldest list you own, which converts a disappointing quarter into a damaged domain.
- **The branch nobody writes down: if any segment produced fewer than about 100 clicks, you have no verdict at all.** Extend that segment rather than concluding from it. A 190-address segment cannot produce a readable conversion rate in three weeks, and treating its noise as signal is how a good offer gets killed and a bad one gets scaled.

## Part eleven. The decision rule

- **You have accounts, usage and billing data, and clicks you trust.** Run the full plan in tier order.
- **You have billing data but no usable usage data.** Run tiers A and B only. Those four segments are definable from billing alone, they are where the value per hour is, and they are typically a few percent of the list. Instrument usage before the next campaign.
- **Your last send to this list produced complaints above 0.3%, or the domain is currently in trouble.** Do not run this yet. Fix deliverability first, then start again at Tier A with a fraction of the volume.
- **Consent age or origin is unresolved for part of the list.** Run only the part where it is resolved. Do not let an unresolved segment ride along because it is in the same export.
- **You cannot tell, because engagement data is missing or was never tracked.** This is common and it has a real first move rather than a shrug. Go to the billing system, a complete timestamped per-person record that exists even where email analytics never did, and which gives you checkout abandoners, failed-payment churn, cancellation churn and current payers exactly. That is the entire warm half of the campaign reconstructed with no engagement history, and it is where the value per hour sits anyway. Then take last-login timestamps from the accounts table, almost always stored even by teams who tracked nothing else, and split never-activated from everyone else on whether any login exists. If you have neither billing records nor login timestamps, do not mail the list to find out what is in it: verify a random sample of 500 addresses, send it one plain offer-free message with a single question, and read the bounce and complaint rates. That sample is your instrument, and it tells you whether there is a campaign here or a hygiene problem wearing a campaign's clothes.

## Worked example, compressed

A self-serve invoicing tool for freelancers, four years old, 41,200 addresses, standard plan 32 per month. Somebody has proposed one email to everybody with 30% off.

**Suppression first:** 3,100 unsubscribed, 420 previous complainers, 1,860 historic hard bounces, 240 deletion requests. 5,620 removed, leaving 35,580 mailable.

**Segments:** currently paying 610, failed-payment churn 190, checkout abandoners 540, engaged-but-free 1,120, cancellation churn 830, trialists 6,400, never-activated 25,890. The never-activated segment is 73% of the mailable universe. The highest-converting revival segment is 0.53% of it.

| Segment | N | c | V | Value | H | Per hour | Rank |
|---|---|---|---|---|---|---|---|
| Failed-payment churn | 190 | 47.5% | 384 | 34,700 | 2 | 17,350 | 1 |
| Trialists | 6,400 | 2.25% | 384 | 55,300 | 4 | 13,830 | 2 |
| Engaged-but-free | 1,120 | 10% | 336 | 37,600 | 4 | 9,400 | 3 |
| Checkout abandoners | 540 | 10% | 384 | 20,700 | 3 | 6,900 | 4 |
| Cancellation churn | 830 | 17.5% | 160 | 23,200 | 5 | 4,640 | 5 |
| Never-activated | 25,890 | 0.4% | 384 | 39,900 | 9 | 4,430 | 6 |

**A segment of 190 ranks first, ahead of one thirty-four times its size.** And the cancellation segment carries a `V` of 160 rather than 384, because a 60-day free window plus the share who lapse again inside it prices a reactivation there at under half a normal one, which stays invisible until `V` is written down separately.

**The risk constraint then moves one row.** Trialists rank second on value per hour and go fourth in the send order, because 6,400 messages is more than the entire warm tier combined and that value depends on delivery the warm tier has not yet earned. Send order: A payers, B intent segments, C trialists, D never-activated.

**At the gate, end of week three**, counting only started subscriptions: failed-payment churn, 190 sent, 76 reactivated, 2,432 in new monthly recurring revenue. Checkout abandoners, 540 sent, 58 trials started, 31 converted, 992. Engaged-but-free, 1,120 sent, 71 took the half-price offer billed at 16, 1,136. Gate total **4,560** against a proceed number of 4,000 committed before send one. It clears.

**190 addresses, 0.53% of the mailable list, produced 53% of the new recurring revenue.** Under the proposed broadcast those 190 people would have received a discount code for a subscription they never chose to end.

**Two other numbers dissent.** The failed-payment segment produced 61 clicks in total, under the hundred-click floor, so its 40% rate is a promising observation rather than a verdict. And Tier B came in at a 0.12% complaint rate, above the stop threshold.

**Verdict: hold Tier C until the complaint cause is found, extend the failed-payment sequence rather than declaring its rate, and do not mail Tier D as a campaign at all.** Tier D is 25,890 addresses with an expected yield of roughly 78 to 129 conversions, against a verification bill on all of them, an expected 20% to 40% invalid rate, and the complaint profile most capable of taking a healthy domain past the published ceiling. It gets a verified 2,000-address sample with one no-offer email, abandoned outright if that sample complains above 0.1% or bounces above 2%. **The largest segment on the list is the one that does not get the campaign, and that is the right answer rather than a loss of nerve.**

That 73% figure is also publishable, and an honest failure statistic about your own product travels further than a feature announcement. Write it up mid-campaign, while the segments are still being worked.

## Failure modes

**Biggest first.** The largest segment is mailed first because the target is a headcount. Bounces and complaints land before any positive signal exists, the failed-payment segment gets filtered, and the debrief blames the offer.

**Failed payment treated as churn.** A dead card is read as a decision, so those people get the same win-back discount as everyone else. It converts far below what a no-card apology would have, and the team concludes reactivation offers do not work having never tested one.

**The blanket discount.** One code across the list. Current customers learn to wait for the next one, checkout abandoners are offered money off a form they could not complete, and the discount becomes permanent in everyone's price expectation.

**The new-list unsubscribe leak.** The campaign is built as a fresh list in a new tool, the global suppression file does not come with it, and people who opted out years ago receive it. This is the failure that is a legal problem rather than a performance one.

**Opens read as truth.** The open gate passes at 24%, everyone proceeds, and clicks sit at 0.4%. Prefetching produced the opens, and a colder tier is committed on a number that measured a mail client rather than a person.

**A verdict from ninety clicks.** A small segment posts a striking rate on a handful of clicks, the rate is written into the plan as fact, and the next campaign is sized on noise. Same error whether the noise looked good or bad.

**Stop rules read at the end.** The thresholds were written down and reviewed in the wrap-up deck. Every one is a post-mortem finding rather than a decision, which is the same as not having written them.

**The cancellation-reason breakdown ignored.** About half of cancellations typically record no reason and payment failure is frequently the largest stated one, which makes a share of what everyone calls churn a recovery problem rather than a product problem. The roadmap then fixes the wrong thing.

**Reviving into a product that has not changed.** The cancellation email goes out with nothing genuinely new behind it. Reactivations look good in month one, the second departure is never attributed to the campaign that caused it, and the best-informed segment on the list has now said no twice.

**Success theatre on the reactivation count.** Reactivations, opens and list growth are reported with no day-90 retained-revenue figure. Nobody learns that the discounted engaged-free cohort left in month three, and the same offer is chosen again next year.

## What this skill does not do

- It does not decide the legal position, and that is not a hedge. It names the questions in part eight. A qualified adviser answers them, before send one.
- It does not audit or fix the sending infrastructure. A perfect send order on a broken domain achieves nothing.
- It cannot supply the conversion bands or the list-shape profile. Every figure in both was observed on a single account so the ranking arithmetic can be run at all, and both should be replaced after two sends.
- It cannot see whether your engagement data is real. Where opens are inflated by prefetching, the segmentation and the gates both read a signal with no person behind it.
- It does not recover failed payments. The billing system does that automatically and better, and the failed-payment email is for the people the automation could not save.
- It does not fix the product, the onboarding or the reason people left. Run against an activation problem, it refills the top of a funnel that has already shown it cannot hold anyone, and it gets credited with the reactivations rather than the second departure.
