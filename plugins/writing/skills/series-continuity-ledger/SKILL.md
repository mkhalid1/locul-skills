---
name: series-continuity-ledger
description: Builds and maintains the continuity ledger for a long-running bylined series or column, so no later piece contradicts an earlier one, repeats a format or an anecdote, or repeats a promotion inside one week. Covers the canon ledger and its subsections, the append-never-restructure rule, standing corrections recorded as un-revertable, the burned-asset list, the shared cross-byline run log the promotion rule depends on, and the file-naming discipline that keeps the ledger portable between Windows, macOS and a build server. This skill should be used when a series has more than a couple of pieces, when more than one person writes under a byline, or when runs are separated far enough in time that nobody remembers what was already said.
---

# Series continuity ledger

## The claim this skill is built on

Each piece in a long-running series can be fine on its own while the series is incoherent. The failure is cumulative and it is invisible from inside any single draft, which is why editing harder does not fix it.

"Read the previous pieces before writing" fails for three reasons that compound. The back catalogue grows until reading it is a morning's work. The contradiction is nearly always a number stated in passing, not a claim in a heading, so a skim misses it. And a run that starts fresh has no memory of anything the previous run decided but did not publish, including the corrections.

So you keep one file whose entire job is to be read first and written last. Not a style guide, not an archive: a ledger of what the byline has already committed to, in print, with the piece and the line each commitment came from.

---

## The ledger and its sections

One ledger per byline or per series, in a fixed order so a run can find a section without searching:

1. **Voice profile.** How this byline writes. Short, because voice matching is a separate job.
2. **Topic fits.** What this byline may write about, and what it may not.
3. **Published and drafted pieces.** Title, URL, date, target term, format.
4. **Promotion history.** What was promoted, in which piece, in which week.
5. **Format history.** Which shape each piece took.
6. **Cached research lookups.** Query, date, source, result.
7. **Topic seeds and question bank.** What is still unwritten.
8. **Canon ledger.** The load-bearing section.
9. **Long-form history.** Spines, endings and rhetorical devices already used.
10. **Burned assets.** Anecdotes, statistics, metaphors and openers already spent.
11. **The standing setting.** The stable backdrop the byline writes from.
12. **Standing corrections.** Decisions that must never be reverted.
13. **Open flags.** Known contradictions with already-published work.

---

## The canon ledger, and why these subsections

Canon is every concrete claim the byline has made in print. The subsections that earn their place:

- **Role and employer.** Stated once, defended forever.
- **The organisation's established facts.** What it does, roughly how big it is, where it operates, what it sells.
- **Every number stated about the author or the team.** Years of experience, team size, how many of a thing they have done, how long something took, prices, dates. All of them.
- **Tools named as their own.** Which products the byline says they use, and in what role.
- **Recurring characters.** Colleagues, clients, an archetype who reappears, with what has been said about each.
- **Practices committed to in writing.** "We review this weekly." "We never do that." A promise in print is a fact about the byline.

Numbers and named tools are where contradictions actually happen, which is why they get their own subsections rather than being folded into a general notes field. A number gets stated once as colour, in a subordinate clause, and it becomes a fact the moment it publishes. A tool named as "the one we use" cannot later be the one they evaluated and rejected.

**Every entry has four fields**: the claim, the piece it came from, the exact line, and the date. A claim with no source cannot be checked and cannot be retired, and an unretirable claim is worse than no claim at all.

---

## Append, never restructure

**Never reorganise or delete a section a previous run wrote.** Append.

The instinct to tidy is exactly wrong here. A run that consolidates the canon ledger will merge two entries that look redundant and are not, because one is the team size in March and the other is the team size in July. It will drop source lines to save space, which turns checkable canon into folklore. And it will reorder sections so the next run's diff is unreadable, which removes the only cheap way to see what changed.

Retire a claim in place: mark it superseded, with the date and the reason, and add the new claim beneath it. The ledger grows, and that is correct. If it becomes unwieldy, move retired entries to a section at the bottom. Never delete them, because the retired value is what tells you the archive is wrong rather than that you misremembered.

---

## Promotion history, and the rule that needs a shared log

**Two pieces published in the same week must not promote the same thing.** Repetition inside one week reads as a campaign rather than as an opinion, and it burns a byline's credibility faster than a factual error does, because a factual error looks like a mistake and a repeated promotion looks like a motive.

The operationally important part: **the rule is enforced across the whole series, not per byline.** A per-byline ledger structurally cannot see what another byline promoted on Monday, so every individual ledger will report full compliance while a reader sees three pieces pushing the same thing in five days.

That is the reason a single shared cross-byline run log exists, and it is the only part of this system that has to be shared. Everything else can live inside a byline's own folder.

The log is append-only, one row per published piece, written at publish time, with five fields: publish date in UTC, ISO week number, byline, piece, and what was promoted. Use ISO-8601 week numbering and record the date in UTC, and state both conventions at the top of the file, because "this week" on a Sunday evening means two different things in two time zones and the ambiguity always breaks in the direction that hides a violation.

