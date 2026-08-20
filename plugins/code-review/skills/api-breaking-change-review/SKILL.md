---
name: api-breaking-change-review
description: Reviews a change to a JSON, REST or gRPC API for compatibility, using the full catalogue of what actually breaks a consumer rather than only what a schema diff can see. Covers field removal and renaming, type narrowing, newly required inputs, tightened validation, changed defaults, enum additions, pagination and sort semantics, error body and status code changes, altered rate limits and timeouts, protobuf field numbering and reserved ranges, and the deprecation headers defined by RFC 8594 and RFC 9745. This skill should be used when reviewing a pull request that changes an API response, request schema, proto file or OpenAPI document, when planning a deprecation, or before removing any field.
---

# API breaking change review

## The claim this skill is built on

Most reviewers hold a short list of breaking changes: removing a field, renaming a field, changing a type. That list is correct and it is roughly a third of the real one.

The changes that actually cause incidents are the ones that leave the schema intact. The field is still there. Its type is unchanged. It is still optional. And the value inside it now means something else, or arrives in a different order, or is rejected by a validator that used to let it through. No schema diff tool fires, because nothing in the schema moved.

So the review cannot be a diff of the schema. It has to be a diff of the **promise**, and the promise includes everything a consumer could reasonably have built on, which is more than the schema is able to say.

## The catalogue: what breaks a JSON consumer

Work through this list against the change in front of you. Items are ordered by how likely they are to be missed, not by severity.

**1. Changing the meaning of a field while keeping its name and type.** The worst item on the list, and it leads because nothing detects it. An amount that was in minor units is now in major units. A timestamp that was in local time is now in UTC. A `status` of `active` used to include trialling accounts and now does not. Every consumer keeps parsing successfully and every consumer is now wrong. There is no automated defence. The only defence is that a reviewer asks, on every changed field, whether the values it can now hold mean what they meant before.

**2. Adding a value to an existing enum.** Reviewers file this under additive. It is not. A consumer that switches exhaustively over the known values, or whose generated client validates against the enum and throws on an unknown member, breaks the first time the new value appears in production data. This is why enum handling should be open from the start: consumers should carry a default branch, and the API should document that new values may appear. If you cannot guarantee every consumer does that, adding an enum value is a breaking change and needs the same treatment as one.

**3. Tightening validation.** A field that accepted a 200 character string now accepts 100. An email regex got stricter. A previously ignored unknown field is now rejected. Each of these is usually motivated by real bad data, and each of them turns a working integration into a stream of 400s. Tightening is breaking even when the data it rejects was always invalid.

**4. Making an optional request field required.** Obvious once stated, and routinely shipped as part of "improving validation".

**5. Narrowing a type.** Integer to enum, string to a constrained format, nullable to non-nullable on the request side. Widening a response type is also breaking in the other direction: a field that was always an integer and can now be a string breaks every strictly typed client.

**6. Changing a default.** A request parameter whose default page size moves from 100 to 20, a flag that defaults on instead of off, an optional field that used to be omitted and is now emitted as null. Consumers depend on defaults precisely because they never send those parameters.

**7. Pagination semantics.** Changing the default page size, changing whether the last page is signalled by an empty array or a null cursor, changing the lifetime of a cursor, or moving from offset to cursor pagination, which removes the ability to jump to page 40 even though nothing in the response shape changed.

**8. Sort stability.** If a list endpoint sorts on a non-unique key and the underlying ordering changes, offset-paginated consumers start seeing duplicated and skipped rows, without any error anywhere. Adding a unique tiebreaker to the sort is one of the few changes that is both breaking and unambiguously an improvement.

**9. Error codes and error body shape.** Moving a failure from 400 to 422, splitting one error code into three, or restructuring the error body from an ad hoc object to a standard problem document. Error handling is the least tested part of most clients and the most brittle. If you are moving to a standard shape, the specification to move to is the problem details format, defined in RFC 9457, which replaced RFC 7807 in 2023.

