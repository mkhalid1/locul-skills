---
name: growth-loop-audit
description: Tests whether a claimed growth loop is a loop or a funnel drawn in a circle, then computes it properly. Applies the output-feeds-input definition, classifies the mechanism against a taxonomy of five real loop types with the step each one most often fails to close, factorises the branching factor into separately measurable terms, computes total amplification and annual compounding using both branching factor and cycle time, and specifies the cohort measurement needed to see whether the loop is still closing. This skill should be used whenever a growth loop, viral loop, referral mechanic or flywheel is being designed, forecast, funded or defended.
---

# Growth loop audit

## The claim this skill is built on

Most things called loops are funnels drawn in a circle.

A funnel takes traffic in one end and produces customers at the other. Drawing an arrow from the customer back to the traffic does not make it a loop. It makes it a funnel with an arrow on it, and the economic difference is total: a funnel has to be refilled from outside every single cycle, forever, and each refill costs money or effort that does not decrease. A loop refills itself, partly or wholly, from what the previous cycle produced.

Confusing the two is expensive in a specific way. A team that believes it has a loop under-invests in the channel that is actually delivering its users, because that channel is understood as a temporary priming mechanism rather than the whole engine. Then the priming stops, the "loop" does not spin, and the growth model was never real.

So the audit has two halves. First, is this a loop at all. Second, if it is, what are its two numbers, because one of them is almost always missing from the conversation.

## Part one. Is it a loop?

**The definition that does the work: the output of one cycle is the input of the next.** Not "related to". Not "supports". The same units, feeding back in.

Three questions settle it, in order.

**Question 1. What does one cycle produce, counted in units?**

Force a countable noun. Invitations sent. Public artefacts created. Indexed pages published. Pounds of gross profit available for reinvestment. If the answer to this question is "a happy customer", "brand awareness" or "trust", the mechanism has no output and cannot have a loop. This is where most circle diagrams fail, and they fail immediately.

**Question 2. By what mechanism does that unit become a new user? Name the surface.**

Not "people hear about it". Which surface does a non-user encounter, what does it say, where does it lead, and what fraction of the people who see it are not already users? A loop whose artefacts are only ever seen inside private channels by existing users produces no new anything, and this is common enough to be worth checking first: an artefact shared into a team's internal chat reaches nobody new.

**Question 3. Does the new user perform the same step that produced them?**

This is the question that separates a loop from a one-generation bounce. If existing users create public artefacts and the people who arrive from those artefacts read but never create, the second generation does not reproduce, the mechanism runs once, and it dies out no matter how good the first generation was. The share of arrivals who become producers is a term in the arithmetic later, and it is frequently the smallest one.

If any of the three fails, stop. It is a funnel. Say so, and go and improve the channel that is genuinely delivering users, because that is where the growth is coming from anyway.

## Part two. A taxonomy of real loops, and the step each one fails to close

Five shapes cover nearly everything. For each, what it requires and where it breaks.

**1. The output-visibility loop.** The product's own output is seen by people who do not use it. Design tools, form builders, scheduling links, embedded widgets, exported documents.

*Requires:* the artefact genuinely reaches non-users, it carries attribution, the attribution is clickable, and it lands somewhere that converts a curious stranger rather than a page written for people who already know what this is.

*The step that fails to close:* **attribution surviving the export.** The artefact is downloaded as a file, screenshotted, embedded without the footer, or the branding is removed on the paid plan, which means your best and most prolific users produce the least attributable output. Second most common: the attribution link lands on a homepage written for existing customers, and the visitor bounces.

**2. The self-interested invitation loop.** A user invites someone, and the invitation serves the sender.

*Requires:* the sender wants the recipient there for the sender's own reasons, independent of your growth. Collaboration is the honest version: the task cannot be completed alone, so inviting is not a favour to you, it is the product working.