The check before writing: read the log for the current ISO week, list what has already been promoted by anyone, and treat that list as banned for this piece.

---

## Format history and the burned-asset list

Record the format of every piece with its date: listicle, teardown, case narrative, question and answer, contrarian argument, annotated example. The working rule is no format repeated within four consecutive pieces. That number is a default from one operation rather than a law, and it is there so the constraint is checkable instead of a feeling.

**The burned-asset list is the part people leave out.** Every reusable asset consumed gets recorded and marked never-reuse: the opening anecdote, the statistic somebody dug out of a report, the metaphor that worked, the chart, the stock scenario. Without it, a good anecdote reappears in piece nine presented as new, and the readers who noticed are exactly the readers you cannot afford to lose.

---

## Cached research lookups

Every external lookup, with the query, the date, the source and the result. This stops the same figure being re-fetched or re-purchased on every run, and it makes staleness visible.

Two staleness rules worth writing down. A cached figure older than about ninety days is re-fetched before it is restated as current. A cached figure being used as history keeps its original date, is quoted with that date in the piece, and does not need re-fetching. Most arguments about stale data are really arguments about which of those two a number is.

---

## Standing corrections, recorded verbatim

Some corrections have to survive runs that would naturally re-break them. Record each one verbatim, with the reason, the date and who or what made it.

The shape that matters most: a byline has no affiliation with a product, so the "I work there" angle is permanently banned for them, **even though earlier published pieces wrongly imply otherwise**. That final clause is the whole point. Without it, the next run reads the archive in good faith, sees the implication, treats it as canon, and restores the error. A standing correction that does not say the archive disagrees with it will be undone by exactly the diligence you wanted.

Every standing correction carries a do-not-revert marker and its reason. The reason matters more than the rule, because a rule with no reason gets overridden by any run that finds a plausible-looking exception.

---

## Open flags

Known contradictions between the ledger and already-published work. Each flag names three things: the exact piece, the exact line, and the single edit that unblocks it.

"Fix the affiliation wording somewhere in the archive" is not a flag, it is a worry, and it will still be there in a year. If a contradiction cannot be reduced to one edit in one place, split it into flags that can be.

---

## Never guess a fact about the subject

Everything factual comes from the product truth pack first and the live source second. If neither has it, the piece does not say it.

**Deduced limitations are banned outright.** Writing "it does not support that" because you did not see it mentioned is the single most damaging thing a series can do: it is confidently wrong, it is quotable, and it is the kind of error that gets screenshotted. Absence of evidence in a truth pack is absence of evidence.

---

## Naming and file discipline, on both platforms

Ledger and run files use **lowercase-hyphen names**: ASCII only, no capitals, no spaces. This is not tidiness, it is portability, and it is exactly where Windows and macOS diverge from the machine that builds the site.

- **Case.** macOS volumes are case-insensitive but case-preserving by default, and Windows NTFS is case-insensitive. Most Linux build servers and CI runners are case-sensitive. So a file committed as `Canon-Ledger.md` and referenced as `canon-ledger.md` works perfectly on both writers' machines and fails only on the server, after publish, which is the most expensive place to find out.
- **Case collisions.** Two files differing only in case can both exist in a repository and cannot both exist in a checkout on Windows or on a default macOS volume. One silently shadows the other, and the diff shows nothing wrong.
- **Spaces.** They become `%20` in a URL, break shell commands that were written without quoting, and are handled inconsistently by tooling that splits paths on whitespace.
- **Path separators.** Never hardcode one. A path written into a script as `series\ledger.md` fails on macOS and Linux, and `series/ledger.md` hardcoded into a Windows-only tool fails the other way. Use the platform's path join.
- **Reserved names on Windows.** `con`, `prn`, `aux`, `nul`, `com1` to `com9` and `lpt1` to `lpt9` cannot be used as file or folder names, with or without an extension. A folder called `aux` for auxiliary research is a genuinely appealing name and it cannot exist. Trailing dots and trailing spaces are also stripped, so `notes .md` and `notes.md` collide.
- **Path length.** Windows caps paths at 260 characters unless long path support is enabled, which has been opt-in since Windows 10 version 1607. A scheme of dated folders plus long slugs plus per-language subfolders reaches that faster than anyone expects, so keep run folders short: `runs/2026-08-20/`.

**One dated folder per run**, named with an ISO date, so the folder list sorts chronologically in every file browser on both platforms without anyone configuring anything.

**Generated media is not version-controlled**, which has a consequence people discover late: the image is not the record. The **placeholder block inside each piece is the durable record of what every image was meant to be**, and it carries the intended subject, the exact values it displays, the alt text, the aspect ratio, and where it sits in the piece. When an image is lost, regenerated or replaced by a different tool a year later, the block is the specification that makes the replacement correct. Deleting the block once the image exists is the mistake, and it always looks like tidying.

