---
name: lifetime-deal-listing-kit
description: Assembles the complete submission packet a third-party lifetime-deal marketplace requires before it will build your sales page, in the order the pieces depend on each other. Covers the qualification arithmetic that decides whether to list at all, the tier ladder and seat semantics that stop stacking being gamed, the redemption code batch, the zero-value order check that decides whether a fully discounted redemption provisions anything, the fifteen-item asset manifest with per-asset rules, and the reply that goes back to the platform contact. This skill should be used when a lifetime-deal or one-time-payment marketplace has asked for assets, when a lifetime tier is being priced against an existing monthly plan, or when redemption codes are about to be generated.
---

# Lifetime deal listing kit

## The claim this skill is built on

A lifetime-deal marketplace is not a channel you launch on. It is a third party that owns the storefront, sets the required fields, takes a share of every sale and builds the sales page out of assets you send it. The work is packet assembly and integration verification, and the two things that go wrong are not copywriting.

**The first is order.** The required assets are not independent. Most of them quote a price, an allowance or a tier name, so anything written before the ladder is fixed gets rewritten. Teams reliably draft the long description, the FAQ and the comparison chart first, then rebuild all three the afternoon the pricing moves. Everything below is sequenced so that no step needs a decision from a later one.

**The second is that the whole deal rests on one path through your own billing system that has probably never run in production: an order worth nothing.** Everything the marketplace sells is redeemed with a fully discounted code, and a zero-total order can take a different route through a billing integration than a paid one. When it does, nothing errors, and you find out from refund requests on a public review wall on launch day.

**On the numbers in this file.** Tier counts, code volumes, discount bands, expiry windows and the asset list are the shape common across these marketplaces as of August 2026. Each sets its own and changes them without announcement, so treat every figure as a question for your account manager and confirm the current requirements in writing before assembly starts.

## Part one. Whether to list at all

A lifetime licence sold through a marketplace is a one-off payment against a perpetual obligation. If your product carries real recurring cost per account, you are not running a promotion, you are acquiring a permanent liability at a discount you set yourself. The qualification step has to be able to answer no, and it needs arithmetic rather than a feeling.

### The arithmetic

```
P  = net proceeds per redemption
   = deal price
     × (1 − platform revenue share)
     × (1 − refund rate)
     − any remittance or tax cost that falls on your side

m  = recurring marginal cost per active account per month
   = infrastructure attributable to one account
   + metered third-party pass-through per account
   + (support tickets per account per year × minutes per ticket
      × loaded cost per support minute) / 12

H  = P / m        the payback horizon, in months, that one redemption buys
```

Three notes on the inputs, each a place people flatter themselves.

**Revenue share is a contract term, not a published fact.** A share in the region of 30% of the deal price is commonly reported for these platforms, but it varies by marketplace and by negotiation. Get the number in writing, and separately confirm whether refunds come out of your share or the platform's, because that clause moves `P` more than the price does.

**Price the cost side off the heavy tail, not the average.** Lifetime buyers do not behave like subscribers: some go dormant within a quarter and some use every unit of the allowance precisely because it is already paid for. Use the ninetieth percentile of allowance use, because dormant accounts do not refund the cost of the heavy ones.

**Support is the term people leave out**, and usually the largest one for a desktop or self-hosted product. Lifetime cohorts generate more tickets per account than subscribers do, not fewer.

### When the cost grows with age

If `m` is flat, `H = P / m` is the whole story. If it grows, usually because the account accumulates retained data, the flat calculation overstates your runway. Model it as `m(t) = m₀ + g·t` and find the month `T` where cumulative cost passes `P`:

```
m₀·T + (g·T²)/2 = P
```

A retained-storage product with a modest growth term comes out months short of what the flat calculation promised, and the gap widens every year the account stays open. **A cost that grows monotonically with account age is the shape that makes lifetime deals dangerous**, because the liability compounds while the revenue was banked once.

### The decision rule

