---
name: link-target-registry
description: Builds the curated allowlist of real non-article URLs a writer or an automated run may link to, grouped into tables by page class with a routing column that says when to link each one, and sorted into three honesty buckets: confirmed live, likely live but verify first, and an explicit list of pages that do not exist. Includes an external section that keeps off-site URLs out of the internal link quota, positioning bans on paths that exist but must not be linked, dated liveness flags, and an exact-match validation rule covering anchor fragments. This skill should be used before a content programme starts producing pages, after a navigation redesign, or when link checks keep surfacing 404s on plausible-looking paths nobody deliberately invented.
---

# Link target registry

## The claim this skill is built on

The obvious approach is to tell writers to link only to pages that exist. Everyone agrees, and it does not work, because the failure is not disobedience. It is confidence.

Consider the paths a documentation site for a project management tool would obviously have: `/integrations`, `/pricing`, `/templates`, `/security`, `/docs/api`, `/use-cases/agencies`, `/compare/alternatives`. Half of those probably exist. The other half are the same shape, read identically in a sentence, survive every editorial review, and 404. Nobody invented them in the sense of making something up. They inferred them, correctly in form and incorrectly in fact.

So the artefact is not a rule. It is a list, and the list has to be honest in a specific way that most inventories are not: **it must say which pages do not exist**, because in a plain allowlist an absence is ambiguous, and a writer under time pressure resolves ambiguity in favour of the link.

One scope boundary before anything else. This registry covers **non-article URLs only**: product pages, tools, hubs, use-case pages, comparison pages, utility pages. Article slugs live in the published corpus index and are validated against that instead. The two sets are validated separately and a writer does not distinguish between them at all, which is why both must exist before validation means anything.

## Step 1: crawl the live navigation, not the site map in someone's head

Start from the rendered navigation of the live site: header, footer, and any hub or index page linked from either. Follow one level down from each. Record for every URL found:

- the exact path, including any trailing slash the site actually serves
- the status code
- the page title
- whether the URL was reached through a redirect, and from where

The redirect column is the one people leave out. A path that returns 200 after a hop is a working link and a slightly wrong one, and it is the single most common way a registry stays technically accurate while every link it authorises passes through a redirect.

Do this from a crawl rather than from a content management system's page list. The page list tells you what records exist. The crawl tells you what is served, and those diverge for exactly the reasons that make this file necessary.

## Step 2: group into tables by page class, and add the third column

Group the inventory into sections, each a table:

- Product and feature pages
- Free tools and calculators
- Use-case and persona pages
- Topic hubs and index pages
- Comparison pages, including any `#anchor` fragments that are linkable in their own right
- Resource and utility pages, meaning pricing, security, changelog, status, contact

Every table gets **three** columns, and the third is the one that does the work:

| Path | What it is | Link from here when the topic is... |

The third column is the routing logic. Without it, a writer with a list of forty valid URLs picks whichever one is nearest the top, and a registry becomes a source of technically-valid irrelevant links. With it, the choice of target is a lookup rather than a preference. Write the third column as topic conditions, not as descriptions: "the reader has already chosen a tool and is asking how to connect it" routes; "our integrations page" does not.

## Step 3: three honesty buckets

Every row belongs to exactly one of three states, and the labels are part of the file rather than a review process.

**Bucket 1: confirmed, safe to link.** Verified live, status recorded, date recorded. This is the only bucket an automated run may link from without further checks. It is normally much shorter than the site feels, and that shortness is accurate rather than a gap to fill.

**Bucket 2: likely live, verify before linking.** Candidate anchors whose fragment identifiers are inferred rather than observed, paths seen in a menu but never fetched, pages behind a login. Each row says **why** it is uncertain, in a short phrase: "fragment identifier is a guess, confirm on the live page", "seen in the footer, never fetched", "requires authentication, unverified for logged-out readers".

**Bucket 3: pages not confirmed to exist. Do not invent and do not link.** An explicit list of the obvious paths a writer would assume are real. Each row carries one line saying it is unverified rather than overlooked.

Bucket 3 is the part that makes the file different from an inventory, and the reasoning is worth stating plainly. In a two-bucket registry, a path's absence carries no information: it might not exist, or it might simply not have been checked yet. A writer who wants to link `/integrations` and cannot find it in the list has no way to distinguish those, and the cost of guessing wrong feels low. Once the path is written down under a heading that says it does not exist, the guess is no longer available. Absence is converted into a statement.

Keep bucket 3 populated by the same method that fills it initially: whenever anyone proposes a link that turns out not to exist, the path goes into bucket 3 rather than being quietly deleted from the draft. It is the only bucket that grows from mistakes, and it should.

## Step 4: dated liveness flags on individual rows

Some rows are true on a date and not before or after. Flag them in the row itself, with the date:

- `not in the public index yet, do not link until confirmed`
- `live within 24 hours of <date>, verify before linking`
- `scheduled for retirement <date>, stop linking now`

