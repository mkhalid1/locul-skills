---
name: serp-first-seo-article-writer
description: Writes a finished, publish-ready SEO article for one target keyword, starting from a captured results page rather than a template. It classifies the dominant format the page actually rewards, a free tool, a long-form guide, a listicle, a vendor comparison, a forum thread, or a genuine mixed split, and derives the article's depth, structure and even whether to write at all from that. It grounds every on-page decision, titles, headings, freshness, keyword use, in Google's own documentation rather than agency folklore, and closes with a head-to-head check against the three real competitors captured at the start. This skill should be used whenever a keyword has already been chosen and the next step is turning it into a finished article, before an outline exists.
---

# SERP-first SEO article writer

## The claim this skill is built on

Writing to a keyword produces a different result from writing to what that keyword's results page actually rewards, and the gap between the two is where most SEO effort is wasted. A results page is a standing answer to what people typing that query wanted and got, not ten interchangeable slots for a good article to compete over. When every result is a free tool, that is the answer, not a gap: people want to do the thing, not read about it. A 2,000 word article published there is well written and will not rank, because it answers a question nobody asked.

The obvious approach fails in two compounding ways. It picks a format from habit, an article, regardless of what the results page rewards. And once writing starts, it follows on-page rules, a word count floor, a density target, a strict heading hierarchy, that read like SEO wisdom and are, on inspection, either unsourced or contradicted by the search engine's own documentation. Both failures stay invisible from inside the process, because "pick a keyword, write a comprehensive article, optimise it" has no step at which the format or the rules get checked.

This file fixes both, in order. Read the results page and classify what it rewards. Derive format, depth and structure from that. Write inside a ruleset built from what Google's documentation actually says. Then check the finished draft against the three real competitors captured at the start, not a checklist. A brief that stops after classification is a smaller product; this file goes all the way to a draft that can be judged.

## Part one. What to unlearn before drafting anything

Google's SEO Starter Guide carries a section titled, verbatim, "Things we believe you shouldn't focus on" (last updated 2025-12-10 UTC). Treat it as the load-bearing citation for this whole method, and carry the following into every draft as a checklist of claims never stated as fact.

- **Word count.** "The length of the content alone doesn't matter for ranking purposes (there's no magical word count target, minimum or maximum, though you probably want to have at least one word)." Write until the question, and every question the results page raised, is actually answered. That is a stopping rule, not a target.
- **Keyword density.** John Mueller, SEO Office Hours, January 2023: "Google does not have a notion of optimal keyword density... Keyword density does not matter, but being explicit does matter." State the target term once, clearly, in natural language, near the top, then vary the wording afterward.
- **"LSI keywords."** No such mechanism exists. Mueller, 2019 and again 2023: anyone telling you to sprinkle latent semantic indexing terms is repeating a label attached to a real 1980s retrieval technique Google's ranking systems do not consume. Its documented semantic machinery is neural matching, BERT and MUM; write naturally with varied vocabulary instead.
- **Keyword-rich domains, URL paths and TLD choice.** "The keywords in the name of the domain (or URL path) alone have hardly any effect beyond appearing in breadcrumbs," and "Google Search doesn't care which TLD you're using." A documented exact-match domain system damps sites built to game this.
- **Strict heading rules.** "It doesn't matter if you're using them out of order... There's also no magical, ideal amount of headings." Keep heading order sane for the reader and for screen readers, the accessibility reason Google gives, not an SEO one.
- **The duplicate content "penalty."** Google's own scare quotes: "There's no such thing as a 'duplicate content penalty.'" The documented mechanism is clustering and consolidation, not punishment. Copying someone else's content is different: that falls under scraping, a named spam policy with real consequences.
- **E-E-A-T as a ranking factor.** The Starter Guide answers this, in four words, "No, it's not." E-E-A-T is a concept inside Google's content self-assessment guidance and the Search Quality Rater Guidelines, used by human raters who do not set rankings. The behaviours it describes, real expertise, first-hand experience, citations, are worth writing in as content quality, never as a ranking mechanism to chase.
- **Meta description and meta keywords as ranking inputs.** Since 2009, never retracted: "we still don't use the description meta tag in our ranking." The keywords meta tag "has no effect on indexing and ranking at all."

None of this means these elements do not matter. A title still needs to be descriptive, headings still need to make sense. None of them is the lever the folklore claims, and stating one as a ranking mechanism is the easiest way for SEO advice to be visibly wrong.

## Part two. Capture the results page before deciding anything

This skill needs an actual capture, not a memory of what usually ranks. For the target keyword, in the country and language that matter, record:

- The top ten organic results: URL, page title, and approximate body word count.
- The result type of each, judged by what the page actually is, not its title. A page titled "Invoice Templates" that is really a generator is a tool, not an article.
- Every results-page feature and where it sits: an AI-generated answer block, a featured snippet and which page fills it, People Also Ask, image or video packs, and how many ads sit above the first organic result.
- The People Also Ask questions and the related searches, verbatim.
- How and when the capture was made. Results move month to month and shift with location and personalisation; a capture with no date attached cannot be trusted later.

