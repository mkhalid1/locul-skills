---
name: giveaway-campaign-design
description: Designs a list-building giveaway engineered to reproduce, rather than an advertisement with a prize attached. Covers the four-term viral coefficient and the time-boxed generation arithmetic, the two hard prize criteria and the anti-example that fails both, authority piggybacking, a seven-step partner outreach sequence with the escalating ask and the To and Cc rule, a media kit specification with per-partner tracked links, a deliberately light promotion plan, the legal and platform questions that need a qualified check, and a post-campaign cohort measurement. This skill should be used when a giveaway, sweepstake, prize draw or competition is being designed for list growth, when partners are about to be approached for prizes, or when a previous giveaway produced entrants and no revenue.
---

# Giveaway campaign design

## The claim this skill is built on

A giveaway is a growth loop with a deadline, and almost everybody builds it as an advertisement with a prize attached.

The obvious approach: choose an attractive prize, build an entry page, tell your list, buy some promotion, wait. It fails invisibly, because entrant numbers rise either way. What fails is reproduction. An entrant's job ends the moment they type their email address, so total reach is capped at whatever you can buy, borrow or beg, and the campaign is an advertisement that took six weeks and eight favours to run.

The decision that settles the outcome is what appears on the screen immediately after entry, and it is usually made last, by whoever builds the form, from a template. If that screen offers three social follows, the campaign cannot reproduce. If it offers a pre-filled, one-click, referral-tracked share worth extra entries, and offers it first, the campaign can produce more entrants than you put into it.

Everything else here is an attempt to move one of four numbers.

## Part one. The coefficient, defined properly, and why it comes first

**Call it `k`: the number of new entrants produced by one entrant over the life of the campaign.** Above 1 the campaign reproduces, so for every 10 entrants at least an 11th arrives through them. Below 1 it amplifies your seed and stops.

Factorise it before you try to improve it, because as a single number it can only be argued about:

```
k = s × i × c × e

s = completed share actions per entrant, averaged across all entrants including the many zeros
i = qualified impressions per share action, meaning people who see it and have not already entered
c = click rate per impression
e = entry rate per click, measured on the entry page
```

What sets each term is worth knowing before any design work starts.

- **`s` is set by the post-entry screen and by almost nothing else.** How many share actions are offered, where they sit in the list, whether each is one click with copy and image pre-filled, and what each is worth in extra entries. It is the term you control most directly and the one most campaigns leave at whatever the template did.
- **`i` is set by the prize's relevance.** A share of a prize aimed at a specific kind of person reaches fewer people who matter more. Estimate it from the median reach of an ordinary personal post, never from follower counts, because on most networks a personal post reaches a low single-digit percentage of a person's followers.
- **`c` is set by the pre-filled share copy and the image.** A share an entrant writes themselves outperforms one you wrote, but hardly anyone writes their own, so what you pre-fill is what runs.
- **`e` is set by the entry page, and above all by the number of required fields.** Every additional required field costs conversion. Email only is the design. Names, company and role belong on an optional screen after the entry is banked.

### The time-box changes the arithmetic

An always-on loop is judged by its asymptote. A giveaway runs for three weeks, so it is judged by how many generations fit inside the window.

```
generation time  = median delay from an entrant entering to the entrants their shares produce
g                = campaign length / generation time
total entrants  ≈ n₀ × (1 + k + k² + ... + k^g)
                 = n₀ × (k^(g+1) - 1) / (k - 1)      for k not equal to 1
```

Generation time on a giveaway is short, typically one to three days, because the share happens seconds after entry and the person who sees it acts that day or never. A 21-day campaign with a 2.5-day generation time gives about 8 generations, and that number is what turns a coefficient into an outcome.

### A worked calculation

Seed `n₀` of 1,200, made of your own list plus two partner lists. Eight generations.

- `k = 0.15`: the series barely moves, total about **1,420**. The campaign added 220 people and spent every partner relationship you had.
- `k = 0.97`: the series sums to 7.99, total about **9,590**.
- `k = 1.18`: the series sums to 19.1, total about **22,900**.

