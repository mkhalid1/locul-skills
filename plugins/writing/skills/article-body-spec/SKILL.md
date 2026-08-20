---
name: article-body-spec
description: Produces the per-publication article production spec: the required elements in order, the count for each one, a word-count ladder keyed to demand with a reason at both ends, heading rules, the markup discipline the pipeline depends on, banned lists with carve-outs and replacements, and the SEO field character limits. Use it when a publication has no written spec, when a second writer joins, or when a downstream artefact such as FAQ schema or a translation keeps coming out malformed. This skill should be used when you need to fix an article's shape as countable rules rather than assess a draft that already exists.
---

# Article body spec

## The claim this skill is built on

A house style that says "write clearly, be useful, include an FAQ" cannot be checked, so every review relitigates the same points and every new writer restarts the argument. The fix is not better adjectives. It is a spec: a short file per publication that fixes the elements, their order, a count for each one, and the string limits, so that a draft either satisfies it or does not.

The second claim matters more and is less obvious. Most of what goes wrong downstream is decided by structure and by strings, not by prose quality. The FAQ schema is generated from your headings. The translated page inherits your character limits. The infographic restates your table. The front end escapes your HTML. A publication with beautiful prose and no spec produces malformed schema, overflowing meta titles, infographics that contradict the article, and anchor tags printed as literal text on the page, and none of those are writing problems. They are all decided at authoring time, which is why the spec has to exist before the first article rather than after the first incident.

## What the spec is

One file per publication, checked into the same repository as the drafts, using lowercase-hyphen naming so it resolves identically on Windows, macOS and a case-sensitive build server. It has nine parts: the element list and order, the counts, the word ladder, the heading rules, the markup rules, the emphasis rules, the two banned lists, the SEO field limits, and the localisation clause. Every part must be checkable by a person in under a minute or by a script in under a second. Anything that is not checkable belongs in the commissioning brief.

---

## Step 1: fix the element list and its order

The default set, in order:

1. Title
2. Opener
3. Takeaways box
4. Body H2 sections
5. Comparison table
6. Worked scenarios
7. FAQ
8. Related links block
9. One call to action

The order is not decoration. The opener has to come before everything because the standalone answer lives inside it, and an answer pushed below a takeaways box is an answer outside the window that gets quoted. The FAQ must be one contiguous block because the schema emitter reads exactly one `## FAQ` heading. The related links must sit before the call to action, so the last thing on the page is an action rather than a set of exits.

Mark each element required or conditional in your spec, and give the condition. "Comparison table: conditional, see the decision rule" is a spec entry. "Comparison table: where appropriate" is not.

---

## Step 2: the standalone answer window, 100 to 200 words

The opener is 2 to 5 sentences with no warm-up, and the core query must be answered inside the first 100 to 200 words of the body.

The reason is specific. That opening span is what an answer engine can lift and present without the rest of the page, and what a reader scanning on a phone sees before the first scroll. Under about 100 words you rarely have a complete answer, only a restatement of the title. Past about 200 words the answer stops being a quotable unit and becomes the first section of an article, which means anything quoting it has to decide where to cut, and it will cut badly.

Two rules make the window real:

- **It must stand alone.** The answer names its own subject rather than relying on the title for the noun. "It retries three times over 24 hours" fails. "Failed webhook deliveries are retried three times over 24 hours" passes.
- **It must contain the specific thing.** A number, a limit, a named mechanism, a yes or a no. An opener that promises the article will explain something has not answered anything.

**The deletion test.** Delete everything after the first 200 words. Does what remains answer the question a person typed? If not, the opener is a preamble and needs rewriting before anything else in the draft is discussed.

---

## Step 3: the count for every element

These are the working defaults from a multi-site operation. Adjust them for your publication, but write down whatever you choose, because an unwritten count is not a count.

