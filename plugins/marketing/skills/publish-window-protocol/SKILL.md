---
name: publish-window-protocol
description: Produces the operating protocol around the act of publishing a social post, plus the per-post record schema that makes a later diagnosis possible. Covers the ordered pre-publish window and why the warm-up belongs before rather than after, the forbidden actions inside the first hour and the reason each ordering is load-bearing rather than superstition, the reported signal hierarchy stated without the borrowed multipliers, a tracker schema where every column names the future question it answers, a decision rule with an explicit branch for ambiguous early signals, and a thirty-day review loop for replacing borrowed defaults with your own rows. This skill should be used when a publishing routine is being written down, when a post tracker is being designed or has turned out to be useless, when reach has fallen and nobody can say what was different, or immediately before a post goes out.
---

# Publish window protocol

## The claim this skill is built on

Publishing is not an event, it is a window, and the actions inside that window are ordered. Two of them are in the wrong place almost everywhere.

The usual sequence: write the post, publish it, go and engage with the feed for a while, come back tomorrow and see how it did. Every individual action in that list is correct. The sequence is wrong twice, and both corrections are free.

The first is the warm-up, which belongs before the publish rather than after it. The second is the record, which belongs before the publish rather than after the numbers arrive.

The second one is the expensive mistake and it is almost invisible. A post history with no recorded conditions cannot support a diagnosis. Not a weak diagnosis, none at all. When distribution falls, the only question worth asking is what was different about the weak posts, and if the sheet holds a post number, a title, a status and a date, then not one of those four is a condition and the question has no answer available at any level of effort. The conditions were recoverable for about a day and then they were not.

## Part one. Why an ordering claim survives when a weight claim does not

Advice about social publishing carries two kinds of claim, and they have very different standing.

A **weight claim** asserts a magnitude: a comment is worth some multiple of a reaction, an in-body link costs some percentage of reach, an edit inside the first day costs some further percentage. No platform has published any of these. They are aggregated from vendor studies and individual accounts, and they cannot be checked from outside the ranker. Repeating one as a fact is the fastest way to make everything around it untrustworthy.

An **ordering claim** asserts only that one arrangement of the same actions dominates another. It can be right for reasons that never touch the mechanism:

1. **Cost symmetry.** Fifteen minutes of commenting costs fifteen minutes whichever side of the publish it lands on. If the earlier placement has a plausible upside and the later one has none, you are not betting on a mechanism, you are declining to pay the same price for the worse arrangement.
2. **Reversibility.** Actions inside the window differ in whether they can be undone. Publishing again cannot be undone. An edit cannot be un-edited, and LinkedIn marks an edited post as edited, which is visible to every reader. Waiting can always be undone by acting later. Put the reversible before the irreversible and you keep your options.
3. **Record integrity.** An action taken inside the window that is not written down turns that row into one you can never compare with any other. This is the rule that connects the protocol to the schema, and it is worth stating as an instruction on its own: **if you could not record an action as a column, do not take it.**

Everything in the rest of this file is either an ordering claim, a schema, or a number explicitly labelled as somebody else's estimate.

## Part two. The three zones around a publish

### Zone A, from twenty minutes before to the publish

Three things, in this order.

**1. The clean check, at about twenty minutes out.** It goes first because it is the only step that can still cancel the publish. Confirm there is no external link in the body. Confirm the hashtag count is inside your cap, where three is the widely repeated convention and is not documented anywhere by the platform, so treat it as a default you will later test. Confirm the publishing route is the native app or a tool that publishes through the official API, not a browser-automation or scraping tool: LinkedIn's User Agreement prohibits scraping and automated access, which is documented, while the rate at which accounts using such tooling are actually restricted is not, and the figures that circulate come from vendor studies. Confirm the first two lines survive the point where the feed collapses the post behind a see-more control. Confirm hours since your previous post clears your spacing floor.

**2. Pre-fill the tracker row, at about fifteen minutes out.** Every condition column in part four goes in now. Written now this takes ninety seconds and is accurate. Written tomorrow it is reconstruction, and reconstruction is how "I think that one had a link in it" enters a dataset you intend to make decisions from.

One of those fields is a single sentence saying what this post was for. Write it before you see any numbers, because otherwise intent gets rewritten to match the outcome. A post that lands becomes "the framework post we planned". The same post, flat, becomes "just a quick thought". Fixing the intent in writing beforehand costs one line and removes an entire category of self-deception from the review.

