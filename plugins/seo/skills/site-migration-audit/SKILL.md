---
name: site-migration-audit
description: Reviews a site migration before and after launch: the release sequence, the redirect map and its sourcing, what gets preserved, and what the traffic afterwards actually means. Checks that the map was built from the union of a crawl, analytics, search impressions, server logs and backlinks rather than from the sitemap, that mappings are one to one and single hop, and that internal links, structured data, hreflang, canonicals, robots directives and analytics configuration survived the move. Includes separate pre-launch and post-launch checklists, a post-launch timeline by hour, day, week and month, and a rule for telling a normal dip from a broken migration. This skill should be used when a redirect map is drafted, immediately after a launch, or when somebody asks whether a post-migration drop is normal.
---

# Site migration audit

## The claim this skill is built on

A migration that loses traffic has usually not been broken by a bad redirect. It has been broken by
an unanswerable question.

Somebody changed the URLs, the templates and the copy in one release. Traffic fell nineteen per cent.
Was it the redirects, the new template, the rewritten pages, the loss of an internal link block, or
something that had nothing to do with the release? Nobody can say, because five variables moved at
once. And because the content changed, going back is not a revert but a second migration.

So the first rule is not about redirects at all.

**Change one thing at a time.** In order:

1. **Move the URLs.** Same content, same templates, same internal link structure, new addresses.
   Let it settle. Two to four weeks on a small site, longer on a large one.
2. **Change the templates.** Same URLs, same words, new markup and layout.
   Let it settle.
3. **Change the content.** Same URLs, same templates, new words, in batches rather than sitewide.

Each step is separately measurable and separately revertible. The order is not arbitrary: URLs first,
because that is the change that everything else has to be updated to point at, and doing it last means
redoing the internal links, the canonicals and the sitemaps twice.

**When you genuinely cannot phase it.** A platform replacement often changes the URLs and the
templates in the same act, because the old templates do not exist on the new platform. The honest
answer is not to pretend, it is to reduce the number of moving parts that remain optional and to buy
attributability another way. Freeze content edits for the whole window, ship one section first as a
pilot rather than the site at once, keep a segment of the site unmigrated for as long as possible so
it acts as a comparison against seasonality and engine updates, and record a full baseline before
launch. A pilot section is the single most useful thing a bundled migration can do, because it turns
one unrepeatable event into a small one you can learn from.

## The redirect map, and where it comes from

The map is the central artefact, and the most common defect in it is not a wrong rule. It is a
missing row.

Almost everybody builds the map from the sitemap or a content export. Both describe what the site
believes it publishes, which is not the same as the set of URLs that exist, are requested, or carry
value. Use all five of these and take the union.

**1. A full crawl of the live site.** Every URL reachable by following links from the home page, with
its status code and its canonical. This is the backbone and it is the only source that gives you
structure. It misses orphans by definition, since it can only find what something links to.

**2. Analytics: every page that received a visit in the last twelve months.** Twelve rather than
three, so seasonal pages are included. This finds URLs nothing links to any more but people still
reach: pages linked from an old email campaign, a PDF, a slide deck, a partner's intranet, or a
bookmark. Sort by visits descending and the tail is the interesting part.

**3. Search Console: every page that received an impression.** This finds URLs that are indexed and
ranking but get few or no clicks, and it finds parameter and variant URLs that are in the index and
that nobody on the team knows about. It is also the only source that tells you which URLs the search
engine currently considers to be your site.

**4. Server logs.** The only source that sees every request actually made, by anyone, including
crawlers. It finds URLs that get no traffic, no impressions and no links, but are still being fetched:
feeds, API paths, legacy asset paths, old URLs that already 404 and have been quietly doing so for
years. It is also the only place you can see which of your old URLs crawlers still bother with, which
is a direct signal about which redirects matter.

**5. The backlink list from a link tool.** The highest-value rows and the ones the other four cannot
produce. A URL that was deleted two years ago, links to nothing, ranks for nothing and receives no
traffic can still carry external links, and a redirect from it recovers something that is otherwise
permanently lost. Export referring pages by target URL and sort by number of referring domains.