- **`m` is near zero, because the work happens on the buyer's own machine and there is no per-seat server cost.** List. Name the residual honestly: support, updates and platform compatibility never go to zero, and you owe them for as long as the product exists.
- **`H` is 60 months or more and the population is bounded by the code batch.** List. Cap the batch, because the batch is the liability, not the individual code.
- **`H` is between 24 and 60 months.** List only with two conditions in writing: a hard cap on the number of codes, and an allowance capped per code rather than laddered without limit. Write the dormancy assumption down, so that when it turns out to be wrong you can see which assumption failed.
- **`H` is under 24 months.** Do not list. You are selling a subscription with the payments removed, at a discount to a discount.
- **`m` grows with account age and cumulative cost passes `P` inside 36 months.** Do not list at that ladder. Cap the growing term with a stated retention or usage limit and recompute, or decline.
- **You cannot tell, because cost has never been separated by account and there is no ticket-per-account data.** This is the common case and it gets a procedure rather than a guess. Do not list yet. Run a 30-day instrumentation pass: tag infrastructure and metered spend by account, count support tickets per account, then compute. If you genuinely cannot instrument in the time available, the only defensible listing is a bounded pilot: one small tier, a hard cap of a few dozen codes, an allowance small enough that the outcome is a number you can name out loud, and a written internal review date before any second batch.

## Part two. The agreement block, four items, in the first reply

These gate everything downstream and take days to obtain if the company is not yours alone. Answer them in your first reply, even if the rest of the packet is empty.

1. **Legal entity name** exactly as registered, which is what appears on the contract and the payout details.
2. **Registered address.**
3. **Named director or officer** authorised to sign.
4. **Plan details:** every plan you currently sell, their prices and their allowances, and which of them the deal maps onto.

Ask in the same message for their current submission form and asset specification. Item four is the one that quietly matters: the plan set you state here is the set the marketplace builds tiers against, and changing it later means renegotiating rather than editing.

## Part three. The tier ladder, before any asset

**Three paid tiers is the shape most of these platforms expect as of August 2026.** Leave your free tier in place on your own site as the non-deal entry point, and keep it out of the deal, so the listing never cannibalises self-serve signup.

**Set a retail lifetime anchor for each tier first, then discount from it.** A lifetime price around 8 to 9 times the monthly plan that tier maps onto is a common anchor, and the marketplace tier sits roughly 25% under it. These platforms generally expect the 20 to 40% off band: under 20% they push back, and over 40% you have permanently devalued your own pricing page, which the deal page outranks in search for your brand plus the word discount.

**Frame the discount as marketplace-exclusive launch pricing, and then honour it.** Your own site must not sell the same thing cheaper, or at the same price, for the duration of the window. That is usually a contract term as well as a courtesy, and it constrains any promotion you had planned for those weeks.

**Everything you list in a tier has to be live at go-live.** Lead time from asset submission to a live listing is commonly around three weeks, so a feature described as shipping soon has to be genuinely shipped by then or it comes out of the tier. That is why the roadmap and the tier contents are separate assets: one is a promise, the other is what a buyer receives on the day.

**Ladder the metered allowance rather than the price alone.** Roughly 1x, 4x and 6.5x the standard plan's allowance across the three tiers reads as generous without inventing a new product. Then add a **one-time bonus grant of about 6 times that tier's monthly allowance**, which is the most useful move in the ladder: a one-time grant is a bounded liability, whereas raising the recurring allowance raises `m` forever.

**Decide seat and stacking semantics explicitly, and write them in one sentence.** The wording that survives contact with buyers is: one code upgrades one account, codes do not stack allowances on a single account, and a buyer may purchase several codes and distribute them to separate accounts. **Put that sentence in the sales copy and in the FAQ in identical words.** Ambiguous stacking is the single most common post-launch dispute, and the marketplace will side with whatever the published page says.

Where a platform requires stackable tiers, and some do, the ladder has to define the stack: state exactly what two codes of the same tier produce, and cap the stack at a named number. An uncapped stack on a metered product is an uncapped liability.

## Part four. The code batch itself

