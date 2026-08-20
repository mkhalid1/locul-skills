---
name: linkedin-post-audit
description: Audits a LinkedIn draft against the platform's real constraints before it is published. Covers where the feed truncates on mobile and desktop and why that makes the first two lines the whole advertisement, the character ceilings for posts, articles and comments, the accessibility defect created by producing bold text from Unicode mathematical alphanumeric symbols, link placement and what moving a link to the first comment actually costs, document and carousel page ratios and limits, image and video crop and caption requirements, what hashtags do now, why early engagement from your existing network means audience fit matters more than post quality, a catalogue of opening patterns, and the specific tells that make a draft read as machine generated. This skill should be used immediately before publishing a post, when exporting a carousel, or when a model-written draft is about to go out under a person's name.
---

# LinkedIn post audit

## The claim this skill is built on

Most advice about posting here is about the writing. The binding constraints are not about the writing.

A post is collapsed behind a see-more control after a short span, and everything past that point is read only by people who have already decided to keep reading. So the first two or three lines are not the introduction, they are the whole advertisement, and a draft that spends them on context has already lost the readers it was written for. Then there is a second constraint that almost nobody names: the styled text people use to make those lines stand out is not styled text at all, it is a different set of characters, and it is unreadable to anyone using a screen reader.

Those two facts decide more outcomes than any amount of editing does, and neither is a matter of taste. So this audit runs the hard constraints first, the distribution mechanics second, and the writing last.

**Every figure here is approximate and dated to August 2026.** The platform changes limits without announcing them. Verify in the composer before relying on any number, and treat a published help page as more authoritative than this file.

## Tier 0. The hard constraints

### Truncation, which is the single most consequential fact

The feed shows a preview and hides the rest behind a see-more control. The visible span is short and it differs by surface:

- **Mobile**, roughly 140 characters, or about three rendered lines.
- **Desktop**, roughly 200 to 220 characters, or about two to three rendered lines.

Both figures are approximate, both have moved before, and both should be checked by posting something and looking at it on a phone. What does not change is the consequence: **write the first 140 characters as if they are the entire post**, because for most of the audience they are.

Two mechanical details follow. Line breaks count towards the collapse, because the cut is by rendered lines as well as by characters, so three short one-word lines can consume the whole preview while saying almost nothing. And the preview is what the reader judges, which means a post whose payoff sits in paragraph four is a post whose payoff does not exist.

### Character ceilings

- **A feed post**: approximately 3,000 characters.
- **An article** through the publishing tool: on the order of 110,000 characters, with a headline capped at around 100.
- **A comment**: approximately 1,250 characters.

**What happens at the boundary is the part that catches people.** The composer stops accepting input at the ceiling, and pasted text is truncated silently rather than rejected. A post drafted in another editor and pasted in can lose its final paragraph, including the call to action, with no error and no visible marker. Always check the last line after pasting.

The ceiling is not a target. A post at 2,900 characters is a post very few people finish. If the material genuinely needs the length, the article format exists and is a different distribution mechanism with different expectations.

### Formatting, and the accessibility defect nobody mentions

**The composer supports no markdown.** There is no bold, no italic, no heading. The bold and italic text you see in the feed is produced by substituting characters from the Unicode mathematical alphanumeric symbols block, which occupies the range U+1D400 to U+1D7FF and exists so that mathematical notation can distinguish a bold vector from an italic scalar. Those characters are not the letter A with a style applied. They are separate code points that happen to look like letters.

The costs are real and they are borne by other people:

- **A screen reader announces them character by character, or by their Unicode names.** A styled headline is read out as a stream of descriptions such as "mathematical sans-serif bold capital T" repeated for every letter, or skipped entirely. What was meant as emphasis becomes noise, and in the worst case the most important line in the post is the only line a blind reader cannot access.
- **The text is not searchable.** Neither the platform's search nor a browser find-in-page matches a styled word against its ordinary spelling, so the term you most wanted to be found for is the one term that cannot be found.
- **Rendering is inconsistent.** Not every device font covers the whole block, so some readers see empty boxes where the headline should be.
- **It does not copy usefully.** Anyone quoting you gets characters that break in most editors.

