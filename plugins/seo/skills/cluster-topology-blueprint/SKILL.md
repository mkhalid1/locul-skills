---
name: cluster-topology-blueprint
description: Produces a pillar and cluster topology as a machine-parseable specification: one literal greppable pillar declaration per cluster, existing and planned supporting articles, posture and saturation labels from fixed vocabularies, a five-slot link matrix, per-row ownership walls, and a cross-property deduplication section. Enforces a closure pre-flight that resolves every link target and parent value before anything is written, and the hub-slug publishing rule that keeps a flagship article from publishing at its title slug and orphaning every child link. This skill should be used when planning the structure of a content programme, when a link or pillar check keeps reporting nothing on a site known to be broken, or when several articles in one cluster have started restating the same thesis.
---

# Cluster topology blueprint

## The claim this skill is built on

The obvious approach is a diagram. A hub in the middle, spokes around it, arrows pointing inward. Everyone recognises it, everyone agrees with it in the meeting, and it is close to useless six weeks later.

It fails for one reason: **nothing can read it.** A topology is only worth anything if later work can be checked against it, and checking requires extraction. If the pillar for a cluster is stated in a sentence, no script can find it, so the pillar-link check that somebody proudly added to the pipeline silently matches nothing on every run. It does not error. It reports zero findings. Zero findings reads as compliance, and the report is filed as good news for as long as the programme lasts.

That is the central claim: **a machine-parseable declaration is a precondition for any later check existing at all.** Not a nicety. A precondition. Most of the value of this file is upstream of any linking advice, in making the topology a thing a program can resolve.

The second claim is that closure is a pre-flight, not a review. A map where a link points at a page nothing creates is not a slightly imperfect map. It is a map that guarantees broken links, because a writer given a slug will use it.

## Step 1: declare the governance split at the top of the file

Three documents govern a content programme and they must not blur.

- **The topology governs WHERE.** Which cluster a piece belongs to, what it links to, what it may not absorb.
- **The strategy document governs WHY.** Positioning, audience, the argument the domain is making.
- **The writing specification governs HOW.** Structure, depth, voice, claim sourcing.

State this in the first ten lines. It is not decoration. Without it, every future edit to the topology arrives carrying an opinion about tone, and the file that decides structure becomes a second, contradictory style guide.

## Step 2: the cluster record, with one literal pillar declaration

Every cluster is a heading followed by the same fixed field set, in the same order, in every cluster without exception.

The pillar line uses **one literal form** and nothing else:

```
**Pillar:** `cluster-hub-slug` at /path/to/hub
```

Pick a form, write it down, and never vary it. The exact form matters less than that it is exact. What must be true is that a five-line script can extract every pillar slug and every pillar URL from the file without a language model, because the moment extraction needs judgement, extraction stops happening.

The remaining fields per cluster:

- **Existing supporting articles.** Slugs that already exist, or the literal phrase `none yet, greenfield`. A blank field is ambiguous between "nothing yet" and "nobody checked".
- **Sub-clusters**, each with its own pillar slug, declared in the same literal form.
- **A strategy paragraph.** Two or three sentences on what this cluster is arguing.
- **Routing.** Which angles, intents or audiences send a reader here.
- **Posture** and **saturation**, from the vocabularies below.

## Step 3: posture and saturation, from fixed vocabularies

Two labels per cluster, each drawn from a closed list. Closed lists rather than free text, because a free-text posture field fills up with sentences and stops being sortable, and the first thing anyone wants to do with a topology is sort it.

**Posture**, meaning what to do next:

`Extend` / `Extend aggressively` / `Maintain` / `Hold` / `Hold, merge into other clusters` / `Expand, highest-upside net-new` / `Optional`

**Saturation**, meaning how much room is left:

`High` / `Saturated` / `Medium` / `Low` / `Open`

The pairing carries the decision. `Saturated` plus `Extend aggressively` is a contradiction that should be caught in review, and it is only catchable because both fields come from a vocabulary.

