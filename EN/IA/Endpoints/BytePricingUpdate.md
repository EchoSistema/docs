# IA — Byte Pricing: Update

## Endpoint

```
PUT /ia/admin/pricing/bytes/{product}
```

Updates description and/or single price of a BYTE product. Title is not updatable.

## Authentication

Required — Bearer {token} with Sanctum.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}` |
| Accept-Language | string | No       | IETF locale |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| product   | string | Yes      | Product route key |

### Request body

```json
{
  "price": 150,
  "description": "New description"
}
```

| Field       | Type    | Required | Description |
| ----------- | ------- | -------- | ----------- |
| price       | integer | No       | New price value (smallest unit; e.g., cents). Saved as USD default. |
| description | string  | No       | New default description. |

## Examples

### Request (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"price":150,"description":"New description"}' \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes/{product}"
```

### Response (200)

```json
{ "data": { "uuid": "...", "measurement_type": "byte" } }
```

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found (when product is not BYTE)
- 422: Unprocessable Entity

## Errors

```json
{ "message": "[message]" }
```

## Notes

- Title for Byte Pricing is immutable and will not be updated by this endpoint.

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Updated for single-price updates and immutable title.
