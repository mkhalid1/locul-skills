---
name: traffic-drop-forensics
description: Diagnoses why organic traffic to a site or a section fell, in an order that prevents the wrong fix. Establishes that the drop is real against comparable windows, separates a ranking loss from an impression loss from a click-through loss, verifies what a crawler actually receives, then works through a catalogue of defects that are invisible in a browser: cached not-found pages, dead metadata fields, blended position averages, crawler blackouts from firewall rules, synthetic freshness dates, sitemap bloat and half-finished internationalisation. This skill should be used when organic traffic has fallen and nobody yet knows why, or when a page that used to rank has quietly stopped.
---

# Traffic drop forensics

## The claim this skill is built on

Most traffic investigations fail in the first ten minutes, and they fail the same way. Somebody
opens the affected page in a browser, sees a perfectly good page, concludes there is nothing
mechanically wrong, and moves straight to content and algorithm speculation.

A browser is the worst available instrument for this. It sends the headers the site expects. It
follows redirects without telling you. It runs the JavaScript. It is served from a cache that may
have been populated on a different day, by a different code path, than the one that produced the
response a crawler received. Every defect in the catalogue below renders as a perfectly good page
for the person looking at it, and several of them render as a perfectly good page while the page
is, as far as any search engine is concerned, gone.

So the method is: prove the fall is real, classify which of three different losses it is, prove
the page still exists for a crawler, and only then go looking for a mechanism. The order is not a
style preference. Each step invalidates work you would otherwise do in the wrong direction.

## Step 1. Establish the drop is real before you explain it

- **Compare like windows.** Twenty-eight days against the previous twenty-eight, and the same
  twenty-eight a year earlier. Never against the all-time peak, which is by construction the most
  favourable point that ever existed and guarantees the answer is always "traffic is down".
- **Align the days of the week.** A window that starts on a different weekday to the one it is
  compared against shifts the count on a site with any weekday pattern, which is most B2B sites.
- **Allow for reporting lag.** The most recent two to three days in most reporting are incomplete.
  A fall that exists only in the last forty-eight hours is usually the pipeline, not the site.
- **Check whether the category fell.** If every page in a section fell by a similar proportion on
  the same day, that is one cause, not fifty content problems. If demand for the query set fell in
  step, there may be nothing to fix, and saying so is a real finding.
- **Read the deploy log before speculating.** What shipped that day, that week, and to which
  templates. Infrastructure and template changes are the cheapest hypotheses to test and the most
  common causes of a sharp, sectional fall.

**Stop rule.** If the fall is inside normal weekly variance for the property, roughly under fifteen
per cent on a site with ordinary volatility, and no single day carries a step change, report that
there is no drop to explain and stop. Investigating noise produces confident fiction.

## Step 2. Classify the loss: ranking, impression, or click

Three failures wear the same clothes on a traffic chart. They have different causes and the fixes
actively conflict.

- **Impressions flat, position flat, clicks down.** A presentation change. Either the listing lost
  its appeal, or something above it started answering the question. The lever is the title and
  description, or accepting the loss.
- **Impressions down, position flat.** Fewer queries matched, or the query set itself shrank. This
  is demand, an intent shift, or a results-page feature taking the space. Rewriting the page does
  not create demand.
- **Position down, impressions down, clicks down.** A genuine ranking loss. Now content, links and
  competition are relevant.
- **Everything collapsing towards zero on one date.** Not a ranking problem at all. Go to Step 3
  immediately, because this is delivery.

**Why the order matters.** The instinctive response to a poor click-through rate is a punchier
title. If the real problem is a ranking loss, and the punchier title drops the term the page was
matching on, you convert a position you could have recovered into one you cannot. Classify first.

## Step 3. Prove the page still exists for a crawler

Skipped more often than any other step, because the page is right there on the screen.

Fetch the URL the way a crawler would, without your session, without your cache, without your
headers, and record: the status code, the full redirect chain, whether the initial HTML contains
the body copy or only a shell, the `X-Robots-Tag` response header, the robots meta tag, the
canonical, and the status code of `/robots.txt` itself. Read that last status code carefully,
because the two failure families behave in opposite directions and it is very commonly reported
backwards. **A 4xx on `/robots.txt`, other than 429, is treated as though the file does not exist,
which means no crawl restrictions at all.** It does not stop crawling. What it does is silently
discard every directive you wrote, so anything you had disallowed becomes fair game, and it is a
loud symptom that whatever returned it is very likely returning the same thing on your content
URLs. **A persistent 5xx, or a 429, is the case that can genuinely suppress crawling of the host.**
Google's own guidance says explicitly not to use 401 or 403 to limit crawl rate, for exactly this
reason. Verified against the published robots specification, August 2026.