Build the batch before the integration check, because the check has to be run with a real code from the real batch.

- **Roughly 100 single-use codes per tier**, 300 in total, is a common first batch, and the platform may specify its own size or issue codes itself. Ask before generating anything.
- **Reconcile the batch size against the volume the platform expects from the window**, which can run into the low thousands for a promoted deal. Agree the reissue procedure in advance, in writing, because running out of codes mid-window is a stockout on somebody else's storefront and you cannot fix it at the weekend.
- **Every code 100% off and restricted to the deal product.** An unrestricted fully discounted code is an unbounded liability the moment it escapes.
- **A distinct human-readable prefix per tier**, so a mis-redemption is visible at a glance in a support ticket rather than needing a lookup.
- **One expiry for the batch, roughly six months out.** Codes with no expiry surface on coupon aggregator sites years later and still work.
- **Stamp the campaign name and tier onto every coupon and every code as metadata.** This is the only thing that makes the clawback query in part nine possible, and it cannot be added retrospectively to orders that have already been placed.
- **Pin your payment processor's API version when bulk-creating.** Promotion-code objects get redesigned between releases, and a batch built on a drifting version can behave differently from your live checkout.
- **Generate in live mode, then read one code back from live before delivering the file.** A batch generated in a test environment looks perfect in the spreadsheet and returns no such code on every redemption.
- **Deliver as a spreadsheet with one sheet per tier, one combined sheet and a summary sheet**, in a format that opens on any operating system without a conversion step.

## Part five. The zero-value order check

**This is the step that decides whether the deal works at all, and it is invisible in advance.** Run it before you promise the platform anything, because the fix can be a code change with its own release cycle.

The mechanism, stated precisely, because the vague version of it prompts nobody to act. A 100%-off code, or several stacked to the same effect, produces a checkout session whose total is zero. **A zero-total order does not necessarily emit a payment event, so a fulfilment path triggered by a payment, a charge or a paid invoice never fires.** Checkout completes, the buyer sees a confirmation, nothing provisions. Because every buyer on the deal arrives through that same path, it does not fail for a few edge cases. It fails for the entire cohort, on day one, in front of the marketplace's whole audience.

Trace one zero-total order end to end through the real storefront, and confirm three things.

**One: your checkout accepts promotion codes at all, and does not demand a card it does not need.** On several hosted checkouts, code entry is off unless explicitly enabled. On Stripe Checkout, verified against the published API reference as of August 2026, `allow_promotion_codes` is a nullable boolean that stays unset until you enable it, so a checkout that has never taken a discount shows no code field and the buyer's report is that there is nowhere to put the code. In the same reference, `payment_method_collection` defaults to `always`, with `if_required` documented as collecting a payment method only when there is an amount due, so a fully discounted session can still ask a buyer who owes nothing for card details.

**Two: your fulfilment webhook routes on order metadata, never on the amount paid.** The metadata it should route on is the plan key plus an explicit lifetime flag you set when the session is created, so provisioning is a lookup rather than an inference. On Stripe, a session's `payment_status` is an enum of `paid`, `unpaid` and `no_payment_required`, the third being documented for sessions that require no payment at that moment. A handler written as "if payment status is paid, provision the account" is therefore testing one value of a three-value enum, and which value your zero-total flow actually reports depends on the mode you use and how the discount is applied. Do not reason that out from first principles: read the reference for the flow you have built, confirm the value on a real order, and key provisioning off the session completing plus your own metadata rather than off an amount. The same caution covers anything waiting on a charge, because an order that charges nothing may create no charge object to wait for.

**Three: a zero-value order provisions identically to a paid one.** Same entitlement record, same plan assignment, same expiry semantics, same welcome and receipt emails, same behaviour when the account is later upgraded or refunded. Check the entitlement row in your own database, not the confirmation screen.

**Do it once, yourself, end to end, in production**, with a real code drawn from the live batch, on a real account, through the live storefront, then revoke and reissue that code. A sandbox that fires a synthetic payment event will pass a check the live zero-total path fails, which is the worst possible outcome: a green result that proves nothing. The failure you are hunting only appears on the path you did not take.

