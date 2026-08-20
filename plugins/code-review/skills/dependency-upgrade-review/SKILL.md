---
name: dependency-upgrade-review
description: Reviews a dependency upgrade by reading the lock file diff rather than the manifest, separating direct bumps from the transitive packages they drag in, and screening every changed package against the registry metadata that predicts trouble: maintainer changes, new install scripts, install size jumps, and missing provenance. Covers npm, Python and Go modules, including why Go's minimal version selection makes the same review a different job. This skill should be used when reviewing a pull request that changes a manifest or a lock file, when triaging a vulnerability advisory, or before merging an automated dependency bot's pull request.
---

# Dependency upgrade review

## The claim this skill is built on

A version number is a claim made by a stranger about their own work, and nothing enforces it.

Semantic versioning is a social promise. The publisher decides, alone and without review, whether their change was a patch or a minor. There is no test that fails when they get it wrong, no registry check that rejects the publish, and no notification to you when the promise turns out to be false. The result is that the risk in an upgrade correlates only weakly with the position of the digit that changed.

The obvious approach, reading the manifest diff and checking whether the leading number moved, therefore fails in both directions at once. It waves through a patch that changed a default value, and it blocks a major that only dropped support for a runtime version you stopped using two years ago.

The review that works reads a different artifact. The lock file is the only file that describes what will actually be installed, and the registry metadata is the only place the publisher's behaviour is visible. Everything below is about reading those two things properly.

## Part 1. The four ways semver breaks, with the shape of each

Learn these as a catalogue, because you will not derive them under time pressure.

**The patch that changes a default.** A configuration option flips from off to on, a timeout goes from infinite to thirty seconds, a parser starts rejecting input it used to coerce. The publisher considers this a bug fix, because the old default was arguably wrong. Every consumer relying on the old behaviour is broken by a patch. The tell is a changelog entry containing the words "now defaults to" or "no longer".

**The minor that tightens a peer range.** A plugin publishes a minor release that narrows its peer dependency from a wide range to a narrower one. Nothing about the plugin's own API changed, so a minor is defensible. But since npm 7, peer dependencies are installed and enforced rather than warned about, so a narrowed range in one plugin can make the whole tree unresolvable, and the failure appears as an install error naming packages nobody touched.

**The type-only break in a patch.** Under the DefinitelyTyped convention, the major and minor of a `@types` package track the library it describes, which leaves the patch position as the only place changes to the typings themselves can go. A stricter type, a removed overload, or a union narrowed to one member all ship as a patch. The compile error looks like your code broke. It did not.

**The major that is not one for you.** Dropping an old runtime, removing a deprecated export you never imported, changing a build target. These are honestly labelled majors that carry no work at all. Treating every major as a project is how upgrade backlogs become unpayable.

## Part 2. Read the lock file, not the manifest

The manifest says what you asked for. The lock file says what you get. When they disagree, the lock file wins at install time, so it is the diff you review.

**Separate direct from transitive before reading anything closely.** A direct bump is a package your manifest names. A transitive bump is a package that arrived because something else wanted it. One direct bump commonly moves dozens of transitive entries, and those entries are where the majors hide: your direct dependency moved by a patch, and inside its tree a package crossed a major boundary because the resolver was free to take it.

Count three things and write them at the top of the review:

1. how many direct entries changed,
2. how many transitive entries changed,
3. how many packages are **new to the tree entirely**, meaning they appear in the lock file after and not before.

The third number is the one that matters most for supply chain risk, because a package that was never installed before has never been looked at by anyone on your team, and its install scripts have never run on your machines.

`npm explain <package>` prints the chain that pulled a package in, which turns "why is this here" into one command. `npm ls <package>` shows every version of it present in the tree at once, which is how you find out you now ship two copies of the same library.

**Lock file formats change and the diff lies about size.** npm has shipped three lock file formats: version 1 from npm 5 and 6, version 2 from npm 7, which carries both the old and the new representation of the same tree, and version 3, which drops the legacy block. A repository whose lock file was regenerated by a different npm version produces a diff of thousands of lines containing no dependency change at all. Check `lockfileVersion` at the top of the diff before you conclude anything from its size.