**Treat it as a defect, not a tactic.** The replacements that cost nothing: short paragraphs, a blank line before the line you want noticed, a genuine list with hyphens, and putting the important words first in the sentence. Emoji used as bullet markers have a related problem, since a screen reader announces the emoji name before every item, so a list of eight items bulleted with the same symbol is that name read out eight times.

If the composer has gained native formatting since this was written, native formatting is always the right answer and the substitution is never needed. Check before assuming either way.

## Tier 1. Distribution mechanics

### Links, stated honestly

It is widely reported that a post containing an external link in the body reaches fewer people than one without, and that moving the link to the first comment recovers some of that. **This is an observed and much repeated effect whose size is disputed.** The measurements come from third parties sampling posts they chose, the platform has at times denied applying a blanket penalty, and no independent audit of the ranking system exists or can exist. Treat it as a real pattern with an unknown coefficient, not as a law.

What is not disputed is the cost of the workaround. A link in a comment loses the preview card, which is a large tappable target and usually the biggest single driver of clicks. It adds a step, since the reader has to open the comments. And it can be pushed down the comment order once other comments arrive, unless it is pinned.

**The decision rule:**

- **The click is the goal**, for example a signup, a registration or a purchase, and the audience is modest. **Keep the link in the post.** Accept the reach cost and keep the preview card, because a smaller audience that can act beats a larger one that cannot find the link.
- **Reach is the goal**, for example awareness or follower growth, and the link is supporting material rather than the point. **Put it in the first comment and pin that comment**, and say in the post that it is there.
- **You cannot tell**, because nobody has stated the goal or you have no history to compare. **Do not guess from a general rule.** Run both placements across several posts on your own account and read your own numbers, because the only measurement that describes your audience is the one taken on your audience.

### The first hour, and why audience fit dominates

Distribution is shaped by how the first cohort responds. That cohort is drawn largely from people who already have a connection to you, and the signals that matter most are the expensive ones: a comment weighs more than a reaction, and time spent reading weighs more than a click on a control.

**The consequence is uncomfortable and worth stating plainly. A genuinely good post shown to the wrong network will do worse than a mediocre post shown to the right one.** If your connections are recruiters and your post is for platform engineers, the first cohort does not engage, the post does not travel, and no amount of editing changes that. So when a post underperforms, the first question is not what was wrong with the writing. It is who saw it.

Which means the bigger win, almost always, is fixing the network rather than the draft: connecting with people in the market you are actually addressing, commenting where they already are, and accepting that this takes months. Practical implications that follow from the mechanism rather than from folklore: be available to reply for the first hour, because replies are engagement and they pull the post back into circulation; do not publish twice in a short window, because the second post competes with the first; and post when the people you want are awake, which is a fact about your audience and not about a chart of universal best times.

Engagement pods, where a group agrees to comment on each other's posts, corrupt exactly this mechanism. They manufacture a first cohort that looks nothing like your buyers, so the post travels to an audience that will never act, and coordinated engagement is a policy problem as well as a strategy one.

### Hashtags

Hashtags no longer do most of what they are still credited with. Following a hashtag as a discovery mechanism has been substantially de-emphasised, hashtag pages are far less prominent than they were, and as of 2026 they are close to inert as a distribution device. They remain clickable, they still categorise loosely, and a wall of ten or more reads as spam to a human reader.

The honest position: zero to three, at the end, only where a genuine community actually uses that exact tag. Do not build anything on them, and do not let them occupy characters in the preview span.

## Tier 2. Media constraints

### Documents and carousels

A carousel is a document upload, in practice a PDF, with slide decks converted on upload. The constraints worth designing around:

- Page count in the low hundreds, commonly documented as up to 300 pages, and a file size ceiling around 100 MB. Verify both, and note that no reader has ever wanted 300 pages.
- **The feed viewer is roughly square, so a portrait 4:5 or square 1:1 page reads best.** Designing at 1080 by 1350 pixels or 1080 by 1080 is the safe choice. A 16:9 landscape deck is the most common defect: it wastes the vertical space the feed gives you and shrinks the text to the point where a phone reader cannot read it without pinching, which they will not do.
- **Roughly 8 to 15 words a page.** A carousel is read at thumbnail size while scrolling. Anything denser is a document that happens to be in the feed.
- The first page is the hook and is the only page most people will see. It should work standing alone.