## Step 4: the hub-slug publishing rule

This is the rule that costs the most when it is missed, and it takes one sentence to state.

**A cluster's flagship article publishes at the hub slug, which is the exact value its children carry as their parent, not at the slugified version of its own title.**

The failure is ordinary. A pillar is declared as `invoice-automation`. The flagship gets briefed with a full title, something like "The complete guide to automating invoice approvals in 2026". Whoever publishes it lets the platform derive the slug from the title, and the page goes live at `the-complete-guide-to-automating-invoice-approvals-in-2026`. The page is fine. It ranks. Nothing errors. But every spoke in that cluster names `invoice-automation` as its parent and links to it, and that URL will never exist, so every one of those links 404s.

On one real site, two clusters had declared pillars that were never published under the declared slug, and every child link in both clusters was dead. The pillar pages themselves were live and performing, which is why it took months to find.

Two defences, and use both. Pass the slug explicitly on the publish call rather than letting it be derived. And add the flagship's hub slug to the closure set as a queued row, so the pre-flight fails if the plan ever schedules it at a different slug.

## Step 5: closure pre-flight, and it is a hard stop

Before anything is written, resolve **every** internal link target and **every** parent value in the map against the union of three sets:

1. Slugs the queued rows will create.
2. Slugs in the published index, meaning the live corpus rather than anyone's recollection.
3. Approved non-article pages, from the link target registry.

If a single target does not resolve, **stop**. Do not write the article. Do not warn and continue. Do not add a note to a review column.

The reason this must be a stop rather than a warning is behavioural. A writer handed a plan containing a slug will use the slug, because it is written down and it looks like every other slug in the file. The link renders normally, passes visual review, and returns 404 to the first crawler that finds it, which is usually weeks later. The only reliable moment to prevent it is before the plan is handed over.

When the pre-flight passes, **assert it inside the file itself**:

```
Closure: verified closed on <date>. Every link target resolves to a slug
some row creates, a slug in the published index, or an approved page.
```

The assertion is dated because it decays. It is true on the day the map is written and untrue the first time a row is deleted from the backlog.

## Step 6: the link matrix, so the rule becomes a column

Abstract linking rules do not survive contact with a spreadsheet. State the matrix as **five named slots**, so each row of the backlog carries a cell rather than an instruction:

```
[parent_pillar, cta_page, sibling_1, sibling_2, bridge_or_comparison]
```

And state the directions with counts, because "link generously within the cluster" produces both three links and thirty.

- **Child to pillar: always.** Every member of a cluster links up. No exceptions and no judgement.
- **Pillar to child: mandatory, no exceptions.** This is the one that gets skipped, because it is an edit to an already-published page rather than part of writing a new one. A pillar that does not link down is a menu, not a hub, and the cluster never forms.
- **Sibling to sibling: two to four within the cluster.** Below two, the cluster is a star with no internal structure. Above four, later members have nothing left to link to that is not already linked.
- **Cross-cluster: zero to two**, or zero to one if it is a bridge or comparison piece. Cross-cluster links are how a topology becomes a mesh, and a mesh has no hubs.

These counts are working defaults from one multi-site operation. The reasoning is what transfers: enough internal links that a cluster reads as a set, few enough that any individual link is a deliberate choice.

## Step 7: saturation guardrails, in numbers

Four concrete caps. Each one is a default with a reason, not a published law.

**Cap an audience-adjacent cluster at roughly 15 per cent of any multi-week publishing window.** Audience-adjacent means a cluster your readers plausibly care about that is not what you sell. It is the cluster that grows without resistance, because the topics are easy and the traffic is real. Fifteen per cent is one row in seven, which is enough to hold a position and not enough to change what the domain appears to be about.

**Keep a marginal cluster to five to eight rows, or reassign it.** Below five, a cluster has no internal linking to do and its members are effectively orphans with a label. Above eight, it is no longer marginal and needs a strategy paragraph that says why.

