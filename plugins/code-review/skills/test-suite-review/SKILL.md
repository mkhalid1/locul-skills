---
name: test-suite-review
description: Judges whether an existing test suite is worth what it costs, using fault detection, determinism, coupling to implementation and signal quality rather than a coverage percentage. Contains a named taxonomy of bad tests with the observable tell for each, the rules for separating a flaky test from a genuine intermittent bug, the determinism techniques that stop tests failing only inside a suite, and a six-axis scoring procedure with a mechanical probe per axis. This skill should be used when inheriting a codebase, when a suite has become slow or unreliable, when a coverage threshold is being proposed, or when deciding whether to repair or delete a body of tests.
---

# Test suite review

## The claim this skill is built on

A test suite is an asset with a running cost, and almost nobody measures either side of that.

The number everyone reaches for is coverage, and coverage measures the wrong thing in a specific and knowable way. It records which lines were executed while the tests ran. It has no opinion on whether anything was asserted about what those lines did. A test that calls every function in a module and asserts nothing produces the same coverage figure as a thorough one, and it will never fail while the code still compiles.

So coverage is one-directional evidence. **Uncovered lines are definitely untested**, and that is genuinely useful: a coverage report is a good list of places nobody has looked. **Covered lines are of unknown quality**, and treating them as tested is the mistake that produces suites of thousands of tests that catch nothing.

The question a suite has to answer is: if this code changed in a way that broke it, would something go red. That question has a direct measurement, and it is not coverage.

## What actually answers the question

**Mutation testing.** The tool makes a small change to your source, a comparison operator flipped, a boolean negated, a return value replaced, a statement removed, then runs the tests. If a test fails, the mutant is killed and your tests detect that class of fault. If every test still passes, the mutant survived, and you have found a specific change to your production code that nothing in your suite objects to. The mutation score is killed mutants over the total, usually excluding mutants that provably cannot change behaviour.

Two practical cautions. Some surviving mutants are equivalent, meaning the mutated code genuinely behaves identically, and detecting those in general is undecidable, so a perfect score is not a target. And a full run is expensive, because it is roughly the suite runtime multiplied by the number of mutants. The usable form is sampling: pick the three to five modules where a defect would hurt most, run mutation testing on those, and treat the result as a probe rather than a certificate.

**Coverage variants, briefly, because the distinctions matter.** Line coverage counts lines executed. Branch coverage counts each side of each decision, and is strictly more informative. Condition coverage looks inside compound conditionals. Modified condition and decision coverage, which requires demonstrating that each condition independently affects the outcome, is required for the highest criticality level of airborne software under DO-178C, and outside that world it is almost never worth the cost. If you are going to set a threshold on anything, set it on branch coverage, and set it as a ratchet that cannot fall rather than as a bar to clear.

## The taxonomy of bad tests

Each entry has a tell: something observable that identifies it in a codebase you did not write.

**The assertion-free test.** It exercises code and asserts nothing, so it can only fail by throwing. Tell: the test body contains no assertion call, or contains a call that builds an assertion without completing it, which most frameworks accept silently. This is searchable, and a search usually finds more than anyone expects.

**The test that mocks the thing under test.** The subject is replaced by a double, so the test verifies the double. Tell: the name of a mocked module matches the name of the file under test, or the only assertions are on whether a mock was called. This is the purest form of a test that cannot fail for a real reason.

**The change detector.** It fails whenever the code is restructured, regardless of whether behaviour changed. Tell: it asserts on the sequence of internal calls, on private structure, or on the exact shape of an intermediate object. The measurable version of the tell is the ratio of test lines changed to source lines changed on a commit that changed no behaviour. A refactor that breaks a hundred tests is a report about the tests.

**The time-coupled test.** It depends on wall-clock time or on the machine's timezone. Tell: the test or the code it calls constructs the current time directly rather than receiving a clock. Symptoms cluster on boundaries: it fails on the last day of a month, during a daylight saving transition, on 29 February, or for the one contributor whose machine is not on the same side of a date line as everyone else's.