**10. Rate limits and quotas.** Lowering a limit breaks a working integration with no code change on either side. So does changing the window from a fixed minute to a rolling one, or changing which key the limit is counted against. A 429 has been the standard status since RFC 6585 in 2012, and the honest way to lower a limit is to publish the new one, return `Retry-After`, and give consumers a period during which the limit is logged rather than enforced.

**11. Timeouts and latency.** Nobody writes this down as an API change and it is one. A client with a 5 second timeout breaks when your p99 moves from 2 seconds to 6, and the change that moved it was an internal refactor with no schema impact at all. If an endpoint's latency profile is changing materially, that belongs in the review.

**12. HTTP status and redirect changes.** Returning 200 with an error body inside means retries do not fire, monitoring does not alert, and every client has to parse the body to know whether the call worked. Changing a 201 to a 200 breaks clients that check the exact code, and 201 also carries the expectation of a `Location` header pointing at the created resource. Redirect changes matter more than they look: 307 and 308 preserve the method and body, while 301 and 302 have a long history of clients turning a POST into a GET, so switching between those families silently changes what arrives at your server.

**13. Cache header changes.** Adding `Cache-Control` with a max age to a response that was previously uncacheable means some consumers now read stale data and cannot tell. Removing `ETag` breaks conditional requests. Changing `Vary` can cause a shared cache to serve one tenant's response to another, which is a correctness and a security change at once.

## When adding a field is not safe

Adding a response field is the standard example of a safe change and it usually is. The exceptions are specific and worth knowing:

- Consumers validating the response against a strict schema, which in JSON Schema means `additionalProperties: false`. Any new field is a hard failure.
- Consumers that hash or sign the whole payload, where an added field changes the signature.
- Consumers that persist the whole body into a strictly typed column or a table with a fixed set of columns.
- Discriminated unions, where a client picks a variant by which fields are present. Adding a field can make an object match two variants.
- Response size limits, in mobile clients and in gateways with a maximum body size.
- Name collisions, where a consumer already synthesises a field of the same name locally and now finds it overwritten.

None of these are reasons not to add fields. They are reasons to know which of your consumers is in one of these categories before you do.

## Protobuf and gRPC

Protobuf compatibility has a different shape because the wire format is positional.

**Field numbers are the contract. Names are not.** The number is what appears on the wire. You can rename a field freely without breaking any binary consumer. You cannot change its number under any circumstances, and you cannot reuse a retired number, because old data and old clients still carry the previous meaning at that position. This is what `reserved` is for: when a field is deleted, reserve both its number and its name, and the compiler will refuse to let anyone reuse them years later when everyone who remembers has moved on.

There is a trap inside the rename freedom. Proto3 JSON mapping derives the JSON key from the field name in lowerCamelCase, so a rename that is invisible to every binary client breaks every JSON one. If a proto is serving both, a rename is breaking, and `json_name` is the escape hatch that lets the wire name and the JSON key diverge.

**Field numbers 1 to 15 encode their tag in a single byte** and 16 upwards take two. Spend the low numbers on fields that appear in almost every message, and do not spend them on a field you expect to retire.

**Type changes that parse are not therefore safe.** The varint family, `int32`, `int64`, `uint32`, `uint64` and `bool`, is mutually parseable, so changing between them never fails to decode. It does change values: a negative `int32` read as a `uint32` becomes an enormous positive number, and a large `int64` truncated into an `int32` loses its top bits. The signed zigzag types, `sint32` and `sint64`, use a different encoding and are not compatible with `int32` and `int64` at all. `string` and `bytes` are compatible only while the bytes are valid UTF-8. Treat any type change as breaking unless you have checked the specific pair.

**The proto3 presence trap.** Before explicit presence was added, a proto3 scalar field set to its default, 0 or the empty string or false, was indistinguishable from an unset field and was not written to the wire at all. This is why "the client sent zero" and "the client sent nothing" collapse into the same thing, and why partial update endpoints built on proto3 messages cannot express "set this to zero". Explicit presence via `optional` on a proto3 field, which became stable in protobuf 3.15 in early 2021, restores the distinction. Adding `optional` to an existing field changes the generated API surface in most languages, so it is a source-level break even though the wire is unaffected.

