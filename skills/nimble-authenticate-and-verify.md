---
name: Authenticate against the Nimble CRM API and verify access
description: Obtain and present a Nimble credential, then confirm the token works and discover which account it belongs to before doing any other work.
api: openapi/nimble-users-api-openapi.yml
operations:
  - get-myself
generated: '2026-08-13'
method: generated
source: https://www.nimble.com/developers/docs/#tag/Authentication
---

# Authenticate against the Nimble CRM API

Base host is `https://app.nimble.com`. Nimble versions per resource family, so
paths carry `/api/v1`, `/api/v2` or (for two deal-tag operations) `/api/2` —
never assume one prefix for the whole API.

## Choose a credential

Nimble accepts three forms. Pick by who you are, not by preference.

1. **Account API key as a Bearer token** — the current default for
   first-party work. Send `Authorization: Bearer <API-KEY>`. Nimble's support
   documentation states this is "the only form of accepted authentication"
   for current users. The key is generated from the account's API access
   settings.
2. **`X-Nimble-Token` header** — the only scheme declared in the machine-readable
   OpenAPI `securitySchemes`. Same key material, different header name.
3. **OAuth 2.0 `authorization_code`** — required when acting on behalf of
   another Nimble user (third-party integrations, older accounts).

## OAuth flow, when acting for another user

Client credentials are **not self-service**. Email `api-support@nimble.com`
with App Name, App Description and the OAuth callback Redirect URL to be
issued a Client ID and Client Secret.

1. Redirect the user to
   `GET https://app.nimble.com/oauth/authorize` with `client_id`,
   `redirect_uri`, `response_type=code`, and `scope`.
   - Scopes are `basic`, `contacts`, `deals`, joined by comma or `+`.
   - Implicit flow is not supported; `code` is the only `response_type`.
   - `redirect_uri` must match the URL registered in application settings —
     it may only override the path portion.
2. Exchange the returned `code` at
   `POST https://app.nimble.com/api/oauth/token` with
   `Content-Type: application/x-www-form-urlencoded; charset=UTF-8` and body
   `grant_type=authorization_code`, `code`, `redirect_uri`, `client_id`,
   `client_secret`.
3. **Access tokens expire in roughly 599 seconds (~10 minutes).** Any
   long-running agent must implement the `refresh_token` grant against the
   same token endpoint — do not cache an access token across a session.

There is no `/.well-known/oauth-authorization-server` and no OIDC discovery
document on any Nimble host. These endpoint URLs cannot be discovered
programmatically; they are the values above.

## Verify the credential

Call `get-myself`:

```
GET https://app.nimble.com/api/v1/myself
Authorization: Bearer <API-KEY>
```

A 200 confirms the credential and identifies the account and user the token
acts as. Do this once before any write, so that a later failure is
unambiguously about the operation rather than the credential.

## Scope limits to respect

Nimble's scopes are coarse and have **no read-only variant**. Granting
`contacts` grants create, update and delete on contacts. If a task only needs
to read, the API cannot enforce that — enforce it in your own behavior.

## Failure handling

- `409` with `"code": 245` — validation error. Read `message`; on contact
  writes read the `errors` map, which names the offending field and its
  `field_id`.
- `402` with `"code": 108` — the account hit a subscription quota (for
  example the 25,000 contact-record plan allowance). Do not retry; it will
  not clear without a plan change.
- `403` — the authenticated user does not own or cannot administer the
  object.
- `500` with `"code": 107` — Nimble server error. Retry with backoff.

Nimble publishes no rate limits and returns no `RateLimit-*` or `Retry-After`
headers, so there is no runtime backpressure signal to honor. Pace requests
conservatively and treat a `500` as the only retry trigger.