Sit with the last two. The gap between them is 0.21 on the coefficient and about 13,300 people. Near 1, the result is violently sensitive to small movements in any of the four terms, which is exactly why the terms are worth measuring separately, and why a campaign designed by feel lands anywhere at all.

**Why the arithmetic comes first.** Every later step in this file exists to move `s`, `i`, `c` or `e`. Choosing the prize before you know which term is binding is how a team spends three weeks negotiating a bigger prize, which nudges `c`, when the binding term was `s`, which one afternoon on the post-entry screen would have fixed. Do the arithmetic before you owe anybody anything.

## Part two. The prize, on two hard criteria

Both are hard criteria. A prize that fails either one is not a weaker version of a good prize, it is a different campaign.

**Criterion one: your target customer specifically wants it, and a non-customer does not.**

The failure here is the universally desirable consumer device. It maximises entrants and minimises entrant value at once, invisibly, because on the way in a person who wants a free laptop is indistinguishable from your buyer. Ninety days later they are not: they do not open, do not activate, do not buy, and mark you as spam at a higher rate than any cohort you have.

**A prize being worthless to a non-customer is a feature, not a compromise.** It suppresses junk entries, it raises `e` because the entry page reads as written for the person on it, and it holds `i` up through later generations because shares land in networks where the prize means something.

The test, stated so it can actually be run: write the prize as one headline of twelve words or fewer. Read it to five people who match your ideal customer description and do not work with you. If the first thing they say is a question about the catch or about eligibility, there is no pull. If the first thing they say is how do I enter, there is.

**Criterion two: enough pull that the target stops and acts now.** Relevance without pull produces the campaign everybody approves of and nobody enters. The usual way to get both is a bundle: five to eight items the target already pays for or wants to try, assembled so the headline number is large and every single item is on target. A bundle also solves a partner problem, because eight small asks land more easily than one large one.

**Decide the prize value with part seven open.** Several jurisdictions attach registration and bonding requirements above a stated prize-pool value, so pushing the headline number up to look impressive can quietly add weeks of lead time and a bond.

## Part three. Authority piggybacking

Where you can, pick a prize owned by an organisation that will then help promote the giveaway.

Two effects, and the second is the one people miss. You borrow their audience, which raises `n₀` directly. And you are guaranteed the borrowed audience is on target, because they are already customers of something adjacent to what you sell. A prize you buy yourself has to go and find its audience. A prize a partner supplies arrives with one.

There is a third effect further down the arithmetic. Partner-sourced entrants share into networks of similar composition, so `i` and `c` hold up through generations three and four instead of decaying as the campaign leaks into general audiences. A campaign seeded from a general-interest source is off target by generation two, which is usually why a promising first day becomes a disappointing second week.

**Prefer prizes a partner can supply at low cost and high perceived value:** a licence, a seat, an annual plan, a ticket, a place on a course, an hour of consultancy. Marginal cost near zero, headline value high, no shipping, no customs, and a yes that does not need a budget holder. Physical goods cost the partner real money, need a second decision from someone you will never meet, and add a delivery failure you cannot control.

## Part four. Partner outreach, seven steps, in this order

**1. List target organisations.** Twenty to forty. Three criteria: their audience overlaps the customer you described in part two, they are not a direct competitor, and somebody there has marketing in their job title. Adjacent tool vendors, communities, publications, course sellers, event organisers.

**2. Find two named people per organisation: the chief executive and the marketing lead.** Names, not role addresses. A message to a general inbox is a message to nobody.

**3. Email TO the chief executive and copy the marketing lead. Never the reverse.** This is the most load-bearing detail in the sequence and it looks like nothing. Addressed to the executive with the marketing lead visible, either the executive forwards it, which reaches the marketing lead as a delegation with accountability attached, or the marketing lead sees an executive-level request landing in their own area and champions it upward to be seen doing so. Reverse the fields and neither happens, because each can assume the other owns the reply, and a message two people can each assume the other will answer is a message nobody answers.

