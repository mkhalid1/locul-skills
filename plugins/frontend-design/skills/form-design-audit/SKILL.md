---
name: form-design-audit
description: Audits a form against the HTML standard's autocomplete token vocabulary, the type and inputmode pairings that produce the right mobile keyboard, validation timing that does not fire on the first keystroke, error construction and placement with aria-describedby and aria-invalid, the name, address and postcode assumptions that break outside one country, current password guidance, and the double-submit and autofill styling traps. This skill should be used when building or reviewing any form that collects a name, an address, a payment method, a password, or more than three fields of any kind.
---

# Form design and validation audit

## The claim this skill is built on

Most form advice is about the parts a designer can see: label position, field grouping, how many
columns, whether the button is the right colour. Those matter, and they are also the parts that get
attention already.

The parts that decide how much work the form actually is are invisible in a screenshot. Whether the
browser can fill a field depends on a single attribute value that is either the token the
specification names or is not. Whether a phone keypad or a full QWERTY keyboard appears on a phone
depends on two attributes that interact. Whether an error is announced to a screen reader depends on
an id reference. Whether a slow network produces one order or three depends on what happens between
the click and the response.

None of that is visible in review, all of it is decided in a single commit, and almost all of it is
cheap at authoring time and expensive later. That is the case for auditing forms as markup rather
than as layout.

## The order of the passes

Run these in order. The order matters because pass 2 changes what pass 4 has to validate: a field
the browser fills correctly is a field that produces far fewer errors, so fixing tokens first
shrinks the error-handling work rather than the reverse.

1. Census every field.
2. Assign an autocomplete token to each, or record why it has none.
3. Assign `type` and `inputmode`.
4. Set validation timing.
5. Construct and place the errors.
6. Check the international assumptions.
7. Check submission, buttons and autofill styling.
8. Run the applicability pass and cut what does not apply.

## Pass 1. The field census

One row per field: label, purpose, whether it is genuinely required, the token, the type, the input
mode, the validation trigger, the exact error copy, and where the error renders. A field with an
empty cell in that table is not finished.

Two questions to ask while writing the census, both of which delete fields. Who reads this value
downstream, by name? And what happens if it is wrong? A field nobody consumes should not exist, and
a field whose wrong value costs nothing does not need validation.

## Pass 2. The autocomplete token vocabulary

This is the change with the largest effect per line of code in a form, and it is a lookup rather
than a judgement. The tokens below are from the HTML standard's autofill section. They go on the `input`,
not on the `form`.

**Identity**

`name` for a whole name in one field. `honorific-prefix`, `given-name`, `additional-name`,
`family-name`, `honorific-suffix`, `nickname`. Note that `additional-name` is the middle name and
that there is no "first" or "last" token, because the standard deliberately avoids ordering.

**Contact and account**

`username`, `email`, `tel`, and the tel parts `tel-country-code`, `tel-national`, `tel-area-code`,
`tel-local`, `tel-extension`. Also `url`, `impp`, `organization`, `organization-title`, `language`,
`bday` with its `bday-day`, `bday-month` and `bday-year` parts, `sex`, `photo`.

**Address**

`street-address` for a multi-line free-text address. Otherwise `address-line1`, `address-line2`,
`address-line3`, then the administrative levels: `address-level1` is the largest subdivision, which
is a state, province or region, and `address-level2` is usually the city or town, with
`address-level3` and `address-level4` for the rarer cases. Then `postal-code`, `country` for the
code and `country-name` for the display name.

The level numbering is where people go wrong from memory. There is no `city` token and no `state`
token. If you write either, autofill does nothing and the failure is silent.

**Payment**

`cc-name`, `cc-given-name`, `cc-family-name`, `cc-number`, `cc-exp` for a combined MM/YY field,
`cc-exp-month`, `cc-exp-year`, `cc-csc` for the security code, `cc-type`. Also
`transaction-currency` and `transaction-amount`.