| Element | Count | The rule that makes it non-trivial |
|---|---|---|
| Opener | 2 to 5 sentences | No warm-up. The answer is inside 100 to 200 words |
| Takeaways | 3 to 7 bullets | Every bullet carries a number, a named feature or a concrete claim |
| Body sections | 4 to 9 H2s | 150 to 300 words each. A 60-word H2 is a paragraph wearing a heading |
| Comparison table | 3 to 8 rows | 3 rows of real substance minimum, 8 maximum so it fits a phone |
| Worked scenarios | 1 to 2 | Third person, present tense, 3 to 5 sentences, named features, real step or time counts |
| FAQ | 4 to 8 H3s | Under exactly one `## FAQ` H2, each phrased as typed into a search box |
| Related links | 3 to 6 | Internal, and each one relevant to the reader who finished this page |
| Call to action | Exactly 1 | One, at the end. Not one per section |

The takeaways rule decays first. A box whose seven bullets contain no numbers is a second set of subheadings, and readers learn within two articles that it is skippable.

Third person and present tense in the scenarios force a description of what happens rather than a promise about what could. "A team lead exports last week's entries, spots two missing days, and fixes them in about five minutes" is a scenario. "Teams can effortlessly manage their timesheets" is a claim in a scenario's clothing.

---

## Step 4: the word ladder, with a reason at both ends

Word count is keyed to a demand band assigned before writing, not to the writer's judgement on the day.

| Band | Target | Why the floor | Why the ceiling |
|---|---|---|---|
| High | 1,800 to 2,400 | Below this the piece cannot cover a contested question as completely as the pages already ranking for it | Above this it starts answering the adjacent question, which dilutes the target term and collides with your own next article |
| Medium | 1,500 to 1,800 | Below this the subtopics that make it useful get cut | Above this it is padding to look thorough |
| Low | 1,200 to 1,700 | This is a floor and it is the point at which the page reads as a real answer rather than a stub | The ceiling exists but is rarely the problem. Low hits the floor and does not pad |

State both reasons in the spec. A range with no reasons is treated as advisory and ignored under deadline; a range with a reason at each end tells a writer which direction they are allowed to miss in.

**If a demand band already exists elsewhere in your operation, adopt its ladder rather than this one.** If a backlog that assigns bands and a spec that assigns word counts disagree, the backlog wins, because it holds the volume figure that produced the band. Two ladders in one operation shows up as writers quoting whichever number suits them. These figures are working defaults from one operation with a particular mix of comparison, how-to and requirements pages, not thresholds anyone has published.

---

## Step 5: heading discipline

- Exactly one H1, which is the title, and the body never contains a second one.
- H2 for every top-level section.
- H3 only inside the FAQ, and inside the numbered items of a listicle. Nowhere else.
- Never skip a level. An H2 followed by an H4 is a defect even where it looks fine.

The H3 restriction is not tidiness. It exists so that the set of H3s under `## FAQ` is unambiguous to whatever parses the file, which is the subject of the next step.

---

## Step 6: the FAQ headings are a technical decision

This is the part of the spec most likely to be treated as style and most likely to break something.

The emitter walks the markdown, finds the `## FAQ` heading, takes every H3 beneath it as a question string, and takes the prose from that H3 until the next H3 as the answer string. The rendered FAQ on the page and the emitted question and answer pairs come from the same markdown. **The structure is the schema.** Anything malformed leaks in both places at once, which is why a broken FAQ usually looks fine to the person who wrote it and wrong to everything downstream.

Consequences you have to write into the spec:

- **Phrase each H3 exactly as someone would type it into a search box.** "How long does a failed webhook retry for?" is a question string. "Retry timing" is a heading, and it becomes a question string that reads as a fragment wherever the pairs are consumed.
- **Exactly one `## FAQ` per article.** Two of them, which is what happens when two drafts are merged, means one block's questions are dropped or the emitter takes only the first. The page still shows both, so nothing looks wrong.
- **Nothing between the `## FAQ` heading and the first H3.** A lead-in paragraph there belongs to no question and is silently discarded.
- **Answers are self-contained, roughly 40 to 60 words.** The answer is quoted without the question's surroundings, so an answer beginning "As mentioned above" is worthless the moment it leaves the page.
- **No raw HTML inside an answer.** See step 8. Answers are where stray anchors get pasted, because people copy them out of an old system.
- **Nested lists and tables inside an answer get flattened.** If the answer needs a table, it is not an FAQ answer, it is a body section.