**4. Lead with the exposure benefit and make a small ask.** Never ask for free product in the first email. Five sentences or fewer: who you are, who the audience is with a number attached, what the campaign is, that they would be listed and promoted to that audience, and one small ask. The small ask is a yes or no on something cheap: whether you may send the one-page mechanics, or whether they want to be on the partner list. An opening email that asks for free product is asking a stranger for budget, and the answer to that is silence rather than no, which is worse, because silence cannot be told apart from a message that never arrived.

**5. Only after they say yes, escalate.** The second message carries the three things the first deliberately did not: social proof, meaning who else has joined by name; the mechanics, meaning dates, entry flow, how the prize reaches the winner, and where they appear; and the larger ask, meaning the specific prize, the promotional commitment and the dates. The order is not politeness. A small yes changes what the larger ask is, because it is now a request to a participant rather than to a stranger.

**6. Collect every code and every prize before launch.** Set a hard cut-off: all prize assets in hand five working days before entries open, stated in the second message so the deadline belongs to them too. Anything not in hand at the cut-off does not appear in the prize list. For physical goods, the partner ships directly to the winner and is named in the rules as the fulfiller, so you never take custody and never become the shipping department for eight other companies.

**7. On go-live, send every partner a media kit with tracked links.** That is part five.

Keep one record per organisation: executive name, marketing lead name, date of first email, date of yes, prize supplied and its stated value, tracked link, promotional commitment, and what they actually did. That record is your debrief, and it cannot be reconstructed afterwards.

## Part five. The media kit, specified

The media kit exists so a partner's marketing person can promote you without writing anything. Anything they have to write is a thing that does not go out.

- **Two email swipes**, an announcement and a last chance. Assume a narrow mobile client shows roughly the first 40 characters of a subject line, so put the prize noun first. Bodies under 150 words, one link.
- **Social posts written to each network's cut, not to its ceiling.** For a short-post network, under 280 characters including the link. For a professional network, the first roughly 140 characters have to carry the whole offer, because the feed collapses everything after that behind a see-more control. Supply two variants per network so two partners posting on the same morning do not post identical text.
- **Graphics in the sizes that are actually used:** a 1200 by 630 pixel link preview card, a 1080 by 1080 square, and a 1080 by 1920 vertical, exported as PNG, each with and without the prize value overlaid, because some partners cannot publish a monetary figure. Include one plain text file of every piece of copy, so a partner on any operating system can use the kit without a design tool.
- **A one-page mechanics sheet:** prize list with suppliers, opening and closing date and time with the time zone stated, eligibility in one line, a link to the full rules, the named sponsor, and the exact disclosure line partners are required to include.
- **A unique tracked link per partner**, and where practical per placement.

**The tracked-link requirement is more specific than it sounds.** One distinct link per partner, resolving to the entry page and never to your homepage, with the partner identifier carried through the entry form so an entrant is attributed at entry and not only at click. Click attribution tells you who sent traffic. Entry attribution tells you who sent entrants. The ratio between the two tells you whose audience was on target, which is the number that decides who you invite next time.

## Part six. Promotion, deliberately light

Five channels, and the lightness is a design decision rather than a budget constraint.

1. **Your own list: exactly two emails.** A launch email and a last-chance email, 24 to 48 hours before close. No segmentation: it costs days and removes seed, and every list member is a potential first-generation entrant whose shares are worth more than their own entry. The last-chance email earns its place because entries keep accruing extra weight through sharing right up to the close.
2. **Your own social accounts.** Open, midpoint, 48 hours out, close.
3. **Partner lists**, running from the media kit.
4. **About twenty hand-picked individuals** with the right audience, asked personally, one message each, no template.
5. **A small paid push targeted at followers of the prize brands.** Cap it before launch, and a workable ceiling is 10% to 20% of the prize pool value, because the temptation to raise the cap arrives at exactly the moment raising it is the wrong answer.