**Write thin individual terms as roll-ups.** Twenty near-identical single-entity pages, one per tool, one per integration, one per city, compete with each other, split their own links, and read as templated. One roll-up covering all twenty carries the same demand and concentrates everything. Split it later if a single entity earns its own page on evidence.

**Over-invest early in the one genuinely open cluster.** Most portfolios have exactly one cluster labelled `Open`, and the window on it is shorter than the window on the saturated ones. Front-loading is the only cap here that pushes upward.

## Step 8: cross-property deduplication, written first

If a sibling property in the same portfolio covers the same subject, write this section **before** any cluster records, because it changes what the clusters are allowed to contain.

Three parts, all concrete:

- **Acceptable angles.** Named. "Implementation detail for practitioners" is an angle. "Different tone" is not.
- **Not-acceptable angles.** Named, with the property that owns each one. This list is longer than the acceptable one and it is the useful half.
- **A shape test.** One sentence that decides an ambiguous case without a meeting. For example: if the piece answers "should we" it belongs to the other property, and if it answers "how do we" it belongs here.

Written as a vibe rather than as three lists, this section is ignored within a month, and both properties end up publishing the same article with different headings.

## Step 9: ownership walls, per row, before anything is written

The map assigns topics. Ownership walls assign **boundaries**, and without them three articles in a cluster each restate the whole thesis, because each writer independently decided the reader needs the background.

Write walls as explicit sentences naming specific rows:

- "Row A defines the concept and builds the thing. The diagnosis belongs entirely to row B. A links out to B rather than absorbing it."
- "Row C is the vision piece only and must not give the how-to. The how-to is row D."
- "Each of the three thesis rows cites a different proof, so the domain reads as depth rather than repetition."

The third form is the one people skip and it is the one that decides whether a cluster reads as authoritative or as padding.

## Step 10: never invent a URL, and version the file

If a pillar page does not exist live yet, link the nearest existing parent and flag it in the notes column. Do not write the future URL. A plausible URL in a plan becomes a real link in an article, and the plan is where it stops being catchable.

Version the file with a changelog, and make each entry say **which rule is load-bearing** in that revision. A changelog listing what changed is a diff. A changelog saying "this revision makes the hub-slug rule mandatory because two clusters shipped with dead child links" is the thing that stops the rule being quietly relaxed next quarter.

## Decision rule: this row wants to link to a page that does not exist yet

1. The target is a slug some queued row will create, and that row is scheduled **before** this one. **Allow it.** The page will exist by publication.
2. The target is a queued row scheduled **after** this one. **Do not allow the link.** Either reorder so the target comes first, or drop the link. A forward reference is a 404 with a publication date.
3. The target is a non-article page and it appears in the link target registry as confirmed. **Allow it.**
4. The target is a non-article page that is not in the registry. **Do not invent it.** Link the nearest confirmed page instead, or drop the link and note it.
5. **You cannot tell.** The target looks live, the published index does not list it, and the registry says nothing. Do not guess and do not construct the URL. Fetch it once. A 200 means the index is stale, so record the discrepancy and allow the link. A 404 or a redirect means treat it as non-existent under rule 4. If it cannot be fetched at all, because the environment has no network or the page sits behind authentication, **halt the row and escalate it by name**. It does not silently become allowed, and it does not silently become a warning in a column nobody sorts by.

## Worked example, compressed

A documentation site for a fictional project management tool. Four clusters proposed, one sibling property already covering part of the subject.

**Governance split** stated in six lines at the top.

**Cross-property section written first.** The sibling property owns "should we adopt" content. Acceptable angles here: implementation, migration, administration. Not acceptable: tool selection, buying guides, pricing comparison. Shape test: if the reader has not chosen yet, it is not ours. Two proposed clusters lose three rows each to this rule before any of them are written.

