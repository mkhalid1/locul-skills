---
name: activation-first-run-design
description: Designs a product's first run so a new user reaches real value in one session, starting from an activation event that is proved against retention rather than assumed. Runs the retention-correlation test that decides whether a candidate event qualifies, marks the definition provisional when the data cannot yet support it, chooses between freemium, trial and demo on marginal cost and time to value, seeds first-run empty states instead of explaining them, and instruments every step of the path separately. This skill should be used when designing or rebuilding onboarding, when choosing an access model, when activation is rising while retention is flat, or when a first-run experience needs specifying before it is built.
---

# Activation and first-run design

## The claim this skill is built on

Activation is a behaviour that predicts retention. It is not a screen, a percentage complete, or the end of a tour. If the event you call activation does not separate the users who stay from the users who leave, every improvement to the first run is optimising a number that carries no information, and the number will obligingly go up.

The failure has a recognisable shape. A team picks the milestone easiest to instrument and spends two quarters raising it. Signup-to-activation goes from 34 per cent to 51 per cent, retention at week four does not move, and nobody can say which change caused the rise, because there is one blended number, or why retention ignored it, because the definition was never tested against retention.

The obvious approach fails in three places at once. It defines activation by what is convenient to instrument rather than by what correlates with staying. It substitutes explanation for value, so the answer to an empty screen is a description of what could be there. And it charges a stranger a toll of fields and decisions before anything has been demonstrated in return.

The output is a first-run design in six parts: an activation definition carrying either its evidence or the word provisional, an access-model decision with both inputs stated, a path with its required decisions counted, a seeded first-run state, an instrumentation list, and return triggers keyed to where users stopped.

## Output shape and why the order is fixed

Write one document per product at your repository's conventional location for planning material, for example `docs/activation/2026-08-20-first-run.md`. Use an ISO date prefix, which sorts correctly on every operating system, and forward slashes in any path, which both Windows and Unix-like tooling accept.

The six sections follow the steps below, and that order is load-bearing. The definition comes first because it is the target every later decision optimises towards. The access model comes second because it decides what the path may ask for. The path precedes the first-run state, since you cannot seed a screen until you know which screen is reached first. Instrumentation follows seeding, because seeded records must be excluded from the events and a team that instruments first forgets. Return triggers come last, keyed to steps that must already have names.

## Step 1. Write three to five candidate activation events

A candidate is a behaviour, observable in your event data, that the user performs rather than receives. Generate candidates from the core job, not from the funnel diagram. Three tests before one goes on the list:

- **It is an act of production or consumption of value**, not configuration. Connecting a data source is configuration. Reading the first report built from it is value.
- **It is repeatable.** Something a retained user does again next week. A one-time setup step can correlate beautifully and still be useless as a design target, because you cannot ask for it twice.
- **It is instrumented, or can be within a week.** A candidate you cannot measure is a hypothesis, and hypotheses do not go in the definition.

Include one deliberately weak candidate, normally the one the team currently uses. A test that only ever confirms is not a test.

## Step 2. The retention-correlation test, which decides whether a candidate is activation at all

This is the empirical core of the skill and the step that gets skipped, because opinion is faster and reads the same on a slide.

**The split.** Take one date-bounded signup cohort, big enough to meet the floor below, and split it into two arms: users who performed the candidate behaviour inside a fixed observation window, and users who did not. Keep that window identical across every candidate, otherwise you are comparing behaviours and windows at once. The default is the first session for products used ad hoc, the first seven days for products with a weekly rhythm.

**The horizon.** Compare the share of each arm still active at a horizon of **at least three natural usage cycles**, where the cycle is set by the core job rather than the calendar. A tool used daily has a cycle of a day, so week four is fair. A tool used at month end has a cycle of a month, so the honest horizon is 90 days and a 30 day read flatters everything. A horizon shorter than three cycles is the most common way to make a weak candidate look strong, because the first cycle measures novelty.

**What passes.** Adopt the candidate satisfying both:

1. A gap of **at least 20 percentage points** in retained share at the horizon.
2. Curves **still separated at the horizon** rather than converging towards it. Plot both arms at every cycle. Arms 30 points apart at cycle one and 6 points apart at cycle three have measured enthusiasm, and enthusiasm decays.

Rank passing candidates by gap multiplied by **coverage**, the share of the cohort that performed the behaviour. A 45 point gap performed by 4 per cent of users is still worth adopting, but say so, because a rare winning behaviour means the path work is large rather than small.

