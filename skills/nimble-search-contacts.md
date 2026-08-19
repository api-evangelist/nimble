---
name: Search Nimble contacts with the advanced query syntax
description: Find contacts by keyword or by structured JSON query, page through results correctly, and avoid the mutually-exclusive parameter trap that returns 409.
api: openapi/nimble-contacts-api-openapi.yml
operations:
  - list-contacts
  - list-contacts-ids
  - list-contacts-for-identifiers
  - list-contacts-fields-metadata
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Search-Contacts
---

# Search Nimble contacts

`GET https://app.nimble.com/api/v1/contacts` — operation `list-contacts`.

## Pick one search parameter, never both

- `keyword` — simple text lookup.
- `query` — the advanced structured search, a JSON object.

**Sending both returns `409` with `"code": 245`** and the message *"You can
specify either `keyword` or `query` parameter, not both!"*. This is the single
most common failure on this endpoint.

## Advanced query syntax

A query is a JSON object of terminal clauses joined by `and`, `or`, or `not`
(`not` takes a single subquery).

Terminal shape:

```json
{"<field>": {"<operator>": "<value-or-object>"}}
```

Composite:

```json
{"and": [
  {"skype id": {"is": "john.doe"}},
  {"record type": {"is": "person"}}
]}
```

### Operators

- **Text**: `contain`, `not_contain`, `starts_with`, `is`, `is_not`,
  `is_empty`, `in`. (`not_in` exists only for specific fields such as `_id`.)
  `contain` matches normalized tokens; `starts_with` is prefix matching.
- **Datetime**: `in_the_last`, `not_in_the_last`, `range`, `not_in_range`,
  `day_month_range`, `is_empty`. Units for `in_the_last`: `second`, `day`,
  `week`, `month`, `year`. Date ranges use `start_date`/`end_date` as
  `%Y-%m-%d`; datetime ranges use `start_datetime`/`end_datetime` as RFC 3339.
- **Numeric**: `is`, `is_not`, `is_empty`, `in`, `gt`, `gte`, `lt`, `lte`,
  `range`, `not_in_range`.

### Normalization

The Nimble Search Engine lowercases and ASCII-folds accents on **both** the
index and the query, so `cAr`, `čar` and `ČAR` all match `car`. Do not
pre-normalize or case-fold yourself.

### Synthetic fields

- `names` (alias `name`) — person/company name only. Built from
  `first_name`, `middle_name`, `last_name`, `company_name`.
- `__any` — broad keyword lookup across many contact attributes; `names` is
  effectively a subset of it.

Prefer explicit fields (`email`, `domain`, `address.city`) over `__any` when
you need precision — `__any` spans phone, social description and employment
fields and will over-match.

### Record type

`record type` controls the contact kind and takes exactly one of `person`,
`company`, `all`. Place it as a regular clause inside the top-level `and`
when combining filters.

## Common fields

`__any`, `name`/`names`, `first name`, `last name`, `company name`, `title`,
`email`, `phone`, `domain`, `skype id`, `address`, `city`, `state`, `country`,
`street`, `zip`, `created`, `updated`, `birthday`,
`company last contacted`, `user last contacted`, `tag`, `record type`, `_id`,
`saved_search`.

The authoritative per-account field list comes from
`list-contacts-fields-metadata` — call it rather than assuming, because
contact fields are user-definable.

## Paging

Use `page` (1-indexed) and `per_page`. The response is
`{"meta": {...}, "resources": [...]}` — read `meta` for the paging state.

Pagination is **not uniform across this API**: some list operations take
`limit` instead of `per_page`, and `list-activities` uses a `next_tstamp`
timestamp cursor. Read the parameters of the specific operation you are
calling.

## Cheaper variants

- `list-contacts-ids` (`GET /api/v1/contacts/ids`) returns identifiers only —
  use it to size a result set or to drive a batch without pulling full records.
- `list-contacts-for-identifiers` (`GET /api/v1/contact`) fetches a known set
  of contacts by identifier.

Use `fields` to restrict the returned field set and `contexts` to request
related blocks. `last_contacted`, `files_data` and `leads_data` are deprecated
in favour of `contexts`.