**The diagnostic rule, which is the most useful line in this file: if you find yourself needing to over-promote the giveaway, the giveaway is wrong. Go back and fix the prize.** Write the tripwire down before launch, while you are still capable of acting on it: if measured `s` is below 0.4 and day-three entries are under a quarter of day-one entries, you will not add budget. Adding budget to a campaign with `k` below 0.5 buys entrants at advertising prices and files them under viral growth, which destroys the only thing the campaign could have taught the next one.

## Part seven. The legal and platform layer, which a skill cannot decide for you

**Say this plainly to whoever is running the campaign: this is the one part of the plan a skill cannot decide.** The rules vary by jurisdiction, they vary by platform, they change, and getting them wrong is not a growth problem. What follows is the list of questions to take to a qualified check, not a set of answers, and it is not exhaustive.

- **The three-element test.** In many jurisdictions a promotion combining a prize, chance and consideration is a lottery, which private operators may not run. Lawfulness comes from removing an element, usually consideration, which is where no purchase necessary comes from. Where a paid route exists, a free alternative method of entry of equal standing normally has to sit beside it.
- **Skill requirements.** Some jurisdictions require a skill element. Canada's mathematical skill-testing question exists for this reason.
- **Registration and bonding.** Some United States states require registration and a bond above a prize-pool value. New York and Florida have both used a 5,000 dollar threshold with different lead times, and Rhode Island has a lower one applying to retail promotions. Thresholds, lead times and forms change, so check the current position before the prize value is fixed, not after.
- **The United Kingdom** separates a free prize draw from a lottery under the Gambling Act 2005, and the CAP Code's promotional marketing rules require significant conditions stated up front, a closing date, and winners' details made available.
- **Quebec has historically run a separate publicity contest regime** with filing, duties and French-language requirements. Treat the current position as something to verify rather than assume.
- **Sponsor identification and disclosure.** The promoter has to be identifiable, and where partners, affiliates or creators promote the campaign, their relationship to it usually has to be disclosed.
- **Platform promotion rules, which constrain the core mechanic directly.** Large social platforms generally require you to state that the promotion is not sponsored, endorsed or administered by them, and generally prohibit making a share to a personal timeline or the tagging of friends a condition of entry. **This is not a footnote, it is a design constraint on the mechanism this file is built around.** Sharing has to be offered and rewarded, never required, and the reward verified through your own referral link rather than a platform action you cannot see. Design it that way in the first sketch, because retrofitting it means rebuilding the entry flow after the launch date is public.
- **Data protection.** An entry is a consent event. Where consent-based marketing rules apply, bundling marketing consent into the act of entering can invalidate that consent, which matters more here than anywhere, because list building is the point of the campaign. The consent wording, its separation from entry, and the lawful basis for marketing to these people afterwards all need checking.
- **Tax and reporting.** Prize value may be reportable, by the winner or by you.

**The cheapest control available is geographic eligibility.** Limiting entry to jurisdictions you have actually checked is normally far cheaper than the compliance work for the ones you have not, and it is one line in the rules.

## Part eight. The decision rule

Run this on the design, before anything is committed.

- **The prize maps to a named ideal customer and measured `k` is 1.0 or above.** Launch, with a window of 14 to 21 days. Hold the paid budget, because at this coefficient the campaign does not need it and spending it makes `k` unreadable next time.
- **Prize maps, `k` between 0.5 and 1.0.** Launch, and describe it internally as a multiplier rather than a viral campaign. At `k = 0.8` over 8 generations you get roughly 4.2 times your seed, which is a good campaign and not a compounding one. Size the seed accordingly, because at this coefficient the result is set by `n₀` and not by the mechanic.
- **`k` below 0.5.** Do not launch. You get roughly twice your seed at best and you spend real partner goodwill to get it. Fix the binding term first, and it is almost always `s`.
- **The prize does not map to a named ideal customer.** Do not launch at any coefficient. A high coefficient on the wrong prize is a faster way to build a worse list, and it looks like a success for about ninety days.
- **You cannot tell, because you have never run one and have no basis for the four terms.** This is the common case and it has its own procedure. Do not guess and do not launch on a guess. Run a seed test: take about 300 people from your list, run the real entry flow with the real post-entry screen and one real prize for 72 hours, and measure `s`, `i`, `c` and `e` with tracked links. Then use the branches above. If you cannot even run the seed test, because there is no list and no referral tracking, the honest branch is that this is not a growth campaign. It is a paid awareness spend with a prize in it, and it should be budgeted and measured as advertising, where at least the cost per acquired address is knowable.