**A dated note on why you still do this.** In 2023 Google narrowed FAQ rich results to well-known government and health sites, so for most publications the visible rich result is no longer the payoff. The pairs still matter, because they are consumed by other parsers, by internal search, by help centre imports and by anything that reads your structured data. The rule survives its original reason, which is worth saying out loud in the spec so nobody deletes it as obsolete.

---

## Step 7: markdown discipline that survives the pipeline

- Real `##` headings. Never a bolded line used as a heading. It produces no anchor, no table of contents entry and no outline, and it is invisible in a rendered preview because it looks like a heading.
- One blank line before and after every block element: headings, lists, tables, code fences, blockquotes. Missing blank lines are the single most common reason a list renders as one paragraph in one renderer and correctly in another.
- Match the element to the content. A comparison is a table. A sequence is a numbered list. Parallel points are bullets. Everything else is prose. Bullets used for prose is the failure that produces a page nobody can read straight through.
- Paragraphs of 2 to 4 sentences.
- **A `---` divider between top-level sections is required, not optional.** On a long page the heading alone does not read as a break, especially on a phone where the heading and the previous paragraph sit within one thumb-width of each other. The divider is a readability device with a job, so the spec treats a missing one as a defect rather than as a preference.

---

## Step 8: every link is markdown, and this is not stylistic

All links are `[text](url)`. Never a raw HTML anchor. This includes FAQ answers, table cells, takeaway bullets and captions.

The failure is specific and it is nasty. Where the front end escapes HTML in article content, which is the safe default and what most content fields do, an anchor tag pasted into the body is rendered as literal text. The reader sees the tag itself printed in the middle of a sentence. It renders correctly in the editor's preview, because the preview does not apply the same escaping, so the defect is invisible to the author and visible to everyone else.

Two checks belong in the spec, and both run the same way on Windows and macOS:

```
node -e "const t=require('fs').readFileSync(process.argv[1],'utf8');for(const p of ['<a ','</a>','&lt;a ','<br','<img','<div']){const n=t.split(p).length-1;if(n)console.log('raw HTML',JSON.stringify(p),n);}" draft.md
```

```
node -e "const t=require('fs').readFileSync(process.argv[1],'utf8');const m=t.match(/^## FAQ\s*$/gm)||[];console.log('FAQ H2 count',m.length);console.log('H3 count',(t.match(/^### /gm)||[]).length);" draft.md
```

The second one exists because the two-FAQ defect has no visible symptom.

---

## Step 9: emphasis, highlights and pull-quotes

- **Exactly two approved highlight colours**, defined once with a stated meaning for each. A third colour introduced by one writer becomes four within a quarter.
- **Maximum five highlights per article, and fewer is better.** Highlighting is a contrast device and it stops working at volume. Five is a cap, not a target. Short phrases only, never a whole heading, paragraph or list item.
- Keep a **good highlight candidates** list so the cap gets spent well: the real cutoff number, the caveat that changes the answer, and the measured benchmark. Those earn a highlight. An adjective does not.
- **Pull-quotes are plain-text blockquotes**, 1 to 2 sentences, roughly 6 to 22 words, placed about 60 to 85 per cent of the way through. Never adjacent to the infographic, because two attention devices in one screen cancel each other. Never an image: an image pull-quote cannot be selected, translated, searched or resized, and it is a common source of text that exists on the page and nowhere in the file.

---

## Step 10: two banned lists, with carve-outs and replacements

Keep them separate, because they fail differently.

**List one, the vocabulary.** Every entry carries both a carve-out and a replacement. A ban with neither gets ignored the first time a writer meets a sentence where the word was correct, and once a writer decides the list is wrong they stop consulting it entirely.