**Cluster records.** Four clusters, each with the literal pillar declaration. Cluster one, `task-automation`, posture `Extend`, saturation `Medium`, eleven existing articles. Cluster two, `reporting`, posture `Expand, highest-upside net-new`, saturation `Open`, none yet, greenfield. Cluster three, `integrations`, posture `Hold, merge into other clusters`, saturation `Saturated`, because it is nineteen near-identical single-integration pages, which the roll-up rule converts to two roll-ups plus three genuinely-earned individual pages. Cluster four, `remote-team-culture`, posture `Optional`, saturation `High`, and it is audience-adjacent, so it is capped at 15 per cent of the window, which over a twenty-row window is three rows.

**Hub-slug check.** Cluster two's flagship is briefed as "How to build a reporting layer your finance team will actually read". Its hub slug is `reporting`. The plan records both, and the publish instruction carries the slug explicitly. Without that line the page would have shipped at the title slug and all six planned children would have linked to nothing.

**Closure pre-flight.** Twenty rows, sixty-two link targets. Fifty-eight resolve. Four do not: two point at a pricing page that the registry lists as unconfirmed, one points at a cluster-three page that the roll-up rule just deleted, and one points at a row scheduled two weeks later. The run **stops**. Two links are dropped, one is repointed at a roll-up, one row is reordered. Re-run: sixty-two of sixty-two resolve.

**Ownership walls.** Cluster one's row 3 defines and builds. Row 7 diagnoses. Row 3 links out to row 7 and is explicitly forbidden from covering diagnosis.

**Verdict: the map is closed and publishable, at nineteen rows rather than twenty-six.** Seven proposed rows were removed by the cross-property rule and the roll-up rule before anyone wrote a word, one cluster is on a stated cap, and the closure assertion is written into the file with the date it was verified.

## Failure modes

**The pillar declared in prose.** From the outside: a link check that has reported zero findings every run since it was added, on a site where people know the pillars are a mess. Nothing is broken except that the check matches nothing, and silence is being read as health.

**The flagship at its title slug.** The pillar page is live, indexed and performing. Every child link to it 404s. Because the pillar itself is fine, the diagnosis usually starts in the wrong place, and the fix arrives after the children have already lost whatever they were going to gain.

**A cluster past its cap.** The audience-adjacent cluster is now a third of the corpus, the domain's most-visited pages are all about a subject you do not sell, and any attempt to prune is resisted because those pages have traffic. There is no clean fix available afterwards, which is why the cap is a pre-commitment.

**Absent ownership walls.** Three articles in one cluster, each spending its first six hundred words restating the same argument before reaching its own topic. Reads as padding, is really three writers each being conscientious in isolation.

**Cross-property dedupe as a vibe.** Both properties publish the same article within a quarter, the two pages compete, and neither team did anything wrong by the guidance they were given.

**The unresolvable target that became a warning.** The pre-flight found four broken targets, someone recorded them in a notes column, the rows were written anyway, and three of the four shipped as live links because the writer had a slug and no reason to doubt it.

**Closure verified once and never again.** The assertion in the file is eight months old. Four rows have been deleted from the backlog since, and every link that pointed at them is now a forward reference to nothing.

**A perfect map with nothing under it.** Every cluster labelled, every declaration parseable, closure asserted, two articles published. The topology is not wrong. It is just not doing anything, and its tidiness makes the programme feel further along than it is.

## What this skill does not do

- It does not do keyword research and cannot measure demand. It arranges a portfolio somebody else validated, and it will arrange a bad one just as neatly.
- It does not write the schedule. Row order, article type quotas and word count targets belong to a backlog, and this only constrains what that backlog is allowed to contain.
- It cannot confirm that any URL it names is served. Every claim about a live page needs a real request, and a crawler in list mode is the right tool for that.
- It has no opinion on search intent for individual terms, so a cluster boundary it draws is a subject-matter judgement rather than a demand-based one.
- Its caps and counts are working defaults from one operation, not published thresholds, and a cluster that is your entire business is a legitimate reason to ignore all of them.
- It publishes nothing and edits nothing. The pillar-to-child links it mandates are edits to existing pages that somebody has to actually make, and that is the step most often skipped.
