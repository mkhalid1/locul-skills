---
name: crawl-control-audit
description: Audits how a site controls crawling and indexing across robots.txt, robots meta tags, X-Robots-Tag headers, canonicals and status codes. Finds the conflicts where one mechanism silently cancels another, most commonly a URL that is disallowed in robots.txt while also serving a noindex that can therefore never be read. Covers group matching and precedence rules, what each response code on the robots file itself means, faceted URL explosion, and staging environments exposed because they were defended by robots.txt rather than by authentication. This skill should be used when deciding how to remove something from search results, before a staging or faceted section ships, or when a URL is indexed and nobody can work out why.
---

# Crawl control audit

## The claim this skill is built on

There are two separate jobs here and one word for both of them in most people's heads.

**Crawl control** decides whether a bot is allowed to fetch a URL. That lives in robots.txt.
**Index control** decides whether a fetched URL may appear in results. That lives in a robots meta
tag or an `X-Robots-Tag` response header.

They are not two ways of saying the same thing, and they interact in one direction only. Index
control is delivered inside the response, so it can only be obeyed if the fetch is allowed. Crawl
control blocks the fetch, so it destroys index control.

That asymmetry produces the single most common self-inflicted indexing problem there is. Somebody
wants a page out of search results, so they add a `Disallow` line for it. The crawler stops fetching
the page. It never sees the `noindex` that is sitting right there in the HTML. The URL stays in the
index, usually rendered as a bare listing with no description, because there is no permitted content
to describe. Then somebody adds a second Disallow, and a third, and the pages stay.

**The rule that follows is the opposite of the instinct: to remove a URL from the index, you must
allow it to be crawled.** Allow the fetch, serve `noindex`, wait for the recrawl, and only then, if
you still want to save the crawl, consider disallowing it.

## What the robots file's own status code does

This is the second counterintuitive piece, and it is where a deploy accident turns into a search
incident. The behaviour below is as set out in RFC 9309, the Robots Exclusion Protocol, which became
a standards-track RFC in September 2022, and as documented by major crawler operators. Verified
against the published specification in August 2026.

- **2xx.** The file is parsed and the rules apply. Normal case.
- **3xx.** Redirects are followed, and the specification asks crawlers to follow at least five
  consecutive hops. Beyond that the file is treated as unavailable. Redirects expressed in HTML or
  JavaScript rather than in a status code are not followed at all.
- **4xx, other than 429.** The file is treated as though it does not exist, which means **there are
  no restrictions**. This is the one people get backwards. A 403 or a 404 on robots.txt does not lock
  a crawler out. It opens the whole host. Your directives vanish and nothing anywhere reports it.
- **5xx.** The file is unreachable, and the specification uses mandatory language: crawlers must
  assume a complete disallow. After a long outage, given as roughly thirty days in the specification,
  a crawler may fall back to treating it as unavailable, meaning no restrictions, or keep using a
  cached copy. So a persistently failing server is the case that actually suppresses crawling.
- **429.** Too many requests. Handled in the same bucket as 5xx by Google's documented behaviour
  rather than in the 4xx bucket, so it is a genuine slow-down signal.

Two practical consequences. First, published crawler guidance explicitly warns against using 401 or
403 responses to limit crawl rate, precisely because a 4xx other than 429 reads as "no restrictions"
rather than "go away". The documented way to ask a crawler to back off is 503 or 429, and only as a
temporary measure. Second, robots.txt is commonly cached for up to about 24 hours, so neither a fix
nor a mistake takes effect instantly, and testing a change by watching your logs for five minutes
proves nothing.

## Robots.txt syntax that actually decides the outcome

Most of the file is uncontroversial. These are the parts that change the result.

**Only one group applies.** Records are grouped by `User-agent`, and a crawler obeys the single most
specific group that matches its product token. It does not merge that group with the wildcard group.
So if your file contains a named group for a crawler and a `User-agent: *` group, everything in the
wildcard group is invisible to that named crawler. A rule added to the wildcard group to block a
directory will simply not apply to any bot that has its own group above it. This is the most common
robots.txt bug that is not a typo.

**Longest match wins.** Between an `Allow` and a `Disallow` that both match a path, the rule with the
longer path pattern takes precedence, not the one that appears first. `Disallow: /reports/` plus
`Allow: /reports/public/` permits the public subtree because the Allow is longer. Where two rules are
equally specific, the less restrictive one wins.

**Wildcards and the end anchor.** `*` matches any run of characters, `$` anchors the end of the URL.
`Disallow: /*?` blocks any URL containing a query string. `Disallow: /*.pdf$` blocks paths ending in
`.pdf` but not `/file.pdf?download=1`, which is exactly the sort of gap that leaks.

