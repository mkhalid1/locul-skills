---
name: serp-title-meta-writer
description: Writes the title element and, where one is warranted, the meta description for a web page, working from Google's documented rewrite triggers and width-based truncation rather than from a character count. It diagnoses why an existing title is being replaced in the results, decides whether a written description will be used at all or whether Google will generate its own from the page, and produces a candidate checked against a real results page rather than a counter. This skill should be used when a page's title or meta description is being written or rewritten, when a title shown in search does not match the title element in the source, or when a bulk description rewrite is being planned across a templated set.
---

# SERP title and meta description writer

## The claim this skill is built on

Neither Google nor Bing documents a character limit for a title or a meta description. Not a soft one, not a recommendation, none.

Google's title-link documentation states it directly: "While there's no limit on how long a `<title>` element can be, the title link is truncated in Google Search results as needed, typically to fit the device width." Its snippet documentation, last updated 20 April 2026, says the same of meta descriptions: "There's no limit on how long a meta description can be, but the snippet is truncated in Google Search results as needed, typically to fit the device width." Bing's current guidelines say nothing numeric either, only that "missing, duplicate, or overly short title tags and meta descriptions may reduce indexing reliability, ranking, and eligibility for grounding results and citations". The widely quoted Bing figures of 5 to 65 characters for titles and 25 to 150 for descriptions come from a Bing Webmaster Blog post of 17 July 2009 which no longer exists at a live URL.

So the 50 to 60 and 150 to 160 that appear in essentially every SEO checklist, tool and template have no primary source at either engine. They are back-calculations from third-party pixel measurement at an assumed average character width, which is a reasonable thing for a third party to have done and an unreasonable thing to publish as a limit.

The obvious approach fails twice over. It optimises against a constant nobody published, in a unit that cannot be right, since a width-based cutoff varies with which characters you used. And it optimises the wrong question: a title of exactly 57 characters can still be discarded by Google for reasons unrelated to its length, and a description of exactly 155 characters can be ignored entirely because Google generated its own snippet from the page body. Length is the last thing that decides whether your text reaches a reader, and the order below reflects that.

## Part one. The two survival questions, in order

A title you wrote has to pass two gates before anybody reads it, and they are not equally important.

**Selection.** Does Google use your title element at all, or write its own? A Search Central blog post of 17 September 2021 said that after changes to its system, "title elements are now used around 87% of the time, rather than around 80% before". That is the only first-party number on this, it is from September 2021, and Google has never refreshed it, so treat it as a 2021 figure rather than a current one. The same post notes Google has used text beyond the title element since 2012, so none of this is recent.

**Truncation.** If your title is used, does it fit the available width on the device the reader is on?

Selection is the larger lever and the one almost nobody works on, because the character-count habit points at truncation instead. It is also the more tractable of the two: Google documents the conditions that cause a rewrite, while publishing no figure at all for the width at which truncation happens.

Run selection first. Six of the seven documented triggers are template or content-management problems rather than writing problems, so fixing a template fixes every page of that type at once. Rewriting one title by hand fixes one page and leaves the trigger in place, ready to regenerate on the next page published.

## Part two. The documented rewrite triggers, and what each looks like from outside

Google names these conditions in its title-link documentation and its September 2021 post. For each, the observable symptom and the fix.

1. **Half-empty titles.** A template slot came out blank, so the rendered title is something like "| Company Name" with nothing before the separator. Symptom: a run of results in the same page type showing a bare brand with a leading separator. Usually found on the page types nobody audits, deep pagination and filtered listings. Fix in the template, with a fallback value, not on the page.

2. **Obsolete titles.** The page's visible headline has been updated and the title element has not, so the title says 2025 while the page says 2026. Symptom: Google shows the current year and your source says the old one. Fix by making the year a single variable used in both places, or by removing it from both.

3. **Inaccurate titles.** A static title on content that changes, so it stops describing the page. Symptom: a listing or feed page whose title names something no longer on it.

4. **Micro-boilerplate titles.** The same title across a subset of pages. Symptom: sibling pages whose titles differ only in a trailing number, or a documentation section where every page reads "Word | Docs | Brand". Fix by giving each page a qualifier that is actually about that page.

5. **Boilerplate titles across the whole site.** The site-wide version of the same fault.

6. **No title element at all.** Symptom: Google shows text drawn from the page body and the source has no title tag, often on a page type generated outside the main template.

7. **A title in a different language or writing system from the page's main content.** Symptom: a localised site where the template header was translated and the title was not, or the reverse. Google's guidance is explicit that the title should match the language and writing system of the page's primary content.

