# API Reference

Base URL:

```text
https://api.yourdomain.com
```

## Authentication

Send your API key in the `X-API-Key` header.

```http
X-API-Key: tac_live_xxxxxxxxxxxxxxxxxxxx
```

Do not send API keys in query strings.

## Rate limits

Recommended default limits:

- 5 requests per minute per API key
- 20 requests per minute per IP
- 1,000 requests per day per API key

You can tune these limits by plan.

## Endpoints

### `GET /v1/health`

Returns API health status.

Example response:

```json
{
  "success": true,
  "status": "ok",
  "service": "tacdatabase",
  "version": "1.0.0"
}
```

### `GET /v1/tac/:imeiOrTac`

Returns TAC metadata for an 8-digit TAC or a 14/15-digit IMEI.

This is the only data endpoint exposed by the public API.

Example request:

```http
GET /v1/tac/356938031234567
X-API-Key: tac_live_xxxxxxxxxxxxxxxxxxxx
```

Example response:

```json
{
  "success": true,
  "data": {
    "tac": "35693803",
    "brand": "Apple",
    "model": "iPhone 14 Pro",
    "modelName": "iPhone 14 Pro"
  }
}
```

Example not found response:

```json
{
  "success": false,
  "error": {
    "code": "TAC_NOT_FOUND",
    "message": "Unable to check device at the moment. Please try your free check again in 24–48 hours—our device database is updated regularly."
  }
}
```

## Error codes

- `401` invalid or missing API key
- `403` key suspended or plan blocked
- `404` TAC not found
- `429` rate limit exceeded
- `500` unexpected server error