**Empty means allow.** `Disallow:` with nothing after it permits everything. `Disallow: /` blocks
everything. One character apart, opposite meanings, and both appear in real deploys by accident.

**Paths are case-sensitive; user agent names are not.** `/Reports/` and `/reports/` are different
rules.

**Location is strict.** The file must sit at `/robots.txt` at the root of the host, and it governs
exactly one scheme, host and port combination. A file on the main host says nothing about a
subdomain, nothing about the same host on a different port, and, strictly, nothing about the other
protocol. Every hostname that serves pages needs its own.

**Size.** Crawlers are required to parse at least 500 kibibytes, and Google documents that as its
limit, with everything after the cut ignored. A generated robots file that grows with the catalogue
will eventually cross it, and the rules that fall off the end are the newest ones.

**Comments** run from `#` to end of line. **`Sitemap:`** is independent of the groups and may appear
anywhere in the file.

**Unsupported directives.** Google stopped honouring `noindex`, `nofollow` and `crawl-delay` inside
robots.txt on 1 September 2019. A file containing `Noindex:` lines is doing nothing at all for that
crawler. Some other engines have historically honoured `Crawl-delay`, so check the specific engine
rather than assuming either way.

## X-Robots-Tag, and why it is not a duplicate of the meta tag

The meta robots tag only exists inside HTML. **`X-Robots-Tag` is the only way to control a non-HTML
resource**: PDFs, images, spreadsheets, video files, plain text. If your site publishes PDFs that
outrank their own landing pages, a header is the only lever you have short of deleting them.

The two are additive, not competing. When a page carries both a meta tag and a header, the union of
the directives applies and **the most restrictive value wins**. `index` in one place and `noindex` in
the other resolves to `noindex`. Both can also be aimed at a named crawler rather than at everyone,
and a directive aimed at a specific crawler takes precedence over the generic one for that crawler.

Directives worth knowing:

- **`noindex`.** Do not show this in results. Broadly supported.
- **`nofollow`.** Do not follow links from this page. Note this is the page-level directive, which is
  a different thing from the `rel="nofollow"` link attribute; that attribute became a hint rather
  than a directive for Google's crawling and indexing from 1 March 2020.
- **`none`.** Equivalent to `noindex, nofollow`.
- **`noarchive`.** No cached copy link.
- **`nosnippet`.** No text snippet at all.
- **`max-snippet:[n]`**, **`max-image-preview:[none|standard|large]`**, **`max-video-preview:[n]`.**
  Granular preview limits. Google documents these; support elsewhere is patchy and you should verify
  per engine rather than assume.
- **`notranslate`.** Do not offer a translation of this result.
- **`noimageindex`.** Do not index images on this page.
- **`unavailable_after: [date]`.** Stop showing this after a date, which is genuinely useful for
  expiring event and offer pages.

Where you are unsure whether a given engine honours a given directive, say so in the report rather
than asserting it. The directive set is stable for Google and materially different elsewhere, and the
answer-engine crawlers are a moving target.

## Crawl budget, honestly

Most advice about crawl budget is folklore, repeated on sites that will never encounter it.

Crawl budget is a real constraint for two populations. First, very large sites: Google's own guidance
is aimed at properties above roughly one million unique pages with content changing weekly, or above
roughly ten thousand unique pages with content changing daily. Second, and more common, sites that
generate URLs faster than anyone can crawl them, which is almost always a faceted listing, a calendar,
an internal search results page that is linkable, or a session parameter appended to every link. That
second category can hit the wall at a few thousand real pages, because the real page count is not the
URL count.

The genuine signals, in rough order of usefulness: the share of crawler requests in your server logs
that land on parameterised or non-canonical URLs rather than on content; a "discovered, currently not
indexed" bucket that grows month over month while your publishing rate is flat; average response time
in the crawl stats report trending up; and new pages taking materially longer to be crawled than they
did a quarter ago.

One asymmetry matters here and it follows from the opening claim. Blocking in robots.txt does save
crawl effort, because the fetch never happens. `noindex` does not, because the page must be fetched
for the directive to be read. So for a genuine crawl budget problem, robots.txt is the correct tool
and noindex is not. That is the one case where reaching for Disallow is right, and it is worth stating
plainly, because the rest of this skill spends its time telling you the opposite.

## The five-way decision table

These five are routinely substituted for each other. Each has one correct use.