**Record the verification and its date in the packet**, and re-run it after any billing change before launch day.

## Part six. The asset manifest, fifteen items

The listing does not get built until this is complete. Draft everything you control immediately and put an owner and a real date against the rest.

1. **Redemption code file**
2. **Written redemption steps**, numbered and literal enough to follow without a screenshot
3. **A demo video that actually works**, showing the product doing the job rather than touring the interface
4. **Product access link**, an evaluation account for the platform's review team
5. **Text in three lengths:** a one-line tagline, a short description of about 50 words, a long description of about 300
6. **Product images** at the platform's stated dimensions
7. **Comparison chart** naming three to five real alternatives, on rows a buyer can verify without your help
8. **Product banner**
9. **FAQs**
10. **Support email and social handle**
11. **Existing customer reviews**, date-stamped, with the avatar assets packaged alongside, and flagged where they describe a release that has since changed
12. **Product roadmap**
13. **Named alternatives**, the products you expect to be compared against
14. **Use cases, ordered by the roles most likely to buy**, not by the roles you find most interesting
15. **A first-person founder's note**

If your product ships on more than one desktop platform, items 3, 4 and 6 have to cover each of them. A demo recorded on one operating system, with an evaluation build for that same one, tells the review team and then the buyers that the other platform is an afterthought.

## Part seven. Per-asset rules that decide whether the asset works

**Redemption steps.** Numbered, six or seven, naming the exact button labels the buyer will see rather than paraphrasing them. End with the instruction to sign in with the same email address used at checkout, because mismatched emails are the largest category of redemption ticket.

**Comparison chart.** Eight or nine rows against three to five named competitors, on criteria a buyer can check for themselves in an afternoon, and at least one row where a competitor wins. An all-green column reads as fiction and gets challenged in the comments by somebody who uses the competitor daily.

**Roadmap, which is a commitment rather than copy.** Three buckets: ships before the listing goes live, next three months, next six months. The wording these marketplaces expect at the end of it is close to "lifetime customers get every feature on this roadmap at no extra cost, for as long as the product exists", so either write that deliberately or scope the roadmap smaller. Never list a shipped feature as roadmap, and never list a roadmap item as live. Anything in the first bucket has to be genuinely live by go-live, because the lead time is short and a slipped promise is on the page permanently.

**Product banner.** Three crops of one composition: 1200 by 630 pixels for the sales-page hero, 1080 by 1080 for the storefront grid and social shares, 800 by 1000 portrait for mobile. The price flag must be legible at thumbnail size, which is where most buyers first see it. Ship PNG plus a layered source file so the marketplace can swap the copy. Match the storefront's own theme, and most are light, so a dark hero reads as a foreign object. In the design brief, ban the category's cliche icon by name.

**FAQs.** Whatever else you include, answer these: what stacking does, in the exact words from part three; the refund window and who administers it; what happens if the company stops trading; what lifetime means, which is the lifetime of the product and not of the buyer; and what is not included.

**The founder's note**, the asset that does the most work and the one most often written last. First person. Name the shared lie of the category, show the bad experience verbatim, then the same task done properly. It is the one place on a marketplace page where the writing is not interchangeable with every other listing that week.

## Part eight. The reply to the platform contact

**Send what you have now**, and list what is outstanding with a specific window such as 48 hours, rather than holding the whole packet until the last asset is perfect. The listing will not be built from a partial package, so the date you give has to be real, but a packet that arrives in two parts still starts the clock. One held for a missing video misses the slot.

**Mirror their numbering exactly**, item one to item fifteen, so nothing reads as missing and nobody has to map your structure onto theirs.

**Restate the commission split as you understood it**, in writing, in the same message. Not distrust, just the cheapest available correction point.

**Ask to see the draft sales page before it goes live.** They build it from your assets and they will get something wrong, usually a price or an allowance, and the time to catch that is before the page is public. Offer a short call to unblock anything, and stay in the existing email thread rather than starting a new one.