### Images and video

- **Aspect ratios that survive the crop.** Portrait 4:5, around 1080 by 1350, occupies the most vertical space on a phone without being cut. Landscape at 1.91:1, around 1200 by 627, is the safe wide format. Square works everywhere. Anything taller than 4:5 is cropped in the feed with a tap to expand, which most readers do not do, so nothing load-bearing goes near the top or bottom edge.
- **Multi-image posts crop into a grid**, so text near any edge is lost. Check the grid, not the individual images.
- **Video autoplays muted.** A video without captions is therefore watched without sound by most of the people who watch it at all, which for a talking-head clip means watched without comprehension. Burn captions in or upload a subtitle file, and make the first two seconds legible without audio. Native upload is treated better than a link to a video hosted elsewhere. Feed video limits are commonly documented at up to about 15 minutes and several gigabytes, with a short minimum, and should be verified.
- **Alt text is supported on images**, with a limit of a few hundred characters. Use it. It is the one accessibility control the platform actually gives you, and it takes ten seconds.

## Tier 3. The writing

### Hooks, and the one discipline that matters

Opening patterns that reliably earn the second line:

1. **The specific number.** "We cut onboarding from eleven days to three."
2. **The dated moment.** "On a Tuesday in March a customer asked me a question I could not answer."
3. **The flat counterintuitive claim**, stated without decoration, with the evidence promised.
4. **The named mistake.** "I priced this wrong for two years."
5. **The quoted line** from a real conversation, anonymised.
6. **The before and after state**, both concrete.
7. **The genuinely open question**, only where you do not already know the answer.
8. **The counted list promise.** "Four things that broke when the team doubled."
9. **The contradiction of standard advice**, followed immediately by the exception that makes it true.
10. **The artefact.** "Here is the actual message that got the meeting."

**The discipline underneath all ten: the first line must be a fact or a specific moment, not a simile and not a metaphor.** Readers have learned that a crafted metaphor in the opening position means an advertisement is coming, and the scroll happens before the point arrives. Concreteness buys the second line: a number, a date, the name of a thing, a sentence somebody actually said. A comparison buys nothing, because it is the sound of someone who has already decided what they want you to feel.

Also avoid the throat-clear, which is any opening that announces the topic rather than entering it, and the label opening, which is the title of the post used as its first line.

### The tells that make a post read as generated

Each of these can appear innocently. Two or more together read as machine written, and once a reader has that suspicion they stop reading rather than arguing:

- **Symmetric one-line paragraphs**, each roughly the same length, with a blank line between every one, producing a rhythm no human writes by accident.
- **A rhetorical question as the opener**, especially one beginning "ever wondered" or "what if I told you".
- **The construction that says a thing is not one thing but another thing**, particularly repeated across consecutive lines, which sounds profound and asserts nothing.
- **The closing line asking what everyone thinks**, or "agree?", which is an engagement request wearing a question's clothes.
- **Triads of abstract nouns**, delivered as if the count were the argument.
- **Round unsourced numbers**, the tenfold improvements and the ninety percents that no one measured.
- **A final line that restates the opening as a slogan.**

The repairs are the same in every case: replace the abstraction with the incident it came from, delete the closing question or replace it with something you genuinely want answered, and break the paragraph rhythm so the shape carries meaning instead of pattern.

## The audit order, and the verdict

Run it in this order and report the highest failing tier first.

1. Does the first line survive truncation and is it a fact or a moment?
2. Is there Unicode substituted formatting, and is the post inside the ceiling?
3. Is the link placement consistent with a stated goal?
4. Do the media meet the ratio, caption and alt text requirements?
5. Do two or more generated-text tells appear together?

End with **publish, revise, or do not publish.** Do not publish is a real verdict, and the case for it is usually that the post is aimed at a network that is not the audience, which is a problem no revision fixes.

## Worked example, compressed