**3. The warm-up, in the last fifteen minutes.** Comment substantively on five to ten posts in your feed. Comments, not reactions. The stated mechanism, which is folk knowledge and not platform documentation, is that this makes you visible to the people whose feeds will carry the first test of your post. Treat the mechanism as unverified. The ordering still holds on the argument in part one: the same fifteen minutes spent afterwards cannot help a post that is already out, and it competes directly with the hour you are about to owe your own comment section.

### Zone B, the first sixty minutes

The first hour is widely described as an evaluation window, on a model where the post is shown to a small cohort, with numbers in the low hundreds repeated across sources, and wider distribution follows from that cohort's behaviour. None of that is platform-published. The behaviour it implies is worth doing anyway, because it is what you would do for a live conversation regardless.

- **Be present.** This is the real scheduling constraint, and it outranks any chart of optimal posting times. Publish when you can sit with the post for an hour. A generically ideal slot you cannot attend is worse than an ordinary slot you can. If you want a default, mid-morning in your audience's main timezone is the common choice, and it is a choice rather than a finding.
- **Reply to every comment, individually, with something that can be replied to again.** A reply that closes the thread and a reply that opens it cost the same amount of typing.
- **Message three to five specific people whose genuine opinion you want, and ask for a comment, not a reaction.** Ask about the actual question in the post. Do not send five people the same text, which reads as a broadcast and earns broadcast-quality replies.
- **At sixty minutes, capture the early columns** and stop. The offset is fixed. Checking again at eighty minutes because sixty looked thin is the first step of capture drift, and it ruins comparability across your own rows.

### Zone C, forbidden inside the window

Each of these has a reason that does not depend on any borrowed number.

1. **Do not edit the post.** Editing inside the first day or two is widely reported to suppress reach, magnitude unverified. Set that aside, because the durable reason is different: an edit changes this row's own conditions mid-flight, so the row can never be compared with any other row in the sheet. Handle a typo with a comment from your own account. It costs nothing, the correction is visible, and a comment is a signal you wanted anyway.
2. **Do not publish a second post.** It cannot be undone, it splits your attention across two windows, and it changes hours-since-previous for the next row as well as this one.
3. **Do not run a rescue burst.** Mass reactions, a flurry of comments, or a message to a group chat asking people to engage produces a spike of shallow signal from people with no interest in the topic. Whether or not that pattern is detected as automation, it contaminates the row beyond recovery.
4. **Do not add the link you deliberately left out.** It is an edit, and it is an edit to the exact condition most likely to be the thing you were testing.
5. **Do not delete the post.** This is the most tempting move at forty-five minutes on a quiet post, and it destroys the only thing a quiet post can still produce, which is a row.
6. **Do not move the reporting offset because the numbers look thin.** See above. The offset is part of the measurement, not part of the post.

## Part three. The signal hierarchy, ordered and unweighted

Engagement signals are widely reported to rank roughly in this order, and this ordering is unverified by the platform:

1. **Dwell time**, meaning how long a reader stays with the post.
2. **Saves.**
3. **Sends and shares**, with a private send commonly described as stronger than a public repost.
4. **Comments**, with genuine back-and-forth described as much stronger than single replies.
5. **Reactions**, the weakest.

Specific multipliers circulate for these, of the form "a comment is worth so many reactions". They are not repeated here. None is platform-published, and a precise multiplier is exactly the borrowed number that turns a useful ordering into a false claim. The ordering is what is worth having, and only because it resolves craft decisions that are otherwise arguments about taste:

- A two-line post cannot produce dwell however good the two lines are, because there is nothing to stay with. A wall of unbroken text fails the other way, by being abandoned. The common heuristic is a post that takes roughly thirty to sixty seconds to read, which is a heuristic and not a measurement, but the structural point holds without it.
- A framework, a checklist or a table of numbers is what produces saves. Almost nothing else does.
- Content that makes one reader think of one specific colleague is what produces sends.
- A closing question exists to produce comments, and it has to have more than one defensible answer, or it produces agreement rather than conversation.
- A punchy fragment post that produces only reactions is optimised against the list.

If your own rows eventually contradict this ordering, your rows win. Part four exists so that they can.

## Part four. The record schema, and the question each column answers

A column earns its place by naming a question somebody will ask later that cannot be answered without it. Three tests, applied in this order:

- **Perishability.** Can it be recovered later? If yes, it is cheap to skip. If no, it is the priority, and almost every column in the first block below is in this category.
- **Queryability.** Will it ever be the thing you group by, filter on, or compare across? If it can never appear in a filter or a pivot, it is decoration.
- **Fillability.** Can you get it every single time, in seconds? A column that needs manual counting will be blank within three weeks, and a half-filled column is worse than no column, because it invites conclusions drawn from whichever rows happen to have data.

### Block one. Conditions, authored before the publish, perishable