## Part nine. Clawback readiness, written before launch

Write the query before the first refund request, not after. Against your payment provider, it lists checkout sessions, filters by your coupon identifiers, and emits **buyer email, tier and timestamp**. That is what makes refunds, abuse and duplicate-account disputes resolvable, and without the metadata from the code batch you cannot write it at all: nothing on the order says which campaign it belongs to.

Then run the reconciliation that catches a silent failure early: **codes issued, codes redeemed, entitlements granted.** Those three numbers must agree. If redemptions exceed entitlements, part five failed and every buyer in the gap is currently holding a receipt for nothing.

## Part ten. The window, and why it changes what you submit

The shape below is observable platform practice rather than published policy, from deal-flow mechanics broadly consistent since roughly 2018 to 2020. Marketplaces vary and terms change, so confirm the current version with your contact rather than planning against this paragraph.

**The deal typically goes soft-live around two weeks before the promoted launch.** Early buyers purchase, use the product and leave feedback while the offer can still be changed. That period is not a formality. **Bad early reviews can end the deal before it is promoted at all**, and the promotion generally only proceeds if some combination of margin and satisfaction rating clears an internal threshold you never see.

Three consequences. The soft-live fortnight is the real integration test, in front of buyers, so part five has to have passed before it starts rather than before the promoted date. The first handful of redemptions produce the reviews that decide the rest, so redemption steps and support responsiveness matter more then than in any week after. And do not try to influence reviews on the platform during the window: incentivised reviews are commonly prohibited outright, and being seen to solicit them loses a listing faster than a bad review does.

**The promoted window itself usually runs about 7 to 10 days, closed by a last-chance email roughly 24 hours out.** Two emails is the whole campaign, launch and last chance, with existing buyers excluded from the second. A successful window can produce buyers in the hundreds to low thousands, and that, not the size of your code batch, is the number to multiply your per-account cost by in part one.

## Worked example, compressed

**Case one: a desktop schema comparison tool for developers.** Monthly plan 15 a month, including three connected projects and 200 comparison runs. Diffing executes on the buyer's own machine.

Costs: infrastructure for licence checks and settings sync at 0.10 per active account per month, no metered third-party cost because the compute is local, and support at 0.8 tickets per account per year, 11 minutes each, loaded at 0.75 per minute, which is 0.55 a month. So `m` is 0.65 and flat.

Ladder, built before any asset. The three plans it maps onto sell at 15, 29 and 45 a month, so the retail lifetime anchors are 129, 249 and 379, each about 8.5 times monthly, and the marketplace tiers sit at 99, 189 and 289, which is 23 to 24% under each anchor. Projects ladder 3, 12 and 20, roughly 1x, 4x and 6.5x. Comparison runs ladder 200, 800 and 1,300 a month, each tier carrying a one-time bonus grant of six times its own monthly allowance.

Net proceeds on the entry tier: 99, less a 30% platform share, less a 9% refund rate, about 63. Payback horizon `H` is 63 divided by 0.65, about **97 months**.

**Verdict: list.** Ninety-seven months against a flat cost is comfortable, and it is flat precisely because the expensive work happens on the buyer's own hardware. Size the liability against the window rather than the batch: at 2,000 buyers and 0.65 a month, the obligation is about 1,300 a month in perpetuity, a number you can say out loud in a meeting. Proceed to the agreement block, the code batch, then the zero-value order check before agreeing a date.

**Case two: a hosted video transcription service.** Monthly plan 29 including 300 minutes. The proposed ladder is 179, 299 and 449, with 500, 1,200 and 2,000 minutes a month plus one-time bonus grants.

Costs on the top tier: infrastructure 0.40 a month, transcription pass-through at 0.015 per minute against a ninetieth-percentile use of about 1,100 minutes, which is 16.50, and support at 0.35. So `m₀` is about 17.25, and because media and transcripts are retained indefinitely, cost grows at roughly 0.45 a month per account.

