---
name: positioning-gate-spec
description: Compiles a decided product position into an enforceable, per-article publish gate rather than a style note. Produces the strategic_angle enum with exactly one value per differentiator plus combined, a default angle assigned per content cluster so backlog rows arrive already tagged, numbered checkbox items that demand a mechanism rather than a mention, a threshold that escalates with row priority, placement rules capping promotional mentions at one mid-article plus one closing call to action, a non-promotional word floor, and a one-revision-then-defer failure path. This skill should be used when setting up or repairing a content programme's positioning rules, when drafts keep coming back generic, when a new differentiator ships and the angle column has to change, or when an editor needs a rule to point at instead of an opinion.
---

# Positioning gate spec

## The claim this skill is built on

Positioning fails at the article, not at the strategy offsite.

The position is usually fine. Somebody did the work, four real differentiators got written down, and everyone agreed. Then forty articles get commissioned and the instruction attached to each one is "make sure our positioning comes through". That instruction has no output. It cannot be passed or failed, so nothing is ever blocked, and six months later the blog is a pile of competent category-tips articles that would read identically on a competitor's site with the logo swapped.

The obvious repair is to tell writers to mention the differentiators. That produces the second failure, which is worse because it looks like success: every article now name-drops the product, the checklist passes, and nothing has changed. A mention is not a position. The article has to explain the work getting done in a way that only makes sense if the capability exists.

So the gate is built on two moves. First, the angle is decided before the writing starts, at the cluster level, so nobody chooses it while staring at a blank page. Second, each checkbox demands a mechanism, and the reviewer has a deletion test for the cases where mechanism and mention are hard to tell apart. Everything else in this file exists to keep those two from eroding.

## Step 1: fix the differentiator list at four

Write exactly four differentiators, in priority order, each with a one-word or two-word short name in capitals. Three works. Five is the ceiling. More than five does not work, for a reason that is operational rather than aesthetic: the short names have to be memorable enough that a writer can hold the whole set in mind while drafting, and a default has to be obvious for every cluster. At six the set stops being memorable and the defaults start being arbitrary.

Priority order is load-bearing later, because it decides which differentiator leads when an article legitimately touches two.

Each short name needs one sentence stating the capability as a mechanism, not as a benefit. "Faster invoicing" is a benefit and it is unenforceable. "The invoice is raised on the engineer's phone before they leave the driveway" is a mechanism, and a reviewer can check whether an article explains that or not.

## Step 2: the enum, and the rule against a sixth value

Lock a `strategic_angle` enum containing exactly one value per differentiator plus one more, `combined`. Four differentiators give five values. Write this instruction into the spec in these words:

> Do not invent a sixth value. If a row seems to need one, it almost certainly fits `combined`.

The rule is there because the failure is silent and gradual. Somebody adds `general` for a row that did not fit. The next person adds `awareness`. Within a quarter the column holds nine values, four of which mean "I could not decide", and the column no longer supports any query, any default or any threshold. At that point the gate still runs and still passes everything, which is why nobody notices.

Two clarifications that prevent the common misreading. A row tagged `combined` must cite at least two differentiators, by definition of the word. A row tagged with a single differentiator may cite more than one, and the tagged angle is simply the one that leads the article. The enum records intent. The gate counts what actually appeared.

## Step 3: the per-cluster default, so rows are born tagged

Assign a default angle to every cluster in the content topology, at the point the topology is designed. Not to every row. To the cluster.

This is the item in the file with the best return on the time it costs, and it costs about twenty minutes, once. When the backlog is generated, each row inherits its cluster's default, so a row arrives at the writer already carrying an angle. A writer who disagrees can override, and the override is a note on the row that somebody reads. What they cannot do is arrive at an untagged row and improvise, which is what produces a backlog where the angle distribution is an accident of who was on shift.

A worked shape, for an invented field service scheduling tool sold to plumbing and electrical firms:

| Cluster | Default angle |
| --- | --- |
| Emergency callouts | `offline` |
| Route planning and drive time | `routing` |
| Van stock and parts | `parts` |
| Getting paid | `invoicing` |
| Software comparisons and buying guides | `combined` |

Make `strategic_angle` a required column on every backlog row. A row without one does not get written. This is a validation rule, not a convention: an empty angle blocks the row from entering the writing queue at all, which is far cheaper than blocking a finished draft.

## Step 4: write the gate as numbered checkboxes that demand a mechanism

One numbered checkbox per differentiator, each phrased to demand the mechanism. The wording that does the work:

> Names the real capability as the way the work gets done, not just a passing mention.

Add two more items that are pointers rather than facts, so the gate never becomes a second, staler copy of the product documentation:

- **Product accuracy.** Every product statement traces to the product truth pack or the live site. The gate does not restate a single product fact, because two copies of a fact drift and the newer copy is not always the correct one.
- **No fabricated figures.** Any number attached to the product comes from the pack with its attribution intact.

**The tie-break, for when you cannot tell whether a mention is a mechanism.** Delete the sentence containing the product name and reread the paragraph. If the paragraph still explains how the job gets done, the mention was decorative and the checkbox does not count. If a hole opens where the explanation was, it was a mechanism and it counts. If deleting it changes nothing because the paragraph never explained anything in the first place, the checkbox fails and the article has a bigger problem than positioning.

## Step 5: the threshold ladder

Thresholds escalate with the value of the row.

- **A normal row:** at least 1 of 4 checkboxes yes.
- **A priority 1 or priority 2 row:** at least 2 of 4 yes. These are the rows with real search demand behind them, they get the internal links, and they are the ones a buyer actually reaches. A high-value row that says nothing distinctive is the most expensive kind of miss.
- **A row tagged `combined`:** at least 2 by definition.

Three variants are worth knowing, because the right ladder depends on the property. A programme with a mature position runs 2 of 4 on everything and never lowers it. A programme with three differentiators runs 2 of 3, which is proportionally stricter and works when the three are genuinely distinct. A property whose entire job is neutral third-party authority runs a different structure entirely, covered below, where all eight items must be yes.

State the consequence of a zero in reader language, in the spec itself, because a number alone does not persuade anyone:

> 0 of 4 means it is generic category-tips content that could run on any competitor's blog.

That sentence is the one that changes behaviour. The count is only the trigger.

## Step 6: placement, and the 1,200-word non-promotional floor

The gate has to constrain where the product appears as tightly as whether it appears, or you get the same four checkboxes satisfied by an advertisement.

- **The opener contains no product mention at all.** Define the opener as everything above the first H2. The reader arrived with a problem. The first thing they meet is the problem, described more precisely than they could describe it themselves.
- **At most one contextual mention mid-article.** One, sited where the mechanism genuinely belongs in the explanation.
- **One call to action, near the end.** Never stack multiple hard calls to action. Two is not twice the conversion, it is a signal about who the page was written for.
- **The customer is the hero throughout.** The article is about their job. The product is an instrument that appears in one paragraph of it.

Then the anti-thin clause, with a hard number attached, because every other rule here can be satisfied by a well-formatted advertisement:

The article must fully resolve the query on its own. It answers every question the results page surfaced, it delivers whatever artefact the title promised, and it is useful to a reader who never clicks anything. **The non-promotional body must be at least 1,200 words.**

Measure the floor by subtracting, not by estimating: total body words, minus the paragraph containing the mid-article mention, minus the closing call-to-action block, minus any comparison table row naming your own product. What is left is the non-promotional body, and it clears 1,200 or the row fails.

> A hollow wrapper around a call to action fails the gate even if every other rule passes.

## The decision rule at the gate

Take exactly one branch per row.

- **Threshold met, placement clean, floor cleared.** Publish. Emit the run-record line.
- **Below threshold on the first pass.** Revise the body once. Name which differentiators are missing in the revision note, so the second pass has a target rather than a mood.
- **Still below threshold after one revision.** Mark the row FAILED, with notes reading `positioning thin: <which differentiators missing>`. Surface it as DEFERRED in the run record. Do not publish. There is one revision and only one, and the justification matters more than the number: a first pass written from a brief is the honest signal about whether the topic and the position connect at all. A fifth pass tells you the reviewer got tired. Never publish weak positioning intending to fix it later, because later has no owner and the article is already indexed.
- **Placement violated but threshold met.** Fix placement in the revision. It counts as the one revision. Placement is a mechanical edit, so a row that fails only on placement almost always passes the second pass.
- **You cannot tell whether the topic can honestly carry any differentiator.** Run the deletion test on every candidate mention first. If it still cannot be resolved, this is not a writing failure, it is a backlog failure, and it belongs upstream. Send the row back to the topology with a note, retag it to a cluster whose default it can actually satisfy, or retire it. Do not have the writer bolt a differentiator onto a topic that does not want one, which is how the gate teaches people to write name-drops.

## Step 7: force the result into the run record

Every published article writes one strict line into the run record:

```
Angle: <angle>, USPs cited: <list>. Gate check: <N>/<M> passed.
```

The format is fixed so it can be grepped and counted later. The point is accountability rather than reporting: a published article showing a below-threshold count is a contract violation and it is visible in the log, which means it can be found weeks afterwards by somebody who was not in the room. A gate whose result is not recorded is an opinion that happened once.

## The inversion, for a property whose job is neutral authority