Deduplicate the union, normalise the protocol and host variants, and classify every row: has an
obvious equivalent, has a related parent, has nothing. That classification is the work.

## One to one, and why the home page is a trap

**Map each old URL to the single most equivalent new URL.** This is not a stylistic preference.

Redirecting a large set of removed pages to one irrelevant destination, typically the home page, is
documented as being treated as a soft 404 rather than as a redirect. The consequence is worth stating
precisely: the destination is not credited with anything, the old URL is dropped from the index as
though it had returned a 404, and any external links pointing at it deliver nothing. So the move that
feels like the safe, tidy option, catching everything so nothing 404s, produces the same search
outcome as doing nothing while also being worse for users, who clicked a specific result and landed
on a generic page with no explanation.

Where there is genuinely no equivalent, there are three honest options and one rule for choosing.

- **Redirect to the closest genuinely relevant page**, usually the parent category or the section
  index. Credited, because the relationship is real. Correct when the topic still exists but the
  specific page does not.
- **Serve 410 and let it go.** Correct when the content is retired, has no successor, and nothing
  external points at it. It is honest, it is fast to process, and it keeps your index clean.
- **Keep the content.** If the URL has meaningful external links or ongoing traffic and there is no
  target that honestly serves the same intent, the cheapest answer is often to port the page rather
  than to argue about where to send it.

The rule: check external links and trailing traffic first. If either is significant, find a relevant
target or keep the page. If neither is, 410 it. The home page is never the answer for more than a
handful of URLs.

## Redirect mechanics

**Permanent versus temporary.** A 301 or 308 says the new URL is now canonical and signals should
consolidate there. A 302 or 307 says the original is still the canonical one and this is a detour.
Using 302 for a permanent move delays consolidation, and a long-lived 302 may eventually be
reinterpreted as permanent, which is a behaviour to rely on never. Use 301 for a move, 302 only for
something genuinely temporary such as a maintenance page or geographic routing.

**Chains must be collapsed to one hop.** Old URL to new URL, directly. Crawlers follow a limited
number of hops, documented by Google as up to ten before the chain is reported as an error, and each
hop adds latency for real users. Chains rarely get built deliberately. They accumulate: the migration
rule fires, then the platform's own trailing-slash or lowercase canonicalisation rule fires on the
result, and now every URL on the site is a two-hop redirect that nobody wrote. A previous migration's
rules still in the config are the other usual source, and that produces three and four hop chains
quickly.

**Loops.** A to B to A. Usually a migration rule fighting a canonicalisation rule, or two rules with
overlapping patterns in the wrong order. A loop is a hard failure for users, not just for crawlers.

**Never redirect to a URL that itself redirects.** Rewrite the map so every source points at the
final destination. When the map is generated from a list of old and new pairs, this has to be checked
after generation, because the generator does not know the targets moved.

**Protocol and host variants multiply.** For every path there is potentially `http` and `https`,
`www` and bare, with and without trailing slash, and case variants on a case-sensitive server. Four
host variants is the common case, and a rule set that handles only the one people tested leaves the
others 404ing or looping. Test all four.

**Query strings.** A rewrite that drops the query string silently breaks paginated URLs, filtered
URLs and every campaign-tagged link ever sent. Decide explicitly whether the query is preserved,
consumed or dropped.

**The map must be tested against a running server, not reasoned about.** Rule order, regular
expression greediness, trailing slashes, encoding, and the platform's own canonicalisation interact
in ways nobody predicts correctly by reading a config file. Fire the entire old URL list at staging,
record final status and final URL for each, and treat anything that is not a single hop to a 200 as
a defect. This is the single highest-value pre-launch activity and it is routinely skipped because
the map "looks right".

## What is routinely lost

Everything below survives only if somebody explicitly does it. None of it is automatic.

- **Internal links rewritten to point at final URLs.** Links that go through a redirect work, so this
  never blocks a launch, and it ages badly: when the redirect rules are eventually cleaned up, every
  one of those links becomes a 404. Rewrite the links in the content, not just the navigation.
- **Canonical tags updated to the new URLs.** A new page canonicalising to its old URL, which now
  redirects, is a common template oversight and it is self-defeating.
