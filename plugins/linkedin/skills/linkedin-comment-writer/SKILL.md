---
name: linkedin-comment-writer
description: Writes a single LinkedIn comment for a specific target post, built to clear the platform's default Most Relevant sort rather than sink to Most Recent. It runs a computable audience-overlap check on the post's own commenters before writing a word, picks from six named comment archetypes suited to the post's content, and applies a decision rule that can end in commenting publicly, not commenting, or recommending a private message instead. This skill should be used when a specific post is already open and it is not yet decided whether, or how, to comment on it, when a daily commenting routine risks becoming a volume habit rather than a selective one, or when a comment already posted needs a verdict on whether to edit or extend it.
---

# LinkedIn comment writer

## The claim this skill is built on

Most commenting advice treats a comment as either politeness, leave a nice note under something good, or a volume tactic, comment on twenty posts before lunch and see what sticks. Both treat every comment as equally visible once posted, and neither survives contact with how LinkedIn actually shows comments to anyone other than the person who wrote them.

LinkedIn documents that comments are sorted by Most Relevant or Most Recent, that the reader controls which view they see, and that "some comments will only be visible under Most recent view" (LinkedIn Help, checked August 2026). That single sentence rewrites the exercise. A comment that never earns the default Most Relevant sort is not a quiet, low-reward comment sitting somewhere near the bottom. For almost every reader who opens that thread, it is not there at all, because nobody switches to Most Recent to go looking for it.

That is what makes volume the tactic that fails hardest, rather than the safe fallback it is usually treated as. LinkedIn separately documents that it "may limit how many comments a member... can make in a certain time period" and that it "may limit the visibility of those comments" where it detects "excessive comment creation or use of an automation tool" (LinkedIn Help, checked August 2026). A high-volume commenting routine does not just spread effort across a larger number of low-yield comments. It can actively suppress the visibility of the comments it produced, on the platform's own stated terms.

The method below treats commenting as a ranking-and-selection problem, worked in this order: decide whether the post is worth a comment at all, choose an approach built to earn a reply rather than agreement, know the documented mechanics that govern a comment after it is posted, and cap the day's volume by how many posts genuinely clear the bar rather than by a target count. Writing first and deciding these questions afterwards, which is what both the politeness model and the volume model do by default, skips every step that actually decides whether anyone reads the comment at all.

## Part one: the mechanics that decide what happens after you post

Six documented facts govern a comment's life after publishing, and mixing them up with adjacent but different rules is the most common way this goes wrong in practice.

**Length.** The comment character limit is not documented anywhere by LinkedIn; the Comments API specifies no maximum for the comment text field. Third-party testing puts the practical ceiling at roughly 1,250 characters, though other testing reports figures as high as 1,750, and the sources disagree with each other. Treat 1,250 as a rough working ceiling, not an official wall, and never repeat it as a documented figure.

**Truncation.** Whether a long comment collapses behind a "see more" cut is itself disputed, and no character count for such a cut has been found anywhere. Some testing reports the full text always displays; other testing reports that long comments do collapse, without naming a threshold. Do not import the familiar post-truncation figures, roughly 210 characters on desktop and 140 on mobile, into a comment. Those describe a different object. Front-loading the point of a comment is good practice on its own merits, not a defence against a documented comment cutoff, because no such cutoff is currently documented.

**Editing.** A comment can be edited at any time, with no documented time limit, and an edited comment carries an "(edited)" label once changed. This is a genuine, useful contrast with LinkedIn direct messages, which carry a documented 60 minute edit window. The two get conflated constantly. In practice: a comment posted with a typo or a softer version of the point than intended can be corrected hours or days later without the deadline pressure a message edit carries.

**What a comment can hold.** A comment can carry an @mention, an emoji, a GIF, or an image attachment. An image is the most underused of these for the archetypes in part three, since an actual screenshot of a number or a chart is a stronger version of the counter-data approach than describing the same number in prose. Reserve a mention for someone whose presence is genuinely relevant; a mention with no comment text attached is its own failure mode, covered below.