If the property is a review site, a comparison hub, or anything whose value depends on reading as a neutral third party, invert the placement rules rather than relaxing them.

- The plug is a fixed structural element, and it sits late. In an eight-part structure it is element seven, after the analysis and before the closing summary.
- The voice is third person throughout. The product is described the way any other product on the page is described.
- Every item must be yes, all eight of eight, because on a neutral property the credibility of the whole site is the asset and a single unearned recommendation spends it.
- A row that cannot pass the first checkbox is not a weak article for this site. It is a repurpose or retire candidate, and it should move to a property where the plug is allowed to be structural.

## Worked example

The field service scheduling tool above. Cluster: emergency callouts. Default angle: `offline`. Priority 2. Title: "What to do when a job comes in and the engineer has no signal".

**First pass, 1,850 words.** Gate check:

1. `offline`: the product is named once, in the closing call to action. Deletion test: removing that sentence changes nothing in the explanation. Not a mechanism. **No.**
2. `routing`: absent. **No.**
3. `parts`: one line reading "some tools will check van stock for you". Generic, and true of the category. **No.**
4. `invoicing`: absent. **No.**

Placement: the product name appears in the second sentence of the opener. Violation. Non-promotional body: 1,640 words, clears the floor. Score 0 of 4 at priority 2, where the threshold is 2.

**Revision, one only.** The section on dispatch is rewritten so the actual step is the mechanism: the job is queued on the phone, the engineer works from a cached copy, and it syncs when the van reaches signal. The van stock check is named as the reason the engineer is not dispatched without the part, which is the answer to a question the article had already raised and left hanging. The opener mention is deleted. The mid-article mention stays, one of it. The closing call to action stays.

**Second pass.** `offline` yes, `parts` yes, `routing` no, `invoicing` no. 2 of 4 at priority 2. Placement clean. Non-promotional body 1,710 words. Angle stays `offline`, because it leads.

**Verdict: publish.** Run record line: `Angle: offline, USPs cited: offline, parts. Gate check: 2/4 passed.`

**The sibling row that did not make it.** Same cluster, a row titled "How to write a callout policy". Two passes, both 0 of 4, because the topic is a document template and touches nothing the product does. Marked FAILED, notes `positioning thin: offline, routing, parts, invoicing`, surfaced as DEFERRED, not published. The correct fix was upstream: the row belonged to a general-advice cluster that this programme had decided not to fund, and it should never have been generated.

## Failure modes

**The gate becomes a mention-counter.** Every article passes because every article name-drops the product. Recognisable from the outside because the pass rate is 100 per cent and the drafts still read like anyone's blog. The deletion test is the fix, and it has to be run by somebody who is willing to fail a colleague's draft.

**No per-cluster default, so angles get assigned at write time.** The symptom is a backlog whose angle distribution is lopsided in a way nobody chose, usually heavy on whichever differentiator is easiest to write about. It looks like a strategy and it is a scheduling artefact.

**Unlimited revision loops.** The row never fails, it just keeps coming back. The tell is a tracker with rows in progress for weeks and a review queue nobody can clear. One revision, then defer.

**The plug migrating into the opener.** Usually introduced by somebody optimising for a conversion metric on a single page. The tell is a bounce rate that worsens on exactly the pages that were edited, and an opener that answers a question the reader did not ask.

**Hollow wrappers passing rules one to six.** All four checkboxes yes, placement clean, and the article is 700 words of setup around a call to action. This is what the non-promotional floor exists for, and it is the only rule in the file that catches it.

**The enum quietly growing a sixth value.** Nobody announces it. A row appears tagged `general`, then three more, and the column stops meaning anything. Check the distinct values in the column once a month. It takes one query and it is the cheapest audit in the programme.

**A threshold that never escalates.** Every row at 1 of 4, including the pillar pages that get all the internal links and all the traffic. The high-value rows are exactly the ones where a generic article costs the most, and a flat threshold treats them as though they were the cheapest.

**The gate line missing from the run record.** The gate ran, somebody remembers it passing, and there is no evidence. Six weeks later a below-threshold article is live and the question of how it got there has no answer.

## What this skill does not do

- It does not decide the position, test it, or tell you whether the four differentiators are the right four. It enforces whatever it is handed, with complete consistency, including a wrong list.
- It does not verify product facts. Whether a capability ships today, and whether a number may be quoted, belong to the product truth pack, and the gate holds only a pointer.
- It does not write or improve the article. It produces a pass, a revision instruction naming what is missing, or a deferral.
- It cannot see search demand, rankings or conversions, so it cannot tell you whether a passing article was worth publishing.
- It does not survive a team that is unwilling to defer a row. The failure path is the whole mechanism, and a gate that never blocks anything is a form with no consequence attached.