## Part 3. Screen the packages, not the versions

For every package that is new to the tree, and every package whose publisher may have changed, run this screen. It is six registry lookups and it takes a couple of minutes.

- **Maintainer set.** Compare the maintainers of the previous version and the new one. A change of hands on a package with millions of weekly downloads is the single highest-signal event in this list. It is usually benign, since maintainers do hand over packages they no longer use, and it is also the shape that the best-documented registry compromises have taken: an established package is transferred, and a release soon after carries a payload that is present in the published tarball but not in the source repository.
- **Install scripts.** A `postinstall`, `preinstall` or `install` script that was not there before is a change in what running `install` does to your machine. Very few libraries need one. The legitimate cases are native compilation and binary downloads, and both are visible in the package's own description of itself. Installing with scripts disabled is a real defensive option for a large class of dependencies.
- **Published size and file count.** The registry exposes `dist.unpackedSize` and `dist.fileCount`. A library that grew from tens of kilobytes to several megabytes between two patch releases has not fixed a bug. Compare the numbers rather than the changelog.
- **Files that are not source.** Minified or bundled files in the published tarball, when the previous version shipped readable source, means the tarball can no longer be compared to the repository by reading it. `npm pack` gives you the tarball to inspect, and `npm diff` compares two published versions directly, which is a much better basis than the repository's own diff.
- **Provenance.** npm publish gained a provenance option in 2023, which attaches a signed attestation linking the published artifact to the source repository and the workflow run that built it. A package that had provenance on the previous version and does not on this one is a specific, checkable regression, and it is the clearest available evidence that a release did not come from the usual pipeline.
- **Repository identity.** Compare the `repository` field between versions. A package now published from a different repository, or from a fork, is worth a question even when everything else looks ordinary.

## Part 4. What an advisory actually tells you

`npm audit` matches the versions in your tree against advisory ranges. That is all it does. It knows nothing about whether your code can reach the vulnerable function, nothing about whether the input that triggers it crosses a trust boundary, and nothing about whether the package runs in production at all. Its severity comes from the advisory's own score, which was assigned for the worst plausible consumer rather than for you.

Two consequences follow, and both are routinely got wrong.

**A dev dependency advisory is usually not the emergency it looks like.** A vulnerability in a test runner or a bundler plugin is not reachable by your users, because your users never send input to your test runner. It is not zero risk, since build machines hold deploy credentials and a build-time compromise is a serious event, but it is a different risk with a different urgency. Split the audit output by whether the package is present at runtime or only at build time before you decide anything. `--omit=dev` gives you the runtime-only view.

**`audit fix --force` is a semver-major upgrade wearing a safety label.** It will happily move a direct dependency across a major boundary to clear an advisory, which is a larger change than the one you were reviewing.

The two questions that actually decide urgency are: is the vulnerable code path reachable from input you do not control, and does the package execute at build time or at runtime. Answer those two and the severity number stops mattering.

## Part 5. Pinning, integrity and the pipeline

**Exact pins versus ranges** is not a style question, it is a question about who you want to be surprised. Exact pins mean the tree changes only when a human changes it, and it means you carry the whole upgrade burden explicitly. Ranges mean fresh transitive fixes arrive without work, and it means a stranger's publish can change your build. The workable position for an application is: ranges in the manifest, an exact tree in the lock file, and a bot generating the upgrades. For a published library the position is the opposite, because your ranges become your consumers' constraints, and a narrow range in a widely used library causes resolution conflicts for thousands of people.

**`npm ci` and `npm install` are not interchangeable in a pipeline.** `install` may modify the lock file to satisfy the manifest, which means a build can silently install a tree that nobody reviewed and that no longer matches what is in version control. `ci` installs exactly the locked tree, fails if the manifest and lock file disagree, and never writes to the lock file. A pipeline running `install` has no reproducibility guarantee at all, and this is the single most common way a "we pin everything" policy turns out to be false.

## Part 6. Ordering, when packages are coupled

When several packages must move together, the order changes whether the intermediate states resolve.

