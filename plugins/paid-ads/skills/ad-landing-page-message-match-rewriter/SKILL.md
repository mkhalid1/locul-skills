---
name: ad-landing-page-message-match-rewriter
description: Rewrites the landing page an ad clicks through to, so the page continues the ad instead of restarting the conversation. It takes the ad's headline and body, its call-to-action verb, the query or audience it serves and the current page copy, then returns the rewritten hero, subhead, opening section, form and button in full, plus a changelog of what moved and why. This skill should be used when an ad is about to serve and its destination page was written for something else, when several ads point at one page, or when click-through rate looks fine and conversion rate does not.
---

# Ad to landing page message match rewriter

## The claim this skill is built on

Message match gets discussed as a feeling and it is not one. It decomposes into identities you can check one at a time: the same promise, the same figure, the same verb, the same offer name, the same qualifier, the same kind of proof. Either the page repeats them or it does not, and the reader settles that almost immediately without consciously comparing anything.

The obvious approach fails in two opposite directions. The first is a list of observations about the page, which nobody ships, so the copy gets written from scratch a week later by somebody who never read the ad. The second is to echo the query into the hero, usually with dynamic text insertion, which Google's live Quality Score article says is unnecessary, read on 31 August 2026: "Keep in mind, the word-for-word phrase from a search term doesn't need to be on your landing page." Echoing a string is not continuing a promise.

So the output is a rewritten page, section by section as final copy, then a changelog. The changelog is the second half of the deliverable and never the first.

One ordering rule governs the rest: extract the match points from the ad before reading the page. Read the page first and you anchor on its structure, and out comes a tidier version of what was already there.

## Part one. Extract six match points from the ad

Write these down before opening the page copy. Each is a value, not a judgement, and the six are this skill's own list rather than a platform taxonomy.

| # | Match point | What to take from the ad |
|---|---|---|
| 1 | The promise | The specific outcome named in the ad headline, in the ad's own nouns |
| 2 | The figure | Any number: price, percentage, trial length, quantity, turnaround time |
| 3 | The verb | The call-to-action verb, exactly: "Get quote", "Book demo", "Start trial" |
| 4 | The offer name | How the thing is spelled in the ad: the plan, the product, the deliverable |
| 5 | The qualifier | Who it is for, and any condition: "for teams of five or more", "no card needed" |
| 6 | The proof type | The evidence the ad leans on: a rating, a customer count, a guarantee, a credential |

Then run the recoverability test. Points 1, 2 and 3 must be recoverable above the fold at a narrow phone width, this skill's own working default of roughly 360 to 400 CSS pixels rather than any published requirement, without scrolling and without reading body copy. The desktop preview is where every hero looks fine. A promise that only arrives in the third paragraph has restarted the conversation.

Points 4, 5 and 6 need only be present and consistent. An offer name that changes between ad and page is the quiet version of a broken promise: the reader cannot tell whether this is the right product or a different one.

## Part two. What the platforms document, which is what makes this a compliance argument

All of this was read on 31 August 2026. Neither help centre date-stamps its articles, so "current" means what the page said that day and nothing stronger.

**Google, the quality side.** Landing page experience is one of Quality Score's three components, and Quality Score itself is not an auction input: Google states "Quality Score is not an input in the ad auction" and calls it a diagnostic tool. What is an input is auction-time ad quality, defined to include "the quality of their experience once they reach your landing page". The published best practice reads, verbatim, "Keep messaging consistent from ad to landing page. Ensure the page follows through on the ad's offer or call to action." The four-part list every guide quotes as Google's landing page factors, relevant original content, transparency, easy navigation, faster loading, comes from a page Google retired in spring 2021, whose URL now serves the Quality Score article instead: archived guidance, not current documentation.

**Google, the policy side.** Destination requirements enumerates eleven named disapproval reasons, rebuilt with 2025-era article IDs. Three matter to a rewrite.

- **Destination mismatch is about domains, not about your copy.** Documented triggers: a display URL domain not matching the final URL, a subdomain that does not distinguish the site, a final URL redirect to another domain, and a tracking template leading somewhere other than the final URL. Non-obvious documented causes include URL casing differences and geo-redirection, and the published tracking fix is `{ignore}` before the tracking parameter in the final URL. Ad-to-page relevance appears nowhere on that list, and believing it does has caused a lot of pointless rewrites.
- **Destination experience does police page behaviour.** Named triggers include pop-ups or interstitials that interfere with seeing the requested content, sites that disable or interfere with the browser's back button, pages that do not load quickly, and links that initiate a direct download. Pop-up is defined maximally, "Any window that opens in addition to the original landing page is considered pop-up". Interstitials are allowed only "if they don't make it difficult for a user to leave a site".
- **Insufficient original content** names destinations "replicated from another source without adding value" and destinations "solely designed to send users elsewhere".

Two more. Destinations must be crawlable by AdsBot, and Google gives the reason directly, so it can check users reach a destination reflecting the ad they clicked, which makes a robots.txt block a disapproval risk rather than only a measurement gap. And no policy requires the page to be topically relevant to the ad, so say so when somebody calls a rewrite urgent for policy reasons.