This is the block nobody keeps, and it is the one that makes a diagnosis possible.

- **`post_id`.** Joins the row to the post itself and to any screenshot. Answers: which row is this, once the feed has moved on.
- **`published_at`**, full timestamp with timezone, raw and unbucketed. Answers: does anything vary by hour or weekday. Bucketing at write time discards the only version you cannot rebuild.
- **`hours_since_previous_post`.** Answers: were the weak ones crowded together. This is the most commonly missing column in any tracker and the one without which spacing cannot be argued about at all.
- **`pillar`.** Answers: which topic is being amplified. The thirty-day review is a grouping on this column and on nothing else.
- **`format`**, one of text, text with image, document or carousel, video, poll, repost with comment. Answers: does one format do the saving while another does the commenting.
- **`links_in_body`**, zero or one, plus **`link_destination`**. Answers: is the link cost real on my account. The highest-value flag in the sheet: it is the most commonly blamed cause of a reach collapse, the cheapest thing to test, and invisible afterwards if anyone edits the post.
- **`first_comment_link`**, zero or one. Answers: does the workaround carry its own cost. The workaround is contested, and you cannot form a view on it without separating it from the in-body case.
- **`hashtag_count`.** Answers: does the cap I inherited matter. Cheap to record, and the only way to eventually retire a rule you never chose.
- **`publishing_route`**, native app, official-API tool, or other. Answers: did the tool do this. Turns a suspicion into a filter, and it is the column that catches a one-off exception three months later when nobody remembers it.
- **`intent_sentence`.** One line, before publishing, on what this post was for. Answers: was this a success, as a fixed question rather than one re-decided after the numbers land.
- **`target_segment`.** Answers: who was this aimed at, which is what turns the inbound-request column from a count into a rate.

### Block two. Outcomes, captured at fixed offsets

**Put the offset in the column name so drift is visible.** Write `comments_60m`, never `comments`. A number captured at a drifting age cannot be compared with any other number in the sheet, and drift is the quietest way a tracker stops meaning anything. Three offsets are enough: sixty minutes, forty-eight hours, seven days.

- **`comments_60m`.** The input to the decision rule in part five.
- **`your_replies_60m`.** Separates "the post got no comments" from "the comments got no replies", which are different failures with different fixes and are indistinguishable in a single engagement number.
- **`comment_depth_48h`**, counting replies that are themselves replies to a reply. Answers: was this a conversation or a receiving line. Top-level comment count cannot answer it, which is why depth is a separate column rather than a derived one.
- **`saves_48h`.** The second-ranked signal, and the direct test of whether your reference-grade content is landing.
- **`sends_or_shares_48h`.** The third.
- **`profile_views_48h`.** Answers: did the post make anyone curious about you rather than about the post, which is the step immediately before an inbound message.
- **`inbound_requests_7d_from_target`.** The closest thing to a business outcome the platform will hand you. Read it as a rate against `target_segment`, not as a count.
- **`reactions_48h`.** Keep exactly one column. You need it as a denominator for a comment-to-reaction ratio and to spot a rescue burst after the fact. You do not report it.
- **`impressions_48h`.** Log it, do not report it. It is the denominator that turns every other column into a rate, and a rate is the only form that survives a change in follower count. Reporting it as the headline number is the habit this whole schema exists to replace.
- **`outcome_note`**, filled at seven days. One line on what actually came of it: replies, a call, a signup, nothing.

**A hard caveat on availability.** Not every platform exposes every field to the author. On LinkedIn, author-side post analytics reliably surfaces impressions, reactions, comments and reposts, while saves and sends are not consistently surfaced, and what you can see differs between a personal profile and a company page. Check your own analytics screen before you build the sheet, and where a field is unavailable, **delete the column rather than filling it occasionally**. A mostly blank column invites an average taken over the handful of rows that have data, which is the worst analysis in this file.

### Block three. Columns not worth logging, and why

- **`follower_count_at_publish`.** Moves slowly and in one direction. It will never separate two rows a week apart, and where scale matters you want a rate, which `impressions_48h` already provides.
- **A stored `engagement_rate` composite.** Store the parts and compute the composite in the view. A stored composite silently changes meaning the day you change its definition, and it hides which part moved.
- **A self-rated quality score.** Unfalsifiable, and it is rated by the judgement the sheet exists to check. In practice it tracks the author's mood on the day.
- **Comment sentiment.** Expensive to fill, almost never the thing you group by, and the most interesting case, a post that provoked real disagreement and therefore depth, reads as negative.
- **A bucketed `time_of_day`.** Keep the raw stamp. A bucket is derivable from a stamp and a stamp is not derivable from a bucket.
- **Free-text topic tags beyond the pillar.** They proliferate to roughly one per row within a month, at which point they group nothing.
- **Post number, title, status and date on their own.** This is the tracker that motivates the schema. Not one of the four is a condition, so a sheet made only of them can tell you what you published and never why it did what it did.