Upgrade in this order, one commit per coupled set:

1. **The runtime**, if it is moving at all, since everything else's support matrix is expressed against it.
2. **The core package** that the others declare as a peer.
3. **The plugins and adapters** that peer on it.
4. **The type packages**, last, because they describe the result of the previous three.

Doing this in any other order produces failures that point at the wrong package. Upgrading a plugin before its core gives an unmet peer error naming the core, which invites you to upgrade the core in the same commit and lose the ability to bisect. If the set genuinely cannot be split, say so explicitly and make the commit message list the members, because the next person to bisect a regression needs to know that this commit is atomic by necessity.

## Part 7. The same review in other ecosystems

**Python.** The default tooling has no lock file. A `requirements.txt` of exact pins is close, but it does not record hashes unless you ask, and pip's hash-checking mode is all or nothing: once any requirement carries a `--hash`, every requirement must. Without hashes you have pinned a name and a version, not an artifact. Newer tooling closes this: uv, which arrived in 2024, produces a cross-platform `uv.lock`, and PEP 751 standardises a lock file format, `pylock.toml`, so the ecosystem is converging on something npm has had for years.

The Python-specific hazard is install-time code execution. Installing a wheel unpacks an archive and runs nothing. Installing a source distribution runs the project's build backend, which is arbitrary code, on your machine, as you. So "no wheel available for this platform" is not only a convenience problem, it changes the trust model of the install. Check whether the new version publishes wheels for your platforms.

The resolver matters too: pip's backtracking resolver became the default in pip 20.3 in late 2020, and before that pip would happily install a tree with conflicting requirements. A project whose pins were chosen under the old resolver may not resolve at all under the new one, which surfaces as a long backtracking stall rather than a clear error.

**Go modules behave genuinely differently, and this is worth understanding rather than skimming.** Go uses minimal version selection: the version built is the maximum of the minimum versions required across the whole module graph, not the newest version available. Nothing floats. Adding a dependency does not change any other dependency's version unless that dependency's own requirements force it. This means a Go build is reproducible from `go.mod` alone, and it means upgrades are always deliberate acts rather than a consequence of when you ran install.

Two things follow that people get wrong. First, `go.sum` is not a lock file. It is a set of expected cryptographic hashes for module content, including for modules and versions that are not selected, and it is verified against the public checksum database by default. It answers "is this the same code everyone else got", not "which version am I on". Second, because there is no floating, a stale Go dependency stays stale silently for years. `go list -m -u all` is the command that shows what upgrades exist, and it needs to be run deliberately.

Go's advisory tooling is also better than range matching. govulncheck performs call graph analysis and reports only vulnerabilities whose affected symbols your code can actually reach, which is the reachability question that npm audit cannot answer. Since Go 1.16 the build no longer edits `go.mod` implicitly, so a build that would need a dependency change fails instead, which is the behaviour you want in a pipeline.

## Decision rules

**Should this upgrade be merged today?**

- **If** the change is a patch or minor within one direct dependency, the lock file diff touches no new packages, and the test suite covers the code paths that dependency serves, **then** merge it. Reviewing this is more expensive than reverting it.
- **If** a package is new to the tree, or a package changed maintainers, or an install script appeared, **then** hold and run the Part 3 screen before anything else. This is the branch where the cost of being wrong is not a failed build.
- **If** the bump is transitive only but crosses a major boundary inside the tree, **then** find the code path that reaches it. A transitive major is exactly as dangerous as a direct one and gets a fraction of the attention.
- **If you cannot tell** whether the vulnerable path is reachable, and you often cannot, **then** decide on placement instead of reachability. A package that executes at build time on a machine holding deploy credentials is treated as reachable. A runtime package that touches input crossing a trust boundary is treated as reachable. If neither is establishable, take the upgrade when it stays inside the current major and the lock file diff is small, and open a dated ticket with the specific question you could not answer when it is not. Do not record "probably fine" as a decision, because nobody can audit it later.