**Meta.** Meta's named "Non-Functional Landing Page" policy no longer exists: the URL returned HTTP 404 on 31 August 2026, with the removal somewhere in 2025, so anything citing its Consistency or Interference guidelines describes a deleted page. What Meta publishes today is three sentences under Relevance, the third of which is the whole published landing page requirement: "The products and services promoted in an ad must match those promoted on the landing page." The destination is in scope for review. Meta's model is binary where Google's is graded, and the two halves of that are sourced differently. Live, on the standards page: "Lower quality ads which do not necessarily violate our policies may experience impacted performance." Archive only, from an April 2024 capture of an ad quality page that now blocks every fetch: "No. If we detect that an ad violates our Advertising Standards, we reject it." That same capture is the only source for low-quality attributes including "post-click experiences, including landing pages", and for repeated low quality leading Meta's systems to treat all ads from a Page, domain or ad account as lower quality.

**Speed, honestly.** Slow loading is a named Destination experience trigger, the strongest citable statement that speed can stop a Google ad running. But neither platform publishes a load-time threshold, documents Core Web Vitals as an ad factor, or names any coefficient linking speed to cost. LCP, INP and CLS are engineering targets, never a compliance claim in your changelog.

## Part three. The decision rule: rewrite in place, modular hero, or a dedicated page

This branch decides whether the rewrite helps or quietly damages nine other ads. Neither platform documents anything about ads sharing a destination, so the thresholds below are the skill's own. Establish two inputs. **N** is the number of live ads whose final URL is this page, counting every responsive search ad in every ad group and every ad in every Meta ad set. **Divergence** is how many of the six match points differ across them.

- **N is 1, or divergence is zero on points 1, 2 and 3.** Rewrite in place. This is the default.
- **N is 2 or more, the promise differs, the figure and offer name are identical.** Go modular: one page, only the hero block swapped, headline, subhead and one proof line, driven by a URL parameter set per ad group or ad set. The threshold that ends this branch: modular works while the divergent copy fits inside the hero plus one section. Once body sections have to change too, you are maintaining two pages inside one file.
- **The figure or the offer differs.** Build a dedicated page. A different price, trial length or deliverable is the one mismatch a reader catches instantly, and it reads as bait rather than as an inconsistency.
- **A dedicated page nobody can judge.** Before building one, check the ad will send it enough clicks for its own conversion count to clear the read floor you judge campaigns on. A page collecting a handful of sessions a month cannot be evaluated and will rot. That floor is a campaign-side number, set from budget and target cost per action, and it is not computed here.
- **The maintenance ceiling.** Every dedicated page has to stay live, stay crawlable and stay clear of the Insufficient original content triggers above, which is the real ceiling on how many you should build.

**The two you-cannot-tell branches, which fire often.**

- **The ad was not supplied.** Do not reconstruct it from the page or guess it from the product. Ask for the headline, the body, the CTA button label and the query or audience. If those cannot be produced, write the rewrite in slot form: full copy with the six match points marked as named placeholders, and a changelog line naming which words must be replaced with the ad's own before publishing. A page rewritten to continue an imagined ad is worse than the original, because it is now confidently specific about the wrong thing.
- **The page also serves organic traffic.** Check whether it sits in the sitemap, carries internal links, or shows search entrances. If any is true, do not rewrite it in place: organic readers arrive with no prior promise and need the opposite treatment, an explanation before a request, plus the heading structure the ranking depends on. Branch to a dedicated ad page and leave the original alone. If you cannot tell, assume it earns them: that assumption costs one extra page and the other costs a ranking.

## Part four. The form is part of the message match

**Field count against offer weight.** The number of fields is a price the reader pays, and it has to sit under the weight of what the ad promised. Neither platform publishes a field count, so the bands below are this skill's working defaults rather than anything documented.

- Something instantly consumable and free, a calculator, a template, a price: one or two fields by default. Email, plus at most one qualifier.
- A human, a demo, a quote, an audit: four to six fields is defensible on the same reasoning, because a reader understands that a person has to prepare something.
- A trial or a purchase: this is not a lead form. Any field not required to open the account or take the payment is friction with no story attached.

**The unannounced field**, again the skill's own rule and not a platform one. A field asking for something the ad never mentioned changes what the reader believes they agreed to. A phone number on a page whose ad promised a PDF reads as a sales call, and the reader is reading it correctly. Every field must trace to something in the ad, or carry its reason where it is asked, "we send the quote by text". A field that passes neither test is deleted, with the deletion in the changelog.

**The button.** The button verb is match point 3 and it is not negotiable. If the ad says "Get quote", the button says "Get quote", never "Submit", and never a verb that makes the reader re-decide at the moment they had stopped deciding.

**Two structural notes.** A multi-step form whose first step asks only what the ad already implied keeps the match at the point of highest doubt. And do not add an overlay. Google names no exit-intent modal, but the Destination experience triggers in part two do the work: a pop-up is any window opening in addition to the landing page, and an interstitial is allowed only while it does not make leaving difficult, which is the thing an exit-intent overlay exists to prevent. Removing the site navigation is a copy decision; interfering with the browser's back button is a named disapproval trigger.