This skill does not fetch this itself. It needs the capture handed to it, from a browser, a rank tracker, or an API, dated.

## Part three. Classify the dominant format, and let it set the format, the depth and the structure

Count the result types in the top ten. Whichever type has a clear majority is what the page rewards, and it sets the format, the depth and the structure together.

| Dominant type | What to write | What decides the depth |
|---|---|---|
| Free tool or generator | A working tool, not an article. Prose will not compete. | Not applicable; length is not the axis. |
| Template or asset gallery | A gallery of real, usable assets with minimal copy. | Only enough to orient the user to the assets. |
| Long-form guide | A guide that beats the incumbents on a stated axis, not merely matches them. | Until every captured question, including every People Also Ask item, is answered. |
| Ranked listicle | A genuinely evaluative list with a stated methodology. | One paragraph of reasoning per entry, not a word count per item. |
| Vendor comparison | A head-to-head page, a different shape from a listicle. | Enough to make the choice between two named options concrete. |
| Forum thread | Candour is the axis; aggregate real, specific opinions, or reconsider whether a page can compete at all. | Depth matters less than credibility. |
| Documentation or reference | Often unwinnable if a vendor owns the query; target a neighbouring, less literal query instead. | Not applicable if declining. |

**The decision rule, with its branches.**

- **A clear majority exists and you can produce it.** Write to it, sized and structured as the table sets out.
- **A clear majority exists and you will not or cannot produce it**, most often a tool. Say so: this is a build request, not a content request, and that is often the most valuable output of the whole process. Second option: check People Also Ask and related searches for a neighbouring query whose results page is informational where the head term is transactional, often the real opportunity at a fraction of the difficulty.
- **The results are mixed, with no majority.** First check whether the split is real or a coincidence. Six of ten one type against four spread across two others is a majority, not a tie; treating it as ambiguous is a way of skipping the classification work. A true split, close to even between two distinct types, usually means the query serves two intents cleanly, so the output is two separate pages in two formats, not one page trying to be both.
- **You genuinely cannot tell**, because the split is close to even and the two dominant types cannot be mapped to two distinguishable intents from the query, People Also Ask, or related searches. State this rather than guessing. Either gather more signal, a second capture, a different location, a check of which result holds the featured snippet, or fold the keyword into the cluster map rather than commit it to one article on a coin flip.
- **Nothing reachable beats the bar.** Even after checking for a neighbouring query and a build option, sometimes every reachable format loses to entrenched incumbents with no closable gap, or an AI-generated answer block fully satisfies the query in a way that suppresses any click. The correct output is a documented decision not to publish, with the reason recorded, so the keyword does not quietly resurface next quarter as if it had never been checked.

## Part four. Write inside the ruleset

**Title and heading.** Draft a working title carrying the target term near the front. Neither Google nor Bing documents a character limit for a title; truncation is by device width, not a fixed count. This skill drafts the working version only; the click-optimised, width-checked final tag is a separate, narrower task.

**Body structure.** Order headings for a reader working through the topic, and answer every captured People Also Ask question somewhere with a clear heading or sub-section. There is no ideal heading count and no required strict hierarchy for ranking; keep the order sane because a reader, and a screen reader, needs it, not because a rule requires it.

**Keyword use.** State the target term explicitly, once, clearly, in the opening section, then write naturally with varied, related language throughout. Do not build a synonym list to "cover LSI keywords." Do not count occurrences against a density target.

**Freshness and dates.** Only change the publish date when the content has genuinely, substantially changed. Google names the alternative directly, among the signs of search-engine-first content: "Are you changing the date of pages to make them seem fresh when the content has not substantially changed?" If the content is untouched, leave the date untouched.

**Internal and external links.** Place internal links where they genuinely help the reader move to the next relevant page, roughly two to five per article, with descriptive, specific anchor text. Google's quality bar is explicit: "descriptive, reasonably concise, and relevant," and its named bad examples are "Click here," "Read more," and a bare "website" or "article." Which destination page each link points to, and how the site's anchor text portfolio balances across many articles, is a separate job this file defers entirely. Add one to three external links to sources that back a specific claim; that is where genuine credibility signals live, not in an author bio written to satisfy an E-E-A-T score.

**The scale check, for a batch.** Before publishing an article written as part of a batch, apply Google's own test: is this page's primary purpose to serve a reader who would want it on its own terms, or to occupy a ranking position. The scaled content abuse policy applies "whether automation or humans are involved" and targets pages "generated... for the primary purpose of manipulating search rankings and not helping users," regardless of production method. Self-test: could this page exist, substantially unchanged, if the target keyword had no search volume at all. If not, it fails the check regardless of how well it reads.

**Where third-party publishing crosses into a different policy.** Freelance content, sponsored posts and native advertising to a publication's own readers are explicitly not violations. Content published mainly to borrow a host site's own established ranking signals falls under the site reputation policy, renamed from "site reputation abuse." As of 28 August 2026, effective 30 August 2026, enforcement diverges inside the EEA, where an affected section is separated to rank independently over time, from the rest of the world, where a manual action still applies directly.

