---
name: internal-link-audit
description: Audits internal linking as a graph rather than a per-page checklist. Parses the rendered body of every page rather than trusting the stored links field, then finds orphans, dead ends, pages too deep from the entry point, and pillar and child pairs that only link in one direction. Covers anchor text distribution, the difference between a body link and a sitewide navigation link, a link budget by page length, and how to reduce two hundred findings to a handful of causes. This skill should be used when auditing internal linking across a site, after a migration or redesign, or immediately after publishing a page that needs inbound links.
---

# Internal link graph audit

## The claim this skill is built on

Internal linking is nearly always audited one page at a time, with a question that cannot detect
anything: does this page have internal links? Yes. Next page.

Every defect worth finding is a property of the graph, not of a page. A page with a dozen outbound
links can be an orphan, because outbound and inbound are different directions. A page can pass every
per-page check and still be five clicks from the entry point, because depth is a path, not an
attribute. Two pages can both link out generously and never link to each other, and that specific
pair was the one that would have helped. None of those are visible from inside a single page.

There is a second claim, and it is the one that decides whether an audit works at all: **most content
systems store a field listing each page's internal links, and it is very often empty or stale.** An
audit that queries that field will report a clean graph on a site full of orphans. That single
mistake is the most common reason an internal link audit changes nothing.

## The four defects, defined precisely

Vague definitions are why these get reported inconsistently. Fix them before you count anything.

**Orphan.** A page with zero inbound links from other pages on the same site. Self-links do not
count. Being present in the sitemap does not count, and being in the sitemap while being an orphan
is an extremely common state, because sitemap generation reads the database and linking is done by
humans. Note also the softer version below: a page whose only inbound links are sitewide templated
blocks is a **contextual orphan**, and for the purpose of this audit it is treated as an orphan.

**Dead end.** A page with zero outbound internal links in its body. Templated navigation does not
rescue it, for the same reason. Some pages are legitimate dead ends: an order confirmation, a
thank-you page, a legal notice nobody should be routed onward from. Declare those explicitly as
exceptions rather than letting them sit in the findings forever.

**Too deep.** Depth is the minimum number of clicks from the entry point, computed over the real
link graph, not from the URL structure. A URL five directories deep can be one click from the home
page and a URL at the root can be six. The rule of thumb: anything you want to rank should be
reachable within three clicks, and anything beyond four is a structural problem rather than a page
problem, so fix the structure rather than adding a link to that one page.

**Broken pillar and child reciprocity.** A pillar page and one of its children where the link runs
in one direction only. Define the relationship by topical containment, never by URL depth: a page is
a child of a pillar if the pillar's subject genuinely contains it. The pillar must link down to
every child, and every child must link back up. The single most common failure is the pillar linking
down to all twenty children and eleven of them never linking back.

## Why the stored links field cannot be trusted

The mechanisms, so you can recognise which one you have:

- The field was populated once by a migration or an import script and has never been written since.
- It is an editor-managed "related content" picker, which is a different concept from links that
  exist in the body, and it is filled in for the first few pages and abandoned.
- The body is edited through a path that does not update it: an API, a bulk edit, a translation
  pipeline, a component that stores rich text separately.
- Links added inside components, shortcodes or embeds never touch the field at all, because the
  field is written by the editor form and the component renders later.

**The procedure instead.** Take the rendered body markup for every page, extract every anchor,
normalise it, and resolve it against the real set of published URLs. Treat the stored field only as
a cross-check, and when it disagrees with the parsed body, the parsed body is right.

**Normalisation rules that change the result.** Strip fragments, so a link to `/guide#setup` counts
as a link to `/guide`. Strip tracking parameters. Resolve relative paths to absolute. Apply the
site's own convention for trailing slashes and case rather than treating variants as different
pages. Follow one hop of internal redirects and count the destination, not the redirecting URL, or
your graph will contain a hundred edges pointing at nothing. Drop `mailto:` and `tel:`. Decide
whether subdomains are internal for your purposes, and write down which you chose, because an audit
that silently changes that rule halfway through is not comparable to itself.

## Anchor text

The anchor is the only per-link description of a destination you get to write, so wasting it is
expensive and misusing it is worse.

**The same anchor pointing at several different destinations is a real defect.** If the phrase
"pricing guide" points at four different pages across the site, none of those pages owns the phrase,
and you have arranged for your own pages to compete for it. This is the internal version of
cannibalisation and it is invisible to any check that looks at links one page at a time. Flag any
anchor phrase that points at three or more destinations.

**A bare URL and "click here" both waste the signal.** They describe nothing, they read badly in a
screen reader's list of links, and they tell a reader who is skimming nothing about whether to
click. Replace them with a phrase that would still make sense if the surrounding sentence were
deleted.