| Banned | Carve-out | Replacement |
|---|---|---|
| delve, dive into | none | "look at", or cut the verb |
| leverage | the financial noun stays | "use" |
| seamless | none | say what the user no longer has to do |
| robust | statistics, where it is a term of art | say what it survives |
| streamline | fluid dynamics | name the step you removed |
| boost | fine where it literally means raising a score or a value | say what rose, and by how much |
| transform | the mathematical and data senses stay | state the before and the after |
| journey | travel writing | "process", or name the stage |
| efficiency | fine where it is a measured ratio with units | give the ratio |
| unlock, empower, elevate, harness | none | "let", "allow", "improve", "use" |
| game-changer, revolutionise | none | say what changed and for whom |
| cutting-edge, state-of-the-art, next-gen | none | give the version and the date |
| synergy | none | name the two things and what one does for the other |
| tap into, supercharge | none | cut the sentence and state the fact |
| in today's fast-paced world | none | cut |

**List two, the constructions.** These survive a vocabulary pass untouched and they are the reason a clean draft still reads as generated: rhetorical-question openers; the three-item list whose third item is filler; "It's not just X, it's Y"; "Whether you're A, B or C"; "it's worth noting that"; a closing summary paragraph, because the article ends with the call to action block and not with a recap; triple-adjective stacking; and hedging every sentence.

**Punctuation.** No em dashes anywhere, no emoji, no exclamation marks outside a direct quote, and no bullet-point soup.

Keep both lists short in the spec. This is a production document that has to be checkable at publish time, and the deep lexical pass belongs in a separate editing step with its own longer list.

---

## Step 11: SEO fields, with character limits

| Field | Limit | Rule |
|---|---|---|
| meta_title | 50 to 60 characters | Leads with the target term |
| meta_description | 150 to 160 characters | Contains the term once, plus a concrete reason to click |
| og_title | 70 characters maximum | |
| twitter_title | 55 characters maximum | |
| twitter_description | 120 characters maximum | |
| Supporting keywords | 8 to 12 | |
| Slug | 3 to 6 hyphenated words | ASCII, lowercase, and no year token, ever |

**Ban year tokens in titles, slugs and meta at write time.** This is the cheapest rule in the spec and the one with the largest cleanup cost when it is missing. On one real portfolio, an audit found 64, 14 and 13 pages across three properties carrying a stale year in the title, every one of them avoidable at authoring, and every one of them requiring a title edit, a meta edit and a reindex to fix. A year in a slug is worse, because fixing it means either a stale URL or a redirect.

**Ban internal mechanics from reader-facing copy.** Never name the content management system, the backlog, the data vendor, the pipeline or the run. It appears as sentences like "as our content pipeline shows" or "this piece was planned from our topic backlog", and it is written by whoever had the plan file open in the next tab.

---

## Step 12: localise the banned lists

A translated body must not use the target language's equivalents of the banned words. Otherwise every ban is a source-language ban, and the translated page reads exactly like the thing you banned while passing every check you run.

The spec needs a per-language block: the translated ban list, the character limits for that locale, and whether each limit is a hard truncation or a soft target. Meta titles in German routinely overflow a 60-character limit that English fits comfortably, so the spec has to say which of "translate then trim" or "write a native title" applies.

---

## Decision rule: does this article get a comparison table?

- **If the target query compares named things**, two products, two plans, two methods, the table is required, and every cell carries a verbatim value from a source you can cite.
- **If the query is a single how-to with no alternatives**, the table is banned. A table with one column of substance is padding wearing a grid.
- **If you cannot tell**, draft the table first. Try to fill three rows with exact values. If you cannot, there is no comparison to make: drop the table and record "no table, no citable values" in the article's spec notes. Never ship a table whose cells say "varies", "depends" or "yes". A cell that cannot hold a value is a cell that should have been a sentence.

Where a table does ship, its values are the source of truth for any infographic built from it. The two must match word for word, because the failure mode is a support ticket quoting a number that appears in the picture and nowhere in the text.