*The step that fails to close:* **send rate, once the incentive is removed.** If the invite exists because you asked for it, or because of a credit, the send rate tracks the incentive and collapses with it, and the invites that were sent came from people optimising for the reward rather than from people who wanted a colleague in the document. A useful test: remove the reward for a slice of users and measure the send rate. What remains is the real number.

**3. The content or user-generated-content loop.** Ordinary use of the product produces public, indexable content, which brings visitors, some of whom become users, some of whom produce content.

*Requires:* the content is public, indexable, and substantial enough to rank; and there is a reason for a reader to become a creator.

*The step that fails to close:* **reader-to-creator conversion.** Almost every UGC loop dies here. The visitor came to consume an answer, got it, and left. A secondary failure is index quality: thin pages generated at scale get demoted, and a loop whose output is a hundred thousand near-identical pages can go from working to not working in a single algorithm update, without anything you did changing.

**4. The paid loop.** Gross profit from the previous cohort funds the acquisition of the next.

*Requires:* the gross profit per customer to be recovered in cash within the reinvestment cycle. This is the only loop in the list bounded by working capital rather than by population, which makes it the only one that can in principle run indefinitely.

*The step that fails to close:* **payback period against the reinvestment cycle.** If payback is fourteen months and you reinvest monthly, you are not looping, you are financing, and the loop is really a debt facility with a growth chart attached. The other failure is that this loop's branching factor is set by the auction, so it falls as you scale, which means it is the one loop that reliably gets worse the better it works.

**5. The two-sided marketplace loop.** Each side attracts the other.

*Requires:* liquidity. Adding one seller must measurably improve the experience of buyers, and vice versa, in a specific market slice.

*The step that fails to close:* **density inside a slice.** Growth is measured nationally while the market is local or categorical, so ten thousand sellers spread across five hundred non-overlapping segments improves nobody's experience. The loop only exists above a liquidity threshold inside one slice, and the correct strategy is almost always to saturate one slice before opening a second, which is unpopular because the aggregate numbers grow more slowly.

## Part three. The arithmetic, done with both variables

Two numbers are needed and only one is ever quoted.

- **Branching factor, `k`:** the number of new *reproducing* users produced per reproducing user per cycle. The word reproducing matters, because a new user who never performs the producing step is not an input to the next cycle.
- **Cycle time, `t`:** the time from a user joining to the users they produce joining. Measure it as the median of the delay distribution, and look at the tail, because a long tail makes the effective cycle slower than the median implies.

### When `k` is below 1

The loop does not sustain itself. It amplifies whatever you acquire from outside, and then stops. Total users produced from an external cohort of `u₀`:

```
u₀ × (1 + k + k² + k³ + ...) = u₀ / (1 - k)
```

- `k = 0.2` gives 1.25 times
- `k = 0.5` gives 2 times
- `k = 0.8` gives 5 times
- `k = 0.9` gives 10 times
- `k = 0.95` gives 20 times

**This is the most useful line of arithmetic in the whole file.** Note the shape: moving from 0.8 to 0.9 doubles total output, while moving from 0.2 to 0.3 improves it by about 14%. Effort spent improving a good loop is worth far more than the same effort on a weak one, which is the opposite of the usual instinct to fix the worst-performing thing.

It also gives you honest language. A `k` of 0.5 is not a growth engine, it is a 2x multiplier on acquisition, and describing it that way is both true and still impressive. Presenting it as a loop invites a forecast it cannot support.

### When `k` is above 1

The loop compounds, and cycle time decides whether that matters.

```
users after n cycles   = u₀ × k ⁿ
cycles per year        = 365 / t
annual multiple        = k ^ (365 / t)
doubling time          = t × ln(2) / ln(k)
```

Same `k`, different `t`:

- `k = 1.1`, `t = 7 days`: about 52 cycles a year, `1.1⁵²` is roughly **142 times**. Doubling time about 51 days.
- `k = 1.1`, `t = 90 days`: about 4 cycles a year, roughly **1.5 times**. Doubling time about 654 days.
- `k = 1.2`, `t = 30 days`: about 12 cycles a year, roughly **9 times**.
- `k = 1.5`, `t = 180 days`: about 2 cycles a year, roughly **2.3 times**.