**How much variety is enough.** Not a formula, a distribution. For any destination, its inbound
anchors should include the core concept in a couple of phrasings, some natural variations, and a
couple of longer descriptive phrases. Two working thresholds: no single anchor phrase should account
for more than roughly half of a page's inbound internal anchors, and no more than roughly a quarter
should be generic ("here", "this page", "read more", a bare URL). Identical exact-match anchors on
every inbound link is not optimisation, it is the pattern that gets a site's linking described as
manipulative.

## Link placement, and why counts lie

A link in the body of a page carries more than a link in a footer or a navigation block. The reason
is mechanical rather than mystical: a sitewide link appears on every page, so it says nothing about
the specific page it appears on, and it dilutes across the entire site.

The consequence is the finding most audits miss. **A page whose only inbound links are sitewide
navigation is effectively still an orphan.** Its inbound count might be four hundred.

**Detecting templated blocks programmatically.** If an identical anchor and destination pair appears
on more than about eighty per cent of crawled pages, treat it as a sitewide element. Exclude those
edges from the contextual graph and keep them in a separate global graph. Compute depth twice, once
over each. The gap between the two depth figures is usually the most informative number the whole
audit produces.

Resist building the audit on finer placement theories, such as which link in the body counts most.
Those are contested, they change, and nothing in this method depends on them.

## The link budget

A rough guide: **two to five in-body internal links per thousand words**, plus one link to the pillar
and one to a sibling. A four hundred word page carrying thirty body links is a link list wearing a
page's clothes.

The honest statement is that the exact number is not the point. **The distribution is.** A site where
twelve pages carry three hundred links and four hundred pages carry none has a perfectly respectable
average and a broken graph. So report the median, the count of pages with zero contextual inbound
links, and the count at each depth. Never report the mean on its own, because the mean is the number
that lets a broken site pass.

## The reciprocal linking pass, done properly

Run this after publishing, when the related pages are still fresh in your mind.

1. Find genuinely related existing pages by reading, not by tag. Tags are an organising convention,
   not a statement of relatedness.
2. Add one or two contextual links from those pages, placed where the sentence was already about
   that concept. If you have to write a new sentence to hold the link, that is a signal, not a step.
3. **Skip any page that already carries a lot of outbound internal links.** One more link there is
   diluted, and that page is already doing its job.
4. **Skip the hub pages.** They already link to everything. Adding a row to a list is not a
   contextual link and does not behave like one.
5. **Never force a link that does not read naturally.** A forced link is worse than no link: it
   damages the paragraph, it costs reader trust, and it produces an anchor that describes nothing.
   The reliable tell is a sentence that exists only to hold the link.
6. **Cap the pass.** Two to four new inbound links is a complete job for a new page. Fifteen is the
   pattern that gets described as a scheme.

## `nofollow` on internal links

Almost always a mistake, and worth knowing the history because the advice outlived its reason.

The attribute was used to try to control how internal signal was distributed. That stopped working
in 2009, when the model changed so that a link marked `nofollow` still consumes its share of what
the page has to give, meaning the practice threw the share away rather than redirecting it. Since
2019 the link attributes have been treated as hints rather than directives, so the setting is not
even a reliable instruction.

On an internal link it achieves nothing you want. It does not prevent the destination from being
discovered, it does not reliably save crawl budget, and it discards a signal you fully control. The
legitimate uses are user-generated content and paid placements, which are about links you did not
choose. If the goal is to keep a page out of the index, use a robots meta directive on the
destination and leave the link followable.

## Hub and spoke, or flat

**Hub and spoke** means one pillar page per topic that links to every child, with each child linking
back to the pillar and across to two or three siblings. Correct when there is a genuine hierarchy,
when the pillar can plausibly rank for the broad term, and when the topic holds more than roughly
thirty pages.

**Flat** means every page links to its relevant peers with no designated parent. Correct for small
sites, for reference and documentation sets where every page is an equally valid entry point, and
for collections with no natural parent concept.

Each fails in a characteristic way. Hub and spoke fails when the pillar is a list of links with no
content of its own, at which point it is a menu and it accumulates nothing. Flat fails when no page
ever gathers enough internal signal to compete for the broad term.

**The decision rule.** If you can state the pillar's own reason to exist in one sentence that is not
"it links to the others", build hub and spoke. If the topic is under about thirty pages and has no
obvious parent concept, go flat. **If you cannot tell**, go flat and revisit at thirty pages, because
a flat structure that later grows a pillar is a cheap change, and a menu pillar that has been
accumulating links for a year is an expensive one.

## The procedure, over an exported list of pages

Input: a list of URLs, and for each, the rendered body markup, the word count, the genuine last
edited date, and any traffic data you have.

1. Extract every anchor from every body. Normalise using the rules above.
2. Build edges. Drop external hosts, `mailto:`, `tel:`, self-links and fragment-only links.
3. Identify templated blocks with the eighty per cent presence rule. Label those edges global and
   keep them separate.
4. For each page compute: contextual inbound count, contextual outbound count, depth from the entry
   point over all edges, and depth over contextual edges only.
