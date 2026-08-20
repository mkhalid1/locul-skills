---
name: pre-publish-page-audit
description: Audits a page before it goes live, in order of how fatal each defect is. Checks whether the page can be indexed at all, whether it competes with a page you already published, whether it is represented correctly in titles, canonicals and structured data, whether anything links to it, and whether it is degraded for users. Includes a cannibalisation decision tree and the structured-data rules that trigger manual penalties. This skill should be used on any page immediately before publishing.
---

# Pre-publish page audit

## The claim this skill is built on

Most page audits are run in the wrong order, so they find the wrong problems.

A typical checklist starts with the title tag, the meta description, and image alt text, works through
heading structure, and ends somewhere near structured data. Every item on it is real. But if the page
carries a `noindex` left over from staging, none of it matters, because the page will never appear.
And if the page duplicates the intent of something already ranking on the same site, fixing the title
makes things worse, because a better-optimised duplicate competes harder with the page that was
working.

So the order matters more than the list. This skill runs the checks in descending order of fatality,
and stops to raise the alarm at the first tier that fails, rather than delivering forty findings of
which one is load-bearing.

The five tiers:

0. The page cannot be indexed at all.
1. The page competes with your own site.
2. The page is indexed but misrepresented.
3. Nothing links to the page.
4. The page is degraded for the humans who reach it.

## Tier 0. Can this page be indexed at all?

If anything here fails, report it first, alone, and loudly. Everything below is moot.

- **Robots meta and header.** A `noindex` in the meta tag or in an `X-Robots-Tag` response header.
  Staging leftovers are the single most common cause of a page that quietly never ranks, and they are
  invisible to everyone who is looking at the rendered page rather than the source.
- **robots.txt.** Is the path or a parent directory disallowed? Are the assets needed to render it
  blocked, which can cause the page to be assessed on an incomplete render?
- **Canonical.** Does the canonical point at this page? Does it match the URL that will actually be
  served, in scheme, host, trailing slash, and extension? A canonical to `http://` on an `https://`
  site, or to `/page.html` when the site serves `/page`, is a self-inflicted deindexing. Cross-domain
  or cross-page canonicals must be deliberate and stated.
- **Server response.** 200, not a redirect chain, not a soft 404.
- **Rendering.** If the content requires JavaScript, does the initial HTML contain it? Content behind
  a client-side fetch may or may not be indexed, and treating it as safe is a gamble.
- **Authentication or paywall.** Content behind a gate is content that will not be assessed.

## Tier 1. Does this page compete with your own site?

This is the check almost nobody runs before publishing, and it is the one most likely to make the site
worse. **Run it before Tier 2**, because the answer may be that this page should not exist.

Compare the new page against everything already published on the domain:

- **Identical or near-identical titles.** Fastest signal.
- **Same target intent**, even where the wording differs. "Business expense categories" and "how to
  categorize business expenses" are one intent, not two.
- **URL near-duplicates.** `/blog/expense-categories` alongside `/blog/business-expense-categories` is
  a strong signal that somebody forgot the first one existed.
- **An existing page already ranking for the intended query**, which is the decisive fact. Splitting
  the signal for a page that already ranks is a straight downgrade.

**The decision tree:**

- Existing page ranks well and covers the intent. → **Do not publish.** Fold the new material into
  the existing page. This is almost always the right answer and it is almost never the popular one.
- Existing page ranks poorly and the new page is clearly better. → **Publish at the existing URL**, or
  publish new and 301 the old one. Never leave both live.
- The intents are genuinely distinct and you can state the difference in one sentence. → **Publish**,
  and cross-link them so the distinction is legible.
- You cannot tell. → **Do not publish yet.** Resolve it. Publishing is easy to do and expensive to undo
  once links accumulate.

The one-sentence test is the useful discipline here: if you cannot state, in one sentence, the
difference between who should land on page A and who should land on page B, there is no difference and
you are about to create a duplicate.

## Tier 2. Is the page represented correctly?

