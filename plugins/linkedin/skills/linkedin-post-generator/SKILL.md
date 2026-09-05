---
name: linkedin-post-generator
description: Drafts a single LinkedIn feed post from raw material, a result, an opinion, a launch, a lesson or a story. It runs a decision rule that picks the post format from the material itself, including a branch for when the material does not clearly fit one type, writes the opening line to survive LinkedIn's undocumented truncation fold, and applies a link, hashtag and media policy built only on what LinkedIn has actually published or consistently observed rather than on repeated but unconfirmed advice such as a golden posting hour or a fixed link penalty. This skill should be used when a founder, marketer or operator has something worth posting on LinkedIn and needs it turned into one complete, publish-ready post rather than a general outline or a list of tips.
---

# LinkedIn post generator

## The claim this skill is built on

A LinkedIn feed post is a truncation problem before it is a writing problem. Most of a post's job happens in the handful of words visible before a "see more" cut, and everything after that cut only gets read by someone who already decided, from those few words, that reading on was worth it. Ask a strong model to write a LinkedIn post and it produces something that reads well end to end and fails exactly there: a throat-clearing first line, a hashtag block at the bottom, a link tucked into a comment "to protect reach," and a note about posting in the next hour "while engagement is highest." Four confident moves, and as of August 2026 none of them survives contact with what LinkedIn has actually published.

Hashtags do not affect distribution, LinkedIn's own VP of Product Management said so directly in December 2025. The exact truncation point is not documented anywhere, on either surface, which makes writing to a specific invented number exactly as reliable as guessing one. The link question is not settled by anyone, including LinkedIn, whose own denial of a penalty sits next to independent studies that disagree with each other by nearly three hundred percentage points. And the golden hour, a sixty-minute test window that supposedly decides a post's fate, appears in no LinkedIn document at all, checked across every feed engineering post LinkedIn has published.

None of that means nothing is knowable. The knowable part is narrower and more mechanical than the advice implies: a documented character ceiling, a documented set of ranking signal categories with no published weighting, a documented list of what LinkedIn says it demotes, and hard mechanical constraints on what a post can physically contain. This file is built on that checkable ground, plus a method for the part LinkedIn genuinely leaves open: which format the raw material wants, and what the opening line has to do regardless of exactly where the fold sits on the day it is read.

## Part one. Set the fold budget before you write the hook

Order matters here specifically because writing the hook first and trimming it to fit afterwards produces a different sentence than writing it inside the constraint from the start. A trimmed hook keeps its original shape and loses its ending. A hook written to the constraint is built to make its whole point before the constraint arrives.

LinkedIn documents a hard ceiling: a feed post's text tops out at 3,000 characters, counting spaces, line breaks and emoji, a figure published in LinkedIn's own API reference. Nothing else about length is published. The "see more" fold, the point at which a reader has to tap to keep reading, is never documented by LinkedIn on desktop or in the mobile app. Third-party figures for desktop range across 200 to 220 characters; for mobile, across roughly 130 to 150. The fold is rendered from viewport width, font size, app version and LinkedIn's own ongoing tests, which is why no source has produced a reproducible number and why this file will not publish one either.

The operational answer is to write for the tightest widely cited figure, roughly 140 characters, and treat that as a design target rather than a fact. A first line built to make a complete, standalone point inside that budget survives every version of the fold anyone has reported, on both surfaces. A first line that only makes sense once the second and third lines arrive survives none of them.

The check this replaces is not optional: before publishing, paste the finished draft into LinkedIn's own composer and look at the live preview on desktop, then open it in the mobile app and look again. That is the only place the real fold for that specific post, on that specific day, can actually be seen. A method built on a wrong constant is worse than a method that admits the constant is unknown and checks instead.

## Part two. Choose the format: the decision rule

Raw material rarely arrives already labelled. A founder's note, a customer conversation, a metric from last week and a half-formed opinion all look similar until a format is chosen, and the format decides almost everything downstream: whether a link belongs, how long the post runs, which hook fits.

**Specific event, a turning point, a changed outcome: use the story format.** Something was at stake, a decision was made, the situation changed. Structure: a scene, the tension inside it, the specific decision taken, the result, one transferable rule at the end.

**A number produced by something you did: use the result format.** A metric, an experiment outcome, a before-and-after. Structure: the number stated plainly in the opening line, the method compressed into two or three lines, one honest caveat named rather than hidden, then the implication.