**gRPC method rules.** Adding a method is safe. Removing one gives callers UNIMPLEMENTED at runtime rather than an error at build time. Changing a method's cardinality, unary to server-streaming for example, is breaking even with identical messages, because the generated stub's signature and the framing both change.

## Deprecation that works

The mechanics matter more than the announcement, because announcements are not read.

**Signal in the response, not only in the docs.** RFC 8594, published in 2019, defines the `Sunset` response header carrying the date after which the resource is expected to become unresponsive, and registers a `sunset` link relation for pointing at an explanation. RFC 9745, published in March 2025, defines the `Deprecation` header field, carrying the date at which the deprecation takes or took effect, with a `deprecation` link relation for the migration guide. Together they let a consumer discover the timeline from a response they were already making, which is the only channel guaranteed to reach every integrator including the one who left the company.

**Measure before removing, and know which measurements are possible.** Request-side usage is directly observable: if a consumer sends a deprecated parameter, it arrives at your server, and you can count it per client. Response-side usage is not observable at all. You cannot see which fields a consumer reads, because reading happens after the response leaves you. This asymmetry is the single most useful fact in deprecation planning, and it produces one hard rule: never remove a response field whose usage you cannot see unless you have another channel of evidence. Acceptable substitutes are a field-level opt-in that consumers must request, per-consumer contract tests you run yourself, or a dark period where the field is emitted as null for a scheduled window while you watch support channels.

**Never remove anything you cannot see usage for.** State it as a policy, because the alternative is a removal justified by "it is probably unused", which is a guess wearing the clothes of a decision.

**Versioning strategies, and their honest costs.** URL versioning is legible, easy to route and easy to document, and it fragments the API: two versions are two implementations, and the second one is the one nobody maintains. Header or media type versioning keeps one URL space and is invisible in a browser, in a curl command pasted into a ticket, and in most gateway logs, which makes debugging harder for everyone. The additive-only approach, where you never remove or change anything and only add, has the lowest consumer cost and the highest internal cost: the response accumulates fields nobody uses and the code accumulates branches nobody can delete.

The trap common to all three is that "we will just version it" is usually a way of postponing the problem. A new version does not remove the old one, and until the old one is removed the change has not happened. Any versioning proposal should come with the sunset date for the previous version, or it is not a plan.

## The review checklist

Run this against every API diff, in this order:

1. **Fields removed or renamed**, in requests and responses separately.
2. **Types narrowed or widened**, including nullability in both directions.
3. **Optionality changes**, especially request fields becoming required.
4. **Validation rules**, including length, format, and rejection of unknown fields.
5. **Defaults**, for every parameter and every emitted field.
6. **Enums**, in both directions. Additions count.
7. **Collections**: pagination, ordering, sort stability, and the empty-page signal.
8. **Errors**: status codes, codes inside the body, and the body shape.
9. **Non-schema promises**: rate limits, quotas, timeouts, idempotency behaviour.
10. **HTTP mechanics**: status codes, redirects, cache and `Vary` headers.
11. **Semantics**: for each surviving field, has the set of values it can carry changed meaning.
12. **Evidence**: for each removal, what measurement supports it.

## Decision rules

- **If** a change is on the catalogue and consumers are external or independently deployed, **then** it is breaking, and it needs a deprecation window with dated headers, not a changelog entry.
- **If** a change is on the catalogue but every consumer is in the same repository and deploys atomically with the server, **then** ship both sides in one change and skip the ceremony.
- **If** the change is a widening, adding an optional response field or accepting an additional input format, **then** ship it, after checking the strict-schema and payload-signing exceptions above.
- **If you cannot tell** whether anyone depends on the behaviour, which is the normal case for response fields, **then** do not resolve it by guessing. Convert it into a measurable question: emit the header pair with a date, add a per-consumer counter on the request side if the change has a request-side proxy, or run a scheduled dark period. If none of those is possible, keep the field, mark it deprecated in the documentation, and record explicitly that it cannot be removed. An undeletable field is a smaller cost than a broken consumer you cannot identify.