Now the conventional checks, which matter once the page can rank and deserves to exist.

**Title.** Unique across the site. Front-loads the thing the page is about. Long enough to be
descriptive, short enough not to be truncated in results, which in practice means somewhere around 60
characters or 600 pixels. It should read as written for a person, not assembled from a keyword.

**Meta description.** Present, unique, and actually a description. A 40 character stub wastes the one
piece of copy you control in the results listing. Around 150 to 160 characters. It does not affect
ranking and it strongly affects clicks, which is a distinction worth understanding rather than
memorising.

**Headings.** Exactly one `h1`. No skipped levels, because the hierarchy is the outline and a jump from
`h2` to `h4` implies a level that does not exist. Headings should describe their sections rather than
being decorative.

**Social cards.** `og:title`, `og:description`, `og:url`, `og:type`, an `og:image`, and a Twitter card
type. **The image URL must be absolute**, with scheme and host. A relative image path is the most
common social card defect and it fails silently: the page looks fine, and every share renders without
an image.

**Structured data.** Two categories of problem, and the second is far more serious.

*Validity.* Dates in ISO 8601, not a local format. `author` as a `Person` or `Organization` object,
not a bare string. Absolute image URLs. Headlines within length limits. Required properties present
for the declared type.

*Honesty.* **Structured data must describe content that is visible on the page.** This is not a
nicety, it is the rule whose violation triggers manual penalties. The specific pattern to watch for:
an `FAQPage` block containing questions and answers that do not appear in the visible copy, frequently
including a promotional question about the product that nobody asked. That is marked-up content the
user cannot see, and it is the textbook violation. Also: no ratings for things nobody rated, no review
markup written by the seller about the seller, and no markup for a type the page is not.

*Two facts that most checklists have not caught up with, verified as of August 2026.* **FAQ rich
results were retired for all sites in May 2026**, so `FAQPage` markup no longer earns a results-page
feature. That does not make it harmless: a fabricated FAQ block is still a policy violation with no
upside left at all, which makes it the easiest thing on this page to simply delete. And **`HowTo`
markup was deprecated in 2023**; a checklist that still recommends adding it is out of date, and so is
one that still talks about keyword density, LSI keywords, or First Input Delay. If the audit you are
replacing mentions any of those, treat its other advice with suspicion too.

Check every block, not just the first. Pages routinely carry two or three, and the broken one is
rarely the first.

**Language and locale.** `lang` on the `html` element. `hreflang` if there are translations, and it
must be reciprocal, or it is ignored.

## Tier 3. Does anything link to the page?

A page nobody links to is a page that is hard to find and carries no internal authority.

- **Is it in the sitemap?** A newly published page absent from the sitemap is a common oversight,
  particularly when the sitemap is generated at build time from a source that does not know about the
  new file.
- **Does any existing page link to it?** If the answer is none, it is an orphan. At least two internal
  links from relevant existing pages, placed in the body where they are contextual, not in a footer
  block.
- **Do its outbound internal links resolve?** Check each against the actual set of pages that exist. A
  link to a page that was planned and never shipped is a 404 you are shipping deliberately.
- **Is any internal link marked `nofollow`?** Almost always a mistake. It blocks internal authority
  from flowing for no benefit.
- **Do the links use meaningful anchor text?** "Click here" and a bare URL both waste the signal.

## Tier 4. Is the page degraded for the humans who arrive?

- **Images.** Every image has an `alt` attribute. Decorative images get `alt=""`, which is a decision,
  not an omission. Content-bearing images, especially an image of a table or a chart, need their
  content available as text somewhere, because an image of a table is invisible to search engines and
  to screen readers alike.
- **The largest image above the fold must not be lazily loaded.** `loading="lazy"` on the hero image
  delays the largest contentful paint, which is the exact metric it is measured by. Lazy loading is
  correct below the fold and harmful above it.