**A stance with no personal narrative attached: use the opinion format.** A claim someone could disagree with. Structure: the claim as the first line, the reasoning in the next two or three, then a stated implication rather than an open call to debate it in the comments.

**A product, feature or event going live: use the launch format.** Structure: what is now true that was not true before, who it is for, then the mechanics, with the link decision from part three applied deliberately rather than by habit.

**A set of takeaways with no single narrative thread: use the lesson format.** A numbered or clearly separated list, each line able to stand alone if it is the only line someone reads or quotes, since list items get shared and screenshotted individually far more than paragraphs do.

**The branch for when you cannot tell.** Raw material regularly straddles two of the above, a lesson wrapped inside a launch, a story really making an opinion's argument. When it is genuinely ambiguous, ask one question: which single sentence, if it were the only thing left after the fold, would still make a stranger stop scrolling? A specific number points to a result post. A claim a reader could agree or disagree with points to an opinion post. A moment in time with something at stake points to a story. Default to story when even that test does not resolve it, because a story's opening line still functions as a complete thought when read as a fragment, which is the actual failure mode a fold creates.

## Part three. The link decision, and why it does not have to be right

The evidence is genuinely contradictory, and any file claiming otherwise is publishing folklore with better production values. LinkedIn's own Senior Director of Product Management said in 2025 that the company does not deliberately limit reach for posts with an external link, with a caveat for posts that exist only to push traffic elsewhere. Independent studies disagree, with each other and with that. One large analysis found company Pages with a link gaining on both impressions and interactions, while personal profiles with a link lost on both. Another found posts with three or more links reaching further. A third, smaller sample found no-link posts winning outright. A fourth, one of the largest disclosed datasets running to April 2026, states plainly that it did not isolate the question at all. The honest position: the sign of the effect differs between personal profiles and company Pages, the size is unsettled by roughly three hundred percentage points depending on whose study is chosen, and several of the loudest cited figures cannot be traced to a real methodology.

A decision rule does not need that resolved. Two mechanical facts hold regardless of which study is closer to right: a post can carry either an image or a link, never both, and a link-carrying post is very often simply a worse post on its own terms, shorter, less self-contained, ending with an instruction to leave rather than a point that has already landed, while every documented ranking signal rewards dwell time and viral actions taken on the post itself, not on what happens after a click.

The rule: write the post to be complete with no link at all, a full story, result or opinion that has already delivered its point. Only then decide whether a link belongs, and if it does, place it after the point has landed, not as the reason the post exists. This costs nothing on either account type: on a personal profile, where the observed direction skews negative, the post never depended on the link; on a company Page, where it skews positive, nothing is lost either. If the material seems to need both a link and an image, that usually means it wants two posts, not one forcing a mechanically impossible pairing.

One placement choice is not neutral: the link in the first comment "to protect reach" is not documented and never had a controlled study behind it, tracing to a single unrepeated anecdote from 2017. What has changed since works against it: LinkedIn's default comment sort, Most Relevant rather than Most Recent, can push the author's own link comment out of view. The tactic meant to hide the link from the algorithm now risks hiding it from the audience instead. If a link belongs in the post, put it in the post.

## Part four. Hook archetypes, and the signature that says one has failed

Each of these works as a first line. Each also fails in a specific, recognisable way, and the failure shows up in the line that follows the hook, not in the hook itself.

**The number hook.** "I sent forty cold emails and heard back from one." Fails when the number never reappears: the body moves on to a different point, the promise is abandoned, and the reader notices even without being able to say why.

**The reversal hook.** "Nobody tells you this about hiring your first engineer." Fails when the claim that follows is something most readers already believe. A reversal only works if the second line complicates the first; if it agrees with the opener, the framing was decoration on an ordinary observation.

**The confession hook.** "I got this wrong for two years." Fails when the confession resolves into a pitch by the third line. A reader who feels led into a product mention reads the vulnerability as bait retroactively, costing more trust than a plainly written launch post would have.

**The direct claim hook.** "This is not true anymore." Fails when no evidence follows inside the next two or three lines, since a sceptical reader gives an unsupported claim about one more line before deciding the post is not worth expanding.

**The scene hook.** "Tuesday, six forty in the morning, the email arrived." Fails when the scene never resolves into a stated point, drifting for ten lines before revealing what it is actually about and losing most readers before the argument starts.

