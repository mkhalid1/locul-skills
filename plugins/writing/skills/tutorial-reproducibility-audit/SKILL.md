---
name: tutorial-reproducibility-audit
description: Audits a tutorial for the reasons it stops working on somebody else's machine. Checks the clean machine assumption item by item, version pinning and where the version statement belongs, whether every command runs exactly as written when copied, Mac and Windows coverage including the divergences that actually break things, whether each step ends in an observable check, and whether the common failures are documented inline. Includes a rot detection procedure that works without running the tutorial. This skill should be used before publishing a tutorial, when a documented command is reported not to work, or when auditing a tutorial that has not been verified for months.
---

# Tutorial reproducibility audit

## The claim this skill is built on

A tutorial is the only documentation genre with a binary pass condition. The reader follows it and either arrives at the working result or does not. Everything else in a documentation set can be partly useful. A tutorial that fails at step 6 has produced nothing, and the reader who hits that failure usually does not file a ticket. They leave.

The obvious approach to keeping tutorials working is to re-read them. That fails, and it fails for a structural reason: **most of the defects are things that are not on the page.** A tutorial breaks because of a tool the author had installed, a variable already exported, a token already cached, a directory that already existed. None of that is in the text, so re-reading the text finds none of it. Reading harder does not help either, because the author's own eyes fill in the missing preconditions automatically.

So this audit does not read for quality. It reads for absence, against a fixed list of things the reader will not have, and it treats each item as present-or-absent rather than as a matter of degree.

## The clean machine assumption

Assume the reader has a freshly installed operating system and nothing else. For each item below, one of three things must be true: the tutorial installs it in a step, the tutorial names it in prerequisites with a version and a link, or the tutorial genuinely does not need it. Anything else is a finding.

- **A tool installed globally, years ago.** Usually a CLI, often at a version the author cannot state. The tell is a command that appears with no install step anywhere above it.
- **An environment variable set in a shell profile.** A PATH entry, an API key, a region, a default project. It works on the author's machine in every new terminal, so it is invisible to them forever.
- **Credentials already present.** A logged-in CLI session, a token in the OS keychain or the Windows credential manager, an SSH key already added to the agent, a host already in `known_hosts` so the fingerprint prompt never appears for the author and always appears for the reader.
- **A package manager already configured.** A private registry, an auth token in a user-level config file, a mirror, a proxy setting. The install command works for the author and returns a 401 for everyone else.
- **A specific language runtime version.** Usually supplied by a version manager rather than the system, which means the author's `python` or `node` is not the reader's `python` or `node` even though the command is spelled identically.
- **A hosts file entry, a local proxy or a local DNS override.** Common on teams that develop against a local domain.
- **A directory or config file that already exists.** A home-directory config, a workspace folder, a data directory created by an earlier experiment.
- **A resource already provisioned.** A database created by hand, a bucket, a project, a port that happens to be free.
- **A browser already logged in**, so a step that reads "open the console" is one click for the author and an account creation flow for the reader.
- **A permission prompt already accepted.** The first-run security dialogue on macOS, or the SmartScreen and Defender prompts on Windows, which appear once per machine and are therefore never seen again by the person writing the tutorial.

**The counting rule.** Walk the list, and for every item write present, installed-in-step, or missing. Missing items are the report. This is boring and it is the highest-yield part of the audit.

## Version pinning

**Every command that installs something must pin.** An unpinned install is a claim that the tutorial works with all future versions of that package, which nobody would write down deliberately. Pin the exact version in the command itself, not in prose next to it, because prose does not get copied.

**Where the version statement belongs**, and what it must cover: a single block at the top, above the first command, before the reader has done anything. It states the language runtime version, the package manager version where the package manager itself matters, every direct dependency the tutorial installs, the operating system versions it was verified on for both Mac and Windows, the version of the product or service it targets, and the date it was last verified end to end. A version block that covers only the product is the most common half-done version of this.

**A tutorial with no stated versions has an unknown shelf life,** which is a worse position than a short one. With the block present, a reader can decide for themselves whether a two-year-old tutorial is still plausible. Without it, nobody can, including the maintainers.