**Is this package worth keeping?** Look at the metadata rather than the readme. No release in eighteen months on a package whose ecosystem moves, an open issue count that grew steadily while closed issues did not, a single maintainer whose other packages are also quiet, and a deprecation notice on the registry entry are each ordinary on their own and damning together. The registry carries a deprecation flag and a per-version publish timeline, so this is a lookup and not an impression.

## Worked example

A bot opens a pull request titled "bump the HTTP client from 4.2.1 to 4.2.3". The manifest diff is one line.

**Lock file diff:** 1 direct entry, 12 transitive entries, and 2 packages new to the tree.

**Direct:** the HTTP client, patch to patch. Its changelog says redirect handling "no longer" forwards the `Authorization` header across origins. That is the patch-that-changes-a-default shape, and it is a behaviour change for anyone whose internal service sits behind a redirect. It is also a security fix, so it is correct, and it will still break someone.

**Transitive:** eleven of the twelve are patch moves inside the same majors. The twelfth is a URL parsing library that moved from 2.9.4 to 3.0.1, a major, pulled in because the client widened its range.

**New to the tree:** two packages. One is a polyfill with no scripts, 14 kB, provenance attached, same maintainer as the parent. The other has no provenance, ships a `postinstall` script that was not present in its own previous version, and its unpacked size went from 22 kB to 1.9 MB between two patch releases. Its published tarball contains a minified bundle where the previous version shipped readable source.

**Advisory check:** one moderate advisory clears in the direct bump. It is on a runtime path, so it counts.

**Verdict: hold.** Not because of the version numbers, all of which are innocuous, but because a package new to the tree gained an install script, lost provenance, grew by two orders of magnitude and stopped shipping readable source, all in a patch. The three asks are: pin that transitive package to its last known-good version or vendor the parent's dependency on it, confirm whether any internal call relies on the redirect behaviour the direct bump removes, and split the URL library's major into its own commit so the two changes can be bisected apart. The rest of the pull request is fine and can go in today.

## Failure modes

**Manifest-only review.** The reviewer reads the one-line manifest diff and approves. Symptom: a build breaks days later on a package nobody remembers adding, and `git log` on the manifest shows nothing relevant. The change was always in the lock file.

**Major-phobia.** Every major is treated as a project, so upgrades stall, and the project ends up several majors behind across the board, at which point each upgrade genuinely is a project. Symptom: an upgrade backlog that only grows, and a security fix that cannot be taken because it lives four majors ahead.

**Audit-score theatre.** Work is prioritised by the advisory's severity label rather than by placement and reachability, so a build-time advisory gets an emergency and a runtime one waits. Symptom: an incident channel full of dev dependency findings and a genuine reachable issue three pages down the list.

**Lock file regenerated in passing.** Somebody runs a different package manager version, the lock file diff is four thousand lines, and the reviewer scrolls past it. Symptom: a diff whose size is uncorrelated with its content. Check the lock file version field first, and ask for the regeneration to be its own commit.

**Coupled upgrade merged as one commit.** Core, plugins and types move together, a regression appears, and the commit cannot be bisected because it is atomic by accident rather than by necessity. Symptom: a revert that undoes four unrelated improvements.

**Pinning without integrity.** Versions are pinned exactly and everyone believes the build is reproducible, but the pipeline runs a command that may rewrite the lock file, or the Python requirements carry no hashes. Symptom: two builds of the same commit produce different trees, and nobody can explain it.

**Trusting the repository instead of the tarball.** The review reads the source on the forge, which looks clean, while the published artifact is what actually installs. Symptom: a package whose repository and published tarball have quietly diverged, which is precisely the state a compromised release is in.

## What this skill does not do

- It does not run your tests, and it cannot tell you whether the upgrade works. Every verdict is static.
- It cannot detect a malicious payload by inspection. It teaches you which packages deserve a closer look, and dedicated tarball analysis services do the looking far better.
- It cannot resolve reachability in dynamic languages. Where a scanner does call graph analysis, believe the scanner over this procedure.
- It has no opinion on whether a dependency should exist. Sometimes the right outcome of a dependency review is deleting the dependency, and that is a design decision made elsewhere.
- It does not cover system and container layers. A pinned application tree inside a base image that floats has not been pinned.