| Situation | Correct tool | Why |
| --- | --- | --- |
| Two URLs, same content, both should stay reachable | `rel=canonical` on the duplicate, pointing at the preferred URL | Consolidates signals without removing anything. It is a hint, not a command. |
| The page should exist for users but never appear in results | `noindex`, crawl allowed | The only mechanism that actually removes a listing. |
| The content has moved and the old URL should stop existing | 301 permanent redirect | Signals consolidate onto the target. Use 302 only when the move is genuinely temporary. |
| The page is gone, and you are not certain it will stay gone | 404 | Honest, reversible, no signal transfer. |
| The page is gone deliberately and permanently | 410 | Same outcome as 404, generally dropped a little faster. Use it when you are sure. |
| The URL is worthless and you never want it fetched again | `Disallow` | Saves the crawl. Does not remove anything already indexed. |

**The destructive combination to look for: a canonical pointing at a noindexed page.** Page A
canonicalises to page B, and page B carries `noindex`. Page A has said "treat B as the real one" and
B has said "do not list me". The likely outcome is that neither is indexed, and the noindex can
propagate to the whole cluster. This appears constantly on paginated and filtered pages where
someone applied noindex to the canonical target as a tidiness measure. The same applies to a page
that carries both `noindex` and a self-referencing canonical pointed elsewhere: the two directives
contradict each other and the resolution is not something you should be gambling on.

The second destructive combination: a canonical or a redirect on a URL that is disallowed in
robots.txt. Neither can be read, so neither happens.

## Parameter and facet explosion, with the arithmetic

The reason filtered listings break crawling is combinatorial, and the numbers are worth doing before
the feature ships rather than after.

Take a listing page with five independent filter groups, each offering six values, each
multi-selectable in the URL. Ignoring multi-select, each group has seven states, six values plus
unselected, so one base category produces 7 to the power of 5, which is 16,807 URLs. Add four sort
orders and ten pages of pagination and it is 672,280. From one category. A catalogue with two hundred
categories has generated over a hundred million linkable URLs from a few thousand products.

Even the modest case is bad: twelve independent binary filters is 4,096 combinations per category,
and nobody thinks of twelve checkboxes as a large feature.

**The answer is almost always to stop generating linkable URLs, not to block them afterwards.** Apply
filters without changing the URL, or behind a fragment, or via a POST, so the combination never
becomes a crawlable link. Then expose deliberately, as real indexable URLs, only the small set of
combinations that people actually search for, and give those real pages with real content.

Blocking afterwards is the weaker fallback for three reasons. Disallowed URLs can still be indexed if
they are linked from anywhere, since the block prevents the fetch, not the listing. The rules become
a long and fragile pattern list that nobody dares edit. And the internal links pointing at blocked
URLs are still consuming crawl attention.

## Staging and duplicate hosts

The classic failure: a staging or preview host is protected by robots.txt with a blanket `Disallow: /`
and nothing else. Six months later the staging site is in search results, competing with production
on identical content.

The mechanism is exactly the opening claim. `Disallow: /` prevents fetching, not listing. Any external
link, any pasted URL in a public issue tracker, any analytics or link tool that reports the hostname,
is enough for the URL to be discovered. It gets indexed on the strength of the link alone, with no
content, and because the crawl is blocked, no `noindex` you add later will ever be read.

**Authentication is the only reliable answer.** HTTP basic auth, an IP allowlist, or a platform-level
password. An unfetchable host cannot be indexed on the strength of a link, because the crawler gets a
401 on the page itself rather than a page it may not read. This also solves the second problem, which
is that a public robots.txt on a staging host is a published list of the paths you consider sensitive.

If a staging host is already indexed, do not add a Disallow. Allow the crawl, serve `noindex` on
every URL or return 410, wait for the recrawl, and use the temporary removals tool in Search Console
to hide it in the meantime. Then, once the URLs are gone, put authentication in front of it so it
cannot happen again.

The same reasoning applies to any duplicate host: a `www` and non-`www` pair both serving 200, an
alternate domain kept from a rebrand, a CDN hostname serving the whole site. Pick one, redirect the
others, and never handle a duplicate host with robots.txt.

## The decision procedure

Start from the outcome you want, not from the file you were about to edit.

1. **Do you want it out of the index, out of the crawl, or both?**
2. **Out of the index.** Is the URL currently crawlable? If yes, serve `noindex` and leave the crawl
   alone. If it is disallowed, **remove the Disallow first**, then serve `noindex`, then wait for a
   recrawl before doing anything else. Only after it has dropped out should you consider blocking the
   crawl again, and usually you should not bother.
3. **Out of the crawl, and it is not indexed.** Disallow it. This is the legitimate use.
4. **Out of the crawl, and it is indexed.** You cannot have both immediately. Choose: removal first
   via noindex with the crawl allowed, then block later; or accept that a blocked URL may keep its
   listing.
5. **It is a non-HTML file.** There is no meta tag. Use `X-Robots-Tag`. If your platform cannot set
   headers on that path, the only remaining options are removing the file or gating it behind auth.
