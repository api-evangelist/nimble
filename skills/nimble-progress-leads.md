---
name: Progress a lead through a Nimble contact pipeline
description: Move a contact-as-lead between stages of a contact pipeline, mark it won or lost, and undo a transition — the surface distinct from deal pipelines.
api: openapi/nimble-leads-api-openapi.yml
operations:
  - list-available-contacts-pipelines
  - list-contact-proceedings
  - move-lead
  - mark-lead-exited-successful
  - put-lead-exit-successful
  - mark-lead-exited-unsuccessfully
  - put-lead-exit-unsuccessful
  - undo-lead-transition
  - delete-lead-transitions
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Contacts-Pipelines
---

# Progress a lead through a Nimble contact pipeline

Nimble has **two different pipeline systems** and confusing them is the
main failure mode on this surface:

- **Deal pipelines** (`/api/v2/deals/pipelines/*`) move *deals*. See
  `nimble-manage-deal-pipeline.md`.
- **Contact pipelines** move *contacts as leads*. That is this skill.

Lead transition operations live on `/api/v2/leads/*`; the pipeline listing for
contacts lives on `/api/v1`. Host `https://app.nimble.com`.

## Step 1 — find the contact pipelines

`list-available-contacts-pipelines` (`GET /api/v1/contacts/pipelines`) returns
the contact pipelines available to the account.

## Step 2 — see where a contact already stands

`list-contact-proceedings`
(`GET /api/v1/contacts/{contact_id}/proceedings`) returns the contact's
current progress. `list-contacts-proceeding-providers`
(`GET /api/v1/contacts/proceedings_providers`) lists the providers behind
them.

Read this before moving anything — a contact can already be a lead in the
target pipeline, and re-adding returns an error rather than silently
succeeding.

## Step 3 — move the lead

`move-lead` (`POST /api/v2/leads/{lead_id}/{pipeline_id}/move`).

Both ids are required in the path: the lead and the pipeline it is moving
within.

## Step 4 — exit the lead

Won:
- `mark-lead-exited-successful`
  (`POST /api/v2/leads/{lead_id}/{pipeline_id}/successful`)
- `put-lead-exit-successful` (`PUT` on the same path) to amend an existing
  successful exit.

Lost:
- `mark-lead-exited-unsuccessfully`
  (`POST /api/v2/leads/{lead_id}/{pipeline_id}/unsuccessful`)
- `put-lead-exit-unsuccessful` (`PUT` on the same path) to amend it.

The POST records the exit; the PUT revises it. Do not POST twice to change a
reason — use the PUT.

## Undoing

`undo-lead-transition` (`POST /api/v2/leads/{lead_id}/{pipeline_id}/undo`)
reverses the last transition. This is the one genuinely reversible step in the
flow, and it is why you should prefer it over deleting.

`delete-lead-transitions`
(`DELETE /api/v2/leads/{lead_id}/{pipeline_id}`) removes the lead's transition
history in that pipeline entirely. It is destructive and not covered by
`undo`. Require explicit human confirmation.

## Retry safety

No operation here carries an idempotency key. `move-lead` and the two exit
POSTs are not safe to blind-retry — re-read state with
`list-contact-proceedings` and branch on what you find instead.

## Errors

`404` unknown lead or pipeline id, `406` where the transition is not legal
from the lead's current stage (for example exiting a lead that has already
exited). See `errors/nimble-problem-types.yml`.