Two facts change what you do about all seven. First, when Google rewrites, its documentation says it "may try to generate an improved title link from anchors, on-page text, or other sources". So the replacement is not arbitrary: it is coming from your H1, your body copy, or the anchor text other pages use to link to you. If you cannot get your title used, you can still influence what is shown, by fixing those. Second, there is no documented way to force Google to use your title. The `data-nosnippet` attribute and the `max-snippet` robots directive affect snippets, not title link selection. Any third-party technique for stopping rewrites is inference from observed patterns, not a documented mechanism.

Google's positive guidance for a good title link, from the same page: give every page a title, write descriptive and concise text, avoid keyword stuffing, avoid repetitive boilerplate, brand titles concisely using a separator such as a hyphen, colon or pipe, make it clear which text is the main title, and match the language of the page's primary content.

## Part three. Width, because a count is the wrong unit

Truncation is by width. That makes character count the wrong unit before you even start arguing about the number, because characters are not the same width. A title of capitals and wide letters occupies far more space than the same count of narrow lowercase characters. Two titles at 58 characters can land on opposite sides of the cut.

**The reference-string method, which needs no invented constant.** Search a query you care about in a private window at the viewport you care about, desktop and mobile separately, and find a result whose title is truncated with an ellipsis. That string, ellipsis removed, is a measured width budget in the exact font, size and layout your readers see, on the day you looked. Keep it in the working document, then paste candidate titles beneath it, one per line, same font family, size and weight, left-aligned and unwrapped, in any editor on Windows or Mac. Anything visibly longer than the reference line is at risk; anything shorter is safe. You have compared like with like without a number either engine refuses to publish.

**Third-party figures, labelled as third party.** Public measurement work puts desktop title link truncation at roughly 580 to 600 pixels and desktop meta description snippets at roughly 920 pixels. Published values disagree, with 515, 561, 580 and 600 all in circulation for titles and 920, 923 and 990 for descriptions, and they move whenever the results layout changes. These are useful for sizing a crawl report. They are not Google's numbers and should never be quoted as such.

**Treat any figure as a snapshot.** Mobile and desktop truncate differently, and Google has changed snippet lengths without notice more than once. The episode between December 2017 and May 2018, when much longer descriptions appeared and were then reverted, is the standard cautionary case.

**Front-load, which makes truncation cheap.** Put the words that distinguish this page from every other page first. If they survive any plausible cut, the exact cut point stops mattering, which is the only durable answer to a variable you cannot measure.

**Verify in the product, not in a counter.** A character counter measures characters. The results page measures the thing. After a change has had time to be recrawled, search the page's own query in a private window on both a narrow and a wide viewport and read what is actually shown. Google documents that it has to recrawl and reprocess the page to notice updates, "which may take a few days to a few weeks", so schedule the recheck rather than declaring failure after two days.

## Part four. The decision rule for whether to write a meta description at all

Start from two documented facts. Google states it does not use the description meta tag in ranking: "Even though we sometimes use the description meta tag for the snippets we show, we still don't use the description meta tag in our ranking." That was posted in September 2009 and has never been retracted. And Google's snippet documentation states that "snippets are primarily created from the page content itself".

Taken together: the description is a click-through instrument with no ranking role, and one of several inputs to a snippet Google may generate for itself, per query. A great deal of advice treats it as a ranking input and writes it as a keyword container, which optimises for a mechanism the search engine says does not exist.

So the question is not how long. It is whether it will be used at all.

- **Branch A. One dominant entry query, one clear proposition.** A pricing page, a product page, a comparison page, a home page. Write one. It has a single job to do for a single kind of visitor, and a written description will usually be the better sales line than anything generated from the body. Write it to complete the title rather than repeat it.

- **Branch B. Many distinct entry queries.** A long reference article, a glossary, a documentation page that answers six different questions. One fixed sentence cannot serve all of them, and Google is very likely to generate a per-query snippet from the body instead. Do not spend the writing effort here. Spend it on the opening lines under each heading and on the headings themselves, since that is the material the generated snippet is drawn from.

- **Branch C. A large templated set.** Thousands of catalogue, listing or location pages. Google's guidance explicitly accepts programmatic generation for large sites. Generate from real per-page fields, and never from a fixed sentence with the page name slotted in, because that produces the boilerplate pattern that is on the rewrite trigger list for titles and reads as filler in a snippet.

- **Branch D. You cannot tell.** A newly published page with no impressions yet, or an existing page whose query spread nobody has looked at. Write one, because the cost is a single sentence and the alternative is guessing, but treat it as a hypothesis rather than a finished asset. Then resolve it with evidence: once the page has accrued impressions, take its top three queries from Search Console, run each in a private window, and read the snippets. If your text appears on most of them, keep it and refine it. If Google generated its own on most of them, stop maintaining it and move that effort into the body copy. If you have no query data at all and no access to get any, stay on branch A by default and revisit when data exists.