**Author tags and thread controls.** When a post's author comments on their own post, LinkedIn shows an Author tag next to it. A thread where the author's tagged replies are frequent is genuinely active and worth noticing before commenting, since a specific question stands a real chance of an answer where the author is visibly still engaged. Separately, a post owner can disable comments entirely, restrict them to first-degree connections, or block a specific member. Check this before spending time on an approach: no archetype below gets past a closed thread.

**One piece of history, dated on purpose.** In 2017, LinkedIn's engineering team documented that most comments generated a "Comment Viral Update" distributed to the commenter's own connections, and that this update type received the highest engagement of any feed update, at roughly 2.5 times the rate of an ordinary connection update. That description is nine years old and describes a feed LinkedIn has since rebuilt; treat it as history, not current mechanics. What LinkedIn currently documents, as of August 2026, is narrower: reactions, comments and reshares are "viral actions" carrying "downstream and upstream network effects" in how a post is ranked. That is a statement about a post's ranking, not a claim that a comment is distributed as its own feed unit. The safe reading is that a comment's exposure still rides on the post it sits under, through the network-effect logic LinkedIn currently documents for posts, rather than assuming the 2017 mechanic still fires as originally described.

## Part two: whose posts are worth a comment, the audience-overlap check

A comment on a post read mostly by the wrong people does not become more valuable by being well written. The check below is built to be computed, not felt.

Open the target post's comment thread on its default Most Relevant view. Read the first ten to fifteen visible commenters. For each, note the stated title or function shown on the name line under their comment, which is usually enough without opening every profile individually. Count how many match your own target audience's function or seniority band, meaning the kind of person you actually want reading your comment, not simply the kind of person who happens to be scrolling that day.

- **At or above roughly four in ten matching.** Strong candidate. The room already skews toward your audience, so a well-built comment reaches people worth reaching regardless of how large the poster's total following is.
- **Between roughly one and four in ten.** Marginal. Worth commenting only if the post itself speaks directly to a problem your target audience states in those terms, since a mixed room still contains some of your audience, and a sharp, specific comment can be the one thing in the thread that speaks to them.
- **Below roughly one in ten.** Skip, even where the poster has a very large following. A large audience that is not your audience does not become your audience because the follower count is high; it means a comment, however good, is mostly overheard by people who cannot act on it.

**The cannot-tell branch.** Some threads make this uncheckable: fewer than about five visible comments to sample, commenters whose titles are not shown, or a thread restricted so it cannot be opened before deciding. When that happens, do not default to commenting anyway on the theory that a good comment is safe regardless of the room. Default instead to a single low-investment test: post one narrow, specific comment, the named-question archetype in part three is the cheapest version of this test, and use whether the author replies, and who replies underneath, as the audience-overlap data that was missing going in. Treat that thread's answer as informing the next decision about that specific poster, not as a general rule about the topic.

## Part three: six archetypes that tend to earn Most Relevant

None of these are guaranteed by a documented ranking signal, since LinkedIn does not publish what moves a comment between Most Relevant and Most Recent. What is documented is that comments, alongside reactions and reshares, function as viral actions with network effects on the post they sit under; a comment that earns replies and reactions is participating in that documented mechanic, whatever the precise sort logic turns out to be. The six approaches below are built around that checkable proxy in the absence of a published formula.

**The counter-data comment.** State a specific, checkable number or example from your own work that supports, complicates, or narrows the post's claim. Suits a post making a general claim with no number attached; a comment supplying the missing number does work the post itself did not do.

**The named disagreement.** State precisely where you diverge, in one sentence, followed by the specific reasoning behind it. Suits a sweeping claim inside your own area of practice, where a genuine competing case exists to point to. This is the highest-risk archetype of the six; without a specific case behind it, it collapses into the generic contrarian comment covered in the failure modes below.

**The extension.** Take the post's own framework or model and apply it one step further, to a case the post did not cover: a different scale, a different failure mode, a downstream consequence. Suits a post presenting a framework or a named pattern rather than a single anecdote, since a framework is the kind of object that invites being run somewhere else.

**The named question.** Ask one specific question that only the author's own stated experience can answer, never a generic prompt such as "thoughts?" Suits any post, and is strongest where the author has already replied to earlier comments in the same thread, since an active author is more likely to answer, and that reply is itself a second engagement event underneath your own comment.

