---
name: error-message-writing
description: Writes and reviews error messages against a required-parts contract: what happened in the user's terms, why to the extent it can be said, what to do next, and a quotable identifier for anything support will see. Covers the split between the end user message, the API response body and the log line, the security rules for authentication and authorisation failures, the grammar that assigns blame, a four-branch recoverability taxonomy, partial and cascading failure messages, and validation message placement. This skill should be used whenever an error, empty, validation or failure state is being written, in a user interface, an API or a command line tool.
---

# Error message writing

## The claim this skill is built on

An error message has required parts, and most production messages are missing the same two.

The obvious approach is to describe the failure accurately, and accuracy is where most messages stop. "Payment failed." "Upload unsuccessful." "An error occurred." Each is true, and each leaves the reader in exactly the position they were in before it appeared, except now they are also stuck. A message that only says what happened is a notification. An error message is a message that also lets the reader get out.

The two missing parts are always the same. **Why** it happened, to whatever extent you can honestly say. **What to do next**, which is the part that determines whether the message is worth displaying at all.

## The required parts

**Part 1, what happened, in the user's terms.** The user's terms means the objects on their screen and in their head, not yours. "The connection to the inventory service timed out" is your term. "We could not load your stock levels" is theirs. Name the operation they attempted, not the component that failed.

**Part 2, why, to the extent you can say.** Sometimes you know exactly: the file is 12 MB and the limit is 10 MB. Sometimes you know a category: the payment provider declined it without saying why. Sometimes you genuinely do not know, and the honest text is that you do not, which is still more useful than silence because it tells the reader not to keep hunting for their own mistake.

**Part 3, what to do next.** One action, specific, and reachable from where the message is. Not "please try again later" without a time. Not "contact your administrator" without saying what to tell them. If the honest next step is waiting, say how long. If the honest next step is that nothing can be done right now, say that, because a reader who knows there is nothing to do stops trying.

**Part 4, for anything a support team will see: a stable identifier.** A correlation or request id that appears in both the user's screen and your logs. Three requirements, and the third is where implementations fail: it must be short enough to read aloud over a phone, it must be present in the log entry for that exact request, and **it must be selectable and copyable in the interface**. An identifier baked into an image, rendered in a canvas, or placed inside a non-selectable toast that disappears after four seconds is not an identifier, it is decoration. If your system already propagates a trace id, use it, since the W3C Trace Context recommendation gives you one that survives across services.

## The audience split

The same failure needs three different texts. Writing one and reusing it is the most common structural error in this whole area.

**The end user message** needs the three parts, in their vocabulary, with no internal detail. Specifically, it must never contain:

- a stack trace or an exception class name
- an internal service, queue, table or host name
- a raw database or driver error string
- a file path from your infrastructure
- an identifier that leaks the existence of another user's record, which includes a message like "that email address belongs to another account"

**The API response** needs a machine-readable shape, and there is a standard one. RFC 9457, published in July 2023 and obsoleting RFC 7807 from 2016, defines a problem details object with `type`, `title`, `status`, `detail` and `instance`, served as `application/problem+json`. The important discipline is that `type` is a stable, documented identifier that a client can branch on, and `detail` is human-readable text that a client must not parse. Clients that string-match on `detail` break every time you improve the wording, which is a reason wording never gets improved. Add your own extension members for anything a client needs to act on, such as the specific field that failed or the number of seconds until a retry is permitted.

**The log line** needs everything the other two cannot carry: the exception, the stack, the correlation id, the identifiers of the records involved, the upstream response, the timing, and the input that caused it, minus anything that should not be written to a log at all, which includes credentials, tokens, card numbers and, in most jurisdictions, more personal data than people assume. A log line that says "failed to save user" and nothing else has cost you the incident.

## The security constraint

**Authentication.** A failed sign-in must not reveal which factor was wrong. "No account with that email" followed by "incorrect password" for a different address turns your login form into a tool for testing whether an address has an account here, which matters when the site itself is sensitive. One message for both cases: the credentials do not match an account. The same discipline applies to password reset, where the confirmation must be identical whether or not the address exists, and to sign-up, where "that email is already registered" is a disclosure. The usual workaround for sign-up is to accept the submission and send an email that tells the real owner what happened.