---

## Read at the start, write back at the end

Both directions are mandatory. A read-only ledger goes stale within about three runs and then it is worse than nothing, because people trust it.

The run sequence:

1. Read the ledger.
2. Read the shared run log for the current ISO week.
3. Draft.
4. Extract the draft's factual assertions and diff them against canon.
5. Publish.
6. Append: new canon entries with their source lines, the promotion row, the format row, any burned assets, any cached lookups, and the write-back timestamp.

**The write-back is part of the run, not a follow-up task.** A run that publishes and does not write back has produced one piece and one debt, and the debt is paid by whoever writes the next piece and contradicts something.

---

## Decision rule: the draft states a number

- **Canon has the same number.** Restate it verbatim, including the units and the qualifier. "About six" and "six" are different claims and the archive will be quoted with whichever one it carries.
- **Canon has a different number for the same thing.** Canon wins and the draft changes. If canon is the one that is wrong, that is a standing correction plus an open flag against the piece the wrong number came from, not a quiet edit to the ledger.
- **Canon has no number, and the truth pack does.** Use the truth pack's, then append it to canon with its source and date.
- **You cannot tell**, because the truth pack is silent and the archive is ambiguous or has been edited since. Cut the number from the draft. Do not estimate, do not round to something safe, do not write "roughly". An estimate becomes canon the moment it publishes, and from then on it is defended rather than checked.

---

## Worked example, compressed

An invented three-byline series for a documentation tooling company. Run 14, byline B, ISO week 2026-W34.

**Read.** The ledger says the team is six people, recorded from piece 3, line 41, dated March. Format history shows question-and-answer used in pieces 11 and 13. The burned-asset list contains the onboarding anecdote. Standing corrections contain one entry: byline B has no affiliation with a particular integration vendor, and the "we use it internally" framing is permanently banned for them, even though piece 6 implies it.

**Read the shared log.** Byline A published on Monday of this week and promoted the same integration this draft was going to lead with.

**Diff the draft.** It says "our team of eight", opens with the onboarding anecdote, is written as question and answer, and describes the integration as "the one we run internally".

**Actions.** The number goes back to six, because canon holds it with a source and the truth pack does not contradict it. The opener is replaced, since that anecdote is burned. The format changes to an annotated example, since question and answer has run twice in the last three pieces. The internal-use framing is cut under the standing correction. The promotion is swapped to a different subject for this week, and can return next week.

**Write back.** Two new canon entries, one promotion row, one format row, one burned asset, one write-back timestamp. One open flag is raised: piece 9 says the team "grew past ten", which contradicts canon; the exact line is named and the single proposed edit is to change that clause to the recorded figure.

**Verdict: publishes after four edits, one open flag raised against piece 9, canon gains two entries and one burned asset.**

---

## Failure modes

**A later piece states a different number for something already quantified.** Nothing errors and no editor catches it, because both pieces are internally consistent. It is found by a reader who has read both, and it costs more credibility than the number was ever worth.

**The same promotion twice in one week.** Each byline's own records are clean. Only a reader following the series sees it, and what they see is a coordinated push rather than three people with opinions.

**A standing correction silently reverted.** A run reads the published archive, sees the old framing, treats it as established, and restores the exact error an editor removed in June. It looks like diligence from the inside.

**A deduced limitation published as fact.** The piece says a product cannot do something, because nothing in the notes said it could. It gets quoted, it is wrong, and the correction is public.

**A section restructured and prior canon lost.** The ledger looks better than it did last month and no longer contains the source line for the claim you now need to defend, so nobody can tell whether the archive or the ledger is right.

**The ledger read but never written back.** Runs one to three look fine. By run six the ledger describes a series that no longer exists, and the first person to notice is the person it misleads.

**A capitalised or spaced filename.** It works on the writer's Mac, works on the editor's Windows machine, and fails on the case-sensitive build server after publish, where the error message is about a missing file rather than about a name.

**A regenerated image that no longer matches the text.** The placeholder block was deleted once the image existed, so the only record of what the picture was supposed to show was the picture, and it is gone.

---

## What this skill does not do

- It does not verify a single fact it stores. A wrong number recorded on run one becomes canon, and canon is defended, so the ledger will make a bad fact more consistent rather than less wrong.
- It does not enforce anything by itself. It is a file. The enforcement is the diff between a draft's assertions and canon, and somebody has to run it.
- It does not match voice. It keeps facts consistent, which is a different problem from sounding like the same person, and the two are often confused because they fail in the same piece.
- It cannot see the live archive. If published pieces have been edited since they were logged, only rereading them settles which version is true.
- It does not decide what to write. Topic selection, demand and cluster fit are settled elsewhere, and the ledger only says what this byline may not contradict.
- It will not fix a series that has already contradicted itself in public. That takes open flags, one edit each, and somebody with permission to change published pages.
