---
name: email-list-giveaway-campaign-builder
description: Plans a complete email list giveaway campaign, from a product, an audience and a subscriber-growth target, optimised for net new subscribers rather than raw entries. Produces the prize stack and the relevance filter that keeps it from attracting prize hunters, the partner outreach sequence with what each message contains, the referral point weighting, the entry flow, the promotion calendar, the viral coefficient arithmetic that predicts before launch whether referrals will compound, and the post-campaign sequence that filters the entry pool down to subscribers who actually engage. This skill should be used when a product, an audience and a subscriber target are known and a full giveaway campaign, from prize selection through the post-campaign nurture sequence, needs to be built end to end.
---

# Email list giveaway campaign builder

## The claim this skill is built on

An email list built through a giveaway is judged on subscribers, not entries. Entries are a vanity count, inflated by exactly the mechanics that produce a worthless list: a prize with universal appeal, an unverified referral loop, a form that asks for nothing more than a click. What matters is net new subscribers who go on to open, click and eventually buy, always a fraction of raw entries once duplicates, existing customers and never-opened addresses are removed. Every mechanic in this file, the prize filter, the opt-in requirement, the points cap, the post-campaign sequence, exists to raise that fraction, not the entry count.

The obvious approach conflates the two: pick something desirable, put up a form, email your list, wait for entries, call a big number a win. That reliably produces entries, not subscribers worth having six months later, because nothing filters for buyer fit or verifies that an entry is a real, engaged address.

What turns a giveaway into a subscriber-acquisition engine rather than a one-off email with a raffle attached is compounding: if the average entrant refers more than one confirmed friend, reach keeps growing without further spend. Whether that average clears one is a single number, and it decides in advance whether the mechanic is self-sustaining, before the prize, the partners or the copy matter.

## Part one: the coefficient, estimated before launch

The standard shape of the number is `k = i x c`, where `i` is the average referral clicks one entrant generates and `c` is the share of those clicks converting into a confirmed entry. Both halves can be estimated before launch, from three inputs you set yourself.

**Share participation**, the percentage of entrants who use the share mechanic, sits near 1 to 3% when sharing is an afterthought below the fold, and can reach 30 to 50% when it is the dominant post-entry action, rewarded with extra entries. **Average reach per share**, clicks per share, runs 2 to 8 for a personal network share on a specialist audience, higher for broad-appeal prizes. **Entry-form conversion**, clicks completing a confirmed entry, runs 20 to 40% on a single-field, purpose-built page, lower on a form demanding an account or several fields.

Multiply through and the arithmetic tells you, before launch, whether the default design is viable. Participation of 20%, reach of 4, conversion of 30% gives `i = 0.8` and `k = 0.24`, well under one. Raising participation to 45% (weighting referral heavily, covered in part four), reach to 6.5, conversion to 35% gives `i = 2.9` and `k ~ 1.02`, clearing the line. The gap sits entirely in decisions you control: the post-entry screen's dominant action, the referral weight, and how many fields the form asks for.

If entries compound generation over generation at rate `k`, the total pool from a seed of `D` direct entrants (everyone from your own list, a partner send, an advert or organic social, none via a referral link) sums as a geometric series: `D / (1 - k)` when `k` is below one, and grows without a fixed ceiling, bounded only by the addressable audience, when `k` is at or above one. This is a projection of entries, not subscribers; part seven covers what fraction survives to become one.

## Part two: the prize stack and the relevance filter

A prize has to pass two independent tests; satisfying only one is the most common way a giveaway produces a large entry count and a low-quality list.

**The relevance test.** Would somebody with no interest in your product category still want this prize? If yes, for a broad slice of the general population, the prize fails regardless of desirability. Cash, general-purpose gift cards, and broad electronics like tablets, phones, consoles and drones fail almost every time, since their appeal sits outside any specific buyer category, producing a large entry count and a list whose open and click rates sit well under your average. This is the filter deciding whether an entrant can become a subscriber at all.

**The pull test.** When your buyer reads the prize in one sentence, do they want to act immediately, or is it reasonable but unremarkable? A prize clearing relevance but not pull produces a quiet campaign nobody bothers to share, why `i` in part one collapses toward zero regardless of point weighting. Both tests must pass together: relevant but unremarkable rarely clears pull, pull without relevance recruits the wrong list.