**The corroborating case.** A short, specific account of the same causal mechanism appearing in your own work, in a different setting. The distinction from plain agreement is that the mechanism is named and shown recurring, not that the feeling is confirmed. Suits a post explaining why something failed or why something worked, since a mechanism claim is the kind of thing a second data point actually tests.

**The narrow correction.** A sourced correction of one specific fact inside an otherwise sound post, offered without undermining the rest of it. Suits the rare case of genuine certainty with a source to point to directly. Used often, or used on something not actually certain, this becomes the "well, actually" failure mode further below.

## Part four: what reliably gets ignored

These are not weaker versions of the archetypes above; they are a different category, built to not require a reply at all.

- **Agreement alone.** "Great point", "So true", "Completely agree", with nothing attached. Nothing in it invites a reply, because there is nothing left to respond to.
- **Congratulation alone.** Appropriate as a private message or a reaction, not as the whole content of a public comment, for the same reason: it closes the exchange rather than opening one.
- **Restatement.** Paraphrasing the post's own point back at the author. It reads as proof of having read the post rather than as a contribution, and an author has little reason to reply to their own idea reflected back at them.
- **The bare reaction in text form.** A single word, a single emoji, or a short string of them with no sentence attached. It carries less information than the reaction button sitting directly underneath the post.
- **The generic prompt.** "Thoughts?", "Anyone else seeing this?", "Curious what others think." It asks the reader to do the specific work the comment itself should have done.
- **The tag-and-run.** An @mention with no comment text, or a single word alongside it. It reads as outsourcing the comment to whoever was tagged rather than as a contribution from the person who wrote it.

## Part five: the decision rule

Run this after the audience-overlap check in part two, once it is clear whether the room is worth commenting in at all.

- **Post clears the audience-overlap bar, a genuine archetype fits, an actual number, disagreement, extension, question, corroboration or correction exists to offer, and the thread is open: comment publicly.** Choose the archetype that fits what is genuinely available, not the one that sounds most impressive; an honest question beats a forced disagreement that is not really held.
- **What there is to say is actually a pitch, a solicitation, or a plug for something sold or represented: do not post it as a public comment, whatever archetype it is dressed in.** This is the covert pitch failure mode below. If the underlying reason to reach the poster is real, that is a case for a private message instead, said plainly as what it is, not smuggled into a thread as disinterested commentary.
- **Post fails the audience-overlap bar and no specific angle exists: do not comment.** Spend the time on a post that clears the bar instead. A comment written to hit a daily count on the wrong room is the volume tactic this method exists to replace.
- **Audience overlap cannot be checked, the cannot-tell branch from part two: post one narrow test comment**, the named-question archetype is the cheapest version, rather than committing fully or skipping outright, and use the reply pattern underneath it as data for the next decision about that poster.
- **What there is to say depends on detail that does not belong in public**, a client's real numbers, a criticism of a named third party, something identifying someone who has not consented to being discussed: take it to a private message regardless of how well the post scores on overlap. Overlap tells you whether a room is worth being in, not whether what there is to say belongs in it.

## Part six: cadence, without a number to hide behind

LinkedIn documents that it may limit how many comments an account can post in a period and may suppress comments it judges excessive or automated, without publishing the number that triggers either. That absence is not a loophole. Cadence cannot be set against a safe count, because no published count exists to set it against.

The rule that survives that absence: cap the day's commenting by how many posts genuinely clear both the audience-overlap bar and a real archetype, not by a target number decided in advance. A day where only two posts clear the bar is a two-comment day. Reaching for a third by lowering the bar, or by writing a comment that fits an archetype's shape without the specific content behind it, produces the templated-comment failure mode below, and templated behaviour by a human typing manually is the closest thing to a working definition of "automated" a person can still produce by hand.

## Worked example, compressed

A freelance data analyst wants visibility with marketing operations leads. A well-known founder posts a broad claim: "Most attribution models are theatre." It has ninety comments within three hours.