The third and fourth lines are the ones to sit with. A branching factor of 1.5 sounds far healthier than 1.2, and on a six-monthly cycle it produces a quarter of the annual growth. **A factor slightly above one with a cycle measured in months is not growth in any useful sense.** Cycle time is the variable everyone omits and it frequently dominates.

### Saturation, stated plainly

**A `k` above 1 cannot be sustained indefinitely, because the addressable population is finite.** This is not a risk to manage, it is arithmetic. As penetration rises, the remaining non-users are by construction the ones your existing users do not know, or the ones least interested, so the effective factor declines, crosses 1, and keeps falling.

So the useful question is not "is `k` above 1". It is **"what fraction of the addressable population does this reach before `k` crosses 1, and how long does that take"**. Estimate the reachable population honestly, estimate the fraction, and check that the saturated business is a business. A loop that saturates at 40% of a 200,000-organisation market has told you to plan for 80,000 customers, which is a far more useful output than an exponential curve.

### Factorise `k` before trying to improve it

`k` is a product of terms, each separately measurable and separately fixable:

```
k = artefacts per producer per cycle
  × non-user impressions per artefact
  × response rate per impression
  × signup rate per response
  × share of signups who become producers
```

Almost every team tries to improve `k` as a single number, which means arguing about the loop in general. Factorised, the conversation becomes which of five numbers is binding, and usually one of them is smaller than the others by an order of magnitude. Fix that one. Improving a term that is already 60% while another sits at 1% is effort spent in the wrong place.

## Part four. Measuring a loop

**Aggregate counting cannot see a loop.** Total signups rising proves nothing, because external acquisition can rise while the loop decays. Concretely:

- Year one: 10,000 users acquired externally, `k = 0.5`. Total `10,000 / 0.5 = 20,000`. The loop contributed 10,000.
- Year two: 18,000 acquired externally, `k = 0.2`. Total `18,000 / 0.8 = 22,500`. The loop contributed 4,500.

Total signups are up 12.5% and everyone reports a good year. The loop's contribution fell by more than half, and nothing in the aggregate view shows it.

**The measurement that works is cohorted.** For each joining cohort `M`, count the new users over the following period whose attributed origin is a member of cohort `M`, and divide by the size of `M`. Plot that by cohort month. A healthy loop shows a flat or rising line. A dying loop shows a monotonic decline, and it shows it months before the aggregate numbers notice.

**The prerequisite, and it is the whole job: per-user attributed origin.** Every new user must be linked to the specific existing user, artefact, invitation or page that produced them. If you cannot do that, you cannot measure a loop, and every statement anyone makes about the loop is a story. Build this before building the loop, not after, because it cannot be reconstructed retrospectively.

**The single number that tells you whether it is closing at all:** the share of new users in a period whose attributed origin is an existing user or an artefact produced by one. If that share is small and flat while the business grows, the loop is not the growth, whatever the diagram says.

**Read the cycle time off the same data**: the distribution of delays between a producing user joining and their produced users joining. Report the median and the 90th percentile, because the gap between them tells you how long you must wait before a cohort's `k` is final.

## Part five. The ethical line, as a practical matter

Three patterns work in the short term, damage the thing that makes the loop work, and are increasingly regulated. The practical objection is the same in all three cases: **each one inflates a number that other decisions depend on.**

**Contact list access.** Uploading an address book and mailing everyone in it produces a large one-off spike in `k`. Then three things happen. The invited cohort never chose to be there, so its activation and retention curves sit well below the organic cohort, and because it is a large share of intake it drags blended retention down and corrupts every lifetime value figure computed from blended data, which in turn corrupts the acquisition budgets set from those figures. Senders learn that the product mails their contacts, and send rates fall in the next cycle, so the loop eats its own `k`. And the sending domain's reputation degrades, which pushes your legitimate transactional email toward spam folders and lowers activation for everybody, including users who arrived by other means.