## Part nine. After it closes

**Select and announce the winner in a documented, auditable way**, on the date the rules stated, by the method the rules stated.

**Thank every partner within three working days, with their numbers:** their clicks, their entries, and their share of total entries. This is the message that gets you a yes next time, and it works largely because almost nobody sends it.

**Never share the collected addresses with partners.** Their consideration was exposure, and they were promoted accordingly. Beyond that, in consent-based jurisdictions, passing entrant data to a third party not named at the point of collection is a plain breach, and the kind that generates complaints from exactly the people you just recruited.

**Import the list carefully.** A large one-off addition of cold addresses is one of the quickest ways to damage a sending domain. Bulk sender requirements at the major mailbox providers, in force since February 2024, expect a spam complaint rate below 0.3% and recommend under 0.1%, and a giveaway cohort will complain at a higher rate than any cohort you have. Verify the addresses, ramp the volume over days rather than mailing everyone at once, and suppress the people who never open.

**Measure the cohort, not the count.** Tag every entrant with the campaign and their source partner, then compare them at day 30 and day 90 against a baseline cohort from your normal channels over the same period, on four numbers: activation rate, retention at 30 days, conversion to paid, and revenue per person. Expect the giveaway cohort to be worse. The question is by how much, and a workable rule of thumb is that if it converts at under a quarter of the baseline rate, the prize was off target no matter how many entrants it produced. That comparison, not the entrant count, is the input to the next campaign.

## Worked example, compressed

A documentation hosting service for engineering teams wants 10,000 new subscribers this quarter. Its own list is 900 people. Two adjacent communities have provisionally agreed to mail their lists, worth roughly 300 more at the seed. So `n₀ = 1,200`, window 21 days.

**The first design, which is the one that always gets proposed.** Prize: a high-specification laptop. Post-entry screen: follow us on three networks, with sharing offered fourth and optional. A 72-hour seed test to 300 people measures `s = 0.20`, `i = 85`, `c = 3.0%`, `e = 30%`.

`k = 0.20 × 85 × 0.030 × 0.30 = 0.153`

Total across the window: about **1,420 entrants** against a target of 10,000, and the ones who arrived through shares wanted a laptop.

**The redesign, one term at a time.**

- **`s`:** the post-entry screen becomes four one-click shares in the first four positions, copy and image pre-filled, each worth three extra entries, each on a unique referral link, offered and rewarded but never required. Measured `s` rises to 1.05.
- **`i`:** the prize becomes a partner-assembled bundle, six annual licences for tools these teams already buy plus two conference tickets. Headline value comparable to the laptop, marginal cost to each partner near zero, and nobody outside the segment cares, which is the point. Measured `i` falls slightly to 78, because shares now land in narrower networks.
- **`c`:** the pre-filled share copy names the bundle rather than the sponsor. Measured `c` rises to 3.6%.
- **`e`:** the entry form drops to one email field, with name and company moved to an optional screen after the entry is banked. Measured `e` rises to 33%.

`k = 1.05 × 78 × 0.036 × 0.33 = 0.973`

Measured generation time is 2.5 days, so `g` is about 8 and the series sums to 7.99. Total: **about 9,590 entrants**, close to target and on target.

**One more move on the cheapest term.** Of the four, `e` sits furthest from its ceiling: the entry page still carries a mandatory tick box and a competing call to action above the form. Removing both takes `e` from 33% to 40% in the seed test.

`k = 1.05 × 78 × 0.036 × 0.40 = 1.179`

The series over 8 generations now sums to 19.1. Total: **about 22,900 entrants**.