**The sample floor.** To separate a 20 point difference around a base of 20 to 40 per cent, at conventional 80 per cent power and 5 per cent significance, you need roughly **80 users per arm**. To separate a 10 point difference at the same base, roughly **350 per arm**. Below **30 per arm**, do not compute the comparison at all: a difference will appear, it will be noise, and it will be quoted for a year.

**The confound.** This test establishes correlation. The users who perform the behaviour may simply be the ones who arrived with more intent. Two cheap checks reduce the risk of acting on a marker as though it were a cause. First, compare the arms on something that happened **before** the behaviour, such as acquisition source or account size; if the did arm is almost all search traffic and the did-not arm is almost all a viral campaign, the gap is about the traffic. Second, ask whether the behaviour plausibly **installs the product into a workflow**: behaviours that create something the user would lose, involve a second person, or connect to a system already relied on are causally plausible, and behaviours that are merely thorough, such as completing a profile, are not.

Record the winner as: the behaviour, the observation window, the horizon, both arm sizes, both retained shares, and the query date.

**Decision rule, including the branch where you cannot tell.**

- **80 or more users per arm at the horizon:** run the split, adopt the passing candidate with the highest gap times coverage, and write the numbers into the definition.
- **Between 30 and 80 per arm:** run it, report it as directional, and adopt only if the gap exceeds 30 points. Put the arm sizes next to the claim so the next reader can discount it.
- **Fewer than 30 per arm, no historical instrumentation, or a product younger than three usage cycles, so you cannot tell:** do not guess, and do not present a guess as a finding. Pick the candidate closest to the core job and write it as *provisional activation definition, set on this date, chosen because it is the core job, to be tested when 80 users per arm exist at the 90 day mark, review date set*, naming the exact events that will settle it. A provisional definition is a legitimate output. A guess presented as evidence is not, and it is hard to undo, because the number reaches a slide and the slide gets reused.

## Step 3. Choose the access model on two axes

Not on preference, and not on what the nearest competitor does. Two inputs.

**Axis one: the marginal cost of one additional free user.** Add variable infrastructure (compute, storage, egress), any per-user fee paid to a third party, any human cost such as review or moderation, and expected support minutes at a loaded hourly rate. Express it per free user per month.

**Axis two: is first value reachable inside one session?** One session means under roughly 15 minutes of continuous attention, no dependency on a second person acting, and no wait longer than the session itself. A crawl, an overnight sync, a batch import, a verification chain through an administrator, or a colleague's approval all take the answer to no.

The branches:

- **Low marginal cost and one-session value: freemium.** Bound the free tier on a dimension tracking the value the user receives, so the limit arrives once the product has proved itself. Bounding on a dimension that tracks your cost instead is a common inversion, since it caps the users getting most out of it soonest.
- **High marginal cost, or multi-session value: a time-boxed trial**, its length set by usage cycles rather than by a round number. The trial must contain **at least two full cycles of the core job**. If the job is monthly, a 14 day trial cannot contain one cycle, which is why extension requests always arrive and why sales ends up granting them one at a time.
- **Neither cheap nor reachable in one session: an assisted trial or a demo**, with a stated qualification bar so human time is not spent on users who cannot buy.
- **A hybrid worth naming: the reverse trial.** Full paid capability for a fixed period, then automatic downgrade to a permanent free tier rather than a wall. It costs more per free user than plain freemium, so it belongs on the low-cost side of axis one.

**If you cannot tell**, because nobody has computed the marginal cost, compute it: the estimate above takes an hour. If you still cannot get within a factor of two, default to a time-boxed trial. It bounds exposure while you learn, and moving from a trial to a free tier later is a welcome announcement, while the reverse is a public retreat that costs you exactly the users who advocated for you.

## Step 4. Move account creation after first value where the product allows it

Every field asked before value is a toll charged to a stranger who has been given nothing.

Where the first unit of work can happen in local or anonymous server state, let it, and ask for the account at the moment the user has something to lose: saving, sharing, exporting, or returning to it.

This works only if the anonymous session's artefacts can be claimed by the account afterwards, and that is a schema decision rather than a front-end one. It needs a nullable owner column or a reassignable anonymous session identifier, plus a retention rule deleting unclaimed sessions after a stated number of days. Retrofitting it later is close to impossible, which is why it belongs in the design document rather than the backlog.

Where deferral is genuinely impossible, because server-side compute is expensive, abuse exposure is real, or the account is itself the product, do not defer: **reduce**. Cut signup to the minimum credential set and move every other field into the product, asked in context, at the moment it changes what happens, and skippable. A field that decides nothing in the first session has no business in front of it.