**Audience-overlap check.** The first twelve visible commenters under Most Relevant: four founders, three in sales, two in engineering, one in marketing operations, two unclear from the name line. One in twelve matches, just inside the marginal band rather than a clean skip, and the topic sits close enough to the analyst's own work to be worth the marginal case.

**Archetype selection.** No genuine disagreement exists; the analyst mostly agrees with the post. The counter-data comment fits instead: "Rebuilt last-touch to a simple three-channel model for a client last quarter, no other change. Reported spend on paid social dropped 40 per cent and the marketing operations team's own dashboard didn't need touching. The theatre wasn't the model, it was the twelve dashboards nobody had reconciled since it was built." Specific, checkable in shape, adds a number the post did not have, and gives a marketing operations reader something to recognise from their own week.

**Decision rule applied.** Marginal overlap, genuine archetype fit, thread open, nothing covert inside it. Comment.

**Verdict.** Post it once. Do not follow up with a second comment restating the same point if it earns no reply within a day; a marginal-overlap thread earns one attempt, not a campaign, and a second comment repeating the point reads as the tag-and-run failure mode without the tag attached. The analyst's remaining time that day is better spent finding a thread where marketing operations leads are the majority in the room, not the minority.

## Failure modes

**The covert pitch.** A comment framed as a general observation that is actually steering the reader toward a product, a service, or a link, without saying so. It reads as manipulative the moment a reader notices the pattern, and readers notice faster than the writer expects, because promotional phrasing survives even when the surface topic changes.

**The templated comment.** A comment built from a reusable shape, an opening compliment, a bridge phrase, a closing question, applied across many posts with only the nouns swapped. It looks fine on any single post in isolation. Read down a feed across several comments from the same account, the shape repeats, and it reads as automated even when every word was typed by hand, which is exactly the pattern LinkedIn's own suppression rule is aimed at.

**The generic contrarian.** Disagreement with nothing specific behind it, "I don't think that's quite right" with no case, number or reasoning attached. It reads as a bid for attention rather than a contribution, and it invites a reply asking for the specifics that were never there to give in the first place.

**The well-actually correction.** A narrow factual correction delivered in a way, or at a frequency, that reframes the exchange as a status contest rather than an addition. The correction itself may be accurate; the failure sits in how often it is reached for, and how little it concedes about the rest of the post it is attached to.

**The wrong-room comment.** A strong, specific comment posted on a thread that never cleared the audience-overlap bar. The comment itself may be excellent and still reach almost nobody who could act on it, because the room was never checked before the time was spent writing it.

**The quota comment.** A comment written to hit a personal daily count rather than because a specific post and a specific angle both genuinely existed. It is the clearest way the volume model this file argues against reintroduces itself after the audience-overlap and archetype steps have already ruled it out on paper.

**The restricted-thread walk-in.** Time spent drafting a comment for a thread that turns out to be restricted to first-degree connections, or for a poster who has the commenter blocked, discovered only after the comment is already written. Checking whether the thread is open belongs before archetype selection, not after.

**The unedited error left to sit.** A comment posted with a wrong number or a garbled sentence, left uncorrected on the mistaken assumption that LinkedIn comments cannot be edited later, or on confusing that fact with the documented 60 minute limit that applies to direct messages instead. Comments carry no documented edit deadline; the fix is a return visit, not a write-off.

## What this skill does not do

- It does not know what actually moves a comment into Most Relevant. LinkedIn documents that the sort exists and that it hides some comments under Most Recent instead; it does not publish the ranking signal, so every archetype here is built on the checkable proxy of replies and reactions, not on a confirmed formula.
- It does not compute the audience-overlap check on its own. Reading a thread's visible commenters and their stated titles is manual work that sits outside this skill.
- It does not write the post being commented on, or the profile the comment links back to. Both affect whether a comment converts into anything beyond itself, and neither is covered here.
- It does not know today's actual comment rate-limit threshold, because LinkedIn has never published one. It can only say the limit exists and that cadence should be set by substance rather than by a guessed number.
- It cannot get past a post where comments are disabled, restricted to first-degree connections, or where the account has been blocked. Those are the post owner's controls, and this method has no way around them.
- It does not draft or manage the private message that the decision rule sometimes points toward instead of a public comment; writing that message is a separate job.