## Part five. The decision rule at sixty minutes

Inputs: `comments_60m`, whether any thread reached a second level, your own reply count, and the condition block you wrote before publishing.

- **Moving.** Comments arriving, at least one thread at a second level. **Do nothing extra.** Keep replying, including past the hour while the conversation is alive, because that is the one action with no cost. Do not add a link, do not edit in a call to action, do not publish the follow-up you now feel like publishing.
- **Flat, and a condition in your own row explains it.** A link in the body, hours-since-previous under your floor, an unusual publishing route, a format you have never used. **Do nothing to this post.** Write the suspected cause into the row now, while your memory of it is accurate, and change exactly one condition on the next post. One. Changing three gives you a row you cannot attribute.
- **Flat, no condition explains it, and the post was a reasonable one in an established pillar.** Close the window and let the forty-eight hour capture happen before forming any view. A single flat post in a pillar with history is noise until it has company.
- **You cannot tell, which is the common case.** A few reactions, one or two comments, no depth, nothing visible in saves. **The instruction is: do nothing and let the window close.** Not consider doing nothing. Do nothing, and close the tab if that is what it takes. Three reasons, and the third is the one that binds. An hour of behaviour from a cohort in the low hundreds is too small a sample to separate a weak post from a slow hour on a quiet day, and that arithmetic holds even though the cohort model itself is folk knowledge. Every intervention available to you is either irreversible, as publishing again and deleting are, or widely reported to carry a cost, as editing is, and not one has a documented upside. And any intervention adds an uncontrolled variable to this row, which is the only durable thing a disappointing post can still produce. Spending the row to feel better at forty-five minutes buys nothing and costs you the evidence that would eventually let you replace every borrowed number in this file with one of your own. The tie-break, for when the reasoning is not to hand: **if you could not record the action as a column, do not take it.**
- **The post is wrong, names somebody incorrectly, or breaches a policy.** This is not a reach decision and the protocol does not apply. Correct it or remove it immediately, and record that you did, with the reason, so the row is marked as an exception rather than sitting in the dataset pretending to be normal.

## Part six. Spacing, volume, and the thirty-day loop

**Spacing.** The widely repeated convention is a minimum of twenty-four hours between posts, forty-eight while recovering from a reach collapse, with two posts inside a few hours described as capable of suppressing both. None of this is platform-documented. Adopt it as the default value of your spacing floor and then test it against your own rows.

**Volume.** Three to five posts a week is the commonly repeated band, with consistency described as mattering more than volume. The durable part needs no citation: this protocol costs an hour of attendance per post, so four posts a week is four hours a week of window. Set volume from the hours you actually have, not from a chart.

**The thirty-day loop.** Group by pillar. Compare on saves, sends and comment depth, never on impressions. Then, before concluding anything, **check the condition columns of the rows you are comparing.** If the weak pillar's rows are also the rows carrying links in the body, you have found a link result wearing a pillar costume. Keep the pillar set at two or three and hold it for a full quarter. Topic authority is described as accruing over a couple of months and resetting when you switch, which is another unverified mechanism, but the measurement reason stands alone: switching pillars means you never accumulate enough rows per pillar to conclude anything about any of them.

**The replacement rule, which is the point of the whole instrument.** At roughly twenty to thirty rows you can start answering your own questions, and at that moment every default in this file becomes a hypothesis you are equipped to overturn. Run three comparisons first, because they are the cheapest and the most contested: rows with a link in the body against rows without, matched on pillar and format; rows above your spacing floor against rows below it; each format against the others on saves. Write your own numbers into your copy of this protocol with the date and the row count beside them. A number of yours with two dozen rows behind it is worth more than any number of ours.

## Worked example, compressed

A two-person data consultancy selling warehouse migration work publishes on a professional network. Their tracker has four columns: number, title, status, date. Their dashboard reports impressions, which have fallen by roughly two thirds over six weeks. The conclusion in the room is that the topic is tired and they should write about something else.

They adopt the protocol in week seven. Over the next six weeks they log twenty-two rows with the full condition block, and then backfill what survives for the previous eighteen posts. Recoverable: publish timestamp, format, and whether a link sits in the body, because all three survive in the post itself. Not recoverable: publishing route, since two people used two different tools; whether any post was edited and when; and intent, since nobody wrote it down.

The thirty-day review, grouped by pillar:

- **Pillar A, migration war stories**, fourteen rows: saves per thousand impressions clearly the highest, comment depth the highest.
- **Pillar B, tooling opinions**, twelve rows: reactions high, saves near zero, depth near zero.
- **Pillar C, company news**, eight rows: everything low.

The naive read is to cut Pillar B. The condition check says otherwise. Nine of the twelve Pillar B rows carry `links_in_body = 1`, because a tooling opinion naturally links to the tool. On the three that do not, saves sit in line with Pillar A. Separately, five of the twelve have `hours_since_previous_post` under twenty, because tooling opinions are quick to write and get published in pairs on the same afternoon.

Pillar B is confounded with two conditions at once, so nothing about the pillar has been established.

**Verdict: keep all three pillars, remove in-body links from Pillar B for two weeks, then hold a twenty-four hour floor for two more, changing one condition at a time. Treat the first eighteen posts as permanently undiagnosable.** That last part is the real finding. The original six-week decline coincides with a period when both people were publishing and the gap between posts was often a few hours, but that cannot be shown, because route and edit history were never written down and cannot be reconstructed. The instrument now exists. Six weeks of rows moved the conclusion from "change the topic", which would have thrown away the pillar with the best saves, to "change two conditions", and the only reason the second conclusion is available is that eleven columns were filled in before each publish rather than after.

## Failure modes

**The warm-up on the wrong side.** The fifteen minutes of commenting happens after publishing, usually as a way of filling the anxious wait. From the outside the routine looks disciplined and identical to the correct one. The tell is the calendar: the commenting block sits after the publish slot rather than before it.

**The typo edit.** A colleague spots a missing word, the post is edited within minutes, and everyone moves on. It looks like conscientiousness. What actually happened is that the row's condition block no longer describes the post that was distributed, so the row is unusable, and the post now carries a visible edited marker.

**The rescue burst.** Forty-five minutes in, a message goes to a group chat. Twenty reactions and four short comments arrive in ten minutes. The post appears to recover. The row now blends organic response with solicited response and can never be compared with any other, and the same tactic gets credited for a result nobody can attribute.

**The four-column tracker.** Number, title, status, date, kept faithfully for a year. It looks like measurement, and it is a publication log. Its uselessness is only discovered on the day somebody asks why reach fell, which is the one day it cannot be fixed retroactively.

**Capture drift.** Numbers get recorded whenever somebody remembers, so early rows are captured at a day and later rows at a week. Every comparison in the sheet now has an age confound baked in, and the direction of the error is invisible because nothing records when the capture happened.

**The impressions dashboard.** Impressions are reported monthly and go up. Saves, sends and depth are not reported at all, so a drift towards content that gets seen and does nothing is invisible for two quarters, right up until somebody asks how many conversations came from any of it.

**The confounded cut.** A pillar is cut on its numbers without checking the condition columns of its rows, and the team removes the topic rather than the link. The evidence for the decision looks solid, because the pillar column really does correlate with the outcome.

**The unlogged exception.** One post goes out from a different tool, or two days after a holiday, or with a link because it was a launch. Nobody logs the exception. Three months later that row sits in the middle of a comparison as an ordinary row, and it is the one pulling the average.

**Retroactive intent.** With no intent written beforehand, a flat post is remembered as a throwaway and a strong post as the planned one. The review then confirms whatever the author already believed, in good faith, every single month.

**Borrowed numbers treated as law.** A percentage from a vendor study becomes a rule, the rule becomes a constraint on what gets published, and nobody ever checks it against their own rows even after two hundred posts, because the number sounds specific enough to be a fact.

## What this skill does not do

- It does not know the ranking. The platform publishes no ranking documentation, and every weight, cohort size, penalty and window in this file that came from outside is labelled as a community estimate. The tracker exists so you can replace them, and the honest expectation is that some of them are wrong.
- It cannot fill a column your analytics does not expose. Saves and sends are not consistently surfaced to authors, and coverage differs between a personal profile and a company page. Check first, and delete a column you cannot fill.
- It does not write the post. A well-run window on a weak post produces a well-documented weak post, and the schema will tell you so honestly at forty-eight hours.
- It records but it does not conclude. Below roughly twenty or thirty rows, or where rows differ on several conditions at once, the sheet holds evidence and settles nothing.
- It is not incident response. An account whose distribution has already collapsed needs a cause taxonomy and a ramp, not a steady-state routine, and running this protocol during a suppression will produce rows measuring the suppression.
- It does not cover paid distribution, employee advocacy, or anything where the audience is bought rather than earned. Every column here assumes organic distribution, and a boosted post belongs in a different sheet.