Two honest caveats. This trades a real usability cost for the disclosure, and for a product where account existence is not sensitive some teams take the trade the other way deliberately. Take it deliberately, in writing, rather than by accident. And the message is only half the job: a response that is generic but arrives 200 milliseconds faster for a nonexistent account has disclosed the same thing.

**Authorisation.** Here there is a genuine design decision. When a user requests a record they are not allowed to see, you can report not-found, which hides whether the record exists, or not-allowed, which is more helpful and confirms existence.

The rule: **report not-found when the existence of the record is itself confidential, and not-allowed when it is not.** A support ticket belonging to another customer of the same platform is confidential in its existence, so a request for it returns not-found. A document inside the user's own organisation that they lack the role for is not confidential in its existence, and returning not-found there produces a user who thinks the document was deleted and a support ticket that wastes an hour. Whichever you choose, apply it consistently across the resource, because a not-found for one identifier and a not-allowed for another is the disclosure you were trying to avoid.

## The grammar of blame

"Invalid input" is worse than naming the field and the constraint, for a reason that is not about tone. It is worse because it contains no information: the reader now knows only that something in a form they just filled in is wrong, which they already suspected.

The rule has two halves, and they point in opposite directions on purpose.

**When the user can fix it, describe the constraint impersonally.** Not "you entered an invalid date". Instead: "Start date must be on or after today. You entered 3 March 2025." No second-person subject attached to a fault verb, and the constraint stated as a rule of the system rather than a failing of the person. The reader gets the rule, their value, and the gap between them.

**When the system failed, own it in the first person.** Not "the file could not be uploaded", which is a passive construction whose entire function is to leave the actor unnamed. Instead: "We could not upload the file." The impersonal passive that is correct for the user's mistakes is evasive for your own, and readers hear the difference even when they cannot name it.

## The recoverability taxonomy, which is the decision rule

Four branches, four message shapes, four affordances.

**1. The user can fix it now.** They have everything they need. Shape: the constraint, their value, and the corrected action. Affordance: inline, next to the field, with their input preserved and focus moved to the first failing field. Never clear the form.

**2. The user can fix it, but not now.** They need something they do not have to hand: a different file, a card, an approval, a permission. Shape: name exactly what is needed, in a form they could act on tomorrow. Affordance: a way to keep their work, a draft, a saved state or an emailed link back to this exact point. This branch is the one most often collapsed into branch one, which produces a message telling someone to do something they cannot do while standing there.

**3. The user cannot fix it and should wait.** Rate limits, dependency outages, transient server failures. Shape: state plainly that it is not their fault and give a time, because "try again later" without a number means "try again immediately", and everybody does. If it is a rate limit, say when the limit resets. On the API side that is a 429, defined in RFC 6585, with a `Retry-After` header. Affordance: automatic retry with exponential backoff where the operation is safe to repeat, and where it is not safe to repeat, an idempotency key so that it becomes safe. Do not render a retry button that fires an identical request within the same second.

**4. The user cannot fix it and should contact someone.** Permission failures, plan limits, account states, genuine bugs. Shape: who to contact, what to tell them, and the identifier to quote. "Contact your administrator" is not this shape unless the message also says what to ask for. Affordance: a link or address, prefilled where possible, with the correlation id already in it.

**Cannot tell which branch?** Default to branch four, with the correlation id, and preserve the user's work. Specifically do **not** default to branch three's retry button. A retry on an operation that failed permanently produces nothing at best and a duplicate at worst, and duplicate submissions on anything that charges money or sends a message are among the most expensive defects a message can cause.

## Partial and cascading failures

Partial failure is a genuinely different message and is almost always wrong in production, because the code path was written as if the operation either worked or did not.