- **Structured data.** It usually lives in the old template. New templates ship without it and nobody
  notices because nothing looks broken.
- **hreflang, and reciprocity.** Every annotation has to be updated to the new URLs on every language
  version, and the references must point back at each other. A one-sided annotation is ignored, so a
  half-updated set is functionally no set at all.
- **Robots directives.** Two failures, in opposite directions: a staging `noindex` shipped to
  production, and a production robots.txt replaced by the staging one. Both are silent.
- **Sitemaps.** Generate new ones and submit them. Also keep a sitemap of the **old** URLs and submit
  that too, temporarily: it is the fastest way to get the old URLs recrawled so the redirects are
  discovered, and it is the step most teams have never heard of. Retire it once the old URLs have
  been processed.
- **Analytics configuration.** Goals and conversions defined by URL path, filters and channel rules
  referencing old paths, the tag container missing from a new template, cross-domain tracking for a
  domain move, and referral exclusions. A migration that also breaks measurement is a migration you
  cannot evaluate.

## Pre-launch checklist

These are different lists and this one is finite. Every item is a yes or a no.

- Redirect map built from all five inventory sources, deduplicated, with every row classified.
- Every mapping one to one, or explicitly justified where it is not.
- Map fired at staging: every old URL resolves in one hop to a 200. No chains, no loops, no 404s.
- All four protocol and host variants covered.
- New site crawled: no `noindex`, correct robots.txt for the production host, self-referencing
  canonicals on the new URLs.
- Internal links, canonicals, hreflang and structured data all pointing at or present on the new URLs.
- New sitemap generated; old-URL sitemap prepared for submission.
- Analytics and tag containers verified on every template; goals rebuilt against new paths; an
  annotation prepared for the launch date.
- Search Console property verified for the new host before launch, because data begins at verification.
- Baseline captured: rankings for the priority query set, indexed URL count, traffic by template, and
  the top few hundred landing pages by traffic and by revenue.
- Rollback criteria written down, with a named decider and a deadline.
- Content and template changes frozen for the window.

## Post-launch checklist, with a timeline

**Within the hour.** Nothing about traffic yet. Fetch the production robots.txt and read it. Check a
sample of thirty to fifty URLs spread across templates with real requests, confirming a single hop to
a 200. Confirm no `noindex` on production. Load the home page and each major template. Confirm
analytics is receiving hits. Confirm certificates are valid on every host variant.

**Within the day.** Crawl the new site in full. Fire the entire old URL list and check every row, not
a sample. Submit the new sitemap and the old-URL sitemap. Look for chains and loops introduced by the
interaction of your rules with the platform's. Watch the server error rate and response time, since a
new stack under real traffic is the likeliest source of a 5xx spike, and a 5xx spike is a crawling
problem as well as a user one.

**Within the week.** Index coverage in Search Console: are new URLs being indexed, and at what rate.
Crawl errors and their shape. The mirror-image test below. Server logs for whether crawlers are
finding the new URLs. Look at traffic, but do not draw conclusions from it yet.

**Within the month.** Rankings against the baseline for the priority queries. The recovery curve
shape. Retire the old-URL sitemap once those URLs have mostly been recrawled. Confirm internal links
were actually rewritten. Only now is the traffic number meaningful.

**What normal looks like.** A dip that is visible from the launch day, bottoms out inside about two
weeks, and is trending back up by week four. Positions wobbling while impressions stay broadly flat.
The count of indexed old URLs falling while the count of indexed new URLs rises by roughly the same
amount, which is the mirror image and is the clearest single indicator that the move is proceeding
rather than failing. Recovery in four to eight weeks on a small site, longer on a large one. Nobody
can give you an honest percentage for the depth of a normal dip, and anyone who does is guessing.

**What a broken migration looks like.** Clicks and impressions falling together in proportion, which
means URLs are not being served at all rather than ranking slightly worse. A fall that is still
deepening in week three. Old indexed URLs falling with no corresponding rise in new ones. Errors
concentrated in one template or one directory, which points at a rule rather than at a general
effect. Crawl error counts climbing rather than settling.

## The rollback question