**Default-on sharing.** Making public what the user did not decide to make public raises `k` immediately. The damage is that the correction is permanent in both directions: a single incident produces a searchable story that outlives the feature, and turning the default off permanently lowers `k`, which means the `k` the business was planned around was never real. There is a cheap way to find out the honest number in advance: make it opt-in for a slice of users and measure. Whatever remains is the loop you actually have.

**Making the exit harder than the entry.** Cancellation flows with more steps than sign-up, retention offers that cannot be dismissed, phone-only cancellation. Regulators in multiple jurisdictions have moved toward requiring cancellation to be about as easy as sign-up, and the direction of travel is consistent enough that a friction-based retention plan should be treated as a legal question rather than a growth one. Check the current position in each market you sell in. The retention damage is that forced retention is not retention: it converts into chargebacks, refund requests, negative reviews and a permanent refusal to buy again, while inflating the retention figure that your acquisition spending is justified by. You then overspend on acquisition on the strength of a number that describes trapped users rather than satisfied ones.

**The practical rule:** any `k` that comes from removing a choice is a liability recorded as an asset.

## Part six. The decision rule

- **Output feeds input, attribution is installed, `k` is 0.3 or above, cycle time is 30 days or less.** A real loop. Factorise `k` and invest in the binding term.
- **Output feeds input, `k` between about 0.05 and 0.3.** A multiplier, not an engine. Total amplification between roughly 1.05 and 1.43 times. Worth cheap improvements. Not worth a dedicated team, and not worth putting in a forecast as compounding.
- **`k` below 0.05.** A rounding error. Stop calling it a loop, and stop letting it justify roadmap decisions.
- **Output does not feed input.** It is a funnel. Rename it and go and fix the channel that is actually delivering users.
- **You cannot tell, because attributed origin is not tracked.** Do not decide. Install per-user attributed origin, wait one full cycle plus the tail, which as a rule of thumb means about three times the median cycle time so that most of the delay distribution is captured, then run the audit properly. In the meantime **assume there is no loop**, because the base rate for a believed loop turning out to be a funnel is high, and staffing an imaginary loop costs a quarter you cannot get back.

## Part seven. When not to build a loop at all, which is most of the time

Four conditions have to hold together. Missing one is usually fatal.

1. **Usage is frequent.** Cycle time is bounded below by how often people use the thing. An annually used product has an annual cycle, and from the arithmetic above, even a healthy `k` on an annual cycle produces nothing interesting inside a planning horizon.
2. **There is a natural artefact or a genuine collaborative need.** If nothing is produced and nothing requires a second person, invitations are a favour to you and the send rate will reflect that.
3. **Friction on the receiving side is low.** If the recipient must create an account, pay, or be approved before they experience anything, the response term collapses and no amount of work on the earlier terms recovers it.
4. **The addressable population is large.** With a few thousand target organisations, saturation arrives before compounding matters, and direct outbound reaches all of them faster, more predictably, and with attribution you already have.

There is a fifth killer that is not about the loop at all: **if the user is not the buyer**, an invitation produces a user and not a customer, so `k` measured in users can be healthy while `k` measured in revenue is zero. Measure the one that pays.

**What to do instead.** Make one non-compounding channel repeatable and boring, and staff that. Build an accumulating asset such as content that ranks, a template library or a public directory: it is not a loop, because its output does not become its input, but it does compound in the sense people usually mean when they say loop, and it is far easier to build. And improve retention, which raises the value of every user the non-compounding channel delivers and, incidentally, raises `k` in any loop you build later, because only retained users produce anything.

## Worked example, compressed

A team scheduling tool claims a growth loop: users send booking links, invitees see a branded booking page, some of them sign up. Headcount is being proposed.

**Question 1, what does a cycle produce?** Booking pages viewed by non-users. Countable. Passes.

**Question 2, what is the surface?** The booking confirmation page carries a "powered by" link. Named and real. Passes.

**Question 3, do new users do the same step?** Only if they go on to schedule meetings themselves, and many sign up to be scheduled with rather than to schedule. Held for the arithmetic.