The dates matter because a registry is read months after it is written, and a row saying "coming soon" with no date is indistinguishable from a row that was accurate two years ago.

The specific failure this prevents is the announced page. Somebody says a feature page ships next week, links start being written against its future path, the launch slips, and forty articles carry a link to a page that will exist eventually, which means they carry live 404s in the meantime and nobody schedules a re-check.

## Step 5: ban paths that contradict positioning, by name

Some pages exist, return 200, and must still not be linked from content. A legacy product line still served for existing customers. A pricing page for a tier being retired. A landing page built for one paid campaign whose copy contradicts current positioning. A regional page that would send readers to the wrong entity.

List these by name in their own short section with one line each on why. If they are simply absent from the registry, somebody will find them in the navigation, notice they are real, and link them in good faith. A ban that is not written down is a preference held by one person.

## Step 6: the external section, which exists to protect a count

The final section is headed **External, do not treat as internal links** and lists off-site URLs that belong to the same organisation and are therefore constantly mistaken for internal ones: an application store listing, a review platform profile, a status page on a different domain, a newsletter archive, social profiles, a community forum on a subdomain.

This section is not documentation. It has one job: keeping those URLs out of the internal link quota. If your writing specification asks for three to six internal links per article, and a run counts an application store listing among them, the article ships with two real internal links and a compliant-looking count. The check passes. The linking does not happen. Making the exclusion explicit means the miscount is caught in the registry rather than in a link graph audit six months later.

## Step 7: the validation rule

Before any page is published, every link in it is checked, and the rule has no discretion in it.

**Every non-article internal link must match a registry row exactly: path and anchor fragment.** Not "starts with". Not "close enough". Exact.

The fragment is included because it is the failure that no status check catches. A link to `/compare#versus-spreadsheets` returns 200 whether or not that fragment still exists on the page, because a fragment is resolved by the browser and never sent to the server. When a content edit renames the heading, the link keeps working in every automated sense and silently starts dropping the reader at the top of the page.

**Every article link must match a slug in the published index.**

**If a target is in neither, do not invent it.** Three permitted responses, in order of preference:

1. Link the nearest confirmed page from bucket 1, chosen by the routing column.
2. Drop the link and leave the sentence intact.
3. Escalate the path by name so it can be crawled and added to a bucket.

Dropping a link is a completely acceptable outcome. An article with four good links is better than one with five where the fifth 404s, and the instinct to preserve a link count is what produces invented URLs in the first place.

## Step 8: pair the registry with a CTA routing table

Most content specifications ask for exactly one call to action per page, which turns "which page do I link" into a decision made repeatedly, under mild pressure, by whoever is finishing the draft. Make it a lookup keyed on the article's intent:

| Article intent | CTA target |
| Informational, explaining a concept | Features or capability page |
| Comparison or alternatives | Conversion page |
| How-to or implementation | Conversion page |
| Role, industry or use-case | Use-cases page |
| Pricing-curious | Pricing page |
| Objection or scepticism | FAQ or a documentation page |

Every target in that table must itself be a bucket 1 row. A routing table pointing at an unverified page is worse than no routing table, because it authorises the link and removes the decision at the same time.

The specific pairings above are working defaults from one operation rather than a general law. What transfers is the shape: one CTA, chosen by intent, from a closed list of verified pages, so nobody picks by feel at the end of a long draft.

## Step 9: re-crawl triggers, and the closing principle

Re-crawl on events rather than on a calendar, because a calendar interval is always wrong in one direction:

- any navigation change, header or footer
- any launch or retirement of a product or feature page
- any URL structure change, including a trailing-slash or locale-prefix change
- a link checker reporting more than a handful of internal 404s
- a platform or theme upgrade, which is the one nobody associates with URLs and which quietly changes them

Then a floor of roughly every 90 days regardless, which is short enough that a missed event costs one quarter of output and long enough that re-crawling does not become somebody's routine.

The closing principle, and it decides most of the arguments about this file: **keep the inventory honest and small rather than broad and complete.** A registry of eighteen verified rows is more useful than one of a hundred and forty where nobody knows which are still true, because the first one gets trusted and the second one gets bypassed.

## Decision rule: a draft contains a link to a path that is not in the registry

1. The path is in bucket 3. **Reject it.** It is a known non-existent page. Use the routing column to pick a bucket 1 replacement, or drop the link.
2. The path is in bucket 2. **Fetch it now.** A 200 with the fragment present promotes it to bucket 1 with today's date, and the link is allowed. Anything else demotes it to bucket 3.
3. The path is in no bucket at all and the environment can fetch it. **Fetch it.** Add it to bucket 1 or bucket 3 based on the result, then apply rule 1 or allow the link accordingly. Record the date either way.
4. The path is an off-site URL. Move it to the external section, allow it if the link is genuinely useful, and **do not count it toward the internal link quota**.
5. **You cannot tell.** The environment has no network access, or the page sits behind authentication, or the fetch returns a status that means nothing useful such as a 403 from a firewall that also blocks ordinary readers. **Do not link it and do not add it to bucket 1.** Put it in bucket 2 with the reason and the date, drop the link from the draft, and name the path in the run summary. An unverifiable path is not a probably-fine path, and the asymmetry is deliberate: a dropped link costs one link, and a shipped 404 costs the reader, the crawler and whoever eventually has to find it.