- **Dimensions on images**, to prevent layout shift.
- **Viewport meta tag** present.
- **Contrast, focus visibility, and keyboard reachability** for anything interactive.
- **Text that is actually text.** Copy baked into images is not readable, not translatable, and not
  indexable.

## Reporting

Group by tier, and lead with the highest tier that has a failure. If Tier 0 fails, the report is
one line and a fix, and the rest is an appendix. Burying "this page has a noindex tag" as item 14 of
a 22 item list is a real failure of the audit, not a stylistic preference.

For each finding: what is wrong, the exact line or attribute, what it causes, and the corrected value.
Not "improve the meta description" but the description to use.

End with an explicit ship or hold, and if hold, the minimum set that unblocks it.

## Worked example, compressed

A guide scheduled to publish tomorrow.

**Tier 0, fatal.** `<meta name="robots" content="noindex, follow">` is present. The page cannot rank at
all. The canonical is also wrong twice: it uses `http://` on an `https://` site and includes a `.html`
extension the site does not serve. Nothing else in this report matters until these are fixed.

**Tier 1, fatal in a different way.** An existing published page carries a byte-identical title and
targets the same intent, and it already ranks for the query this page is aimed at. Applying the
decision tree: the existing page ranks, so this page should not publish as a new URL. Its material
belongs in the existing page. **Fixing the noindex without resolving this would actively harm the
site**, which is exactly why the tiers run in this order.

**Tier 2.** Two `h1` elements. The heading hierarchy jumps from `h2` to `h4`. The meta description is
41 characters where the existing page's is materially better. `og:image` is a root-relative path, so
every share renders imageless. `og:description`, `og:url`, `og:type` and the Twitter card are all
absent. The `Article` block has a date in day-first format rather than ISO 8601, a headline well over
the length limit, a bare string author, and a relative image. The `FAQPage` block contains three
questions, none of which appear in the visible copy, and one is a promotional question about the
product. That last item is the most serious finding in this tier and is a manual-penalty pattern, not
a missed optimisation.

**Tier 3.** The page is absent from the sitemap and nothing links to it, so it is an orphan. One
outbound internal link points to a page that does not exist on the site. Another internal link carries
`rel="nofollow"` for no reason.

**Tier 4.** None of the three images has an `alt` attribute, and one of them is an image of a category
table, so its entire content is invisible to both search engines and screen readers. The hero image
carries `loading="lazy"`, delaying the largest contentful paint.

**Verdict: hold.** The Tier 1 finding means the correct action is not to fix this page but to merge it
into the existing one. If it does ship separately, the noindex and both canonical defects and the
fabricated FAQ markup are blocking.

## Failure modes

**Running the list in the printed order.** Producing twenty findings when one of them makes the other
nineteen irrelevant.

**Auditing the page in isolation.** Tier 1 is invisible if you never look at the rest of the site, and
Tier 1 is where the expensive mistakes live.

**Validating structured data only for syntax.** A block can be perfectly valid and still be a policy
violation. Ask whether a reader can see it.

**Trusting the rendered page.** Robots meta, canonical, and structured data are all in the source. Read
the source.

**Checking the first structured data block only.** Pages carry several.

**Reporting a truncated title as equal in weight to a noindex.** Severity is the whole point.

**Auditing a template once.** If the page is generated from a template, a defect is not one defect. It
is one per generated page, and the report should say so.

## What this skill does not do

- It does not fetch live results, check current rankings, or measure real traffic. Statements about
  what already ranks have to come from analytics or a rank tracker, and it will ask rather than assume.
- It does not measure performance. It flags patterns known to hurt, like a lazily loaded hero, but real
  numbers need a real measurement.
- It does not judge whether the content is good, whether it matches search intent, or whether the
  keyword was worth targeting. Those belong upstream, before the page was written.
- It does not audit sitewide technical health: redirect chains across the site, crawl budget, log
  files, index bloat.
- It cannot verify what it cannot see. Server headers, redirects, and rendered output need the live
  URL, and a source-only audit will say which checks it could not complete.