**Authentication**

`current-password` on a sign-in form. `new-password` on registration and on both fields of a change
password form, which is what makes a password manager offer to generate and then to save rather than
to fill the old value. `one-time-code` on a verification input, which is what lets a phone offer the
code from an incoming SMS above the keyboard.

**Prefixes, which are the part most often missed**

A token may be preceded by `shipping` or `billing`, so a checkout carrying two addresses uses
`shipping address-line1` and `billing address-line1` and the browser fills both correctly. Contact
fields may be preceded by `home`, `work`, `mobile`, `fax` or `pager`, as in `work email`. And any
group may be preceded by `section-` plus a name of your choosing, as in `section-guest email`, to
keep two independent sets of the same fields apart on one page.

**Three rules about tokens**

A wrong token is worse than no token, because it fills the wrong value confidently. If a field has
no matching token, leave it off rather than guessing at something adjacent.

`autocomplete="off"` is widely ignored, particularly for username and password fields, and using it
to defeat password managers is both futile and hostile. The legitimate uses are narrow, such as a
one-off code that must never be stored.

Tokens work best when the surrounding markup agrees: a real `label` with a `for`, a sensible `name`
attribute, and a `type` that matches the content.

## Pass 3. Type versus inputmode

`type` sets validation and semantics. `inputmode` only hints which on-screen keyboard to show. They
are separate decisions and the common bug is using `type` to get a keyboard.

`type="number"` means "a number in the mathematical sense". It attaches spinner buttons, it makes a
mouse scroll wheel change the value while the field is focused, it can strip or reject leading
zeros, and in several browsers a value the parser dislikes is reported as an empty string rather
than as what the user typed. That is correct for a quantity. It is wrong for every identifier that
happens to be made of digits.

The pairings worth memorising:

| Field | type | inputmode | autocomplete |
|---|---|---|---|
| Quantity, age, price | `number` | default | as applicable |
| Postal code | `text` | `numeric` only where the locale is digits-only, otherwise `text` | `postal-code` |
| Card number | `text` | `numeric` | `cc-number` |
| Card security code | `text` | `numeric` | `cc-csc` |
| One-time code | `text` | `numeric` | `one-time-code` |
| Phone | `tel` | inherits a phone pad | `tel` |
| Email | `email` | inherits an email keyboard | `email` |
| Currency with decimals | `text` | `decimal` | as applicable |

`inputmode` values are `none`, `text`, `decimal`, `numeric`, `tel`, `search`, `email` and `url`.
`decimal` and `numeric` differ: `decimal` shows a separator key, `numeric` does not.

Two related attributes. `enterkeyhint` sets the label on the return key, so the last field of a form
can say "go" rather than "return". And `pattern` supplies a client-side format constraint that
survives even when you take over validation, but it must never be the only thing telling the user
the format, because a failed `pattern` with no explanation is a dead end.

## Pass 4. Validation timing

The published evidence for inline validation is old and consistent: a 2009 study reported in A List
Apart, run with a usability consultancy, found inline validation improved completion rates and
satisfaction and reduced errors compared with validating only on submit. What the same body of work
also shows is that inline validation done at the wrong moment is worse than none, because a message
that appears while someone is still typing reads as an accusation of a mistake they have not
finished making.

The rule, in order:

1. **Never validate an untouched field on its first keystroke.** A user typing the first character
   of an email address has not made an error, and telling them so is the most common inline
   validation defect.
2. **Validate on blur.** The user has declared the field finished. This is the moment to check it.
3. **Once a field has errored, revalidate on change.** Now feedback is a reward: the error clears
   the moment the value becomes valid, which is exactly the behaviour that makes the pattern feel
   helpful rather than nagging.
4. **Never move focus while the user is typing**, and never on blur. Auto-advancing between the
   boxes of a split code input is the one common exception, and even there focus must move backwards
   correctly on backspace or the input becomes a trap.