## Part five. What to write, and in what order

Write these out in full, as final copy, in this order. It is the reader's order, and writing the form before the hero produces a form that asks for whatever the old page asked for.

1. **Hero headline.** The promise, in the ad's nouns, not the query's string.
2. **Subhead.** The qualifier and the figure, same number and same units as the ad.
3. **First proof line.** The type of proof the ad used, directly under the subhead.
4. **Opening section.** Answers the question the ad opened, in the order the ad raised it. If the ad led with a problem, this restates and answers it, rather than listing features.
5. **Form**, from part four, with deletions listed.
6. **Button**, carrying the ad's verb verbatim.
7. **Everything below**, with whatever survives named as untouched rather than silently rewritten.

Then the changelog: one row per match point giving the ad value, the old value, the new value and the reason, then the deletions and one line naming the branch taken.

## Worked example, compressed

A firm sells invoicing software to plumbing and heating contractors. Its live responsive search ad, on the query cluster "invoicing app for plumbers", shows the headlines "Invoicing Software For Plumbers", "Get Paid In 3 Days, Not 30" and "Free 14-Day Trial, No Card", with the description "Send an invoice from the van. Card and bank payment built in."

**Match points.** Promise: paid in 3 days instead of 30. Figure: a 14-day free trial, no card. Verb: start a trial. Offer name: free 14-day trial. Qualifier: plumbing and heating contractors. Proof type: none in this ad, which is itself a finding.

**Branch.** Three ad groups point at this URL: invoicing, job scheduling and quoting. The promise differs across them; the figure and offer name are identical in all three. The page is out of the sitemap and shows no organic entrances. That is the modular branch.

**Before, the hero as it stands:**

> The Operating System For Modern Trades Businesses
> Streamline your workflows, manage your team and grow your business with one connected platform.
> [Get started]
> Form: first name, last name, work email, company name, company size, phone number.

**After, the invoicing hero block:**

> Invoicing software for plumbers. Get paid in 3 days, not 30.
> Send the invoice from the van the moment the job is done. Card and bank payment are built in, so the money lands before you have driven home. Free for 14 days, no card needed.
> Used by independent contractors and teams up to 20 engineers.
> [Start my 14-day trial]
> Form: work email, and nothing else, because the ad promised a trial rather than a conversation.

**Changelog, abbreviated.** Promise: "operating system for modern trades businesses" gives way to the ad's own 3-days-not-30 line. Figure: 14 days and no card were absent, now in the subhead and on the button. Verb: "Get started" becomes "Start my 14-day trial". Qualifier: "trades businesses" becomes plumbers, as the ad has it. Proof: absent from the ad, so a modest specific line replaces a rating nobody claimed. Deletions: phone number and company size, in none of the three ads, plus the exit-intent overlay.

**Verdict.** Modular rewrite: one page, three hero blocks keyed to the ad group, shared below the fold. Four broken match points are now carried, two form fields deleted, one overlay removed. No dedicated pages built, because the divergence is confined to the hero.

## Failure modes

**The audit in disguise.** The output is a list of things wrong with the page and no page, so somebody writes the replacement copy later without reading the ad.

**Keyword echo.** The hero is rewritten to contain the search term verbatim, often dynamically inserted. It reads as machine-written, contradicts Google's own guidance, and the figure from the ad still appears nowhere.

**The number that moved.** The ad says 14 days and the page says 30, or the ad says $19 and the page says "from $24". This is the mismatch a reader catches without looking for it.

**The one-ad rewrite that breaks nine.** A page serving ten ads is rewritten hard against the best performer. That ad improves, the other nine mismatch worse than before, and the aggregate looks flat enough that nobody investigates.

**The doorway farm.** A dedicated page per ad, spun from one template with the noun swapped. It is the pattern Google's Insufficient original content policy names, and Meta's archived ad quality guidance puts low quality contagion at domain level, so it risks every campaign on that domain.

**Fixing content when the disapproval was structural.** A "Destination mismatch" notice triggers a copy rewrite. Every documented trigger for that reason is a domain, a subdomain, a redirect or a tracking template, so the copy changes nothing and the ad stays disapproved.

## What this skill does not do

- It does not write or edit the ad. If the ad overpromises, this method makes that more visible rather than less, and the fix belongs in the ad copy.
- It cannot see the page, only pasted copy, so it cannot measure load time, render at a phone width, or check that AdsBot can reach the URL.
- It cannot see the ad account, so the count of ads pointing at this URL is something you supply, and a wrong count makes the branch rule pick the wrong shape.
- It does not build anything: the output is copy and structure, not HTML, a template change, or a URL-parameter implementation for the modular branch.
- It does not know the live policy text on the day you read it, and neither help centre date-stamps articles.
- It is not conversion rate optimisation as a discipline: no test design, no traffic split, no session replay, and no claim about what the rewrite will do to your rate.