When you do write one, Google's own guidance applies: unique per page, specific relevant information rather than a string of keywords, genuinely descriptive of the page. Put the differentiating detail first, because the tail is what gets cut.

## Worked example, compressed

A documentation site for an invented webhook delivery service. The page in question is the retry policy reference.

Current title element: `Retries | Docs | <brand>`. H1 on the page: "Configuring retry policy and exponential backoff". Every other page in the docs section follows the same `Word | Docs | Brand` pattern.

**Selection first.** Two triggers fire from the documented list. Micro-boilerplate, because the title is one word plus a repeated two-part suffix shared by every sibling page. And thin to the point of inaccuracy, since "Retries" does not describe what the page contains. Prediction: Google replaces it, drawing from the H1 or from the anchor text of the pages that link here.

**Check the live results.** Search the page's own top query in a private window. The title link shown is "Configuring retry policy and exponential backoff", which is the H1 verbatim and not the title element. Prediction confirmed, and it also tells us the H1 is doing a better job than the title, which is the useful part.

**Fix.** New title element: `Retry policy and exponential backoff: webhook delivery reference`. Descriptive, unique across the docs set, and the brand suffix is dropped because the brand is not the query and was consuming width on every page. Width check against a reference string captured from the same results page: shorter than the reference on desktop, slightly longer on mobile, but the first five words carry the whole meaning, so a mobile cut costs nothing.

**Template fix, which is the larger one.** The `| Docs |` middle segment is boilerplate across the whole section and is the trigger that will regenerate this problem on every future page. Replace it with a per-page qualifier drawn from the H1.

**Meta description.** Branch B. Search Console shows the page taking impressions on at least six distinct retry-related queries, so a single written sentence cannot serve them and Google is generating per-query snippets already. No bespoke description is written. The existing one stays in place, harmless, and the effort goes into the two sentences under the page's first heading instead.

**Verdict.** One title element rewritten, one template segment changed across the entire documentation set, zero meta description copy produced, and a recheck booked for two to four weeks out, because Google documents recrawl and reprocess as taking a few days to a few weeks.

## Failure modes

**The counter-driven title.** Every title lands between 57 and 59 characters, several still truncate in the live results, and others waste width. Looks like: a spreadsheet column of green ticks that the results page does not match.

**The invisible rewrite.** New titles ship, nobody looks at a results page, and Google has been showing the H1 the whole time. Looks like: a title change followed by no movement in impressions or clicks and no explanation, repeated at the next quarterly review.

**The blank template slot.** Looks like: a run of results reading as a separator followed by the company name, with nothing before it. Almost always on a page type nobody audits, deep pagination, a filtered listing, a tag archive.

**The description written as a ranking asset.** Looks like: a description that reads as a keyword list, restates the title nearly word for word, and gets defended in review with "the keyword has to be in the description". Google's stated position is that the description is not used in ranking.

**The brand suffix eating the width.** A long brand name plus separator consumes a fifth of every title. Looks like: a set of near-identical product titles where the words that distinguish them fall past the cut, so every result in the set reads the same to a scanner.

**The stale year.** Looks like: a title saying 2025 in the middle of 2026 while the page's own headline was updated months ago. This is on the documented trigger list, so it invites a rewrite as well as reading badly.

**Maintaining descriptions Google never uses.** Looks like: a quarterly ritual of rewriting descriptions across a library of long reference articles whose snippets have been generated from the body all along.

**Testing in a logged-in browser.** Looks like: a truncation conclusion drawn from a personalised, localised layout at whatever window size the laptop happened to be, then applied site-wide.

**Declaring the fix failed on day two.** Looks like: a title reverted after 48 hours because "it did not work", when Google documents recrawl and reprocess as a few days to a few weeks.

## What this skill does not do

- It cannot make Google use your title. Google documents no mechanism for forcing title link selection, and the snippet directives, `data-nosnippet` and `max-snippet`, do not apply to it.
- It does not measure pixels or characters at scale. A crawler does that far better across thousands of URLs, and its pixel figures are its own measurement rather than anything either engine published.
- It does not decide which query a page targets, or whether the page should exist. That is upstream work this file assumes is done.
- It does not write the page body, which is where a generated snippet actually comes from, and which branch B explicitly hands off to.
- It cannot open your Search Console account, so the cannot-tell branch has to be resolved by a person with access to the query data.
- It covers nothing about social share previews, which are governed by separate Open Graph and Twitter card tags, or about how pages are named inside AI answer surfaces, which neither engine documents in the same terms.