5. **On submit, validate everything, then move focus once**, to the error summary at the top.
6. **Positive confirmation is optional and cheap.** A green tick on a valid field helps most where
   the format is fiddly and the user is unsure.

## Pass 5. Error construction and placement

A good error message contains three things: what was wrong, in the user's terms rather than the
validator's; what a correct value looks like; and, where the error is fixable only elsewhere, where
to go. "Enter a postcode" is weak. "Enter a postcode, for example SW1A 1AA" is complete. "Invalid
input" is a defect.

Placement rules that are not stylistic:

- The message goes **adjacent to the input**, conventionally between the label and the field or
  immediately after the field. An error placed above the whole form scrolls out of view on a long
  form and the user is left with a red field and no words.
- Every message is referenced from the input by **`aria-describedby`**, pointing at the id of the
  error element, so it is read out when the field receives focus. If the field also has hint text,
  `aria-describedby` takes a space-separated list of ids and both are announced.
- The input carries **`aria-invalid="true"`** while it is in error, and the attribute is removed or
  set to `false` when it is not.
- On a submit-time failure, render an **error summary at the top of the form**, with one link per
  failing field whose href is the field's id and whose text is the same message shown at the field.
  Move focus to the summary. This is the pattern that makes a twenty-field form recoverable, and the
  links are the part that gets skipped and the part that does the work.
- Never rely on colour alone. A red border with no text and no icon fails for a substantial
  proportion of users and also for anyone with a monitor in a bright room.

## Pass 6. The international assumptions

**Names.** A fixed set of name fields fails in a way that is hard to see from inside one country. A
single mandatory `given-name` plus `family-name` pair excludes people with one legal name, which is
common in parts of Indonesia and elsewhere. It also imposes an ordering, which is wrong wherever the
family name is written first. It has no room for multiple family names, which is standard in much of
the Spanish-speaking world. Fields sized at fifteen characters truncate real names. Fields rejecting
apostrophes, hyphens, spaces or non-Latin characters reject real names, and the "no special
characters" validator that was added to stop injection attacks belongs on the output side, not the
input side. The well known 2010 "falsehoods programmers believe about names" list is the readable
summary of the whole problem.

The default that survives contact with the world is one field, labelled "Full name", with
`autocomplete="name"`, and a separate optional field for a preferred or display name if you need to
address the person somewhere.

**Postcodes.** Digits-only is wrong: the United Kingdom, Canada, the Netherlands and Ireland all use
letters. Fixed length is wrong: UK postcodes vary between six and eight characters including the
space. Numeric parsing is wrong: several United States ZIP codes begin with a zero, and
`type="number"` will helpfully remove it. Mandatory is wrong: a number of countries and territories
do not use postal codes at all, so the field's requiredness has to depend on the selected country.
Ireland is the instructive case, because it had no national postcode system until Eircode was
introduced in 2015, and any validator written before then and never revisited still rejects the
whole country.

**Addresses.** Line count varies, administrative levels vary, and ordering varies: several
countries, Japan among them, conventionally write the largest unit first. Do not require a state or
region unless the selected country has one. Where you can, use one multi-line `street-address` plus
country, and only split further where a downstream system genuinely needs the parts.

**Phone numbers.** Do not enforce a national format. Accept spaces, dashes, brackets and a leading
plus, then normalise on the server. A phone field that rejects a valid international number is a
signup that does not happen.

## Pass 7. Passwords

Current guidance, which is a reversal of what was standard practice before 2017 and is now settled
in NIST SP 800-63B and echoed by the UK National Cyber Security Centre:

- **Length beats character classes.** Enforce a minimum length and allow a long maximum. Composition
  rules requiring an uppercase letter, a digit and a symbol push people toward predictable
  substitutions and are no longer recommended.
- **Do not force periodic rotation** without evidence of compromise. Forced rotation produces
  incrementing suffixes.
