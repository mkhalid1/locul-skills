---
name: sentence-rhythm-edit
description: Measures sentence length as a distribution across a passage rather than as an average, then fixes the specific rhythms that flatten prose: runs of similar-length sentences, uniform paragraph sizes, repeated sentence openers, repeated participial phrases, and sentences that trail off into a qualifying clause. Includes target spreads, an opener tally, a cadence check, and the registers where uniformity is correct. This skill should be used when editing a draft that reads as monotonous, before recording anything as speech, or after a factual edit has evened out the sentences.
---

# Sentence rhythm edit

## The claim this skill is built on

The average sentence length of a passage tells you almost nothing. The spread tells you nearly everything.

Generated prose has a characteristic shape when you count it: the lengths bunch tightly around the mean, very short sentences are absent entirely, and the long ones are long by accumulation rather than by design. Prose written by a person who is paying attention has a wide spread. It contains sentences of four words, because sometimes four words is the whole point, and it contains a forty-word sentence somewhere that carries a complicated relationship the writer refused to break into three.

This is why "vary your sentence length" fails as an instruction. It has no target, so the edit converges on a slightly wider band of the same middling lengths. Counting gives it a target.

**One thing this is not.** It is not a way to defeat a detector. Automated detectors respond to many things and their scores are unreliable in both directions, and a passage engineered to hit a variance number reads worse than the one it replaced while scoring differently. The goal is prose someone would want to read aloud.

## The measurement

Work on a passage of at least twenty sentences. Below twenty the distribution is noise and any conclusion is invented.

1. Split the passage into sentences on full stops, question marks and exclamation marks. Headings, list items and code blocks are excluded, because they have their own rules and they will distort the count.
2. Count words in each sentence.
3. Sort the counts and record five numbers: the shortest, the longest, the median, the mean, and the standard deviation. Use the sample standard deviation, the one that divides by the number of sentences minus one, and use it consistently so two passages can be compared.
4. Bucket the counts in fives and look at the shape: 1 to 5, 6 to 10, 11 to 15, and so on. A text histogram of twenty numbers takes ten seconds to read and shows you immediately whether you have a spread or a spike.

**What a healthy distribution looks like in general non-fiction.** These are working heuristics, not laws, and the whole point of stating them as numbers is that you can argue with them.

- Mean somewhere between 14 and 20 words.
- Standard deviation of 8 to 12 words. This is the number that matters most.
- At least one sentence under 6 words in every 200 words of prose.
- At least one sentence over 30 words per few hundred, and it has to earn the length by carrying a relationship that genuinely does not decompose.
- A range from shortest to longest of at least 30 words across the passage.

**What a flat distribution looks like.** Standard deviation of 4 to 6. Nothing under 8 words anywhere. Mean often a little high, 18 to 24, because everything is a complete, well-formed, moderately complex statement. The histogram shows one tall bucket and almost nothing on either side of it.

**Registers where flat is correct.** API reference, error message catalogues, legal and contractual prose, structured specifications, and safety instructions. Uniformity there is a service to the reader, who is scanning rather than reading, and applying this pass to them makes them worse. Check the register before you check the numbers.

## The decision rule

- **If the standard deviation is under 5 and no sentence is under 8 words**, the passage is flat. Run the fixes below, starting with the short-sentence insertion and the opener tally.
- **If the standard deviation is over 14 and several sentences exceed 45 words**, the passage is erratic rather than varied. The long sentences are usually run-ons held together by commas, and the fix is to break them, which will lower the spread and improve the reading at the same time.
- **If the passage is under twenty sentences, or is more than about a third list items and headings, you cannot tell.** Do not compute anything. Read it aloud, and judge it on the two things reading aloud exposes: whether you run out of breath, and whether the sentence openers repeat. A distribution computed on eleven sentences is a number with no information in it, and acting on it is worse than not measuring.

## The five rhythms to break

**1. Three consecutive sentences of similar length.** Similar means within about three words of each other. This is the most audible flatness and the easiest to fix: cut one of the three in half, or fold two of them together. Runs of three are the unit to look for, because two in a row is ordinary and four is unbearable.

**2. Every paragraph the same size.** Four paragraphs of three sentences each is a shape the eye recognises before the reader has processed a word of it. Paragraph length is a separate variable from sentence length and needs its own pass, described below.

**3. Every sentence opening with its subject.** The default English order, correct most of the time and a signature when it is every time. The fix is not to invert everything, it is to open four or five sentences in a passage differently: with a subordinate clause, with a prepositional phrase, with the object, or with a conjunction.