## Worked example

The diff changes a list endpoint that returns orders.

```diff
- "status": { "type": "string", "enum": ["pending", "shipped", "cancelled"] },
+ "status": { "type": "string", "enum": ["pending", "shipped", "cancelled", "partially_shipped"] },
- "total": { "type": "integer", "description": "Order total in minor units" },
+ "total": { "type": "number", "description": "Order total" },
  "items": { "type": "array" },
- "page_size": { "type": "integer", "default": 100 },
+ "page_size": { "type": "integer", "default": 25, "maximum": 100 },
```

Plus, in the handler and not in the schema, the ordering changed from `created_at DESC` to `updated_at DESC`.

**Findings:**

- `status` gained a value. Breaking for any consumer switching exhaustively, and orders that were previously reported as `pending` will now arrive as `partially_shipped`, so this also changes the meaning of an existing value. Two findings, not one.
- `total` changed from integer minor units to a number with no unit stated. This is the worst category: consumers that divided by 100 now under-report by a factor of 100, and they will not error. It is also a type widening, so strictly typed clients break outright. The description change is the only evidence of the semantic move, and descriptions are not enforced anywhere.
- `page_size` default dropped from 100 to 25. Every consumer that omits the parameter, which is most of them, now receives a quarter of the rows and, if they do not follow pagination correctly, silently processes a quarter of the data.
- The sort key change is invisible in the schema and is the most dangerous item here. With offset pagination over a table where `updated_at` moves while a consumer is paging, rows shift between pages, so consumers will both duplicate and skip orders with no error.

**Verdict: hold, and split.** The `total` change must not ship in this form: keep `total` as integer minor units, add `total_decimal` alongside it, and deprecate the old field with a `Deprecation` header and a `Sunset` date once request-side telemetry or contract tests can show who is on the new field. The enum addition ships behind a documented open-enum contract plus an announcement, since it cannot be avoided and can be warned about. The `page_size` default reverts to 100, with the new maximum kept, since the cap is defensible and the default change is not. The sort change ships only with a unique tiebreaker added, `updated_at DESC, id DESC`, and the endpoint should move to cursor pagination before any further ordering change.

## Failure modes

**Schema-diff blindness.** The reviewer trusts the tool, the tool compares schemas, and the change was in the handler. Symptom: a clean automated report on a release that changes sort order, defaults or units.

**Additive laundering.** Anything phrased as "we are only adding" is waved through, so enum values and newly emitted null fields ship as safe. Symptom: an incident traced to a value that had never appeared in production data before.

**Validation virtue.** A tightening is justified by the data it rejects being invalid, so nobody treats it as breaking. Symptom: a spike of 400s from one integrator immediately after a release that "only fixed validation".

**Version as a plan.** A v2 is created, v1 is left running forever, and the change is considered done. Symptom: three live versions, no sunset dates, and the oldest one carrying the most traffic.

**Removal by assumption.** A field is deleted because it looks unused, with no measurement, often because response-side usage cannot be measured and that absence is read as evidence of absence. Symptom: a support ticket from a consumer nobody knew existed.

**Field number reuse.** A proto field is deleted, the number is not reserved, and a year later somebody reuses it for a different type. Symptom: stored messages and older clients decoding garbage into a field that looks fine in the schema.

**Status code drift.** Errors are returned as 200 with an error object, because it makes one client simpler. Symptom: retry logic and alerting that never fire, and an outage measured in hours because every dashboard was green.

## What this skill does not do

- It does not detect breaking changes mechanically. For protobuf, a breaking change detector in continuous integration is strictly better at the wire rules and should be running regardless.
- It cannot see your consumers or their code. Every impact statement it makes is conditional on usage data you have to gather.
- It does not cover GraphQL, whose deprecation model, field-level usage telemetry and federation rules are a separate subject.
- It says nothing about authentication, authorisation or data exposure. A compatible change can still be a security regression.
- It does not decide your versioning strategy. It will tell you the costs of each and insist that any version proposal comes with a sunset date for the previous one.
