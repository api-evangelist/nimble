---
name: Move a deal through a Nimble pipeline
description: Discover pipelines and stages, create a deal, advance it, and read pipeline state — on the /api/v2 surface, which is versioned separately from contacts.
api: openapi/nimble-deals-api-openapi.yml
operations:
  - list-deals-pipelines
  - get-deals-pipeline
  - create-new-deal
  - get-deal
  - put-deal
  - list-user-deals
  - list-pipeline-deals-by-stages
  - list-pipeline-deals-by-owner
  - create-deal-note
  - get-won-last-month-deals-sum
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Deals
---

# Move a deal through a Nimble pipeline

**Deals live on `/api/v2`, not `/api/v1`.** Contacts are v1, deals are v2, and
two deal-tag operations sit on a legacy `/api/2` prefix. Base host is
`https://app.nimble.com`. OAuth callers need the `deals` scope.

## Step 1 — discover pipelines and stages

Call `list-deals-pipelines` (`GET /api/v2/deals/pipelines`). Pipelines,
their stages, and their custom field sets are all account-defined — there is no
default pipeline you can assume.

For one pipeline's full definition call `get-deals-pipeline`
(`GET /api/v2/deals/pipelines/{pipeline_id}`).

Deal fields are per-pipeline. `list-all-user-deals-fields`
(`GET /api/v2/deals/fields`) and `list-column-catalogue`
(`GET /api/v2/deals/column_catalogue`) describe the available field surface.

## Step 2 — create the deal

Call `create-new-deal` (`POST /api/v2/deals`). Bind it to a real
`pipeline_id` and `stage_id` read in step 1; a stale or invented id returns
`404` with an `object_type`/`object_id` body.

**No idempotency contract exists.** A retried `create-new-deal` creates a
second deal. If a create times out, call `list-user-deals` and check before
retrying.

## Step 3 — advance and update

Call `put-deal` (`PUT /api/v2/deals/{deal_id}`) to change the stage, value,
owner or custom fields. `403` means the authenticated user cannot administer
that deal.

## Step 4 — read pipeline state

- `list-pipeline-deals-by-stages`
  (`GET /api/v2/deals/pipelines/{pipeline_id}/deals`) — the Kanban view: deals
  grouped by stage.
- `list-pipeline-deals-by-owner`
  (`GET /api/v2/deals/pipelines/{pipeline_id}/owners`) — the same book grouped
  by owner.
- `list-user-deals` (`GET /api/v2/deals`) — everything visible to the caller.
- `get-deal` (`GET /api/v2/deals/{deal_id}`) — one deal.
- `get-won-last-month-deals-sum`
  (`GET /api/v2/deals/widget/won_last_month`) — the won-value rollup Nimble's
  own dashboard widget uses. Use it instead of summing deals yourself.

## Step 5 — annotate

`create-deal-note` (`POST /api/v2/deals/{deal_id}/notes`) attaches a note.
Files attach through `store-deal-file`
(`POST /api/v2/deals/{deal_id}/files`), and
`get-deal-file-download-url`
(`GET /api/v2/deals/{deal_id}/files/{file_id}/url`) returns a download URL
rather than the bytes.

## Pipeline administration

Creating and reshaping pipelines is a separate, destructive surface —
`post-deals-pipelines`, `put-deals-pipeline`, `post-deals-pipeline-stage`,
`put-deals-pipeline-stage`, `mark-deals-pipeline-stage-archived`,
`mark-deals-pipeline-archived`, `mark-deals-pipeline-unarchived`,
`post-deals-pipeline-lost-reason`, and `delete-deals-pipeline`.

Archiving a stage or pipeline affects every deal in it. Treat all of these as
requiring explicit human confirmation; none of them is reversible through a
single inverse call except the archive/unarchive pair.

## Errors

`409`/`245` validation, `403` ownership, `404` unknown id, `406` on tag,
stage, group and lost-reason mutations that violate a resource constraint.
See `errors/nimble-problem-types.yml`.