For a batch of 200 rows where 197 imported and 3 failed, the message must carry four things: the counts, what happened to the successful ones, exactly which ones failed and why each failed, and what to do with the failures. The critical part is the second: a reader who does not know whether the 197 were saved or rolled back cannot safely do anything, and their instinct will be to run the import again, which is how you get 397 rows.

The failing shapes to watch for: a success message that hides a partial failure, an all-or-nothing message on an operation that was not atomic, and a list of failures with no way to retry just those.

Cascading failures need one more rule: report the failure the user experienced, once, not every layer of it. Three stacked messages describing the same root cause at three levels of abstraction is worse than one, because the reader has to work out that they are the same event.

And the neighbouring case that is not an error at all: an empty result is not a failure, and styling it like one teaches users that the product is broken when it is merely empty. See `form-design-audit` in this directory for the state model that keeps these distinct.

## Validation messages

Placement is most of the value. A validation message belongs next to the field it concerns, visible at the same time as the field, and it should not rely on colour alone to be noticed. A summary at the top is useful in addition when the form is long, and it should link to each failing field.

Timing: validate on blur rather than on every keystroke for anything the user is part-way through typing, since a message that says an email address is invalid while it is being typed is both true and useless. Validate on submit for anything that requires the whole form.

Accessibility has specific, checkable requirements here. The relevant success criteria in WCAG 2.2, published as a W3C Recommendation in October 2023, are 3.3.1 Error Identification at level A, 3.3.2 Labels or Instructions at level A, and 3.3.3 Error Suggestion at level AA, which requires that where the correction is known, it is offered. Programmatically, associate the message with its field using `aria-describedby`, and announce messages that appear without a page change through a live region, which is criterion 4.1.3 Status Messages at level AA.

## Tone

**Humour ages badly.** A joke is read once by the author and many times by a user who is stuck. On the fifth failure it reads as contempt, and a comic error on a screen where somebody has just lost work reads as contempt the first time. It also translates badly and it dates. The rare exception is a failure with no cost, such as a decorative 404 on a marketing site.

**An apology is usually the wrong opening.** "Sorry" in the first position delays the information by a word and reads as automated because it is. Apologise when you actually cost the reader something, and put it after the facts, where it attaches to a specific harm rather than floating over the page.

**Avoid "oops", "whoops" and "uh oh".** They minimise a failure the reader is entitled to be annoyed about.

## A catalogue of real bad messages, rewritten

**"An error occurred."** Zero of the three parts. Rewrite: "We could not save your changes because the connection dropped. Your draft is still here. Try saving again, and if it fails, quote reference 7F3A-9C2E."

**"Invalid input."** No field, no constraint. Rewrite: "Postcode must be between 5 and 8 characters. You entered 3."

**"Error 0x80070005."** An identifier with no meaning attached. Rewrite: "We do not have permission to write to that folder. Choose a different folder, or ask whoever manages this machine for write access to it. Reference 0x80070005."

**"Something went wrong. Please try again later."** on a payment. Wrong branch, and dangerous: the reader does not know whether they were charged. Rewrite: "Your card was not charged. The payment provider did not respond in time. Try again in a few minutes, or use a different card. Reference 7F3A-9C2E."

**"Password incorrect for that account."** A disclosure. Rewrite: "That email address and password do not match an account. Check both, or reset your password."

**"You do not have permission to view this page."** on a record belonging to another customer. Rewrite, applying the rule: return not-found, "We could not find that record. It may have been deleted, or the link may be wrong."

**"Failed to save. Contact your administrator."** No identifier, no instruction. Rewrite: "We could not save this record because your account no longer has edit access to this project. Ask a project owner to restore your editor role, and give them reference 7F3A-9C2E."

**"Upload failed."** for a 12 MB file against a 10 MB limit. Rewrite: "That file is 12 MB and the limit is 10 MB. Compress it or split it, and upload again."

**"Import complete."** where 3 of 200 rows failed. Rewrite: "197 of 200 rows imported. Rows 14, 88 and 141 were skipped because their date column was empty. The 197 are saved, so do not run the import again. Download the 3 skipped rows, fix the dates, and import that file."

