# IA — Byte Pricing: Index

## Endpoint

```
GET /ia/admin/pricing/bytes
```

## Authentication

Required — Bearer {token} with Sanctum.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}` |
| Accept-Language | string | No       | IETF locale (`en`, `pt-BR`, `es`, ...) |

## Parameters

### Path parameters

None.

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| page      | int    | No       | Page number | 1 |

## Examples

### Request (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes?page=1"
```

### Response (200)

```json
{
  "data": [
    {
      "uuid": "...",
      "measurement_type": "byte",
      "title": { "content": "Price per Byte" },
      "text": { "content": "Price per byte" },
      "prices": [ { "currency_id": 2, "value": 123 } ]
    }
  ],
  "meta": { "current_page": 1, "per_page": 25 }
}
```

## JSON Structure Explained

| Field                | Type    | Description |
| -------------------- | ------- | ----------- |
| data[]               | array   | List of products |
| data[].uuid          | string  | Product identifier |
| data[].measurement_type | string | Measurement type (`byte`) |
| data[].title.content | string  | Localized default title |
| data[].text.content  | string  | Default description |
| data[].prices[]      | array   | Prices attached to product |
| meta                 | object  | Pagination metadata |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{ "message": "[message]" }
```

## Notes

- Returns only `measurement_type = byte`.

## Related

- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Updated to single-price model and immutable title.