**Screenshots have versions too.** Every screenshot is a claim about a user interface at a date, and it is the only claim in the documentation that cannot be updated by editing text.

## Every command must run as written

Read every command as though you are pasting it, in order, into a terminal that has just been opened. Six specific defects, each with a fix:

1. **A placeholder that is not marked as one.** `--api-key ab12cd34` or a project named `acme-prod` looks like a real value and gets pasted verbatim. Fix: placeholders in capitals inside angle brackets, plus one line saying what to substitute and where to obtain it. Never use a placeholder that could be mistaken for a working value.
2. **A command split across lines in a way that breaks when copied.** Backslash continuation with trailing whitespace after the backslash, a line the renderer wrapped rather than the author breaking, or a PowerShell backtick continuation pasted into a Unix shell. Fix: prefer one long line, and if you must continue, verify the copy button output rather than the rendered output.
3. **A prompt character copied into the command.** A leading `$`, a `PS C:\>`, or `>>>` in a Python block. Pasting it produces an error that looks nothing like the real problem. Fix: no prompt characters anywhere, and keep output in a separate block from input.
4. **A path relative to a directory the reader is not in.** The `cd` happened three steps ago, in a different block, and the reader has since opened a second terminal for the server. Fix: each command block states its working directory, or every path is written relative to one named project root.
5. **A command that requires elevation without saying so.** `sudo` omitted, or a Windows step that only works in a terminal launched as administrator, or an install into a system directory, or binding to a port below 1024, which needs elevation on macOS and Linux and does not on Windows. Fix: say which steps need elevation, on which platform, and say why.
6. **A command whose output the next step depends on, but which is not shown.** "Copy the identifier from the output" with no example output block. Fix: show a representative output block with well-formed but obviously fake values, and name the exact field to copy.

Two more that are worth a search each: **smart quotes**, which appear when copy has passed through a word processor and produce a baffling syntax error, and **characters the shell will interpret**, which differ per shell. An ampersand behaves differently in `cmd.exe`, an exclamation mark triggers history expansion in an interactive bash shell, and quoting rules in PowerShell are not the quoting rules in bash.

## Platform coverage

Readers arrive on Mac and on Windows in comparable numbers, so a tutorial that gives the commands for only one of them is broken for a large fraction of the people following it. The divergences that actually matter, verified as of August 2026:

- **Path separators.** Forward slash against backslash. Many tools accept both, and scripts, config files and anything doing string concatenation frequently do not.
- **Home directory notation.** `~` and `$HOME` in a Unix shell, `%USERPROFILE%` in `cmd.exe`, `$env:USERPROFILE` in PowerShell. A tutorial that writes `~/.config/tool.yaml` and nothing else has left Windows readers to guess.
- **Environment variable syntax.** `export FOO=bar` in bash or zsh, `set FOO=bar` in `cmd.exe`, `$env:FOO = "bar"` in PowerShell, and `setx` when it must survive the session. These are four different lines, not one line with a note.
- **Line endings.** CRLF against LF. Two visible symptoms: a shell script that fails with an error mentioning a stray carriage return, and a diff in which every line appears changed. Say what the repository expects.
- **A shell command is not a cross-platform instruction.** `ls`, `curl`, `which`, `touch`, `grep` and `rm -rf` are not portable instructions. In Windows PowerShell 5.1, `curl` is an alias for `Invoke-WebRequest`, whose flags and output are entirely different from `curl.exe`, which has shipped with Windows since version 1803. That alias was removed in PowerShell 6 and later. `ls` is an alias for `Get-ChildItem`. A tutorial that assumes the Unix versions will produce errors nobody can interpret.
- **Case sensitivity.** macOS ships case-insensitive by default, Windows is case-insensitive, Linux is case-sensitive. An import whose capitalisation is wrong works on the author's Mac and fails in Linux CI, which is a tutorial defect that surfaces in a pipeline.
- **Path length on Windows.** 260 characters unless long path support is enabled, which bites deeply nested dependency trees.
- **Script permissions.** `chmod +x` has no Windows equivalent, and the default PowerShell execution policy on Windows client editions blocks unsigned `.ps1` files, so a tutorial that ships a helper script needs a line about it.