Decide this before launch and write it down: what would have to be true to roll back, who decides,
and by when.

The non-obvious constraint is that **your rollback criteria must be leading indicators, not traffic.**
Traffic data is too slow. By the time a fall is unambiguous in the numbers, the new URLs have started
being indexed, and rolling back means running a second migration in the opposite direction with all
the same risks. In practice the rollback window is somewhere between twenty-four and seventy-two
hours, and the only things observable in that window are error rates, redirect failures,
indexability, and whether measurement is working at all.

So write criteria like these: more than a stated share of the old URL list failing to resolve in one
hop; any `noindex` or blanket disallow on production; server error rate above a stated threshold;
core user journeys broken. Not "traffic down twenty per cent", which you will not know in time.

And the honest part: **a migration with no rollback plan will not be rolled back, whatever happens.**
Under pressure, with no agreed criteria and no named decider, every organisation chooses to push
forward and fix, because rolling back feels like an admission and fixing feels like progress. The
plan exists to make the decision in advance, when nobody is defending anything.

## Consolidating two sites into one

The hardest case, because the map stops being one to one by necessity.

Two sites that both rank means overlapping coverage, so several old URLs from each will legitimately
map to one destination. Many-to-one is exactly where soft-404 treatment appears, so each grouping has
to be defensible on relevance rather than on convenience.

**Where two pages cover the same intent, merge the content, do not just redirect.** A page ranks
because of what is on it. Redirecting it to a destination that does not contain its material discards
the reason it ranked, and the destination does not inherit the coverage by being the redirect target.
The work is editorial: fold the material in first, then redirect.

**Where content genuinely should not be redirected at all:**

- Pages that exist only for the retired brand: about, team, careers, contact, legal, press. A user
  clicking through wants the retired company, and no page on the surviving site answers that.
  A single explanatory page, or a 410, is more honest than an equivalence that is not real.
- Pages whose audience or product line is being retired with the site. Sending that traffic to the
  nearest surviving product is a bad user outcome and a weak relevance signal.
- Thin or low quality pages that the surviving site would be better off without. A consolidation is
  the one moment when deleting is free, and taking on somebody else's weak inventory is a real cost.

**Do it in waves.** Section by section, with a gap between each, so a fall can be attributed to a
section rather than to an event. A consolidation done in one release is the least attributable change
a site can make.

## Decision procedure: is this drop normal?

1. **Do all the old URLs resolve in one hop to a relevant 200?** Test a few hundred sampled from the
   union list, weighted towards those with external links. Any failures found here are the answer.
   Stop and fix.
2. **Are impressions falling as far as clicks?** Clicks down with impressions broadly held means
   positions moved a little and the URLs are still being served. Both falling in proportion means the
   URLs are not being served, which is an indexing problem, not a ranking one.
3. **Is the fall uniform or concentrated?** Concentrated in one template or directory means a rule.
   Uniform across everything means a site-level directive, a robots or canonical problem, or a
   genuine authority effect.
4. **Run the mirror-image test.** Old indexed URLs down and new indexed URLs up by a similar order is
   normal progress. Both down means URLs have been lost.
5. **When did it start?** On the launch day points at the release. Several days later can still be
   the release, because crawling lags, so check whether anything else changed in that window before
   concluding.
6. **You cannot tell.** This is the common case and it deserves an explicit branch. Without a
   baseline you have nothing to compare against, so build the comparison retrospectively: use the
   same weeks in the previous year to strip seasonality, and use any segment of the site you did not
   migrate as a control for anything sitewide. If you have neither, **hold for four weeks and change
   nothing else.** Layering a second uncontrolled change on top of an unattributable one is how a
   recoverable migration becomes a permanent mystery. Spend the four weeks fixing the things that are
   objectively wrong regardless of the traffic, which are redirect failures, missing directives and
   broken measurement.

## Worked example, compressed

A company site with about 1,100 URLs moves from `/resources/2019/04/some-title` to `/guides/some-title`,
on a new platform, with a refreshed template, all in one release. Three weeks later organic traffic is
down and nobody agrees why.

