# IMEI TAC Database API

<p align="center">
  <strong>Free TAC database access for approved users.</strong><br>
  Public API docs, request flow on imeicheckpro.com, and admin controls for safe distribution.
</p>

<p align="center">
  <img alt="Free" src="https://img.shields.io/badge/Cost-Free%20of%20charge-0ea5e9?style=for-the-badge">
  <img alt="API" src="https://img.shields.io/badge/API-TAC%20Lookup%20Only-111827?style=for-the-badge">
  <img alt="Access" src="https://img.shields.io/badge/Access-By%20Approval-16a34a?style=for-the-badge">
</p>

Public documentation and access workflow for the TAC database API.

This repository is intended to explain:

- how to request access from imeicheckpro.com
- how API keys are issued
- how the public TAC demo uses captcha-gated temporary keys
- how rate limits and IP limits work
- how to call the TAC / IMEI lookup endpoint
- how admin reporting works
- how keys are revoked, suspended, or rotated

## Common search terms

People usually find this service using terms like:

- TAC database
- IMEI TAC lookup
- TAC code API
- free TAC database access
- IMEI lookup API
- device model lookup
- public TAC database API

## Access model

Access is not anonymous.

1. A user submits an access request from the TAC request form on [imeicheckpro.com/tac-access](https://imeicheckpro.com/tac-access).
2. An admin reviews the request.
3. If approved, the system generates an API key and emails it to the requester.
4. The requester uses the key in the `X-API-Key` header.
5. Every request is logged in a separate usage collection with:
   - API key id
   - key owner email
   - IP address
   - endpoint
   - timestamp
   - response status

## Security model

- API keys are high-entropy random secrets.
- Keys are shown only once on creation.
- Keys are stored hashed at rest.
- Each request is rate limited by:
  - API key
  - IP address
  - endpoint
- Suspicious usage can be blocked by:
  - disabling the key
  - tightening the per-key quota
  - blocking the source IP

## Public endpoints

The public API should expose only the minimum required surface area.

- `GET /v1/tac/:imeiOrTac`
- `GET /v1/health`

The website demo also mints short-lived temporary keys after a human verification challenge. Those demo keys are limited to 5 lookups and are tied to the requester IP.

## Live links

- [Request TAC access](https://imeicheckpro.com/tac-access)
- [Try the TAC API demo](https://imeicheckpro.com/tac-api)
- [Contact support](https://imeicheckpro.com/contact)

## TAC not found behavior

If a TAC is not present in the local database, the API should return the same user-facing fallback copy the app already uses for TAC misses, instead of exposing raw internal lookup details.

Recommended response:

```json
{
  "success": false,
  "error": "Unable to check device at the moment. Please try your free check again in 24–48 hours—our device database is updated regularly."
}
```

## What this repo contains

- `docs/api.md` - endpoint reference
- `docs/security.md` - authentication and rate limiting guidance
- `docs/access-request.md` - request/approval flow
- `docs/admin-reporting.md` - what admins can see
- `docs/openapi.yaml` - API contract
- `docs/email-templates.md` - approval and key delivery emails
- `docs/postman/TACDatabase.postman_collection.json` - importable Postman collection

## Recommended deployment flow

- Public website or docs site
- Admin-only dashboard
- API service
- MongoDB or equivalent database
- Redis for distributed rate limiting
- SMTP or email provider for approval emails

## Suggested implementation priorities

1. Store API keys hashed, not plaintext.
2. Attach every request log to `apiKeyId`, `ownerEmail`, and `ipAddress`.
3. Add per-key quota counters with reset windows.
4. Add a public access request page.
5. Add admin approval actions:
   - approve
   - reject
   - suspend
   - delete
   - rotate key
6. Add admin reporting for:
   - requests by key
   - requests by email
   - requests by IP
   - requests by day

## Request page

Open the TAC access request form here:

- [Request TAC Database Access](https://imeicheckpro.com/tac-access)

## Demo page

Open the TAC API demo here:

- [TAC API Demo](https://imeicheckpro.com/tac-api)

## Postman collection

If you prefer Postman, import the collection from:

- [`docs/postman/TACDatabase.postman_collection.json`](docs/postman/TACDatabase.postman_collection.json)

Update the collection variables after import:

- `baseUrl` - your API base URL
- `apiKey` - your issued TAC API key
- `imeiOrTac` - the 8-digit TAC or 14/15-digit IMEI you want to look up

## Positioning

Use this repo publicly on GitHub to make the service easy to find, with clear free access messaging:

- free of cost for approved users
- TAC lookup only
- public request form lives on imeicheckpro.com/tac-access
- live demo page lives on imeicheckpro.com/tac-api
- no extra API surface
- admin-controlled key lifecycle