**The coverage rule:** give both platforms in full, as parallel complete blocks. Never write "adjust the paths for your platform", which converts a step into an exercise. If you genuinely support one platform only, say so in the prerequisites, at the top, before the reader installs anything.

## The success check

Every step should end in something the reader can observe. A step with no check does not remove failure, it defers it, and deferred failure is expensive: the reader discovers at step 11 that something went wrong at step 4, and now has to bisect seven steps in a system they do not understand.

Acceptable checks, in rough order of strength: an exact command output, a file that now exists at a stated path, an HTTP status and a body fragment, an exit code, a specific element visible in the interface, a log line. Weak checks that do not count: "you should see a success message", "everything should now be working".

**The rule:** after any step that creates or configures state, give a one-line verification command and the exact expected output. If the output varies, show the shape and name the part that must match.

## Failure recovery, inline

For each step, document the three or four things that most commonly go wrong, immediately under that step. Not in a troubleshooting appendix, because a stuck reader does not scroll to the end of a document; they search for the error text they can see.

That gives the format: **the literal error string first**, because the literal string is what gets pasted into a search box, then the cause in one sentence, then the fix as a command. Three or four per step is the useful budget, and they come from your support queue rather than your imagination. Common candidates: the permission denied, the port already in use, the command not found because a shell was not restarted after a PATH change, the authentication failure from an expired token, and the version mismatch.

## Rot detection without running it

Signals, strongest first:

1. **No last-verified date, or a date older than the release cadence of the fastest-moving dependency it names.**
2. **Unpinned install commands.** Every one is an open-ended promise.
3. **Version numbers in the prose that are behind the current release.** Check the package registry or release feed.
4. **Quoted interface labels that no longer exist.** Button names and menu paths in the text are checkable against the current product; screenshots are not, which is one more reason to keep both.
5. **Links that 404, or redirect to a documentation root.** A redirect to the root is stronger evidence than a 404: it usually means the page was removed and someone added a catch-all rule, which in turn usually means the feature was renamed.
6. **Deprecated flags or commands** appearing in the text.
7. **Comments, issues or reviews saying it no longer works**, which are the cheapest signal of all and the most often ignored.

**The automated approximation.** Where the tutorial can be executed, run a scheduled job that extracts every fenced command block, replays them in order in a clean container built from the stated base image, and asserts the stated expected outputs. Where it cannot be executed, the affordable substitute is three checks in CI: compare every pinned version in the file against the current release of that package, run a link checker over every URL, and fail the build when the last-verified date is older than a threshold. Set that threshold by cadence: roughly 90 days for a dependency that ships majors more than once a year, roughly 365 days for a stable one.

## Screenshots

**The real cost** is that a screenshot goes stale invisibly. It is not searchable, not translatable, not readable by a screen reader without alt text nobody writes, and it must be retaken for every interface change, in every locale, and in both light and dark themes. It also inflates the repository, which is nobody's favourite problem until it is.

**When they earn it:** when the reader must locate something spatially in an unfamiliar interface, when the control has no reliable name to write down, and when the thing being shown is genuinely visual, such as a colour-coded status or a chart.

**The rule: never show something in a screenshot that is not also in the text.** Every value typed, every button label, every menu path appears in prose, and the screenshot only confirms it. That way, when the screenshot rots, the tutorial still works. Two corollaries: crop tightly to the relevant control, and never screenshot a terminal, because terminal output is text and text can be copied, searched and corrected.

## The decision rule for a step you cannot verify

- **You can run it in a clean container or a fresh virtual machine.** Run it. The verdict for that step is empirical and outranks anything the text suggests.
- **It needs a paid account, a manual approval, physical hardware or a browser flow.** Mark it UNVERIFIED, audit only the text-level defects, and report the count of unverified steps prominently. A pass that quietly includes unverified steps is a false pass.
- **You cannot tell whether a precondition exists on a clean machine.** Assume it is absent. The default is always that the reader has nothing, so anything you cannot rule out becomes a required prerequisite line. This branch resolves the majority of ambiguous cases and it resolves them cheaply, since adding a prerequisite costs one line and omitting one costs a reader.