A draft, presented for review before publishing.

> 𝗧𝗵𝗲 𝗵𝗶𝗱𝗱𝗲𝗻 𝗰𝗼𝘀𝘁 𝗼𝗳 𝗯𝗮𝗱 𝗼𝗻𝗯𝗼𝗮𝗿𝗱𝗶𝗻𝗴
>
> Onboarding is like a first date. If it goes badly, there is no second one.
>
> Ever wondered why so many teams get this wrong?
>
> It is not about the software. It is about the experience.
>
> [eleven further one-line paragraphs, then a link, then nine hashtags]

**Tier 0, blocking.** The first line is set in Unicode mathematical sans-serif bold characters, so a screen reader announces it as a stream of character names and search will never match the phrase. That line also consumes most of the mobile preview, meaning the visible post is a styled label and a simile. The simile is the second defect: "like a first date" is a crafted comparison in the opening position, which is the signal a reader uses to decide an advertisement is coming.

**Tier 1.** The link sits in the body with no stated goal. Nobody has said whether this post is for clicks or for reach, so the placement is unexamined rather than chosen. Nine hashtags occupy space and do close to nothing.

**Tier 3.** Three tells appear together: the rhetorical question opener, the not-one-thing-but-another construction, and thirteen symmetric one-line paragraphs. There is no number, no date and no named incident anywhere in the draft.

**The fix.** Delete the styled headline entirely and open on the concrete case: "A customer took nine days to send their first invoice. The product does it in four minutes." That is a fact, it survives truncation, it is searchable, and it is readable by everyone. Cut to three hashtags or none. State the goal, and if it is reach, move the link to a pinned first comment and say so in the post.

**Verdict: revise.** Nothing here is unfixable and none of it is fixable by editing the middle, which is where the author spent their time.

## What this skill cannot know

- **Your reach.** Impressions, unique views and dwell time live in your analytics and nowhere else, and no reading of a draft predicts them.
- **The ranking system.** It is not published, it changes without notice, and no independent audit exists. Everything in the distribution section is a mechanism inferred from public behaviour, not a rule.
- **Your audience.** Who follows you, what they do, and whether they are the people you want is the variable that dominates every outcome here, and it is invisible from the draft.
- **Whether the numbers above are still true.** They are dated to August 2026 for exactly this reason.

## Failure modes

**Editing the middle.** The middle is read by people who already decided to keep reading. Time spent there is time not spent on the only lines most people see.

**Treating Unicode bold as formatting.** It looks like a small stylistic choice and it is an accessibility failure with a search penalty attached, applied to the most important line in the post.

**Counting characters against the ceiling instead of the truncation point.** The ceiling is 3,000. The number that decides whether anyone reads is closer to 140.

**Pasting from another editor and not checking the end.** The composer truncates silently at the ceiling, so the closing line disappears without an error.

**Exporting a carousel at 16:9.** It is the default in most presentation tools and it produces unreadable text in a feed viewer that is nearly square.

**Publishing video without captions.** It autoplays muted, so an uncaptioned talking head is watched as a silent film by most of its audience.

**Blaming the post for a network problem.** Weeks of rewriting hooks for an audience that is not your market, when the fix was to change who sees the posts.

**Using a metaphor as the first line.** It reads as the opening of an advertisement, and the reader is gone before the substance arrives.

**Chasing engagement with a closing "what do you think?"** It is the most recognisable machine-written tell in the catalogue and it produces comments from people who did not read the post.

## What this skill does not do

- It does not write the post. It audits a draft and returns defects, and if you need a draft this is the wrong shape of tool.
- It cannot see or predict reach, impressions or follower composition, which are the numbers that actually determine whether a post worked.
- It has no access to the ranking system and does not claim any. Where an effect is widely reported but disputed, it says so rather than converting it into a rule.
- It does not judge whether your claim is true, whether the story is yours to tell, or whether the post fits what you want to be known for.
- It cannot fix an audience mismatch, which is the most common underlying cause of a post that did nothing and the one thing no audit reaches.
- Its numbers age. Every limit and every truncation point should be verified against the platform's current published documentation before you rely on it.