**The numbers, per 100 scheduling users per month:**

- Bookings created: 1,200
- Share of invitees who are not already users: 62%, so 744 non-user impressions
- Click rate on the attribution link: 1.1%, so about 8 clicks
- Signup rate from a click: 12%, so about 0.98 signups
- Share of signups who become schedulers themselves: 30%, so about **0.29 new reproducing users**

**Branching factor:** `0.29 / 100 = 0.0029` per reproducing user per month, with a cycle time of roughly one month.

**Total amplification:** `1 / (1 - 0.0029) = 1.0029`. The loop adds about **0.3%** to acquisition.

**Which term is binding, from the factorisation.** The binding term is the 1.1% click rate on the attribution link, which is an order of magnitude below every other term except the reproduction share. Suppose a redesign takes it to 11%, a tenfold improvement and an optimistic one. Clicks become 82, signups about 9.8, new reproducing users about 2.95, so `k = 0.0295` and total amplification is `1 / 0.9705 = 1.030`. **A tenfold improvement in the binding term takes the loop from adding 0.3% to adding 3%.**

**Verdict: this is not a loop, it is a funnel with a rounding error attached, and a tenfold improvement in its best available lever leaves it a rounding error.** Do not assign headcount. The one-engineer-week version of the attribution redesign is worth doing because 3% is free money once it exists, and it should be scoped as a small conversion improvement rather than as a growth strategy. The genuinely interesting finding is buried in the last term: only 30% of signups ever schedule anything, which is an activation problem worth far more than the loop, and it was only visible because the branching factor was factorised rather than argued about as a single number.

## Failure modes

**The circle in the deck.** A funnel with an arrow drawn from the last box to the first, presented as a loop. Nobody can name the unit the cycle produces, and nobody asks.

**`k` quoted without `t`.** A branching factor above one is reported as proof of compounding, and the cycle turns out to be six months, which makes the annual multiple smaller than the paid channel nobody is discussing.

**Aggregate counting.** Total signups rise every quarter while the loop's contribution halves, hidden by increasing paid spend, and the decline is only discovered when the paid budget is cut.

**Unfactorised `k`.** Six months of general improvements to "the loop" while one term sits at 1% and dominates the product. The work was real and the number did not move.

**Assuming `k` is constant.** A forecast extrapolates the early branching factor across three years and implies a user count larger than the addressable population, which nobody notices because the chart is plotted on a log scale.

**Attribution never installed.** Every claim about the loop is a story, and the argument about whether it works is unresolvable in principle, so it is settled by seniority.

**The reproduction gap.** New users arrive and consume but never produce, so the mechanism runs one generation and stops. The first cohort looked spectacular and it was the only cohort.

**The borrowed surface.** The loop depends on a platform's feed, embed, or notification behaviour, and the platform changes it. `k` goes to zero in a week and there is no version of the loop that does not run on someone else's property.

**Retention mistaken for a loop.** Users coming back is not users bringing users. Repeat usage keeps the denominator alive, which matters, but it is not an input to the next cycle and it does not compound the user count.

## What this skill does not do

- It cannot measure anything. Everything here runs on numbers your analytics must produce, above all per-user attributed origin, and without that the audit legitimately ends at "you cannot tell".
- It does not forecast saturation. It establishes that the branching factor falls as the population is consumed and asks you to estimate where. Modelling the curve properly needs penetration data it does not have.
- It does not design the loop. It classifies, computes, and identifies the binding term. Turning a 1.1% click rate into 11% is product and design work.
- It does not judge whether the product deserves a loop. A well-built loop on a product people leave after three weeks simply moves more people through the leak.
- It does not know current platform policies on contact access, sharing defaults or cancellation, which vary by jurisdiction and change. It says which patterns are under pressure and tells you to check.
- It will not replace a growth analyst on an ongoing basis. It is one read of a mechanism, and the mechanism needs watching by cohort, continuously, with a tool.
