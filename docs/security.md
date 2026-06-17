# Security

This API should be designed for public consumption without exposing the full database.

## Key handling

- Generate keys with a cryptographically secure random generator.
- Show the key only once.
- Store only a hash of the key in the database.
- Support key rotation.
- Support immediate revocation.

## Recommended data model

### `ApiKey`

- `id`
- `keyHash`
- `keyPrefix`
- `name`
- `ownerUserId`
- `ownerEmail`
- `status` (`active`, `suspended`, `revoked`)
- `scopes`
- `rateLimitPerMinute`
- `dailyQuota`
- `createdAt`
- `updatedAt`

Admins should be able to:

- suspend a key
- revoke a key
- delete a key record
- rotate the secret while preserving the owner record

### `ApiUsageLog`

- `apiKeyId`
- `ownerEmail`
- `ipAddress`
- `method`
- `path`
- `statusCode`
- `responseTimeMs`
- `createdAt`

This must be a new collection dedicated to API usage.
Do not reuse the existing `public_request_logs` collection, which serves the main site funnel and free-check tracking.

### `AccessRequest`

- `name`
- `email`
- `company`
- `useCase`
- `expectedVolume`
- `website`
- `status` (`pending`, `approved`, `rejected`)
- `reviewedBy`
- `reviewedAt`
- `createdAt`

## Request enforcement

Apply limits in this order:

1. Reject missing or invalid key.
2. Reject suspended or revoked key.
3. Check key quota.
4. Check IP-based limit.
5. Check endpoint-specific limit.
6. Log the request outcome in `ApiUsageLog`.

## Logging guidance

Log both success and failure events so admins can audit abuse patterns.

Do not store:

- raw API keys
- passwords
- secret tokens in plain text