- **Never block paste.** The NCSC published guidance on this in 2017 under the heading of letting
  people paste passwords, and the reason is simple: blocking paste breaks password managers and
  therefore pushes people toward short, memorable, reused passwords. WCAG 2.2, published as a W3C
  Recommendation in October 2023, made this close to an accessibility requirement through success
  criterion 3.3.8, which requires that authentication not depend on a cognitive function test and
  explicitly contemplates password manager and copy-paste support.
- **Check against a breached password list** rather than adding rules.
- **Offer a show-password toggle** with an accessible name that changes state.
- Use `new-password` on registration and change forms, `current-password` on sign-in. Getting this
  backwards is why a password manager sometimes refuses to offer a generated password.

## Pass 8. Submission, buttons and autofill

**Double submission** is the most common defect in this pass and it is invisible in testing, because
testers click once on a fast connection. The guard has two halves: disable the control the moment it
is pressed and keep it disabled until the response resolves, and make the request idempotent on the
server with a key generated by the client, because the network can duplicate a request without any
help from the user.

**A permanently disabled submit button is a cost, not a safety feature.** A button disabled until
the form is valid removes the only affordance the user has for asking what is wrong, is skipped by
some assistive technology because disabled controls are not focusable, and leaves someone stuck on a
form with no visible error and no way forward. Prefer an always-enabled button that validates on
press and shows the error summary. Disable only for the in-flight case.

**The loading state** keeps the button's width stable so the layout does not jump, keeps a label
rather than replacing the text with a bare spinner, and announces the change through a live region
or `aria-busy` so it is not a purely visual event.

**Autofill styling** breaks forms in three specific ways. Browsers based on Chromium and WebKit
apply their own background colour to autofilled fields through the `-webkit-autofill` pseudo-class,
and a plain `background-color` rule does not override it, which is why an autofilled field in a dark
theme turns pale yellow with unreadable text. The workaround is a large inset `box-shadow` in the
intended colour plus `-webkit-text-fill-color`; the standard `:autofill` pseudo-class now exists
alongside the prefixed one. Second, floating labels frequently overlap autofilled text, because the
label's raised state is driven by a value the component did not observe being set. Third, a
controlled component can hold a stale empty value while the browser displays a filled one, so
validate against the DOM value on submit rather than trusting component state alone.

**Multi-step forms** are worth it when the form is long enough that a single page reads as a wall,
when steps have genuinely different subjects, or when a later step depends on an earlier answer.
Three rules make them survivable: save on every step transition so a closed tab does not cost
everything, give each step its own URL so the browser back button works and a step can be linked,
and never put the hardest question first.

## The applicability pass

Do not apply every rule to every form. Go field by field and mark each rule **APPLIES**, **NOT
APPLICABLE** with a mechanism, or **DEFERRED** with a reason. The mechanisms are the deliverable,
because they are what a reviewer checks.

"Not applicable, this form is internal and never sees a non-UK address" is a mechanism. "Users will
not do that" is not. A typical public-facing form of twelve fields will have three or four rules
genuinely out of scope and the rest applying.

## Decision rule: one field or several?

- **If the value has a single canonical format and the standard has one token covering the whole
  thing**, use one field. Email, telephone and postcode are all one field.
- **If the parts are separately consumed by a named downstream system and the standard has a token
  for each part**, split, and put one token on each part. A shipping address that feeds a carrier
  API is the clear case.
- **If the split exists only so the database has tidy columns**, do not split. A name split into
  given and family so that a greeting can say "Hi Alex" is not worth the international failure rate,
  and a separate optional preferred-name field solves the greeting properly.
- **If you cannot tell**, ship one field plus a parser, and log the values the parser cannot split
  for a fortnight. One field with imperfect parsing loses less than a split a meaningful share of
  users cannot fill correctly, and the log tells you whether the split is worth adding later.

## Worked example

A guest checkout step in a general online store: full name, email, address, card, and a "same as
shipping" toggle for billing.