**The order-dependent test.** It passes alone and fails in the suite, or the reverse. Tell: run the suite with randomised order. Go has `go test -shuffle=on`, added in Go 1.17, and most other runners have a shuffle or random-seed option. If a shuffle changes the result, you have an ordering dependency, and its usual cause is the next entry.

**The shared mutable fixture.** Tests share an object, a database row, a cache, a module-level singleton or an environment variable, and one of them mutates it. Tell: fixtures with a scope wider than a single test, module-level constants that are objects rather than primitives, and setup code that creates a record with a fixed identifier.

**The log-line assertion.** It asserts that a specific message was logged. Tell: capturing log output in a test. Two costs: it fails when anyone improves the wording, and it silently promotes your log messages into an API that nobody knows they must not change.

**The unread snapshot.** A recorded output blob, hundreds of lines long, regenerated whenever it fails. Tell: snapshot files updated in the same commit as a behaviour change with no discussion, or an update flag baked into a script. A snapshot nobody reads at review time asserts nothing, it merely records.

**The retry-until-green test.** A retry wrapper, a rerun plugin, or a loop with a sleep in it. Tell: any retry configuration in the test setup. This converts a signal into silence, and it hides the exact class of bug, a race, that is hardest to find any other way.

**The tautology.** It computes the expected value using the same function it is testing, so it passes for any implementation. Tell: the expected value in the assertion is derived rather than written as a literal.

## Flakiness

**Why it is urgent rather than annoying.** Failures compound across a suite. If each of 200 tests independently passes 99.9 percent of the time, the whole run is green only about 82 percent of the time, so roughly one run in five is red for no reason. At 1,000 tests the same per-test rate makes the run green only about 37 percent of the time. Long before you get there, people stop reading failures, and the suite stops working as a gate while still costing everything it costs.

**Separating a flaky test from a genuine intermittent bug.** This is the judgement that matters most, and the default assumption should be inverted from the common one: **a flaky test is more often a bug report about the system than about the test.** Work through it in this order.

1. Reproduce it in isolation with the recorded seed and the recorded order. If it fails alone, the non-determinism is inside the test or the code it calls, not in the interaction between tests.
2. If it only fails inside the suite, it is shared state or ordering, and the fix is isolation, not a retry.
3. If the failure involves concurrency, a timeout, a queue, a connection pool or a retry in the production code, treat it as a genuine bug until proven otherwise. A race that a test can hit under load on a busy machine is a race that production can hit.
4. If the failure is in an assertion about timing or ordering that was never guaranteed, the test asserted something the system never promised, and the test is wrong.

**Quarantine needs a deadline and a name attached.** A quarantined test that is nobody's job becomes a deleted test in six months, and the deletion happens without the conversation that should have accompanied it. The workable policy is: quarantine with an owner and a dated expiry, and on expiry either the test is fixed or it is deleted explicitly with a note saying what coverage was given up.

## The pyramid, and where it is wrong

The test pyramid, from Mike Cohn's Succeeding with Agile in 2009, says: many fast unit tests, fewer service tests, fewest end-to-end tests. Its logic is economic. Lower tests are faster, cheaper and more precise about where the fault is.

The honest critique is that the pyramid optimises for feedback cost, not for truth. It assumes the risk lives in your units. In a system whose complexity is in its wiring, which describes most modern applications, the risk lives between the units: in the object relational mapper's generated query, in the serialisation boundary, in the configuration, in the queue's delivery guarantees, in the identity provider. Unit tests over that architecture can all pass while the system does not work at all, and the resulting suite gives a confident green on a broken deployment.

Integration-heavy shapes are correct when: the code under test is mostly orchestration rather than calculation, correctness depends on a real database's behaviour with real constraints and transactions, the system is composed of many small deployable pieces, or the units are thin enough that a unit test is testing the framework. Integration-heavy shapes are wrong when the domain contains real computation, since a pricing engine, a scheduler or a parser deserves dense, fast, exhaustive unit tests, and testing those through the network is slow and imprecise for no gain.

## Names, messages and determinism

**A good test name contains three things**: the unit under test, the condition, and the expected outcome. "Test user" fails all three. "Rejects a signup when the email domain is on the block list" gives a reader the specification without opening the file, and turns the list of test names into documentation.

