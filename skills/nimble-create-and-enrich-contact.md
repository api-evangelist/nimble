---
name: Create and enrich a Nimble contact
description: Create a person or company contact, add tags and a note, and update fields — respecting Nimble's multi-valued labelled field model and its lack of any idempotency contract.
api: openapi/nimble-contacts-api-openapi.yml
operations:
  - post-contact
  - get-contact
  - put-contact
  - put-contact-tags
  - post-contact-note-for-contact
  - list-contacts-fields-metadata
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Contacts
---

# Create and enrich a Nimble contact

Base host `https://app.nimble.com`. All operations here are on `/api/v1`.
Authenticate first — see `nimble-authenticate-and-verify.md`.

## The field model you must understand before writing

A Nimble contact is **not** a flat record. Fields are a map of field name to an
**array of labelled values**:

```json
"fields": {
  "phone": [
    {"modifier": "mobile", "value": "+1 555 0100", "label": "phone"},
    {"modifier": "home",   "value": "+1 555 0101", "label": "phone"}
  ],
  "last name": [
    {"modifier": "", "value": "Akopyan", "label": "last name"}
  ]
}
```

So one logical attribute carries several labelled values at once, and
`modifier` (`work`, `home`, `mobile`, `other`, `linkedin`, …) is what
distinguishes them. Writing a bare scalar where an array is expected is the
most common cause of a `409`.

## Step 1 — discover the account's field set

Call `list-contacts-fields-metadata` (`GET /api/v1/contacts/metadata`) before
your first write. Contact fields are **user-definable per account**, so the
available field names are not fixed by the API. Do not assume a field exists
because it exists in another Nimble account.

## Step 2 — create the contact

Call `post-contact` (`POST /api/v1/contact`).

- `record type` must be `person` or `company`.
- For a person, first name or last name is required and must be non-empty —
  the documented validation message is *"First name or last name field is
  required for person and should not be empty"*.

**There is no idempotency contract.** Nimble publishes no `Idempotency-Key`
header on any of its 89 operations. A retried `post-contact` creates a
**duplicate contact**. Before retrying a create whose response you did not
see, search for the contact first (see
`nimble-search-contacts.md`) and only create if the search returns nothing.

Watch for `402` / `code: 108` here specifically — contact creation is the
operation that hits the plan's 25,000-record ceiling.

## Step 3 — read it back

Call `get-contact` (`GET /api/v1/contact/{contact_id}`). Use the `contexts`
query parameter to pull related blocks in the same call. Do **not** use
`last_contacted`, `files_data` or `leads_data` — all three are deprecated in
favour of `contexts`.

## Step 4 — update fields

Call `put-contact` (`PUT /api/v1/contact/{contact_id}`). Send the same
array-of-labelled-values shape. A `403` here means the authenticated user does
not own the contact.

## Step 5 — tag it

Call `put-contact-tags` (`PUT /api/v1/contacts/{contact_id}/tags`). This
**replaces** the tag set rather than appending — read the current tags first
if you intend to add one.

## Step 6 — add a note

Call `post-contact-note-for-contact`
(`POST /api/v1/contacts/{contact_id}/notes`). Returns `201`.

Note the API exposes several overlapping note routes —
`post-contact-note` (`POST /api/v1/contacts/notes`) and
`post-contact-note-with-id` (`POST /api/v1/contacts/notes/{note_id}`) also
exist. Prefer the contact-scoped route above; it is the one that unambiguously
binds the note to a contact.

## Errors

`409`/`245` validation (read the `errors` map for the offending field and its
`field_id`), `402`/`108` quota, `403` ownership, `404` bad identifier (body
carries `object_type` and `object_id` instead of `message`/`code`).
Full catalog: `errors/nimble-problem-types.yml`.