## Worked example, compressed

An eight-step tutorial for a hosted job scheduler, published fourteen months ago. The tutorial and every figure in it are invented for this example.

**Version block:** absent. Product version named in passing in step 1, nothing about the runtime, no verification date. Finding, and it is the one that makes every other finding harder to triage.

**Step 1, install the CLI.** `npm install -g scheduler-cli`, unpinned, and no Node version stated anywhere. Two findings.

**Step 2, log in.** `scheduler login` with no expected output shown and no note that it opens a browser. A reader on a headless machine stops here permanently.

**Step 3, create a project.** Output not shown, and step 4 says "copy the project ID from the output". Classic missing-output defect.

**Step 4, write a config file.** The path is given as `~/.scheduler/config.yaml` with no Windows equivalent, and the directory is assumed to exist. Two findings.

**Step 5, run the job.** The command is split with a trailing backslash followed by a space, so it breaks when copied. The block also carries a leading `$`.

**Step 6, verify.** No check at all. The text says "your job should now be running".

**Step 7, add a webhook.** Three screenshots. Two show buttons whose labels appear nowhere in the prose, and the interface was redesigned six months ago.

**Step 8, clean up.** `rm -rf ~/.scheduler`, given only for a Unix shell, and it is the only destructive command in the tutorial.

**Clean machine list:** Node absent, a global CLI installed by step 1 but with no version, credentials handled by step 2 but with no headless path, a config directory assumed, a browser assumed logged in.

**Verdict: hold.** Four blocking defects, in this order. The unshown output in step 3 stops every reader at step 4, so it is first regardless of severity elsewhere. The broken line continuation in step 5 stops the rest. Steps 4 and 8 have no Windows path at all, which makes the tutorial unusable on one of the two supported platforms rather than merely awkward. And with no version block and unpinned installs, nobody can tell whether the remaining oddities are defects or drift. The screenshots in step 7 are a real problem but a second-pass one: put the button labels into the prose first, then retake or delete the images.

## Failure modes

**Auditing on the author's machine.** The fatal one. Everything works, so the audit reports that everything works, and the entire class of absence defects is invisible by construction.

**Fixing the prerequisites list and not the steps.** A thorough prerequisites block is satisfying to write and does nothing about a command that breaks when copied.

**Putting recovery in a troubleshooting appendix.** Written once, reached by nobody. A stuck reader searches for their error text, and if the error text lives at the bottom of a page under a generic heading, it is found by accident or not at all.

**Pinning the application and not the runtime.** The package is at 2.4.1 and the tutorial still breaks, because the reader's Node or Python is two majors away from the author's.

**Windows coverage that is one sentence.** "Windows users should adjust paths accordingly" is not coverage. It converts every path in the document into an exercise for the reader least equipped to do it.

**Screenshots as the primary instruction.** When the only place a button name appears is inside a JPEG, the tutorial cannot be searched, cannot be translated, and dies at the next redesign without anyone noticing.

**A tutorial that has grown branches.** Every support question adds an "if you are using X" clause, until the guarantee of success is gone and it has become a how-to guide with the wrong title. That is a structural problem rather than a reproducibility one.

**Testing the happy path in CI while the prose says something else.** The pipeline runs a script that has drifted from the document, so the badge is green and the page is wrong. Extract the commands from the document itself.

## What this skill does not do

- It cannot execute anything. It predicts breakages from the text and marks what it cannot verify, and a real replay in a clean environment beats it every time.
- It cannot see behind an account, a payment, a browser flow or specific hardware, and the steps that need those come back unverified rather than passing.
- It does not judge whether the tutorial teaches the right thing, or whether it should be a tutorial at all. That question belongs to `documentation-architecture`, and it is worth settling first.
- It does not check wording, terminology or capitalisation consistency. That is `style-guide-conformance`, and it is worth running last.
- It has no view of locale, keyboard layout, corporate proxies, antivirus interference or offline installs, all of which break real readers and none of which are visible in the source text.