## Part five. The head-to-head gate, before anything publishes

Judge the draft against the exact three competitors captured in part two, not a generic checklist.

- **Table stakes.** The union of what all three cover. Missing any of it is disqualifying, not a stylistic choice.
- **A real, nameable gap closed.** First-hand specificity the incumbents lack, an original worked example carried end to end where they stay abstract, genuine breadth on a dimension at least two skip, or structure where they are a wall of prose. "More words" is not on this list; length is a consequence of covering more, never a target.
- **Every People Also Ask question answered**, clearly.
- **The part one checklist clean.** No stated word count target, no keyword density claim, no fixed title character limit, no duplicate content penalty claim, no strict heading rule stated as a requirement, no E-E-A-T-as-ranking-factor claim.

Three verdicts follow. **Publish**, if the draft clearly beats all three and the checklist is clean. **Revise**, with the named gaps still open, redrafted and re-checked against the same three competitors, not a fresh set. **Decline**, if the target or format was wrong; send it back to part three rather than forcing a publish. Log the verdict and the three URLs it was checked against, so the decision is auditable later.

## Worked example, compressed

Target query: "how to write a return policy for an online store." Capture: ten organic results, six long-form guides (900 to 2,200 words), two free policy generators, one forum thread, one platform help page. Classification: a clear majority, six of ten, is the long-form guide, so this is a write decision, though the two generators are worth flagging as a future build opportunity rather than something that derails this call.

Top three: a legal-advice blog at position one, 1,800 words, generic clauses only, dated 2021, no restocking-fee or holiday-window treatment. A platform's own help page at position two, 650 words, narrow and official. A marketing agency guide at position three, 2,400 words, covers clauses and examples but no usable template. People Also Ask: whether a return policy is legally required, how long one should be, and whether "no refunds" is allowed.

Table stakes: timeframe, condition of returned goods, refund method, exclusions. Gaps in all three: no fill-in-the-blank template embedded directly, no worked example of an actual return handled end to end, and the top result is stale against current practice on restocking fees and holiday windows.

Draft closes both gaps: an embedded, directly usable template, and one returned item walked through under three policy configurations. Length lands near 2,100 words, close to the third result, because that is what fully answering the captured questions took, not a target. The target term appears once, explicitly, near the top. Headings answer all three questions directly. The date is current because the content is genuinely new, not bumped.

Gate: table stakes present, two real gaps closed, all three questions answered, checklist clean, no duplicate-content urgency, no claimed title character limit, no E-E-A-T framing beyond real, checkable credentials. **Verdict: publish.**

## Failure modes

**Format matching the query's category rather than its results page.** A commercial-sounding keyword gets a landing page even though its results page is dominated by long guides, or the reverse. The page reads well and sits on page two indefinitely, because it was never competing in the right lane.

**Treating the folklore checklist as confidence instead of doubt.** Hitting a self-imposed 2,000 word count feels like thoroughness. When the actual top three average 900 words, the extra 1,100 words dilute the one explicit mention Google's own guidance actually asks for.

**Using the top three as an outline.** Covering exactly what they cover produces the fourth-best version of an existing page. The gate in part five should catch this but often does not, because the person checking is the person who wrote it.

**Bumping the publish date without touching the substance.** Named directly by Google as a sign of search-engine-first content, and an easy trap when refreshing an old page.

**Batch production with no per-page purpose test.** Ten articles from a keyword list, written in one sitting, each individually readable, none able to pass the question of whether it would exist without the keyword's volume. Read together, that is close to a textbook description of scaled content abuse, regardless of how the pages were produced.

**Declaring a false tie.** Six of ten one type against four split across two others gets waved through as "mixed, cannot tell," when it is a clear majority the writer did not want to act on because it was inconvenient to produce.

**Mistaking a documented quality behaviour for a ranking mechanism.** A detailed author bio, citations, transparent sourcing, these are good practice worth doing. Selling them internally as "improving our E-E-A-T score" repeats the exact claim Google's own guide names and rejects.

## What this skill does not do

- It does not fetch or capture the results page itself. It needs a real, dated capture from a browser or a rank-tracking tool; without one, the classification step is judging a page that may no longer exist.
- It does not choose the keyword or build the topic cluster the article sits inside, including the demand and cannibalisation checks that decide whether the keyword was worth targeting.
- It does not finalise the character-constrained title tag or meta description, only a working title to draft against.
- It does not decide which internal pages a new article should link to, or manage a site's anchor text portfolio across many articles. It only flags where a link belongs and what makes anchor text descriptive.
- It cannot verify the factual claims, prices, statistics or examples inside the draft against the live world. It checks claims about what Google says against Google's documentation, not claims about the subject matter.
- It does not diagnose or refresh a page that already exists and is losing ground. That needs real performance data this file never sees, a different job on a different input.