6. **It is a duplicate rather than something unwanted.** Canonical, not noindex, and never Disallow.
7. **You cannot tell whether the URL is currently indexed.** Do not act. Every branch above depends
   on that answer and the wrong branch is the failure this whole skill is about. Check with URL
   Inspection on a verified property, which is the only authoritative source; a `site:` query is
   indicative at best. If nobody has Search Console access, getting it is the next task, not the
   robots edit. If it genuinely cannot be determined, take the branch that is safe under both
   readings: allow the crawl and serve `noindex`, which is correct whether or not the URL was
   indexed, and costs only crawl effort.

## Worked example, compressed

A documentation and marketing site, roughly 900 real pages, publishing PDFs, with a filtered resource
library. Traffic is flat and several odd URLs keep appearing in results.

**Finding 1, fatal and self-inflicted.** `/account/` is disallowed in robots.txt and every page under
it also serves `<meta name="robots" content="noindex">`. Eleven of those URLs are in results as bare
listings with no description. The noindex has never been read. Fix: delete the Disallow, keep the
noindex, wait for recrawl, and leave the Disallow off permanently since these pages are behind a login
and cost almost nothing to crawl.

**Finding 2, silent and total.** The robots file returns 404 on the `www` hostname, which is the one
that serves the site, and 200 on the bare domain, which redirects. Every rule in it is currently
inert, because a 4xx means no restrictions. Nobody noticed because the file exists in the repository.

**Finding 3, invisible by design.** A `Disallow: /internal-search/` line sits in the `User-agent: *`
group, below a named group for a major crawler that was added to set a `Crawl-delay`. That crawler
matches its own group only, so it ignores the internal search block entirely, and its own group
contains one directive that the crawler does not support. Fix: delete the named group so the crawler
falls through to the wildcard rules.

**Finding 4, wrong mechanism.** Forty-two PDFs carry a meta robots tag in the HTML of their landing
pages, in the belief that this controls the PDFs. It does not. The PDFs need `X-Robots-Tag: noindex`
on the file responses.

**Finding 5, destructive pair.** Filtered library pages canonicalise to `/library/`, and `/library/`
carries `noindex` because somebody decided the unfiltered view was thin. The canonical target is
excluded from the index, so the cluster has nowhere to consolidate to.

**Finding 6, arithmetic.** The library has six filter groups averaging five values, generating on the
order of 46,000 URLs, all linkable, against 340 real documents. Currently handled by four wildcard
Disallow patterns that do not cover the sort parameter.

**Verdict: two blocking fixes and one design change.** Blocking: remove the `/account/` Disallow, and
fix the robots file 404 on the serving hostname. Design: stop generating linkable filter URLs and
expose the eleven combinations with real search demand as real pages. The PDF headers and the
canonical target are the following week's work.

## Failure modes

**Disallowing something to deindex it.** The founding error. It preserves the listing forever and
removes your ability to fix it.

**Assuming a 4xx on robots.txt is safe.** It removes every restriction on the host, and a browser
check shows a perfectly ordinary error page.

**Editing the wildcard group when the crawler has its own group.** The edit is real, deployed, and
completely inert for the bot you cared about.

**Reading the first matching rule instead of the longest.** Produces confident, wrong answers about
whether a path is blocked, in both directions.

**Testing robots.txt on the wrong hostname.** The file is per host, per scheme and per port, and the
file that governs your traffic is the one on the hostname that actually serves pages.

**Canonicalising to a noindexed page.** Quietly excludes the whole cluster, and every individual
directive involved looks reasonable in isolation.

**Treating staging robots.txt as security.** It publishes the paths you want hidden and does not stop
them being indexed.

**Blocking a facet explosion instead of not creating it.** Leaves a fragile pattern list, still leaks
through the parameters nobody enumerated, and does not remove the URLs already discovered.

**Expecting an instant effect.** Robots files are cached for hours and recrawls take days to weeks,
so a change judged after one afternoon will be judged wrong.

## What this skill does not do

- It does not tell you what is in the index. It reasons about what the directives should cause.
  Confirming the state of a specific URL needs URL Inspection on a verified property.
- It does not fetch anything. Real status codes, real headers and real redirect chains need a live
  request, and a review from source alone will name the checks it could not complete.
- It does not analyse logs or crawl statistics, so its statements about crawl budget are inferences
  from site shape rather than measurements of crawler behaviour.
- It does not cover engine-specific behaviour beyond the well-documented cases, and it will say where
  support is uncertain rather than inventing a compatibility claim.
- It does not decide which pages deserve to be indexed. That is a content and inventory question, and
  overlapping pages competing with each other is a different audit.
- It changes nothing. Every output is a file edit, a header rule or an infrastructure decision for
  somebody with deploy access, and on some hosted platforms the robots file is not yours to edit.