5. Flag orphans, contextual orphans, dead ends, and anything deeper than three.
6. Anchor pass. Group anchors by destination and destinations by anchor. Flag any phrase pointing at
   three or more destinations, any destination whose anchors are more than half one phrase, and any
   destination where more than a quarter of anchors are generic.
7. Pair pass. For each declared pillar, check both directions to every child.
8. Report the distribution first, then the findings, then the causes.

## What to do when the audit produces two hundred findings

This is the normal outcome, and it is the reason most internal link audits change nothing. A two
hundred row spreadsheet of hand edits is a document that gets shared, admired and never completed.

- **Group by cause before you group by page.** Two hundred findings are usually fewer than ten
  causes: a template that stopped rendering its related block after a redesign, an import that
  dropped in-body links from a batch of migrated pages, a section that never had a pillar.
- **Apply a value gate.** Do not build links to a page you would not defend. Deleting or merging a
  weak page is a legitimate fix and it removes the finding permanently rather than servicing it.
- **Fix causes in code or templates, fix the top twenty pages by hand, delete or merge the tail.**
  That is a week of work. The full list is a quarter of work nobody will do.
- **Cap the first pass.** At most three template causes and twenty page-level actions. Re-run the
  audit afterwards, because fixing the template usually clears most of the rows.

## Worked example, compressed

A six hundred and twenty page site of product pages and guides, exported with rendered body markup.

**Extraction.** The content system's stored links field reports about 8,900 internal links. Parsing
the bodies finds 2,140. The difference is navigation, and an audit querying the stored field would
have concluded the site is densely linked.

**Templated blocks.** A navigation of fourteen links and a footer of twenty-two appear on every
page. After separating them: roughly 22,000 global edges, and 1,180 contextual edges spread across
620 pages. Median contextual inbound per page: zero.

**Defects.** 402 pages have no contextual inbound links, so they are contextual orphans. Of those,
51 have no inbound links at all beyond the sitemap listing. Depth over all edges is at most three,
because the footer links everywhere, which is exactly why that figure is worthless on its own. Over
contextual edges only, 388 pages are unreachable. The gap between those two depth figures is the
finding.

**Anchors.** The phrase "pricing" points at six destinations. Forty-one per cent of all contextual
anchors are "read more" or a bare URL.

**Causes, not rows.** Three. The guides template stopped rendering its related-links block after a
redesign, which the last-edited dates place in one release. A migration dropped in-body links from
roughly three hundred imported pages. And the guides section has no pillar page at all, so nothing
gathers the children together.

**Verdict.** Two template fixes and one new pillar page written properly, then twenty hand-placed
reciprocal links on the highest-value guides, then re-run the extraction. Nobody opens the 402 row
spreadsheet, because after the template fix most of it will not exist.

## Failure modes

**Querying the stored links field.** Produces a clean report on a broken site and is the single most
common reason this work goes nowhere.

**Counting navigation as internal linking.** Turns four hundred orphans into four hundred
well-linked pages on paper, and hides them until someone asks why nothing in that section ranks.

**Auditing page by page.** Cannot detect an orphan, a depth problem or a broken pair, because none
of them is visible from inside the page.

**Reporting the mean.** A handful of hub pages carrying hundreds of links will drag any average into
a healthy-looking range while most of the site has none.

**Bulk-adding links to hit a number.** Produces forced sentences, generic anchors and a page that
reads worse. A forced link is worse than the missing link it replaced.

**Identical exact-match anchors everywhere.** Reads as templated, and is the shape that gets a site's
internal linking treated as manipulation rather than navigation.

**Nofollowing internal links to shape the flow.** Stopped working in 2009 and has been a hint rather
than a directive since 2019. It discards the signal instead of redirecting it.

**Counting links to redirected URLs as links to their destinations without checking.** Inflates the
graph with edges that resolve elsewhere, and hides chains that should be repaired.

**A pillar that is only a menu.** A list of links with no content of its own accumulates nothing and
gives the children nothing to link back to that a reader benefits from.

**Servicing two hundred findings by hand.** The list is six causes. Fixing the causes clears most of
the rows, and starting with the rows means the causes keep generating new ones.

## What this skill does not do

- It does not crawl. It works from an export of URLs and body markup, and producing that export is a
  crawler's job.
- It does not model how authority flows. It counts edges and describes structure. Any tool promising
  an internal authority score is running a model with assumptions you should read before quoting it.
- It cannot see links that only appear after JavaScript executes, unless the export was captured from
  rendered output.
- It does not judge whether a destination deserves the traffic. Linking hard to a page that should be
  merged or deleted is a way of making a problem permanent.
- It says nothing about external links, which on a competitive query are frequently the binding
  constraint rather than the internal graph.
- It cannot tell whether a proposed link reads naturally. That decision needs a person who has read
  the paragraph, and it is the decision that determines whether the reciprocal pass helps.