## Step 5. Cut the path to the smallest number of required decisions

Count the decisions between arrival and the activation event. Each one is a place to leave.

A decision earns a place only if all three are true: a wrong answer changes what happens in this session, it cannot sensibly be defaulted, and it is reversible in the product without contacting support. Anything failing the third test either becomes reversible or moves after activation. Workspace URLs, data regions and billing currencies are the usual offenders, and they are usually placed first because they are ordered by what the database needs rather than by what the user can answer.

Aim for **three or fewer required decisions** before the activation event. The recurring offenders to remove: choosing a plan before value, naming a workspace before there is anything in it, inviting colleagues before you can tell them what they are joining, choosing from twenty templates, and connecting an integration this session will not use.

## Step 6. Seed first-run empty states, do not explain them

An empty state that describes what would be here is a dead end. One containing a real, editable example is a starting point.

There are three empty states and only one may be seeded:

- **First-run empty**, where the user has never had data. Seed this one.
- **User-cleared empty**, where data existed and now does not. Never seed it. Re-populating a screen someone deliberately emptied reads as a bug, at the exact moment they were expecting confirmation that the deletion worked.
- **Filtered-to-zero**, where data exists but this view has none. Never seed it. An example inside a filtered result looks like data corruption and destroys trust in every other number on the screen.

Rules for the seed:

1. **It renders through the same components and schema as real data.** A special-cased illustration teaches nothing about the product.
2. **It is editable in place**, and editing it is the shortest route to the activation event.
3. **It is removable in one action**, and removing it leaves the user-cleared empty state, which shows exactly one primary action.
4. **It is labelled as an example**, quietly, in the row itself. Not in a modal, because a modal is an explanation covering the thing it explains.
5. **It is excluded from every count**, both the counts the user sees and the events you record. This rule protects step two: a seeded record counted as a created record inflates activation the day seeding ships, leaves retention untouched, and quietly invalidates the definition you tested.

Seeding is wrong where the entire value is the user's own real data, such as a connected bank account or a live calendar, since a fabricated example there misleads. Shrink the import instead: bring in one item, show it working, then ask for the rest. If you want a tour as well, keep it to at most three inline pointers, each naming the single next action, each dismissible, none covering the seeded content.

## Step 7. Instrument every step separately

**The rule: the number of instrumented events on the path equals the number of required decisions plus one**, the plus one being the activation event.

A single signup-to-activation rate tells you there is a problem and never where. Two teams reporting 38 per cent are in completely different situations if one loses everybody at a permissions prompt and the other bleeds evenly across six screens.

Name each event for the user-visible state reached, not for the component that fired it, because components get renamed in a refactor and the historical series then breaks silently. Carry the session identifier, the timestamp and the acquisition source on every event, so step-level rates can be cut by source, which is where most surprises live.

Compute two things per step: step-to-step conversion, and the median time between steps. The second finds a different class of problem, since a step everyone completes and everyone takes four minutes over is confusion rather than loss. Distinguish **did not arrive** from **arrived and failed**: the first is a routing or motivation problem upstream, the second is usually a defect fixable in a day.

## Step 8. Key the return trigger to the step reached

A generic reminder is addressed to nobody. The trigger names the step the user stopped at, offers the single next action, deep links back into that state, and finds the work as it was left.

Send the first inside 24 hours while the intent still exists, space the rest by one usage cycle, stop at three, and carry a suppression rule on every one: stop on activation, and stop if the step they reached is currently broken, because chasing users towards a defect converts a poor first session into an unsubscribe.

One case worth designing for: the blocked step sometimes belongs to a **different person**. When the path needs a second party to act, the message that unblocks the first user goes to the second one.

## Step 9. Put a review date on the definition

Definitions drift. The product changes underneath one, a new segment arrives with a different core job, and the definition quietly optimises for a workflow that is now a minority.

Re-run step two quarterly, and immediately when the core job changes, when a new segment passes roughly a fifth of new signups, or when the access model changes. The symptom of a stale definition is easy to watch for: **activation rising while retention at the horizon stays flat**, which means the event has drifted away from the behaviour it once marked.

## Worked example, compressed

**Product:** an expense-claims tool for small companies. **Core job:** an employee submits a claim with a receipt and an approver signs it off, monthly. **Cohort:** 640 signups over one quarter, with a year of event history.