The census finds eleven fields. Tokens assigned: `name` on the single name field, `email`,
`shipping address-line1`, `shipping address-line2`, `shipping address-level2` for the town,
`shipping address-level1` for the county or state, `shipping postal-code`, `shipping country`,
`cc-number`, `cc-exp`, `cc-csc`. The billing set, revealed when the toggle is off, repeats the
address tokens with the `billing` prefix rather than inventing new field names.

Types and modes: the postcode moves from `type="number"` to `type="text"` with no numeric input mode,
because the store ships to the United Kingdom and Ireland where postcodes contain letters. The card
number becomes `type="text"` with `inputmode="numeric"`, which keeps the numeric keypad on a phone,
allows the spaces people type when copying from a physical card, and removes the spinner and the
scroll-wheel hazard.

Validation: on blur for every field, revalidation on change once errored, and an error summary at
the top on submit with one link per failure. The card number gets a Luhn check on blur so a
transposed digit is caught before the payment attempt rather than as a bank decline.

International: the county field becomes optional and its label changes with the selected country;
the postcode field's requiredness follows the country too.

Submission: the pay button stays enabled and validates on press, disables only while the request is
in flight, keeps its width, and carries a client-generated idempotency key.

Not applicable, with mechanisms: no password rules, because this is guest checkout with no account;
no multi-step design, because eleven fields fit one screen at normal zoom.

**Verdict: the form is competent on layout and defective on markup.** Five fields carry no token,
two carry invented ones, the postcode field is a `number`, errors render above the form rather than
at the field, and the pay button has no in-flight guard. All of it is a one-commit fix and none of it
was visible in the design review.

## Failure modes

**Invented tokens.** `autocomplete="city"` and `autocomplete="state"` look right and do nothing.
The browser ignores an unrecognised token silently, so the field simply never fills and no error is
raised anywhere.

**Tokens on the form instead of the fields.** `autocomplete="on"` on the `form` element does not
tell the browser what any individual field means. It is a permission, not a description.

**`type="number"` as a keyboard shortcut.** The developer wanted a numeric keypad on a phone and got
spinners, scroll-wheel edits, and a postcode with its leading zero removed.

**Validation on the first keystroke.** The field turns red while the user is still typing the third
character of their email address. It is technically inline validation and it reads as hostility.

**Errors placed above the form on a long page.** The user submits, the page scrolls to the top,
they read the message, they scroll down, and the message is now off screen while the field is red
and wordless.

**Error text that is not linked to its field.** Visually adjacent, but with no `aria-describedby`,
so a screen reader user hears the label and the value and nothing about the problem.

**The disabled submit button with no explanation.** Nothing is red, nothing is announced, the button
is grey, and the user cannot discover which of eleven fields is unsatisfied.

**Blocked paste on a password or a card field.** Presented as a security measure, it defeats
password managers, forces short reused passwords, and makes the form unusable for anyone who cannot
type accurately.

**Autofill applied but the component never saw it.** The browser fills the field, the label stays
sitting on top of the text, the framework's state is still empty, and the form reports the field as
missing on submit.

## What this skill does not do

- It does not test with real assistive technology. It checks the attributes that should produce an
  announcement, not whether a given screen reader and browser combination actually produces one.
- It says nothing about server-side validation, which is where the security lives. Every rule here is
  client-side and none of it can be trusted by a server.
- It cannot tell you which field is costing you completions. That needs funnel analytics or session
  replay, and no reading of markup substitutes for it.
- It does not judge whether the form should ask for a field at all. It will tell you that a field is
  correctly built while being entirely wrong to request.
- It does not cover file uploads, rich text editors, signature capture or payment provider iframes,
  where the constraints are set by the embedded component rather than by your markup.
- Browser support for autofill tokens is uneven and changes. A correct token is the necessary
  condition rather than a guarantee, and the only proof is testing on the browsers you support.
