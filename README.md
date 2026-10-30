# Nimble

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Nimble is a relationship-focused CRM that unifies contacts, communications, social profiles, calendar and email into a single shared record for small businesses and whole-company teams. Founded in 2010 by GoldMine co-founder Jon Ferrara and headquartered in Santa Monica, California, the platform combines contact management, sales pipelines, email marketing, sequences, web forms, web chat and workflow automation, and enriches contact profiles automatically from email, calendar, social and web data. Nimble publishes a full OpenAPI 3.0.0 contract for its REST API — 89 operations over contacts, user-definable contact fields, contact and deal pipelines, deals, leads, activities, tasks and messages — authenticated with an account API key or OAuth 2.0 authorization_code.

**Areas:** CRM · Sales · Contact Management · Relationship Management · Marketing Automation · Pipeline Management · Small Business · Email Marketing · Sales Automation · Lead Management

## Nimble on APIs.io

Everything profiled for Nimble is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/nimble/](https://apis.io/providers/nimble/)** — or go straight to one of the APIs or properties below.

### APIs (12)

- [Nimble Activities API](https://apis.io/apis/nimble/nimble-activities-api/) — Account activity stream — list activities across contacts and deals, walked with a next_tstamp timestamp cursor. 1 operation(s).
- [Nimble Contacts API](https://apis.io/apis/nimble/nimble-contacts-api/) — Contact records — the Nimble platform root object. Create, read, update and delete person and company contacts, search them with keyword or the advanced…
- [Nimble Contacts Fields API](https://apis.io/apis/nimble/nimble-contacts-fields-api/) — User-definable contact field schema — fields, choice option lists, field groups and tabs, plus primary-value marks on multi-valued fields. The field set…
- [Nimble Contacts Pipelines API](https://apis.io/apis/nimble/nimble-contacts-pipelines-api/) — Contact pipelines — the lead-progression surface for contacts, distinct from deal pipelines. 8 operation(s).
- [Nimble Deals API](https://apis.io/apis/nimble/nimble-deals-api/) — Deals on the /api/v2 surface — create, update and delete deals, attach notes and files, manage deal tags, list overdue activity and read the won-last-month…
- [Nimble Deals Fields API](https://apis.io/apis/nimble/nimble-deals-fields-api/) — Deal field surface — the user deal field list and the column catalogue backing Nimble's deal listing views. 2 operation(s).
- [Nimble Deals Pipelines API](https://apis.io/apis/nimble/nimble-deals-pipelines-api/) — Deal pipelines and stages — create and reshape pipelines, add and archive stages, set lost reasons, and read the deal book grouped by stage or by owner. 13…
- [Nimble Deals Pipelines Fields API](https://apis.io/apis/nimble/nimble-deals-pipelines-fields-api/) — Per-pipeline deal custom fields — fields, choice lists and field groups scoped to a single deal pipeline. 9 operation(s).
- [Nimble Leads API](https://apis.io/apis/nimble/nimble-leads-api/) — Lead transitions through contact pipelines — move a lead between stages, mark it exited successfully or unsuccessfully, and undo a transition. 7 operation(s).
- [Nimble Messages API](https://apis.io/apis/nimble/nimble-messages-api/) — Message drafts — list and create draft messages. 2 operation(s).
- [Nimble Tasks API](https://apis.io/apis/nimble/nimble-tasks-api/) — Task creation against contacts. 1 operation(s).
- [Nimble Users API](https://apis.io/apis/nimble/nimble-users-api/) — The authenticated user record — the credential-verification endpoint. 1 operation(s).

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [nimble](https://apis.io/servers/nimble/nimble/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Nimble Agentic Access](https://apis.io/agentic-access/nimble/nimble-agentic-access/)

#### Collections (8)

Postman and Open Collection request collections.

- [Nimble CRM API](https://apis.io/collections/nimble/open-nimble/)
- [Nimble CRM Contacts API](https://apis.io/collections/nimble/open-nimble-contacts-api/)
- [Nimble CRM Contacts Deals API](https://apis.io/collections/nimble/open-nimble-deals-api/)
- [Nimble CRM Contacts Fields API](https://apis.io/collections/nimble/open-nimble-fields-api/)
- [Nimble CRM Contacts Messages API](https://apis.io/collections/nimble/open-nimble-messages-api/)
- [Nimble CRM Contacts Notes API](https://apis.io/collections/nimble/open-nimble-notes-api/)
- [Nimble CRM Contacts Pipelines API](https://apis.io/collections/nimble/open-nimble-pipelines-api/)
- [Nimble CRM Contacts Users API](https://apis.io/collections/nimble/open-nimble-users-api/)

#### Overlays (12)

OpenAPI overlays, including conversational phrasing for each operation.

- [Nimble Activities Overlay](https://apis.io/overlays/nimble/nimble-activities-overlay/)
- [Nimble Contacts Fields Overlay](https://apis.io/overlays/nimble/nimble-contacts-fields-overlay/)
- [Nimble Contacts Overlay](https://apis.io/overlays/nimble/nimble-contacts-overlay/)
- [Nimble Contacts Pipelines Overlay](https://apis.io/overlays/nimble/nimble-contacts-pipelines-overlay/)
- [Nimble Deals Fields Overlay](https://apis.io/overlays/nimble/nimble-deals-fields-overlay/)
- [Nimble Deals Overlay](https://apis.io/overlays/nimble/nimble-deals-overlay/)
- [Nimble Deals Pipelines Fields Overlay](https://apis.io/overlays/nimble/nimble-deals-pipelines-fields-overlay/)
- [Nimble Deals Pipelines Overlay](https://apis.io/overlays/nimble/nimble-deals-pipelines-overlay/)
- [Nimble Leads Overlay](https://apis.io/overlays/nimble/nimble-leads-overlay/)
- [Nimble Messages Overlay](https://apis.io/overlays/nimble/nimble-messages-overlay/)
- [Nimble Tasks Overlay](https://apis.io/overlays/nimble/nimble-tasks-overlay/)
- [Nimble Users Overlay](https://apis.io/overlays/nimble/nimble-users-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Nimble Authentication](https://apis.io/security/nimble/nimble-authentication/)
- [Nimble Domain Security](https://apis.io/security/nimble/nimble-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Nimble Scopes](https://apis.io/scopes/nimble/nimble-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Nimble Plans Pricing](https://apis.io/plans/nimble/nimble-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Nimble Rate Limits](https://apis.io/rate-limits/nimble/nimble-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.nimble.com)
- [Documentation](https://www.nimble.com/developers/docs/)
- [Getting started](https://support.nimble.com/en/articles/502755-nimble-api-access)
- [API reference](https://www.nimble.com/developers/docs/)
- [Pricing](https://www.nimble.com/pricing/)
- [Sign up](https://www.nimble.com/register/business_trial/)
- [Support](https://support.nimble.com/en/)
- [Blog](https://www.nimble.com/blog/)
- [Terms of service](https://www.nimble.com/company/terms-of-service/)
- [Privacy policy](https://www.nimble.com/company/privacy/)
- [Status](https://status.nimble.com/)

## API Evangelist rating

**Kin Score 50.1** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 52.1 |
| Contract governance | 4.5 |
| Operational transparency | 15.8 |
| Developer ergonomics | 58.9 |
| Access clarity | 65.8 |

**Agent readiness 32.1** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 34.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/nimble/](https://apis.io/providers/nimble/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Nimble: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/nimble/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