**"500 Internal Server Error: NullPointerException at com.example.billing.InvoiceService.line 214"** shown to a customer. Two failures at once: it leaks internals and it gives no next step. Rewrite the user-facing text to the branch-four shape with a reference, and keep the exception where it belongs, in the log line under that same reference.

## Worked example, compressed

A scheduling tool, with the product and its numbers invented for this example. A user clicks Publish on a rota for next week. The request reaches the service, which writes the rota, then calls a notification service to text 40 staff members. The notification service times out. The rota is saved. Nobody has been texted. The current message is "Something went wrong. Please try again."

**Branch.** Not branch one, since the user cannot fix a timeout. Not branch two. It looks like branch three, wait and retry, and this is exactly the case where you cannot tell, because the notification service may have accepted some of the messages before it stopped responding. Applying the cannot-tell rule: branch four, no retry button, work preserved, identifier present.

**Part 1, what happened, in their terms.** The rota is published. The staff notifications did not go out. Note that the current message implies the opposite, that nothing happened.

**Part 2, why.** The notification service did not respond. We can say that much honestly, and we cannot say whether any of the 40 were sent, so we say that too.

**Part 3, what to do next.** Not "try again", because re-publishing may text some people twice. The action is to check the notification log on the rota page, and to send the notifications again from there once it is clear who received one.

**Part 4, identifier.** Present, selectable, and matching the log entry.

**The message.** "Your rota for next week is published. We could not confirm that the staff notifications were sent, because the notification service did not respond. Some staff may have received a message and some may not. Open the notification log on this rota to see who was reached and send the rest from there. Do not publish again, since that can send duplicate messages. Reference 7F3A-9C2E."

**The API response** for the same event is a 207-style partial outcome or a documented problem type with an extension member naming the notification step, so a client can branch on it rather than parsing the sentence. **The log line** carries the timeout, the upstream response, the correlation id, the rota id and the list of recipient ids attempted.

**Verdict:** the original message was wrong on every part. It described the wrong outcome, gave no cause, offered the one action that makes things worse, and carried no reference. The rewrite is four times longer and it is the only version from which the user can act.

## Failure modes

**The notification that thinks it is an error message.** All of part one, none of parts two and three. The most common shape in production by a wide margin.

**The unquotable identifier.** A reference exists, and it is inside a toast that vanishes, or rendered as an image, or too long to read over a phone. Support asks for it and the user cannot supply it.

**One message for three audiences.** The user sees a stack trace, or the log carries only the polite sentence, and neither audience gets what they need.

**The retry button on a permanent failure.** The user clicks it eleven times. On anything that charges or sends, some of those attempts succeed.

**Partial failure reported as success.** The counts are hidden, the reader re-runs the operation, and now the data is duplicated. The specific tell is a success message on any operation that processes a list.

**Blame grammar reversed.** "You failed to provide a valid date" for the user's typo, and "the file could not be uploaded" for your outage. Both point the wrong way.

**Existence disclosed by inconsistency.** Not-found for one identifier, not-allowed for another, on the same resource type. The pair reveals what each message alone concealed.

**Humour on a costly failure.** A cheerful message on the screen where somebody lost an hour of work, which reads as contempt and is remembered.

**The empty state styled as an error.** Nothing is wrong, and the product looks broken, which trains users to distrust real errors.

## What this skill does not do

- It does not design the recovery path. If the honest next step is that the work is lost, no wording fixes that, and the fix is a product change such as saving drafts.
- It does not know which errors your users actually hit. An error tracker ranks them by frequency and that ranking should decide what you rewrite first.
- It is not a threat model. The disclosure rules here are the common cases, and a security engineer will find side channels, including timing and response size, that no wording rule addresses.
- It does not localise. Fragment-style validation text, humour, and the placement of the constraint relative to the value all behave differently in other languages, and a translator will find problems this cannot.
- It cannot help with a failure nobody detected. A swallowed exception or a success response for an operation that did not happen produces no message to improve.
