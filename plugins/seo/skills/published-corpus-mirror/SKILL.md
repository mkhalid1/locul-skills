---
name: published-corpus-mirror
description: Builds and maintains a local, greppable mirror of every article a site has actually published, one file per article per language, with a fixed frontmatter contract, an extracted heading outline, extracted internal links, and a derived index. Covers the escape and unescape ordering that keeps multi-line values on one line, the one-directional sitemap reconcile that surfaces drift without ever deleting, a hardened sitemap parser, and the second independently sourced snapshot that reveals when the mirror has quietly become fiction. This skill should be used when building or repairing a local content corpus, when downstream coverage or linking checks keep reporting that nothing is synced, or when nobody can say with confidence which articles are live.
---

# Published corpus mirror

## The claim this skill is built on

The obvious approach is to treat the content system as the source of truth and query it whenever something needs to know what has been published. That is correct in principle and it fails in practice for three separate reasons.

It is slow and rate limited. Every downstream question, coverage against a keyword set, near-duplicate detection, internal link structure, locale gaps, wants to read the whole corpus, and each of those reads is dozens of paginated calls that mostly return the same records again.

It is not greppable. A question like "which articles already contain a heading about refund windows" is one command against a directory of files and an afternoon of API work against a content system.

And the reason that actually matters: **a content system knows which records exist. It does not know which pages are live.** A record can sit in draft, be created but never routed, be published under a slug the router does not serve, or be served and blocked from indexing. The set of records and the set of reachable pages are different sets, and every interesting question is about the second one.

So the artefact is two things, not one. A local file per published article, sourced from the content system. And a separate, independently sourced snapshot of what the public site actually serves. The first is convenient. The second is the only thing that can tell you the first is wrong.

## Step 1: one file per article per language

The filename rule is exactly `<slug>.<lang>.md`, with the language code as a middle segment rather than a directory.

One file per language rather than one file with locale sections, because a missing locale then becomes a missing file, and a missing file is countable with a directory listing. Locale coverage stops being a query and becomes arithmetic.

The slug leads because it is the join key. Every other artefact in the pipeline, a backlog row, a link target, a sitemap entry, a redirect map, is keyed on slug, and a filename that leads with a date or an internal identifier forces a lookup at every join.

## Step 2: the frontmatter contract, in fixed key order

Every file carries the same keys in the same order:

```
id, slug, url, language, title, focus_keyword, meta_title,
meta_description, published_at, updated_at, word_count, source,
outline, internal_links
```

The order is fixed and this is not cosmetic. These files are regenerated. If the writer emits keys in whatever order the source object happened to iterate, every regeneration produces a diff on every file, and a real change, a retitled article, a rewritten description, becomes invisible inside three hundred lines of reordering. A stable key order means the version history of the mirror is a readable record of what actually changed on the site.

`outline` holds **every heading in document order**, each prefixed `h2: ` or `h3: `. This is the field that earns its space. With it, matching a list of unanswered questions against what the corpus already covers is a text search. Without it, the same question requires fetching and parsing every body, and so it never gets asked.

`source` records which system the record came from and which mode wrote the file. When two mirrors disagree, the first useful question is which one was written by a backfill and which by an append, and a file that does not record that cannot answer.

## Step 3: escaping, and why the unescape order is reversed

Real metadata contains line breaks. Meta descriptions written in a text area, titles pasted from a document, excerpts assembled by a template: all of them arrive carrying `\r\n`, `\n` or a stray `\r`. Written into frontmatter unescaped, the value ends mid-file, the next line is parsed as a key, and every field after it is wrong. The file still parses. It is simply no longer describing the article.

Escape every scalar in this order:

1. Replace `\` with `\\`
2. Replace `"` with `\"`
3. Collapse `\r\n`, `\n` and `\r` to the two-character sequence `\n`

The order is forced. If you escape quotes before backslashes, the backslash you just introduced in `\"` is then doubled into `\\"`, which reads back as a literal backslash followed by an unescaped quote, and the scalar ends early.

Unescape in the reverse order:

1. Replace `\"` with `"`
2. Replace `\n` with a real newline
3. Replace `\\` with `\`

**Undoing the doubled backslash last is the rule that matters.** Take a value that ended with a backslash immediately before a line break, which is ordinary in a description assembled from a template. Stored, it is `line one\\nline two`. Unescape the doubled backslash first and you get `line one\nline two`, and the newline step then reads that as a line break, producing `line one` and `line two` with the backslash silently eaten. Unescape it last and the newline step consumes the second backslash and the `n` together, leaving `line one\` followed by a real newline, which is what was there.

Be honest about the residual. Sequential replacement is still ambiguous for a value containing a literal backslash immediately followed by the letter `n`, such as a Windows-style path fragment written into an excerpt. No ordering fixes that case. If the corpus can contain such values, replace the three-step unescape with a single left-to-right scan that consumes the escape character and the character after it together, which is unambiguous by construction and about fifteen lines of code.

## Step 4: read the source defensively

Content APIs rename fields between versions, and platforms disagree about casing. Read every field through an alias list and fall back to empty rather than raising:

- identifier: `id`, `_id`, `article_id`
- body: `body`, `content`, `markdown`
- meta title: `meta_title`, `metaTitle`, `seo_title`
- publication time: `published_at`, `publishedAt`, `published_date`
- length: `word_count`, `wordCount`, otherwise compute it from the body

A mirror that crashes on one unexpected record stops running entirely, and a pipeline that stops running is discovered weeks later. A mirror that writes one file with an empty `meta_title` is visibly wrong in a way a text search finds in seconds. Prefer the visible failure.

## Step 5: extract internal links, including bare slugs

Match markdown link targets with a pattern equivalent to `\]\((https?://[^)]+|/[^)]+)\)`, keep the ones on your own domain, and reduce each to a path.

Then accept **two** shapes of article path, not one:

- a prefixed path such as `/blog/<slug>` or `/<locale>/blog/<slug>`
- a **bare single-segment path** matching `[a-z0-9][a-z0-9-]*`

The bare-slug branch is the one that gets left out, and leaving it out is not a partial failure. On a site that serves articles at the root, requiring a `/blog/` prefix drops one hundred per cent of internal links. Every file then carries an empty `internal_links` list, the link audit downstream reports a corpus of orphans, somebody spends a week adding links that already existed, and nothing in the run ever errored.

Guard the bare branch: exclude known non-article single segments such as `pricing`, `about`, `contact`, `login` and anything in the link target registry, or you will record navigation links as article links and inflate every count.

## Step 6: three modes, and what each is for

**`--full`**, a one-time backfill. Paginate the content API to exhaustion with a **hard guard of 200 iterations**, then rebuild the index. The guard is a working default, not a law: at a typical page size of 50 records it covers 10,000 articles, which is comfortably past where a directory of files is the right store anyway. Set it from your own page size, and treat hitting it as an error to report rather than a limit to raise, because the usual cause is a cursor that stopped advancing rather than a corpus that grew.

**`--append-slug <slug>`**, called immediately after each publish is verified. One record, one file, index rebuilt. This is what stops the mirror lagging a full publishing cycle, and it costs one API read.

**`--reconcile`**, the periodic correction, described next.

## Step 7: the sitemap is the authority on what is live, and the diff runs one way

Fetch the public sitemap. Extract the slugs. Compute two sets.

**`live_slugs - local_slugs`**: pages the site serves that the mirror does not have. Pull each one from the content system and write its file. This direction is safe, mechanical and unattended.

**`local_slugs - live_slugs`**: files the mirror holds that the sitemap does not list. **Report these. Never delete them.**

The temptation to make the diff symmetrical is strong, because a two-way sync feels complete and a one-way sync feels half finished. Resist it. A sitemap goes missing for reasons that have nothing to do with the article: a regeneration job that half completed, a caching layer serving a stale copy, a plugin that quietly caps the file at a thousand entries, a locale sitemap dropped from the index during a template change. Every one of those is temporary. Deleting on that signal is permanent. The asymmetry is deliberate: the cheap error is a stale local file that a human looks at, and the expensive error is destroying the only local copy of a record you no longer have credentials to fetch.

## Step 8: harden the sitemap parse

Two rules, and both are about a parser meeting input it did not expect.

**Refuse to XML-parse a document containing a `DOCTYPE` or `ENTITY` declaration.** Fall back to a plain regex sweep for location elements instead. A sitemap is a document fetched from a URL, which makes it untrusted input, and an XML parser that resolves external entities will happily read local files or open network connections on the document's instructions. Most language standard libraries have carried this behaviour by default at some point in their history. The fallback costs a few lines and removes the class of problem entirely. If your platform offers a hardened parser that disables entity resolution outright, use that and keep the regex sweep for the second case.

**Fall back to the same regex sweep on any parse error**, and anchor the slug pattern at both ends. Sitemaps arrive truncated, gzipped without a content encoding header, wrapped in a sitemap index rather than a URL set, or served as an HTML error page with a 200 status. A parser that raises on all of these produces an empty live set, and an empty live set makes the reconcile a no-op that **reports success**. That is the worst outcome in the whole procedure, because it is indistinguishable from being perfectly in sync. Treat an empty live set as an error, always, and say so in the summary.

## Step 9: the index is derived, so regenerate it and never mutate it

Emit exactly:

```json
{
  "site_generated_at": "<timestamp>",
  "count": 0,
  "articles": [
    {"id": "", "slug": "", "url": "", "language": "",
     "focus_keyword": "", "published_at": ""}
  ]
}
```

Build it by scanning the directory from disk, every time, with no incremental path and no append. The rule is absolute because the failure it prevents is silent: an index that is appended to drifts by exactly one row when a write fails halfway, and a `count` of 43 sitting beside 44 files on disk looks exactly like a count. Nothing in a downstream reader can detect it. Regeneration from disk makes the index a projection of the files rather than a parallel record that can disagree with them.

`site_generated_at` is the **corpus snapshot date**, and every downstream reader records it in its own output. A coverage report that does not say which snapshot it read is unfalsifiable, and six weeks later nobody can tell whether it was wrong or simply old.

## Step 10: keep a second, independently sourced snapshot

Keep one more file, produced by a different route: a crawl or a sitemap sweep of the live site, written without consulting the content system at all.

This is the only thing that can detect the failure the mirror cannot see about itself. On one real site, the set of articles the operator believed were published and the set the site actually served overlapped on four slugs out of forty-nine. Every local file was well formed. The index was consistent. The mirror was fiction, and no check that read only the mirror could have said so. That figure is one site's history and not a rate to expect anywhere else, but the shape of the failure generalises: a mirror built from one source can only ever be self-consistent.

Compare the two snapshots on every reconcile and report the overlap as a number. A falling overlap is the earliest available signal that the publishing path has broken.

## Decision rule: a slug appears in one set and not the other

1. In the sitemap, not on disk. **Pull it.** No judgement required.
2. On disk, in the sitemap, fields differ. **Rewrite the file** from the content system and note the changed keys.
3. On disk, not in the sitemap, and the content system still returns the record as published. The sitemap is probably stale. **Keep the file**, flag it as `sitemap-missing`, and re-check on the next run. Two consecutive flags escalate to a human.
4. On disk, not in the sitemap, and the content system returns nothing or a non-published state. The article was genuinely removed. **Still do not delete.** Mark the file `withdrawn` in its `source` field and leave it in place, because a withdrawn article is exactly the record you want when somebody asks in March why that URL now 404s.
5. **You cannot tell.** The sitemap fetch failed, returned an empty set, parsed as HTML, or the content system is rate limiting and returning partial pages. Do nothing to any file. Report `reconcile aborted` with the reason and the counts observed. An aborted reconcile that says so is recoverable on the next run. A reconcile that treats an unreachable sitemap as an empty site is a deletion event waiting for somebody to have enabled deletion.

## Worked example, compressed

A documentation site for a fictional billing service, two languages, believed to hold 49 articles.

**Backfill.** `--full` paginates the content API. Field names do not match the obvious ones: identifiers arrive as `_id` and bodies as `content`. The alias list absorbs both. Pagination stops after eleven iterations, well inside the 200 guard. 61 files are written, not 49, because a second locale had been published and never recorded anywhere.

**Escaping.** Four meta descriptions contain line breaks. One title ends in a backslash before its break. Escaped in order and read back with the doubled backslash undone last, all four round-trip intact. A spot check reading them back with the naive order shows two files where the frontmatter silently ends three keys early.

**Links.** The site serves articles at the root. The first extraction pass, prefixed paths only, finds zero internal links across all 61 files. The bare-slug branch is added with a navigation exclusion list, and the same corpus yields 214 links. Nothing about the first result looked like a failure.

**Reconcile.** The sitemap parses cleanly and lists 44 URLs. `live - local` is empty. `local - live` is 17. Rule 3 applies to all of them: the content system still reports them published. They are flagged, not deleted, and the run reports `17 sitemap-missing`.

**Second snapshot.** An independent sweep of the live site confirms 44 reachable URLs. The overlap between the believed corpus and the served corpus is 44 of 61. The gap is real and it is in the publishing path, not in the mirror.

**Index.** Regenerated from disk: `count` is 61, matching the file listing exactly. `site_generated_at` is stamped and quoted in the run summary.

**Verdict: the mirror is complete and the site is not.** Seventeen articles exist as records and are not served. Nothing was deleted, the seventeen are named, and the next run will tell you whether they were a sitemap problem or a routing problem, because the flag persists and a second consecutive flag escalates.

## Failure modes

**Index stale by one.** The index says 43, the disk holds 44, and every downstream count is quietly wrong. From the outside it looks like a corpus that is slightly smaller than people remember. Caused by mutating a derived index instead of regenerating it, and undetectable from inside the index itself.

**Frontmatter corrupted by an unescaped newline.** A file that parses without error and describes the wrong article from `published_at` onwards, because a meta description ended the scalar early and every subsequent line was read as a key. The article looks present. Its metadata is another article's.

**The eaten backslash.** Values that lose a trailing backslash and gain a line break on every read-write cycle, so the file changes every time it is regenerated and nobody can find the edit that caused it. Caused by undoing the doubled backslash before the newline sequence.

**One hundred per cent link loss from a missing bare-slug branch.** Every article records zero internal links, the link audit reports a site of orphans, and a team spends a sprint adding links that were always there. The extractor never errored, because zero matches is a legal result.

**The silent no-op reconcile.** The sitemap is unreachable, or returns an HTML error page with a 200 status, so the live set is empty, so nothing needs pulling, so the run **reports success**. Repeats daily. The corpus falls further behind every day and the summary is green every time.

**Never backfilled.** Downstream checks all report "corpus not synced" from their first run, the message is correct but useless, everybody learns to ignore it, and by the time the corpus is populated nobody reads those reports any more.

**Two-way deletion on a temporary sitemap gap.** A sitemap regeneration job half completes, the reconcile treats the missing entries as removed articles, and local files are deleted for pages that are live. Recoverable only if someone kept a backup, and the run that did it reported a clean sync.

**A self-consistent fiction.** Every file well formed, every count internally consistent, and a live site serving a fraction of it. Invisible to any check that reads only the mirror, which is why the second independently sourced snapshot exists at all.

## What this skill does not do

- It does not crawl. It needs read access to the content system or an export, plus a reachable public sitemap. Producing either is somebody else's job.
- It cannot tell you whether a page is indexed, ranking or receiving traffic. It answers one question, which is what exists and what is served, and answering that question well is the entire scope.
- It does not judge quality, duplication or coverage. Those are separate checks that read this corpus, and it deliberately holds no opinion about the content it mirrors.
- It never deletes anything, which means the mirror grows monotonically and will accumulate withdrawn articles. Pruning is a manual decision made by a person who can see why a URL went away.
- It does not repair the publishing path. When the second snapshot shows that seventeen records are not being served, this tells you the number and stops. Finding out why is a routing and indexation problem with different tools.
- It has no scheduler. Something else has to run it, and if that something stops, every downstream report keeps producing confident output against a frozen snapshot.