**A good assertion message also has three parts**: what was expected, what was actually observed, and the input or context that produced it. Most frameworks give you the first two automatically and none give you the third, which is the one that saves the debugging time. When a failure says only that false was not true, the reader has to reconstruct the whole scenario from the test body.

**Determinism techniques, concretely.**

- **Seed randomness explicitly and print the seed on failure.** Any test using random data must be reproducible from its output alone. Property-based testing tools do this by default, generating cases, shrinking a failure to its minimal form, and reporting the seed.
- **Freeze time by injection, not by patching globals.** Pass a clock into the code under test. Global time patching works until something you do not control also reads the clock, and it makes concurrent tests interfere. Where injection is impossible, freeze at a fixed instant with an explicit timezone, and pick an instant that is deliberately awkward, such as the last day of a month at 23:59 in a timezone with a half-hour offset.
- **Control concurrency by waiting on a condition, never on a duration.** A sleep is an assertion that the machine is not busy. Wait for the state you actually need, with a generous timeout.
- **Reset state between tests at the level where it lives.** A transaction rolled back per test handles the database. Module-level caches, singletons, environment variables and the working directory all need their own reset, and they are the usual cause of pass-alone, fail-in-suite.

## What is genuinely not worth testing

- **Framework and library behaviour.** A test that asserts the ORM writes a row is a test of the ORM. Test your query, not their engine.
- **Generated code**, unless you wrote the generator, in which case test the generator.
- **Trivial accessors and data holders** with no logic in them.
- **Exact log wording**, unless a machine parses it, at which point it is an interface and belongs in a contract test.
- **Third-party API behaviour.** Test your handling of their responses, using recorded interactions or a contract test, and do not assert on their live service inside your suite.
- **Private methods directly.** If a private method needs its own test, it usually wants to be its own unit with its own name.
- **Code with a scheduled deletion date.** Tests for code that will be gone in a fortnight cost more than they return.

## The scoring procedure

Six axes, scored 0, 1 or 2, for a maximum of 12. Each axis has a probe, so the score has a method rather than a feeling. Record the probe outputs alongside the numbers.

**1. Fault detection.** Probe: run mutation testing on the three to five modules where a defect would be most expensive. Score 2 above roughly 70 percent killed, 1 between 40 and 70, 0 below 40. This is the single most informative axis, so if you only have time for one, run this one.

**2. Determinism.** Probe: run the full suite three times with randomised order, then once with the machine clock shifted forward by six months and set to a different timezone. Score 2 for identical results every time, 1 for one or two failures, 0 for more.

**3. Coupling to implementation.** Probe: find the last three commits that changed no behaviour, refactors and renames, and compute the ratio of test lines changed to source lines changed. Score 2 below 0.3, 1 between 0.3 and 1, 0 above 1, where a suite that changes more than the code it tests is documenting structure rather than behaviour.

**4. Signal quality.** Probe: take five recent real failures and, from the failure output alone, without opening the test, try to name the cause. Score 2 if four or five are diagnosable, 1 for two or three, 0 for fewer.

**5. Cost.** Probe: measure the wall-clock time of the loop developers actually wait for, not the full pipeline. Score 2 under two minutes, 1 under ten, 0 above. Then check whether people are running it locally at all, because an unused fast suite scores worse in practice than its number suggests.

**6. Risk coverage.** Probe: list the eight ways this system would most plausibly hurt its users, from an incident history if one exists, and check which have a test that would catch them. Score 2 for six or more covered, 1 for three to five, 0 for fewer. This is the axis coverage percentages cannot see at all.

**Reading the total.** 10 to 12: the suite is an asset, protect the runtime. 6 to 9: it works, and the specific low axes tell you what to fix first. 3 to 5: it is providing confidence it has not earned, which is worse than having no suite, so fix axis 1 and axis 2 before writing any new tests. 0 to 2: consider deleting most of it and rebuilding around the risk list from axis 6, which is a legitimate outcome and is usually cheaper than repair.

## Worked example