---

## Worked example, compressed

A documentation site for an invented billing service. Target term: "webhook retry policy". Demand band: medium. The commissioning pack contains the retry schedule and two support transcripts.

The spec instance says: 1,500 to 1,800 words, 6 H2s, takeaways 5 bullets, one comparison table of retry windows by plan, 2 scenarios, 5 FAQ H3s, 4 related links, 1 call to action, meta_title 50 to 60 characters leading with "webhook retry policy", slug `webhook-retry-policy`.

The draft comes back at 1,640 words. Checking against the spec:

- Opener answers the query at word 140. Inside the window. Pass.
- Takeaways: 5 bullets, 4 carry numbers, one reads "reliable delivery, handled for you". Fail, rewrite or cut.
- Table: 4 rows, exact windows and counts, all citable. Pass.
- FAQ: 5 H3s, but the grep returns two `## FAQ` headings, because the second half was pasted from an earlier draft. Fail, and invisible in preview.
- One FAQ answer contains `<a href="/plans">plan limits</a>`. Fail, and it would print as literal text on the live page.
- Highlights: 7. Over the cap of 5. Fail.
- Pull-quote: 14 words, but placed at 41 per cent and directly beside the infographic. Fail on both placement rules.
- meta_title: 57 characters, leads with the term. Pass. Slug: three words, no year. Pass.

**Verdict: hold, six defects, two of them invisible in the editor.** The second `## FAQ` and the raw anchor both look correct to the author and are both wrong in production, which is exactly why they are in the spec as greppable rules rather than as guidance.

---

## Failure modes

**Takeaways padded with generalities.** Seven bullets, no numbers, indistinguishable from the subheadings. It looks like readers skipping the box entirely, and writers copying the last article's bullets because they never carried information anyway.

**FAQ H3s written as headings rather than as typed queries.** A section called "Pricing" emits a question string of "Pricing". Nothing errors, the page looks fine, and nobody notices for months because nothing on your own site displays the pairs.

**Two `## FAQ` blocks after a merge.** Half the questions vanish from the emitted pairs while the rendered page shows all of them. This is the defect that most reliably survives review, because reviewing means reading the page.

**A raw anchor leaking as literal text.** Readers see the tag printed inside a paragraph. The author cannot reproduce it, because their preview escapes nothing, so the bug gets filed against the front end rather than the draft.

**The table and the infographic disagreeing.** A number reaches a support ticket that appears nowhere in the article text, because it came from the image, which was built from an older version of the table.

**Low-demand pieces padded to a ceiling.** An 800-word answer wearing a 2,300-word article. The signature is three sections of context before the answer, which also empties the 100 to 200 word window, so the piece fails twice from one cause.

**Year tokens baked in at authoring.** An audit finds dozens of pages across several properties whose titles claim a year that has passed. Each costs a title edit, a meta edit and a reindex, and none needed to exist.

**Banned words re-entering through translation.** The English page is clean, the translated pages read like brochure copy, and every automated check passes because it only ever looked at English.

**A bolded line faked as a heading.** The table of contents is missing three sections, in-page anchors shared in a support reply do not resolve, and anything parsing the page gets the wrong outline.

---

## What this skill does not do

- It does not write the article, and a spec applied to a draft with nothing to say produces a well-shaped article with nothing to say.
- It does not validate emitted markup. Whether the structured data your pipeline produces is accepted is a question for a structured data validator, and the spec only constrains the headings that go in.
- It cannot see your front end, your editor or your renderer, and the escaping, table overflow and highlight-survival behaviours differ between all three. Publish one page and look at it on a desktop and a phone before you trust the spec.
- It has no view on whether the topic deserves an article. Demand, cluster fit and collision with your own pages are decided before this file opens.
- It does not do visuals beyond stating the binding between the table and the infographic. Sizing, placement bands and what may be baked into a raster are a separate job.
- The numbers are working defaults from one operation. Adopt them to get started, then move them when your own data disagrees, and record that you moved them.