**Verdict: build the second version, and do not spend the paid budget.** The design goes from 1,420 to 22,900 entrants with no increase in the prize's headline value and no extra promotion, on three cheap changes to the entry flow and one change to the prize's composition. The largest single movement, about 13,300 people, was a seven-percentage-point improvement in entry-page conversion compounded across eight generations. That is the whole argument for computing the coefficient before designing anything, because the term that produced those 13,300 entrants is the one nobody would have argued about in a planning meeting.

**What is still undecided:** eligibility, the free entry route, the rules text, the sponsor disclosure, the tax position and the platform requirement that sharing be optional rather than a condition of entry. None of that is settled by the arithmetic and none of it is settled here. It goes to a qualified check before entries open, and because it can change the entry flow, it goes there before the flow is built rather than after the launch date is announced.

## Failure modes

**The universal prize.** The campaign hits its entrant target and the list never converts. From the outside it looks like a marketing success followed by an unrelated sales problem, and the two are never connected, because the entrant count was reported in week four and the conversion data lands in month four.

**Engagement actions in the first positions.** The post-entry screen offers follows, likes and comments, with sharing somewhere below. `s` sits near 0.2, `k` stays under 0.2, and the campaign returns roughly its seed. Everyone concludes that giveaways do not work for their market, which is the wrong lesson from a correct observation.

**The cold big ask.** The first partner email asks for free product. The reply rate is near zero and silent rather than negative, so the team cannot tell rejection from a message that never landed, and spends a week chasing people who were never going to answer.

**Reversed To and Cc.** The marketing lead is in the To field with the executive copied. Each assumes the other owns it. No reply arrives, the outreach looks like a targeting failure, and the fix is one field.

**Untracked partner promotion.** Partners promote, entries arrive, and nobody can say who delivered. The consequence shows up in the second campaign, where you re-approach everybody equally and spend the same effort on the partner who sent nine entrants as on the one who sent two thousand.

**Over-promotion as compensation.** Day three is quiet, so budget goes in. Entries rise, the campaign is declared a success, and the measured coefficient is now a blend of organic sharing and paid traffic that can never be separated. The next campaign is planned on that blended number and misses badly.

**Success theatre on the entrant count.** The deck reports entrants, cost per entrant and list growth, and stops there. There is no day-90 cohort comparison, so the question of whether the campaign produced customers is never asked, and the same prize gets chosen again.

**The required share.** Entry is made conditional on posting to a personal timeline or tagging friends, because that is the fastest way to lift `s`. It breaches the promotion rules of most large platforms, and the correction arrives at the worst moment, either as a removed post mid-campaign or as a rebuilt entry flow days before launch.

**The prize that never arrives.** A partner agreed verbally, sent nothing, and is unreachable in the week of the draw. The winner is announced against a prize list that includes an item you cannot supply, which turns a marketing problem into a rules problem in public.

**The list dump.** Several thousand cold addresses are mailed on day one after close. Complaint rates spike, the sending domain degrades, and open rates fall for the entire existing list, including customers who arrived nowhere near the giveaway.

## What this skill does not do

- It does not decide the legal position, and that is not a hedge. Eligibility, no-purchase-necessary requirements, registration and bonding thresholds, sponsor disclosure, data protection consent and each platform's promotion rules vary by jurisdiction and change. It names the questions. A qualified adviser answers them, before entries open.
- It cannot supply the four coefficient terms. Every number in the arithmetic has to come from a seed test or a previous campaign, and a coefficient built from estimates is a hope with a decimal point on it.
- It does not build the entry mechanics. Referral tracking, weighted entries, duplicate and fraud detection and a defensible draw are solved problems in existing platforms, and building them yourself costs more than the prize.
- It cannot judge whether a prize has pull. It gives you a twelve-word headline and five people to read it to. The answer comes from those five people.
- It does not manage the deliverability consequences of the list you just built, which is a separate job with its own thresholds, its own warm-up schedule and its own failure modes.
- It does not fix what entrants land on. A campaign that triples a list feeding an onboarding flow with poor activation triples the number of people who leave, faster, and the campaign will get the credit for a while.