Net proceeds on the top tier: 449, less 30%, less 9%, about 286. The flat calculation gives 16.6 months, which already fails the 24-month floor. Solving the growing version, `17.25T + 0.225T² = 286`, gives about **14 months**.

**Verdict: do not list at this ladder.** Fourteen months of cover on a perpetual obligation, against a cost that rises every month the account exists, is a liability that outlives whoever approved it.

**What would have to change.** Cap the top-tier allowance at 400 minutes a month, state a 12-month media retention with transcripts kept, and price that tier at 249. `m₀` becomes about 4.05 with near-flat growth, net proceeds about 159, and the horizon lands near **39 months**, inside the conditional band: allowed only with a hard cap on the batch and the dormancy assumption written down. If the platform will not run an allowance that modest, and many push for a generous headline number, the answer is no. Counter-offer a time-limited discount on the annual plan, where the obligation ends when the term does.

## Failure modes

**The silent zero-value order.** Provisioning branches on the amount paid or on a payment-succeeded event. Every redemption completes, every buyer gets a receipt, no account is upgraded. From the outside it looks like an outage, and the first report arrives as a one-star review rather than a support ticket.

**The promotion-code field that was never enabled.** The checkout has no code entry because the flag controlling it was left at its default. The batch is fine, the page is fine, and the buyer's message is that there is nowhere to enter the code.

**Codes generated in a test environment.** The spreadsheet is immaculate. Every live redemption returns no such code, and reissuing means asking the marketplace to redistribute codes to buyers who already hold one.

**Price drift between the deal page and your own pricing page.** The ladder was fixed in week one, the pricing page changed in week three, nobody updated the assets. Buyers put the two pages side by side in the comments, and the discount now reads as invented.

**Ambiguous stacking.** The sales copy implies allowances add up, the FAQ implies they do not, or neither says. Buyers stack three codes on one account, expect three times the allowance, and the dispute is adjudicated against the published page rather than what you meant.

**The unrestricted fully discounted code.** A 100%-off code not restricted to the deal product works across the catalogue. One posted screenshot later, it is an open door with your name on it.

**Codes with no expiry.** They appear on coupon aggregator sites eighteen months later, still working, long after the accounting for the campaign was signed off.

**The roadmap promise with no fallback.** A feature is listed as shipping before launch, it slips two weeks, and every review mentions it. The item was true when written, which is exactly why nobody re-read it.

**Version-stale testimonials.** Reviews praising a workflow that no longer exists in the current release. Buyers arrive expecting the described product, and the gap between the quote and the interface reads as dishonesty rather than age.

**Bad reviews in the soft-live fortnight.** Redemption works, onboarding does not, and the first dozen buyers say so on the page. The deal is quietly not promoted, and it reads as the marketplace losing interest rather than a fixable problem nobody was watching for.

**Holding the packet for one missing asset.** The video is not ready, so nothing is sent, so nothing is built, and the launch slot goes to a seller who sent thirteen items with a real date against the other two.

## What this skill does not do

- It does not know your marketplace's current requirements. The asset list, tier count, code volume, revenue share, refund window and image dimensions vary by platform and change without notice. Everything here is the shape to expect; the account manager holds the answer.
- It cannot supply your marginal cost per account. Infrastructure, metered pass-through and support minutes come from your own billing and helpdesk data, and every branch of the decision rule is unusable without them.
- It does not settle the accounting or tax treatment of a perpetual licence sold through a third party, which changes both the deal's shape and how the proceeds may be recognised. That goes to an accountant before the contract is signed.
- It does not verify your billing integration. It tells you what to check and what the failure looks like. Running the check against your live storefront, and reading your processor's own reference for the fields involved, is work only you can do.
- It does not negotiate the contract. Revenue share, refund liability, exclusivity, buyer data ownership and what happens to outstanding codes if the listing is pulled are legal terms, and none of them are writing problems.
- It does not do the design. The banner specification is a brief for whoever makes the artwork, and a price flag legible at thumbnail size is a craft question this file can only state as a requirement.