**Sequencing.** URLs, templates and platform moved together. The pilot option was available and not
taken. Nothing can be cleanly attributed, so the review works forward from the objective checks rather
than backward from the traffic.

**Inventory.** The map was built from the CMS export: 1,096 rows. The crawl finds 1,140. Analytics
adds 61 URLs with visits in the last year that the crawl never reached, mostly linked from old
newsletters. Impressions data adds 38 more, mostly paginated variants. Logs add 27, including a feed
path still fetched daily. The backlink list adds 19 URLs deleted before the migration that still carry
links from a total of 34 referring domains, and none of those were in anybody's map. **The union is
1,285 URLs, so about fifteen per cent of the real inventory had no rule.**

**Mapping.** 84 old URLs with no obvious equivalent were sent to the home page. Under soft-404
treatment those are effectively 404s that also waste the click. Eleven of them carry external links
and need real targets; the rest should be 410 or mapped to their section index.

**Mechanics.** Firing the old list at production shows every URL resolving in two hops, because the
migration rule produces a path without a trailing slash and the platform then redirects to add one.
One loop exists between an old category URL and its new equivalent. The bare-domain variant has no
rules at all and 404s.

**Preservation.** Internal links in body content still point at old URLs, so they resolve through the
two-hop chain. The new article template ships without the structured data the old one had. The old
sitemap was deleted at launch rather than resubmitted. Analytics goals still key on `/resources/`, so
conversions have read as zero since launch, which is a measurement failure independent of the traffic.

**Diagnosis.** Impressions have fallen almost as far as clicks, and the indexed new-URL count has
risen by far less than the old count has fallen. This is not a ranking wobble.

**Verdict: broken, and fixable.** Priority order is the bare-domain rules and the loop, then the
missing 189 URLs from the union, then collapsing the universal two-hop chain, then re-pointing the 84
home-page redirects, then resubmitting an old-URL sitemap to force recrawling. Analytics goals get
fixed the same day because nothing else can be evaluated until they are. The template and structured
data work waits, because adding another variable now would repeat the original mistake.

## Failure modes

**Bundling.** URLs, templates and content in one release, so the outcome has no attributable cause and
no revert path. The most expensive mistake here, and it is made in a planning meeting.

**Building the map from the sitemap.** It describes intent rather than reality, and the URLs it misses
are disproportionately the ones with external links.

**Redirecting the remainder to the home page.** Feels tidy, is treated as a soft 404, and loses both
the user and the signal.

**Accidental chains.** Nobody writes a two-hop redirect. The platform's own canonicalisation adds one
to every URL, and it is invisible unless somebody actually fires requests at the server.

**Testing the map by reading it.** Rule order and regular expression behaviour are not reliably
predictable by inspection, and the variants nobody tested are the ones that 404.

**Leaving internal links pointing through redirects.** Works perfectly until the redirect rules are
cleaned up, at which point the whole internal link graph breaks at once, usually months later and
attributed to something else.

**Judging the migration in week one.** Crawling lags, indexing lags, and a week-one number causes
panic changes that destroy the ability to interpret week four.

**Retiring the old domain or its certificate too early.** Redirects that stop working a year later
delete every signal they were preserving, and it is nobody's job to renew a domain the company no
longer uses.

**No named decider for rollback.** Guarantees the migration is not rolled back regardless of what
happens, because forward always feels like progress.

## What this skill does not do

- It cannot execute or test a redirect. Every claim about what a rule does is provisional until the
  map has been fired at a real server and every hop recorded.
- It has no access to your analytics, search performance data, logs or backlinks, so it works from
  what you supply and will name the sources missing from the union rather than fill them in.
- It does not judge content equivalence between two merging sites, which is the decision that carries
  the most risk in a consolidation and needs somebody who has read both pages.
- It gives shapes rather than thresholds for a normal dip, because the honest numbers depend on site
  size, crawl rate and how much else changed in the same window.
- It does not cover the non-search half of a migration: paid campaign destination URLs, email
  templates, app deep links, partner integrations and printed materials all point at old URLs too.
- It changes nothing and deploys nothing. Server rules, DNS, certificates and the change of address
  signal all belong to somebody with access this does not have.
