---
name: Manage Nimble custom contact fields
description: Read, create and reshape the account's user-definable contact field set — fields, choice lists, groups and tabs — before integrating any data that depends on them.
api: openapi/nimble-contacts-fields-api-openapi.yml
operations:
  - list-company-contacts-fields
  - list-contacts-fields-metadata
  - post-contact-field
  - put-contact-field
  - delete-contact-field
  - post-contact-choice-field
  - put-contacts-field-choice
  - delete-contact-choice-field
  - create-contacts-fields-group
  - put-contacts-fields-group
  - post-contacts-fields-tab
  - put-contacts-tab-by-id
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Contacts-Fields
---

# Manage Nimble custom contact fields

Nimble does not expose an opaque metadata bag. It exposes a **first-class,
user-definable field schema** on the contact record, and that schema differs
per account. Any integration that writes contact data must read the field set
first — this is the step most integrations skip and then fail on with a `409`.

All operations are on `/api/v1`, host `https://app.nimble.com`. OAuth callers
need the `contacts` scope.

## Read the schema first

- `list-company-contacts-fields` (`GET /api/v1/contacts/fields`) — the
  account's contact field definitions.
- `list-contacts-fields-metadata` (`GET /api/v1/contacts/metadata`) — field
  metadata including presentation types.

Field values on a contact are arrays of `{value, label, modifier}` objects, not
scalars — see `nimble-create-and-enrich-contact.md`.

## Create a field

`post-contact-field` (`POST /api/v1/contacts/fields`). Update with
`put-contact-field` (`PUT /api/v1/contacts/fields/{field_id}`).

`delete-contact-field` (`DELETE /api/v1/contacts/fields/{field_id}`) removes
the field **and the data held in it across every contact**. Require explicit
human confirmation. There is no undo and no soft-delete.

## Choice (select) fields

A choice field owns its option list:

- `post-contact-choice-field`
  (`POST /api/v1/contacts/fields/{field_id}/choices`) adds an option.
- `put-contacts-field-choice`
  (`PUT /api/v1/contacts/fields/{field_id}/choices/{id}`) renames one.
- `delete-contact-choice-field`
  (`DELETE /api/v1/contacts/fields/{field_id}/choices/{id}`) removes one —
  again affecting every contact currently holding that value.

Writing a value that is not in the choice list returns `409`/`245`. Read the
options before writing.

## Groups and tabs — the presentation layer

Fields are organized into groups, and groups onto tabs:

- Groups: `create-contacts-fields-group`
  (`POST /api/v1/contacts/fields/groups`),
  `put-contacts-fields-group`
  (`PUT /api/v1/contacts/fields/groups/{group_id}`),
  `delete-contacts-fields-group`
  (`DELETE /api/v1/contacts/fields/groups/{group_id}`).
- Tabs: `post-contacts-fields-tab` (`POST /api/v1/contacts/fields/tabs`),
  `put-contacts-tab-by-id` (`PUT /api/v1/contacts/fields/tabs/{tab_id}`),
  `delete-contacts-tab-by-id`
  (`DELETE /api/v1/contacts/fields/tabs/{tab_id}`).

These change what every user in the account sees. They are administrative
operations, not data operations.

## Primary-value marks

`put-primary-mark-to-contact-field`
(`PUT /api/v1/contact/{contact_id}/field`) and
`delete-primary-mark-from-contact-field`
(`DELETE /api/v1/contact/{contact_id}/field`) set or clear which of a
multi-valued field's entries is primary — for example which of three email
addresses is the one to use.

## Errors

`409`/`245` validation (the `errors` map names the field and its `field_id`),
`404` unknown field/group/tab id, `406` where a constraint forbids the change.
See `errors/nimble-problem-types.yml`.