**4. The participial-phrase opener, repeated.** "Having reviewed the logs, the team found the cause." "Building on this approach, the next release adds caching." "Considering the constraints, the design holds up." One is fine. Three in a page is a tic, and it is one of the strongest syntactic tells in generated prose. It also tends to dangle, attaching the participle to the wrong subject.

**5. The same conjunction joining every compound sentence.** Usually "and", sometimes "while" or "as". Count them. If more than half of the compound sentences in a passage use the same joint, you have found a real pattern, and the fix is often to split rather than to find a different conjunction.

## The opener tally

A countable check that takes two minutes and finds what reading misses.

Tally the first word of every sentence in the passage by class:

- Article plus noun: "The", "A"
- Pronoun or demonstrative: "It", "This", "They", "There"
- Proper noun or bare subject
- Prepositional phrase: "In", "For", "After", "Across"
- Subordinating conjunction: "When", "If", "Because", "Although"
- Coordinating conjunction: "But", "And", "So"
- Adverb: "Often", "Usually", "Instead"
- Participle: "Having", "Building", "Using", "Considering"

**Threshold: no single class should exceed about half the sentences in the passage.** Two specific things to watch. "The" as an opener is usually the largest class by a distance and is easy to reduce without changing meaning. "This" as an opener, in "This means", "This is because", "This allows", is the one that most often signals a sentence restating its predecessor rather than advancing, so a high count there is a content problem wearing a syntax problem's clothes.

## The very short sentence as a tool

A sentence of one to five words does something no other sentence does. It stops. Placed after a long one, it acts as a landing, and the reader carries it forward as the claim of the paragraph.

Two rules for using it well.

Use it for the claim you want remembered, not for a transition. "This matters." is a wasted short sentence. "The index was missing." is a good one, because the short form makes the fact land rather than announcing that a fact is coming.

Do not stack them. One short sentence per 150 to 200 words is generous. Three in a row produces a staccato style that is just as recognisable as the flat one, reads as a sales page, and gets exhausting inside a paragraph. The short sentence works by contrast, so it stops working the moment it is the norm.

## Paragraph length

A separate variable, and one that most rhythm advice ignores.

**Working limit on screen: four sentences or about 90 words, whichever comes first.** The reason it is tighter than in print is mechanical rather than aesthetic. On a page, line length is fixed by the typesetter and the eye has physical landmarks to return to. On a screen the column width is whatever the reader's window happens to be, so a five-sentence paragraph can render as twenty unbroken lines on a phone, and the eye loses its place on the return sweep to the left margin with nothing to anchor it. The paragraph break is the anchor.

Vary paragraph length as deliberately as sentence length. A one-sentence paragraph is a legitimate emphasis device and works for the same reason the short sentence does, by contrast. Two one-sentence paragraphs in a row is not emphasis, it is a list with the punctuation removed.

## Cadence at the ends of sentences

English puts its stress at the end of a clause, so the last content word before the full stop is the one the reader carries into the next sentence. Ending on the important word is not decoration, it is how you control what the reader remembers.

Compare "The migration failed because the index was missing" with "Because the index was missing, the migration failed". Both are correct. The first leaves the reader holding the missing index, the second leaves them holding the failure. Choose based on what the next sentence needs to pick up.

**The trailing qualifier is the specific habit to look for.** Sentences that end with a comma and a participial phrase: ", which is important for performance", ", making it easier to maintain", ", allowing teams to move faster", ", ensuring consistency across the system". The information in these clauses is almost always vague, and their real function is to soften the landing so the sentence does not have to commit to anything. They are the single most common cadence fault in generated prose.

**Countable check:** count the sentences ending in a comma plus an "-ing" clause. More than one in eight is a pattern rather than a coincidence. Fix by cutting the clause outright, which usually loses nothing, or by promoting it to its own sentence if the claim inside it is real enough to survive standing alone.

## Read aloud, and its automated approximations

Reading aloud is the highest-yield diagnostic here, and it works because of what silent reading does not do. Silent reading is partly reconstruction: the eye samples a fraction of the words and the internal voice fills in a rhythm that is smoother than the one on the page, so a monotonous passage gets silently improved before you ever judge it. Reading aloud removes that. It forces every word through a physical channel with a breath constraint, so you run out of air at precisely the point a sentence has overrun, and you hear repeated openers as a chant, which is impossible to unhear once noticed.

If you cannot read aloud, these approximate it in descending order of usefulness:

1. **Text to speech at normal speed.** Not 1.5x. Speeding up smooths out exactly the rhythm you are listening for.
2. **Read the sentences in reverse order.** This breaks the argument, which is the point, because the argument is what distracts you from the sound.
3. **Read only the first three words of every sentence**, as a column. Repeated openers become obvious immediately.
4. **The bucketed histogram**, which is the cheapest and the least sensitive. It finds flatness and misses cadence entirely.