**Build a stack, not a single item, and score it.** A multi-item prize raises perceived value and forces a check on every line, not one headline item. Compute a relevance ratio: retail value of line items inside your buyer's existing spend category, divided by total stack value, held at or above 80%. A broad-appeal item, a gift card covering incidental costs, is acceptable as a minor share, never the anchor, never past roughly 20%.

**Size the stack against your reach, not against ambition.** A working range, not a formula: total retail value between 0.3 and 1 times your reachable list size in dollars usually reads as substantial without becoming a bigger story than the product it sells.

## Part three: the partner acquisition sequence

Extra partners raise the stack's value without raising your cost, add an audience roughly matched to yours (the authority-piggyback effect), and add reach that shows up as extra direct entrants, `D`. The sequence runs in order, since each step gates the next.

**Step one, build the list against a ratio, not a headcount.** Pursue a partner whose audience sits roughly between one tenth and ten times your own reachable list. Far smaller and the added reach is negligible; far larger and you become an afterthought inside their priorities. Screen out direct competitors.

**Step two, find two names per company:** the decision owner, and the person who would execute. A message to only the decision-maker gets forwarded and forgotten; to only the operator, shelved pending an approval that never arrives.

**Step three, send one message to both, about their exposure, not your ask.** State your reachable audience size, describe the format in two sentences, and ask for one small first yes. Do not ask for free product yet: asking for value before establishing any reads as risk, and response rates on the combined ask run consistently worse than the split version.

**Step four, once they agree, send the real ask:** social proof, the mechanic and dates, then a unit or licence count for several winners, with a retail value floor, commonly $150 to $300 per partner, so no contribution drags the relevance ratio down.

**Step five, collect every code, unit and shipping commitment in writing at least ten business days before launch,** the single most common cause of a slipped launch date otherwise; for a physical prize, have the partner ship directly to the winner.

**Step six, at launch send every partner one asset pack:** platform-sized captions, graphics, one swipe-copy email block, and a unique link per partner, the only record of which partnership drove entries versus only reach.

**Step seven, within 48 hours of close, send a thank-you and a partner-specific result.** Tell each partner the entry count their own link produced, but do not hand over the entrant list: a partner's return is exposure, not names they did not collect.

## Part four: entry flow and point weighting

**Weight referral well above any passive action, or the mechanic will not fire.** A workable starting table: a base entry (one email field, no account) is worth 1 point; a confirmed referral, a friend entering through the unique link and completing double opt-in, 3 points; a social follow, 1 point, since it does not recruit anybody; a daily return visit, 1 point, capped at the campaign's length. Cap total referral-earned points per entrant, commonly around 60 (20 confirmed referrals), to blunt abuse.

If referral and a passive action are weighted the same, most entrants take the lower-effort option regardless of prize quality, since the point value is the incentive actually observed. That gap is what pushes participation toward the 40 to 50% band that clears one in part one.

**Require double opt-in confirmation on every entry, not only referrals.** This is the single largest lever on list quality: it blocks the disposable addresses referral farming produces, and means every subscriber has confirmed a real inbox, which is what part seven's engagement numbers are built on. The sign of farming is entry clusters from disposable-mail domains or a referral burst with no matching rise in traffic; the points cap limits the damage even when a batch slips through.

**Make sharing the dominant, first post-entry action**, with several one-click pre-filled options rather than one generic button and no caption to write, since every extra decision before sharing is a chance to close the tab.

**Platform and legal constraints, and they change without notice.** Meta's promotion terms, current as of August 2026, prohibit a native action, timeline sharing, tagging friends, liking a page, as the entry mechanic itself; a referral link an entrant chooses to post is treated differently, but read the live terms before every campaign. CAN-SPAM requires a working unsubscribe link and a real postal address on every email. In Canada, a no-purchase giveaway can still fall under lottery law unless structured correctly, fixed with a skill-testing question before a prize is awarded; Quebec separately requires gaming-regulator registration and a fee past a prize-value threshold it revises periodically. Under GDPR and UK equivalents, a draw entry and marketing consent are different acts, so one pre-ticked box is not valid consent: use a separate, unticked marketing checkbox. A mechanic that fails any of this produces a list you cannot legally mail.