## The catalogue: defects that render perfectly

### 1. A transient error cached as a permanent not-found

**Mechanism.** Page data is fetched during a build or a regeneration. The fetch helper returns
null on any response that is not ok, which folds a timeout, a rate limit and a gateway error into
the same value as a genuine missing record. The framework turns null into a not-found page. The
incremental regeneration layer then writes that not-found into the cache and serves it to every
visitor. The page was fine, the upstream recovered ten minutes later, and the page is now
permanently gone, because a cached not-found is frequently not scheduled for revalidation.

**Tell.** A cluster of pages that returns not-found while the underlying records still exist in
the content source. No deploy removed them. The affected set correlates with a window of upstream
instability rather than with any content change.

**Fix.** Throw on any error that is not a genuine not-found, so the regeneration fails and the
last good cached page keeps being served. Only produce a not-found page when the upstream actually
said the record does not exist. Add the rule that a route which previously returned 200 never has
a not-found written to its persistent cache, and purge the poisoned entries by hand.

### 2. A field that can never render because the fallback is backwards

Writing the general value first and the specific override second, as in `title || meta_title`,
means the second value can never be reached. Nothing errors, no test fails, and an entire
optimisation field is dead across every page on the site.

**Tell.** Export the rendered value and the stored value for two hundred pages. If the rendered
value equals the fallback on one hundred per cent of rows while the override column is populated
on hundreds of them, the operands are the wrong way round.

**General rule.** For every field with a fallback, verify at least one live page where the two
sources differ and confirm the override is what renders. A field nobody has ever seen render is
indistinguishable from a field nobody filled in.

### 3. The position averaging illusion

When a page is cited in an answer summary at the top of the results and also carries an ordinary
organic listing far down, a reporting tool blends the two into a single average position that
describes neither state. The page is either extremely visible or effectively invisible depending
on which listing you mean, and the average is a place the page has never been.

Acting on that average produces a confident wrong diagnosis, and the mechanism is worth stating
plainly. When the blend mixes an answer-box citation with an ordinary listing far down the results,
the queries with the worst click-through look like title problems and are almost always ranking
problems instead, because the title is never seen at a position anyone clicks. The most clickable
title on the site earns close to nothing from a listing on the second page. Split the two
placements before you judge the copy, because until they are split the copy is not what the number
is measuring.

**Rule.** Never diagnose a click-through problem from an average position unless you have
confirmed the page has an ordinary organic listing in a clickable slot. Where the reporting tool
cannot separate the two surfaces, treat any average position in the middle single digits with
suspicion whenever the page is also being cited in summaries, and check three of the queries by
hand before touching any copy.

### 4. A crawler blackout caused by a firewall rule

**Mechanism.** A bot rule or a managed challenge requires something that ordinary browsers always
send and automated clients often do not: a particular client hint, a language header, a referer, a
cookie set by a challenge page. Crawlers do not send it, so they receive a forbidden response.
Frequently this includes `/robots.txt`, and the consequence there is the opposite of the intuitive
one: a 403 on the robots file is read as no robots file, so your directives vanish rather than your
crawl budget. The damage is on the content URLs, which return 403 and therefore cannot be indexed
at all, while the robots failure quietly removes whatever protection your disallow rules gave you.

**Tell.** Coverage falls across the entire site at once, starting on a date that matches an
infrastructure change and matches nothing in the content calendar. Every human check passes. The
site is, from a browser, flawless.

**The test that cannot work.** Spoofing a crawler user agent in a command line request will always
fail, and it will fail whether or not the rule has been fixed. It fails because the same rule
usually blocks that request too, and because verified crawlers are identified by reverse DNS on
the requesting address rather than by the user agent string, so your spoofed request is not the
thing the rule is deciding about. A forbidden response to a spoofed fetch is not evidence. Anyone
using it as a test will conclude the fix did not work and revert a correct change.

**What to use instead.** The search engine's own live fetch and render tool, which requests from
its real infrastructure, and the server or CDN logs, which show whether requests from verified
crawler addresses are receiving 200 responses. Two sources, both real.

### 5. Synthetic freshness

A modified date wired to the deploy timestamp rather than to a content edit. Hundreds of pages
then declare that they were updated at the same instant, which is a clear signal that the date
carries no information about the content.

**Tell.** Sort every page by its declared modification date and look for a large cluster on one
timestamp, then check whether that timestamp is a release.