## Worked example, compressed

A documentation site for a fictional expense management tool. Two writers, roughly thirty non-article pages, a link checker reporting a steady trickle of internal 404s.

**Crawl.** Header, footer and two hub pages, one level down. Forty-one URLs found. Six of them arrive through a redirect, all from a locale prefix change nobody remembered.

**Grouping.** Six tables. The comparison table carries four `#anchor` fragments, each one observed on the live page rather than inferred.

**Buckets.** Twenty-two rows land in bucket 1. Nine land in bucket 2, seven because their fragments were read from a table of contents rather than from the rendered heading identifiers, two because they sit behind a login. Bucket 3 is seeded with the eleven paths the link checker had already reported as 404s, and every one of them is a path a reasonable person would assume exists: `/integrations`, `/security`, `/templates`, `/api`, and eight more of the same shape.

**Bans.** Two live pages are banned by name: a legacy tier's pricing page and a campaign landing page whose headline contradicts current positioning.

**External section.** Five URLs, including an application store listing that had been counted as an internal link in nineteen articles, which is why those articles had been passing a five-link requirement with three real internal links.

**Validation over twenty drafts.** Sixty-three non-article link targets. Fifty-one match bucket 1 exactly. Seven match bucket 2 and are fetched: five promote, two demote to bucket 3. Four match bucket 3 and are rejected, of which three are repointed by the routing column and one is dropped because no confirmed page serves that topic. One target is a fragment that returns 200 while the fragment itself no longer exists, caught only because validation is exact-match including the anchor.

**Verdict: the registry ships at twenty-seven confirmed rows, seven to verify, and thirteen explicitly not real.** Four invented links were prevented before publication, one silent fragment break was found that no status check would ever have reported, and the internal link quota is now counted against a set that excludes the application store listing, which means three articles are correctly reported as short rather than incorrectly reported as compliant.

## Failure modes

**The plausible 404.** A link to a path that reads perfectly in the sentence, matches the site's naming conventions, and does not exist. It survives every human review because it looks exactly like the links around it, and it is found by a crawler weeks later or by a reader immediately.

**The silently dead fragment.** A link with an `#anchor` that no longer matches any heading. Returns 200 forever, never appears in any broken link report, and quietly drops every reader at the top of a long page instead of at the section they were promised.

**The coming-soon link.** Forty articles pointing at a page whose launch slipped. Every one of them is a live 404 for as long as the slip lasts, and nobody scheduled the re-check because the link was correct on the day it was written.

**The auto-fixer rewrite.** A tool that repairs broken internal links by pattern-matching against article slugs converts a marketing page path into an article path that happens to exist. The link now resolves, the report is clean, and the reader is sent somewhere nobody chose.

**External URLs inside the internal count.** Articles passing a link quota while carrying two real internal links and three off-site ones. The specification is being met numerically and defeated in substance, and only an explicit external section makes it visible.

**The stale registry after a redesign.** A navigation change moves or retires a dozen pages. Nothing about the file changes, because a file does not know. A month of output points at redirects and 404s, and the registry is still being cited as the authority the whole time.

**The broad and broken inventory.** A hundred and forty rows, undated, of which nobody knows which are current. Writers stop consulting it because checking a row costs as much as checking the site, and the registry becomes documentation of a former structure.

**The two-bucket registry.** No explicit list of pages that do not exist. A writer looks for `/integrations`, does not find it, reads the absence as an oversight rather than a fact, and links it. This is the failure the third bucket exists to remove, and it recurs with every new writer until the list is written down.

## What this skill does not do

- It covers non-article pages only. The majority of a mature site's internal links point at articles, and those are validated against a published corpus index that this does not maintain.
- It cannot see redirects unless the crawl recorded them. A row that returns 200 through two hops looks identical here to one that returns 200 directly, and only the crawl column distinguishes them.
- It cannot keep anchor fragments current. A fragment depends on a heading identifier that any content edit can change without touching a URL, and no status code will ever report that as broken.
- It does not judge whether a link is worth making. Exact-match validation proves a target exists. A page can satisfy every rule here while carrying six links no reader would follow.
- It has no schedule of its own and goes stale on somebody else's timetable. Its accuracy is entirely a function of when a person last re-crawled, and nothing in the file will tell you that the answer is eight months.
- It does not fix existing pages. Links already published against invented paths stay broken until something else finds and repairs them, and this only prevents the next batch.