## Part five: promotion calendar and campaign length

**Run the promoted window for 10 to 14 days.** Referral chains need several days to propagate, since people check social feeds asynchronously. A longer window does not produce more shares per person, since an entrant will share the same contest once or twice before repeating it reads as spam; it only spaces the same finite shares out while urgency fades.

**Two forced pulses, one optional touch between them.** Day zero: your own list, every confirmed partner send, and social posts, timed together. The final 24 to 48 hours: a last-chance email to your entire audience, unsegmented, since entrants can still earn points by referring before close. Around day six or seven, a single re-engagement touch keeps the referral rate from decaying, ahead of the normal U-shaped curve: a spike on day one, a trough mid-window, a second spike in the final 48 hours. Adding spend or extending the window because the middle looks slow blunts the final push instead.

## Part six: reading the checkpoint, the decision rule

Measure the coefficient live at the 72-hour mark: `k ~ R / D`, referred over direct entrants (everyone else: email, partner sends, organic social, adverts). This is the practical version of the part one formula, since entry-source attribution is a default field in almost any giveaway tool, whereas share-click tracking usually is not.

- **`k` is 1.0 or higher.** Genuinely viral. Let it run without adding paid spend, keep the partner asset pack current, and expect the day-six touch and the last-chance email each to trigger a fresh compounding wave.
- **`k` is between 0.5 and 1.0.** Partially viral. Referral is doing real work but will not compound on its own; total entries via `D / (1 - k)` land at two to three times direct reach rather than growing unbounded. Worth adding paid or partner seed traffic here, but do not budget as though the campaign carries itself.
- **`k` is under 0.5.** Not doing enough work to matter on its own. Treat as a design diagnostic, not a promotion problem: check whether sharing is genuinely the dominant post-entry action, whether the referral weight is high enough relative to passive actions, and whether the prize clears the pull test, before spending further on reach.
- **You cannot tell.** Below roughly 150 to 200 total entries in the window, the ratio is noise: one unplanned reshare from a well-followed account can swing `R` by dozens and flip the read. Extend the checkpoint to day five, or wait until the window crosses that floor before trusting it.

A high `k` says the entry pool will grow, not how much of it survives into part seven as a real subscriber; that filter is separate and comes next.

## Part seven: the post-campaign sequence that turns entries into subscribers

This is where the vanity number from part one becomes real. Most entrants joined for the prize, not because they researched your product, so dropping them into your normal cadence the day the campaign closes turns a large entry count into a damaged list.

**Send the winner and consolation email within 24 hours of close.** This is the highest open rate you will ever get from this list, two to four times a normal send, since the subject line answers a question every entrant wants answered. Use it deliberately, a soft offer for non-winners, since its goodwill carries the rest of the sequence.

**Treat the sudden size increase as a deliverability event, not just growth.** Importing low-intent addresses onto a warmed domain and mailing them at your normal cadence spikes spam complaints and degrades inbox placement for your entire list. Google and Yahoo's bulk-sender requirements, in force since February 2024, set an enforcement ceiling around a 0.3% spam-complaint rate, 0.1% the recommended safe margin, so keep the cohort on a separate segment or sending domain for the first 30 days.

**Days two to ten: reintroduce the product, do not sell it.** Three or four short emails on what it does and who it is for, since this cohort has not done the research a normal signup has.

**Days ten to fourteen: the first real offer, gated by engagement.** Send a discount or trial only to entrants who opened or clicked in the nurture sequence; non-openers get one re-engagement line first. This gate is the real list-quality filter: an entrant who never opens anything is counted in part one's total but never becomes a subscriber.

**Day thirty: sunset what never opened.** Expect 40 to 60% of a giveaway-sourced list to never open a single follow-up, since intent here is lower than an organic signup by design. Send one last email, then suppress non-responders rather than keep mailing a segment damaging your sender reputation. What is left, not the launch-day entry count, is the number to report.

## Worked example, compressed

A desktop scheduling tool for small construction contractors, current list 4,200, target 8,000 net new subscribers who go on to open, click or buy inside one quarter, not 8,000 entries.