An inherited service. 1,240 tests, 87 percent line coverage, a suite that takes 11 minutes, and a team that describes it as solid.

**Probes:**

1. **Fault detection.** Mutation testing on the three most important modules, the pricing calculator, the permission check and the retry handler. Killed 41 percent, 78 percent and 12 percent. The retry handler's tests turn out to mock the client they retry, so nothing about retrying is actually exercised. **Score 1.**
2. **Determinism.** Three shuffled runs produce 4, 2 and 5 failures, all in a group of tests that share a seeded account fixture. With the clock shifted six months and the timezone moved, another 9 fail, all in reporting code that formats dates against the local zone. **Score 0.**
3. **Coupling.** The last three refactor-only commits changed 380 test lines against 210 source lines, a ratio of 1.8. Most of the churn is in tests asserting call sequences on mocks. **Score 0.**
4. **Signal quality.** Of five recent failures, two were diagnosable from the output. The other three ended in an assertion that a boolean was not true. **Score 1.**
5. **Cost.** 11 minutes, and interviews reveal nobody runs it locally, they push and wait. **Score 0.**
6. **Risk coverage.** Of eight plausible user-visible failures, drawn from a year of incidents, three have tests that would catch them. The two most expensive incidents in that year were both permission errors, which is also the module that scored best on mutation testing, so the good tests are pointed at the right place. **Score 1.**

**Total: 3 of 12.**

**Verdict.** The 87 percent coverage figure is not describing a healthy suite, it is describing a suite that executes most of the code without checking it, and the retry handler is the clearest case: fully covered, mocked at the boundary that matters, mutation score of 12 percent. The order of work is fixed by the axes, not by taste. First, isolation: give every test its own account fixture and inject a clock, which addresses 13 known failures and stops the reruns. Second, delete rather than repair the retry handler's tests and rewrite them against a fake transport that can fail on demand. Third, ban assertions on mock call sequences in review, which is what is driving the refactor churn. Split the suite so that the unit tier runs in under two minutes locally and the slower tier runs in the pipeline. Do not raise the coverage threshold, and do not add tests, until axis 2 is at 2, because tests added to a non-deterministic suite inherit its noise.

## Failure modes

**The coverage target.** A percentage is set as a gate, so tests get written to cover lines rather than to detect faults, and assertion-free tests appear because they are the cheapest way to move the number. Symptom: coverage climbing steadily while escaped defects do not fall.

**Retry as a fix.** A flaky test gets a rerun wrapper and the failure stops being visible. Symptom: a suite that is green and a production incident whose cause, when found, matches a test that was retried into silence months earlier.

**Mock-shaped tests.** Everything is doubled, so the tests describe the design rather than the behaviour, and they all have to be rewritten whenever the design changes. Symptom: the refactor that touches no behaviour and breaks two hundred tests.

**The unread snapshot.** Snapshots are regenerated as a reflex when they fail. Symptom: a commit that changes behaviour and a snapshot in the same diff, approved without a comment on the snapshot.

**Suite decay by runtime.** The suite crosses the threshold where people stop running it locally, so it stops shaping the code as it is written and becomes a late gate. Symptom: pull requests that fail on the first pipeline run as a matter of routine.

**Quarantine as a graveyard.** Tests are disabled with a promise to return, no owner, no date. Symptom: a skip list that only grows, containing tests nobody can now say what they covered.

**Fixing the test instead of the system.** An intermittent failure in code involving concurrency or timeouts is made to pass by widening a tolerance. Symptom: timeouts and retry counts in tests that have been raised more than once, each time by a different person.

## What this skill does not do

- It cannot tell you whether the tests assert the correct behaviour. A well-built suite that encodes a misunderstanding of the requirements scores well on every axis here.
- It does not measure fault detection itself. It tells you to sample it with a mutation testing tool and to believe that tool over any judgement it offers.
- It says nothing about test data management, environment parity or fixture provisioning, which are frequently the real reason a suite is slow and unreliable.
- It does not cover performance, load or security testing, whose economics and failure modes are different subjects.
- It will not tell you how many tests to have. The scoring procedure deliberately has no axis for quantity, because quantity is the number that gets gamed.