**Fix.** Store a genuine content-edited timestamp on the record, update it only on a substantive
edit, and let a rebuild leave it alone. If nobody has edited a page in two years, the correct
declared date is two years ago.

### 6. Sitemap bloat from thin taxonomy pages

Automatically generated tag, category, author, archive and pagination pages, most holding a single
item, all submitted for indexing. The submitted set stops being a statement about what matters and
becomes a list of everything the templating system can produce.

**Rule of thumb.** A taxonomy page earns a sitemap entry when it lists at least five items and
carries some text that is not the list itself. Everything else is excluded from the sitemap, and
the genuinely empty ones should not exist as URLs at all.

### 7. Half-finished internationalisation

Three defects that usually appear together, and the first one is the expensive one.

- **Translated pages set to no-index while their hub pages are indexable.** The hub is then an
  indexable page whose entire purpose is linking to pages that cannot be indexed.
- **No reciprocal language annotations.** A language annotation that is not returned by the page it
  points at is commonly ignored altogether, which means the whole set does nothing. Each page in a
  set must reference every member including itself, and a default should be declared.
- **Every page declaring the same language.** A template hard-coding one language attribute across
  a multilingual site contradicts the content on most of it.

### 8. Orphan and dead-end pages, and the stored-links trap

An orphan has no inbound internal links from the same site. A dead end has no outbound internal
links. Both are cheap to fix and both are commonly reported as absent by an audit that trusted the
wrong data.

**The trap.** An audit that reads a content system's stored internal-links field rather than
parsing the rendered body will report zero orphans on a site full of them. That argument, and the
parsing and normalisation rules that follow from it, belong to the Internal link graph audit skill:
take the graph from there and bring the orphan and dead-end sets back here as evidence.

## Prioritisation: hard gates first, then the score

Once you have a list of pages to work on, resist the urge to rank them by a single number.

**The gates. Any failure removes the page from scoring entirely.**

1. **Does the URL return 200 to a crawler?** If not, there is nothing to optimise. It is a repair
   job, and it goes to the top of a different list.
2. **Is there a clickable organic result pool at all?** If the query set is fully satisfied above
   the organic results, or the page has no organic listing anyone would ever reach, improving the
   page cannot produce clicks.
3. **Does the page lead anywhere useful?** A page with no route onward to a page that does
   something is traffic you cannot use. Fix the routing before you buy the traffic.

**The score, applied only to pages that pass all three gates.** Normalise each input to a range
from zero to one, then weight:

- Search volume for the page's query set, log-scaled: **0.30**
- Difficulty, inverted so that easier scores higher: **0.25**
- Staleness, by genuine last-edited date: **0.20**
- Missing or duplicated metadata: **0.15**
- Orphan status: **0.10**

**Why the gates come before the score.** A weighted score is a comparison between candidates, and
it assumes every input contributes something. A gate failure is not a low input, it is a
multiplication by zero: the expected return of the work is nothing regardless of how large the
volume figure is. Put that page into the score and its high volume will float it above healthy
pages with modest volume, and you will spend the quarter optimising a page that returns
not-found. Gates express facts that override the arithmetic. Scores express trade-offs among
options that are all genuinely available.

## Evidentiary discipline

The investigation is only worth as much as the report, and reports rot in predictable ways.

- **Every figure carries its source and its date window.** Not "clicks fell 38 per cent" but
  "clicks fell 38 per cent, performance export, 1 to 28 June 2026 against 4 to 31 May 2026". A
  number with no window cannot be checked later and will be repeated forever.
- **Three independent sources before a finding is called real.** The reporting tool, a direct
  fetch, and the server log are three. The reporting tool viewed three times is one.
- **Write down the hypotheses you disproved, and what disproved them.** Otherwise the next person
  spends their first day re-running them, and so do you in four months.
- **Keep four registers separate.** A decision that was taken and why. A bug that is broken and
  needs fixing. A proposal that nobody has agreed to. And a thing that is already done, with its
  date. Mixing a proposal into the decision list is how a document becomes a source of confident
  wrong facts about the site's own history.

## The decision rule

- Pages return anything other than 200 to a crawler. → **Delivery failure.** If `/robots.txt` also
  returns a 4xx, your directives are being ignored rather than your site being blocked, and the
  content status codes are the finding.
  Stop all other work. Nothing else can be assessed until pages are being served.
- Delivery is fine, position held, clicks fell. → **Presentation.** But confirm the position is a
  real position and not a blend across two surfaces before touching any copy.