**Prize stack.** A rugged job-site tablet with a year of the premium tier (headline item), a construction accounting plan from a partner, a power-tool bundle, a trade insurance discount, and a $700 gift card as the flexible component. Total value $3,800; relevance ratio `(3,800 - 700) / 3,800 = 81.6%`, clearing 80% narrowly.

**Partners.** Four companies with audiences of 3,000, 8,000, 12,000 and 40,000. Against the 4,200 base list, ratios are 0.7x, 1.9x, 2.9x and 9.5x, all inside the 0.1x to 10x band.

**Points.** Base entry 1, confirmed referral 3 capped at 20 referrals per entrant, follow 1, every entry double opted in.

**Checkpoint at 72 hours.** Direct `D = 1,850`, referred `R = 640`, `k ~ 0.35`, well clear of the noise floor: the under-0.5 band, referral is not carrying it.

**Twelve-day projection.** Cumulative direct entrants reach `D = 5,800`. `D / (1 - k) = 5,800 / 0.65 ~ 8,923` total entries, roughly 92% net new, about 8,210.

**Verdict.** Against the actual goal, net new subscribers rather than raw entries, the target is met by roughly 200. Run part seven's engagement gate first: if this list follows the 40 to 60% never-opens pattern, only around 3,300 to 4,900 of those 8,210 ever engage past the winner email, and that smaller number is what next quarter's plan should be built on. The entries came from partner reach, not virality: at `k = 0.35` the design sits in the diagnostic band from part six, so fix the share prominence and point weight before treating this campaign as proof the design works.

## Failure modes

**Share-afterthought design.** The post-entry screen's dominant action is a follow or a like rather than a share, and `k` collapses toward zero, since the path of least resistance never asked the entrant to bring anyone else in.

**Prize-hunter contamination.** The prize clears pull but not relevance: cash, a broad gift card, general electronics. Entries look strong, but the list's open and click rates sit well under the rest of it, and few ever buy, the failure the relevance rule exists to prevent.

**Referral farming.** No double opt-in and no points cap lets entrants create disposable addresses and refer themselves, visible as entry clusters from throwaway-mail domains or a referral burst with no matching rise in traffic.

**A platform-mechanic violation.** The entry rule requires a native share-to-timeline action or a friend tag on a platform whose terms prohibit it. The result is a pulled promotion or a suspended account mid-campaign, and every entry collected so far is lost with it.

**The cold ask for free product.** The first message to a partner leads with a request for licences before establishing what is in it for them; response rates run visibly worse than a split sequence, and most go unanswered.

**Dumping the new list into the regular send cadence.** Low-intent addresses land on a warmed domain and get folded into the usual schedule the day the window closes, spiking spam complaints and degrading inbox placement for the entire list, a subscriber-quality failure with no upside.

**The too-early keep-or-kill call.** A handful of entries in the first few hours produce a `k` reading that is really noise, and a decision to cancel paid support or declare the mechanic broken gets made off a number that had not crossed the reliability floor.

**The prize fulfilment gap.** Codes or units are not collected until after launch. Winners are announced on schedule, delivery slips for weeks, and complaints land on the same channels used to promote the campaign.

**Middle-of-campaign panic.** The natural mid-window trough in the entry curve reads as failure, and paid spend or an extended window mutes the final 48-hour push instead of fixing anything.

## What this skill does not do

- It does not know your list's real share behaviour. The point weights, participation assumptions and relevance rule are defaults, not measured facts about your audience.
- It does not find or negotiate with partners. The company list, the relationship, and the terms of exposure come from you or a partnerships effort.
- It does not configure or build the referral-tracking mechanism or the double opt-in flow. Your platform choice is a separate engineering decision determining which events you can measure.
- It does not track current platform promotion policy in full. Meta, X and other platforms revise their rules without notice, and the live policy has to be read before each campaign.
- It does not cover sweepstakes law, registration thresholds, or consumer-protection rules for your country or state. Those need a lawyer's review before a prize of meaningful value goes live, particularly for Canadian entrants.
- It does not measure your actual post-campaign open and click rates. The 40 to 60% never-opens figure is a planning assumption; your own send data replaces it after the first campaign.