## Worked example

Eight sentences from a draft about a deployment process.

> The deployment pipeline runs a series of checks before any code reaches the production environment. Each check is designed to catch a specific class of problem before it affects users. The unit tests run first, followed by the integration suite, which takes considerably longer to complete. The build step then produces a container image that is tagged with the current commit hash. This image is pushed to the registry and scanned for known vulnerabilities before promotion. The staging deployment happens automatically once the scan has completed successfully. A manual approval is required before the same image is promoted to the production environment. The entire process usually takes around twenty minutes from commit to production availability.

Measurements: 15, 15, 16, 16, 14, 11, 15, 13 words, 115 words in total. Mean 14.4, median 15, shortest 11, longest 16, range 5, sample standard deviation 1.7. Every sentence opens with its subject, and six of the eight open with "The" or "This". Two sentences trail off into a qualifying clause. The histogram is a single bucket. This is about as flat as prose gets.

Rewritten, same facts:

> Nothing reaches production without passing five checks. Unit tests run first. Then the integration suite, which takes about twelve minutes and is the slowest thing in the pipeline. The build produces a container image tagged with the commit hash, pushes it to the registry, and scans it against the known vulnerability database before anything is promoted anywhere. Staging deploys itself once that scan is clean. Production does not: a person has to approve the same image, unchanged, before it goes. Commit to production takes about twenty minutes, and most of that is the integration suite.

Measurements after: 7, 4, 17, 29, 8, 15, 15 words, 95 words in total. Mean 13.6, median 15, shortest 4, longest 29, range 25, sample standard deviation 8.4. Openers now include a bare subject, an adverb, a noun phrase, and two sentences beginning with the thing being described rather than the process. One long sentence at 29 words earns its length by holding a genuine sequence together. The trailing qualifiers are gone, and two sentences now end on the words that matter, "clean" and "before it goes".

**Verdict: fixed.** The sample standard deviation moved from 1.7 to 8.4, the range from 5 to 25, and the passage is 20 words shorter than it was, 95 words against 115. Note what did not happen: no fact was added, no fact was removed, and one number that was vague, "considerably longer", became twelve minutes, which is a rhythm fix and an honesty fix at the same time.

## Failure modes

**Varying the lengths without varying the structure.** The distribution widens, every sentence still opens with its subject, and the passage still chants. The opener tally exists because the length fix alone leaves this untouched.

**The manufactured short sentence.** "It matters." "Simple." "Here is why." Short sentences that carry no content are a tell in their own right and a worse one than the flatness they replaced, because they read as a style being performed.

**Chopping a long sentence at the comma.** The result is two sentences, one of which is a fragment missing its subject, and a relationship that was explicit in the original is now implied and lost. If a long sentence resists splitting, that is evidence it was doing real work.

**Optimising the number instead of the reading.** Adding a 45-word sentence to raise the standard deviation is exactly the failure this file is meant to prevent. The statistics are a diagnostic. They are not the target, and a passage tuned to hit them is a new kind of artificial.

**Applying it to the wrong register.** Reference documentation, legal text and safety instructions are supposed to be uniform, because the reader is scanning for one item and every departure from the pattern costs them time. Check the register first.

**Breaking rhythm inside a list.** Parallel structure across list items is a feature: the reader relies on it to compare items. Sentence variety belongs in the prose, and enforcing it inside a list damages the one place uniformity is doing work.

**Reading aloud at speed.** Text to speech at 1.5x or faster smooths the rhythm and defeats the diagnostic. So does reading aloud in your head, which is silent reading with extra confidence.

**Treating rhythm as the problem when the problem is content.** Rhythm is a stylistic surface. A piece with nothing to say does not become worth reading by varying its sentence lengths, and time spent here is time not spent finding the thing worth saying. If the passage feels empty rather than flat, close this file.

## What this skill does not do

- It does not evade detectors, and it should not be used as if it did. The measurements here are about reading, not about scores.
- It does not touch vocabulary. A passage with a beautiful distribution can still be full of words that give it away, which is a separate pass.
- It does not touch document structure, which is the layer that survives every sentence-level edit and is usually the deeper reason a piece reads as generated.
- It cannot judge whether a long sentence deserves its length, only that it has it. That judgement is yours, and the test is whether the relationship inside it survives being broken in two.
- The target ranges are heuristics from ordinary editing, not measured constants, and they carry no authority beyond being specific enough to argue with.
- It says nothing about voice. Two passages can have identical distributions and sound like different people, and matching a particular person's voice needs a corpus of their writing rather than a statistic.