**Candidates and the split**, observation window seven days, horizon 90 days, which is three monthly cycles:

| Candidate | Did arm | Did not arm | Arm sizes | Verdict |
|---|---|---|---|---|
| Completed the product tour | 61 per cent | 55 per cent | 210 / 430 | Rejected. Six point gap, and the arms were 22 points apart at cycle one, so it converged. |
| Uploaded a receipt | 58 per cent | 41 per cent | 254 / 386 | Rejected. 17 points, under the threshold. |
| Submitted a first claim | 66 per cent | 24 per cent | 188 / 452 | Passes. |
| Claim approved by a second person | 71 per cent | 24 per cent | 114 / 526 | Passes. 47 points, still separated at cycle three. |
| Connected a bank feed | not computed | not computed | 26 / 614 | Below the 30 per arm floor. Not adopted, not discarded, marked untested with the events that would settle it. |

**Confound check:** both arms of the winning candidate had similar acquisition mixes, and the behaviour involves a second person, which is causally plausible. Coverage is 18 per cent, so gap times coverage favours the approval event over the submission event despite the smaller arm.

**Definition adopted:** a claim submitted by one person and approved by a second within the first 14 days, recorded with both arm sizes, both retained shares and the query date.

**Access model:** marginal cost per free user is low, a few pence of receipt storage, but value needs a second person to act, so it is not reachable in one session and axis two forces the trial branch. The cycle is monthly, so the trial spans two: 45 days, or better a usage bound of the first 20 approved claims, which spans cycles without punishing a company whose month end is awkward.

**Path:** seven required decisions cut to three, being company name, first approver, and the first claim. Plan choice, currency, category customisation and colleague invitations all moved after activation, with currency made reversible so it could move.

**First-run state:** one seeded claim, already approved, editable in place, deletable in one action, labelled as an example in the row, excluded from the reimbursement total and from the activation event.

**Instrumentation:** four events, three decisions plus the activation event, each named for the state reached, each carrying session and source. **Return trigger:** the largest drop is claims submitted whose approver never acted, so the trigger goes to the **approver**, naming the pending claim and linking straight to the approval action.

**Verdict:** the adopted definition is evidence-backed at 114 and 526 users per arm with a 47 point gap that persists to 90 days, one candidate stays labelled untested rather than adopted, the access model resolved to a 45 day trial on axis two rather than on preference, required decisions fell from seven to three, four steps are instrumented separately, and the first-run screen holds an editable object rather than a description of one.

## Failure modes

**The vanity milestone.** From outside: the activation chart rises quarter after quarter and the retention chart is a flat line beside it. The definition was never tested, so the work was pointed at a number carrying no information.

**The tour standing in for value.** High tour completion, near-zero use of the same feature in week two, and support tickets asking how to do the thing the tour demonstrated.

**The toll gate.** Paid traffic converts far worse than organic and the sales team calls the leads low quality. The fields before value are filtering for patience rather than fit.

**The dead-end empty state.** Session recordings show users landing on the main screen, reading, and leaving inside a minute with no click. The screen explained itself perfectly and offered nothing to act on.

**The blended number.** One activation rate reported monthly, six months of experiments, no movement, and no two people agreeing which step is the problem. The rate was never wrong, just never locatable.

**Seeded data counted as real.** Activation jumps the day seeding shipped, retention does not, and the definition now includes users who did nothing. Corrosive, because it corrupts the measurement you would use to notice it.

**Correlation acted on as cause.** The team makes the winning behaviour mandatory, activation approaches 100 per cent, retention is unchanged. The behaviour marked intent, and forcing a marker does not transfer the intent.

**The stale definition.** Activation up, retention flat, and a growing segment whose core job the definition does not describe. It tends to appear two quarters after a positioning change nobody counted as a product change.

## What this skill does not do

- It cannot run the query. It specifies the split, the window, the horizon and the floor, and someone with access to the event data has to execute it. Until then the output is a provisional definition.
- It does not establish causation. It finds a behaviour that travels with retention, which is a design target rather than a mechanism.
- It is not interface design. No layout, no copy, no interaction detail beyond what an instrumented step can express.
- It has nothing to say about price levels. It picks between freemium, trial and demo on two inputs, and the number on the page is decided elsewhere.
- It cannot raise a retention ceiling set by the product itself. If the product stops being useful in month two, a better first run fills a leaking bucket faster and the bucket still empties.
- It will not help below a few hundred signups. At that size the sample floor bites, and five recorded sessions plus five conversations is the better instrument.