**The open-question hook.** "What if the biggest mistake in onboarding isn't the one you think?" Fails by sitting closest to a category LinkedIn's own VP of Engineering for Feed and Discovery has named as something the platform works to demote: generic prompts built to harvest low-effort replies. A specific stance outperforms a question with no position attached, and does not risk reading as engagement bait.

## Part five. The shape that converts, not the shape that entertains

Two stories can share an identical arc, setup, tension, resolution, and produce different outcomes because only one of them does the extra piece of work that turns a scene into something a reader can use.

An entertaining story ends on a moral: a general statement that could be pasted onto almost any other story with the names changed. "And that's when I learned persistence pays off." True, unfalsifiable, and transferable to nothing in particular.

A converting story ends on a mechanism. It names the specific decision made at the point of highest tension, not just the fact that a decision was made, and the result that decision produced, ideally concrete enough to be wrong if it were invented: a number, a named category of tool, a specific week. The final line only makes sense because of that mechanism, which is the actual test: read the last line alone. If it could sit under a completely different story with no edits, it is a moral. If it only makes sense attached to the specific decision above it, it is a mechanism, and the post has done the second half of its job.

## Part six. Media, sized to the organic spec, not the advertising spec

Most published LinkedIn image and video advice quotes the advertising specification for an organic post, and the two are genuinely different documents with genuinely different numbers.

For an organic image: the upload limit is 5 megabytes, the minimum is 552 by 276 pixels, and LinkedIn's only stated pixel recommendation for an organic post is 1080 pixels wide. The aspect ratio runs from 3:1 up to 4:5, width to height, and 4:5 is a hard ceiling: anything taller gets centred and cropped to fit it, which makes 4:5 the most vertical space a still image can claim in the feed. A multi-image post follows the same 4:5 cap per image and allows up to twenty images, with the first given the most visual weight. Photos cannot be resized or edited once posted. The frequently repeated organic figures of 1200 by 627 and 1080 by 1350 are both suspect: the first is a link-preview and advertising figure, not an organic one, and the second is a third-party inference from the 1080-wide recommendation combined with the 4:5 ceiling, not a number LinkedIn itself has published. The safe instruction is 1080 pixels wide, no taller than 4:5, checked against the composer's own preview before publishing.

For organic video: file size runs 75 kilobytes to 5 gigabytes, duration runs 3 seconds minimum from desktop or 2 seconds from mobile up to a 15-minute maximum, and the documented aspect ratio range is 1:2.4 to 2.4:1, wide enough that a vertical 9:16 video is permitted organically even though LinkedIn never singles it out as a recommendation. LinkedIn explicitly asks that the edges of a video, top, bottom and both sides, stay clear of text and logos, since its own interface elements sit in that space. Auto-captions exist in ten languages, or a caption file can be uploaded directly as an SRT attached before the post goes live.

One further distinction: LinkedIn's full-screen vertical video feed cannot be posted into directly. A video only appears there if LinkedIn's own relevance judgement surfaces it afterwards, and LinkedIn publishes no recommended organic spec for that placement. Any claimed official spec for shooting vertical video "for the video feed" is, at best, a third party's inference.

## Part seven. Hashtags, and the timing question, closed out plainly

LinkedIn's own VP of Product Management answered this directly on 18 December 2025: hashtags do not impact distribution, and the only remaining reasons to add one are personal tracking and the fact that a hashtag stays clickable and searchable. The official LinkedIn News account has said the same. LinkedIn has also quietly removed its own hashtag how-to help pages, and its Senior Director of Product Management confirmed in 2025 that the hashtag feed itself is being phased out. Any advice recommending a specific hashtag count, three to five is the most common figure, has no documented basis and is now directly contradicted at VP level. Do not add hashtags for reach. Add one, if at all, only to make a post searchable or trackable, and treat every hashtag as visual weight taken from the space before the fold.

The golden hour does not survive the same check. No LinkedIn document, engineering post or member-facing explainer describes a sixty-minute window, a staged rollout to a slice of your network, or an engagement threshold that triggers wider distribution, an absence checked across every relevant LinkedIn source going back to 2017. What is documented instead is narrower and more useful: dwell time and viral actions, reactions, comments, shares, are explicit ranking inputs, recency is a documented content signal, and a post that earns no engagement at all is not pushed further regardless of when it was published. The practical instruction that survives is not about timing, it is about the post: build something worth a genuine reaction or comment, the actual documented lever, and no invented deadline replaces it.