- Impressions fell, position held. → **Demand or query-set change.** Check whether the fall
  tracks the category. If it does, the honest answer may be that nothing is broken.
- Position fell across an entire template or section on a single date. → **Template or
  infrastructure.** Diff every deploy in a three-day window around that date. Do not open the
  content brief.
- Position fell gradually across unrelated pages over weeks. → **Competitive or quality.** This is
  the only branch where content work is the first move.
- **You cannot tell.** → Do not guess, and do not pick the most interesting hypothesis. The
  cheapest disambiguation is two cheap tests run together: a fetch as a crawler on twenty affected
  URLs, and a segmented export split by page type before and after the step change. If the picture
  is still ambiguous after both, say the evidence does not yet support a cause and name the
  specific data that would settle it.

## Worked example, compressed

A documentation site, roughly four thousand pages. Clicks fell over four weeks and the team was
preparing to rewrite the guides.

**Is it real?** Twenty-eight days against the previous twenty-eight is down 38 per cent. The same
window a year earlier is down 34 per cent, so seasonality does not explain it. Segmenting by page
type shows the fall is almost entirely in one section: guides are down 71 per cent, reference is
down 3 per cent. That asymmetry alone rules out most sitewide explanations.

**Which loss?** In the guides section impressions fell in step with clicks and average position
barely moved. So this is not a click-through problem, and the title rewrite that was about to
start would have changed nothing.

**Does it still exist for a crawler?** Forty guide URLs sampled. Eleven return not-found. The
records for all eleven are present and complete in the content source, and no deploy touched them.
The deploy log shows an upstream content API incident lasting about ninety minutes on the second
day of the window, which is when those eleven pages happened to regenerate. Mechanism identified:
a failed fetch became a cached not-found.

**What else does the catalogue turn up?** Rendered titles across the whole guides template are
identical to the on-page headings, on every row of a two hundred page export, despite a populated
override field. Backwards fallback, an entire field dead. Separately, the section's poorest
click-through queries report an average position in the middle single digits, but three checked by
hand show the page cited in an answer summary while its organic listing sits deep on the second
page. That average was a blend, and it was the evidence the title rewrite was based on.

**Verdict.** One mechanism explains most of the loss, one sitewide field has never rendered, and
one planned piece of work was based on a number that describes nothing. Fix order: throw on
non-not-found errors and purge the poisoned cache entries, then correct the fallback operands,
then leave the titles alone and re-measure after two weeks.

## Failure modes

**Explaining a drop that is not there.** Given a fall to explain, an investigation will always
produce an explanation. Run the stop rule honestly.

**Comparing against the peak.** Any period compared against the best week that ever happened
produces a decline. It is arithmetic, not a finding.

**Diagnosing from the rendered page.** The browser sends your headers, follows your redirects and
runs the JavaScript. It cannot show you the response a crawler received, which is the response
that matters.

**Rewriting titles for a ranking problem.** The most common wasted quarter in this work. It also
risks removing the term the page was matching on.

**Trusting a blended average position.** Two surfaces averaged into one number, then treated as a
place the page sits. It is not a place.

**Testing a firewall fix with a spoofed user agent.** The test cannot pass. It will report failure
after a correct fix and get that fix reverted.

**Trusting the stored links field.** Reports a clean internal link graph on a site full of
orphans, because the field was populated once during an import and never again.

**Scoring before gating.** A high-volume page returning not-found will outrank a healthy page in
any weighted list, and the quarter goes into work that cannot pay.

**Numbers without windows.** A figure with no source and no date range cannot be checked, cannot
be reproduced, and will be quoted back at you for a year.

**Treating a template defect as many page defects.** One broken template is one fix and one ticket,
not four hundred. Reporting it as four hundred findings buries the actual cause.

## What this skill does not do

- It does not fetch anything. It has no access to your analytics, your coverage report, your rank
  tracker or your logs, and it will ask for exports rather than assume figures.
- It cannot name an algorithm update as the cause, and it treats timing correlation against a
  public announcement as the weakest evidence available.
- It does not read messages from search engines about manual actions, nor can it see whether a
  penalty exists. That has to be checked by a person with access.
- It does not measure page performance. It flags patterns known to hurt, but real numbers need a
  real measurement from a real device.
- It cannot see your firewall configuration, your CDN rules, or your cache invalidation state, so
  it can describe the mechanism and the test but not confirm the setting.
- Its framework specifics are patterns rather than promises about your version. Confirm current
  behaviour before quoting any of it as a fact about your own stack.