## Worked example, compressed

Raw material, from a founder on a personal profile: a subscription tool crossed forty paying customers after fourteen months, almost entirely from cold outbound, and the team nearly shut the product down at month six before finding the channel that worked. A small pilot for a new feature is also opening, with a signup link.

**Format.** Two candidate sentences survive the fold test from part two. "Forty paying customers after fourteen months" is a number, pointing toward the result format. "We almost shut the product down at month six" is a moment with something at stake, pointing toward the story format. Applying the cannot-tell branch: the second sentence is the one that would still stop a stranger if it were the only line visible, since a number alone invites a shrug where a near-failure invites a question. Story format wins, with the forty-customer figure folded in as the result inside the resolution rather than as the opening line.

**Hook, link, media, hashtags.** A confession hook short enough for the tightest fold estimate: "We almost shut this down at month six." Checked against its own failure signature, the confession must resolve into the specific channel decision made at month six, not a pitch, before any mention of the current pilot. The post is written to be complete without the pilot signup at all, so the link is added only as the final line, after the story has landed, consistent with part three's rule for a personal profile, where the observed evidence skews against links carrying their own weight. No image or video exists, so the link-versus-image constraint never becomes a conflict. No hashtags, since nothing about the post's discoverability depends on one.

**Verdict.** Story format. Opening line "We almost shut this down at month six," kept under the tightest cited fold estimate and checked in both composer previews before publishing. Body carries the specific channel decision and the forty-customer result as the resolution. Signup link appended as the final line. No image, no hashtags, no claim about a best time to post, since none is documented.

## Failure modes

**The abandoned number.** A hook promises a specific figure and the body never returns to it, so the post's actual subject turns out to be something else, and the reader who stopped for the number feels misled by the second paragraph.

**The agreeable reversal.** "Nobody tells you this" is followed by a claim most of the audience already holds, so the contrarian framing reads as performance rather than a genuine complication of the obvious view.

**The bait question.** A generic prompt with no personal stance attached sits inside the category LinkedIn's own engineering leadership has named as something the platform works to demote, and invites exactly the low-effort replies that category is built to catch.

**The moral-only ending.** A story closes on a general statement that would fit under any other story with the names changed, the specific, checkable sign that the piece was built to entertain rather than to transfer anything usable.

**The link-led launch.** A post exists mainly to push a click off the platform and offers little else standing on its own, the one pattern LinkedIn's own product leadership has flagged as genuinely likely to underperform, independent of whether an algorithmic penalty exists at all.

**The hashtag stack.** Five or more hashtags sit at the bottom of a post out of habit, adding nothing to distribution per LinkedIn's own December 2025 statement, and reading to an informed audience as evidence the writer has not kept up.

**The buried first-comment link.** A link is placed in a comment on the assumption that it protects reach, an assumption with no controlled study behind it, while the platform's own default comment sort can hide that exact comment from most readers, defeating the tactic's actual purpose even if the imagined benefit were real.

## What this skill does not do

- It cannot see the live composer preview or the account's actual fold point on the day of publishing. The exact truncation count is undocumented and reportedly moves with viewport, font size, app version and LinkedIn's own testing, so the draft still needs a human check on both surfaces before it goes out.
- It cannot resolve whether a link in the body genuinely costs reach. Published evidence spans roughly minus 60 to plus 236 per cent and the sign flips between personal profiles and company Pages. This file states that contradiction and gives a rule that does not depend on resolving it, rather than inventing a number.
- It does not write the comments that follow a post, or manage the reply thread once the post is live. A separate skill in this library covers that job and the documented comment-ranking and rate-limit mechanics behind it.
- It does not rewrite the profile a post links back to, its headline, About section or Featured selection. A separate skill covers that, and a post built on a mismatched profile under-converts no matter how well the post itself is written.
- It does not build multi-page document posts, the format most people call a LinkedIn carousel, or write a direct message follow-up sequence. Both are separate jobs with their own documented file, page and messaging limits, covered by other skills.
- It does not know LinkedIn's unpublished daily posting limit. LinkedIn's own API confirms a maximum exists, by way of a rate-limit error, but has never disclosed the number.
