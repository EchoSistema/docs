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
      "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
      "measurement_type": {
        "id": "byte",
        "name": "BYTE",
        "title": "Byte"
      },
      "title": "Price per Byte",
      "slug": "price-per-byte",
      "description": null,
      "language": "af",
      "price": 29900,
      "currency": "BRL",
      "formatted_price": "R$\u00a0299,00",
      "created_at": "2025-09-26T04:46:04-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
    "last": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "links": [
      {
        "url": null,
        "label": "\u00ab Previous",
        "active": false
      },
      {
        "url": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Next \u00bb",
        "active": false
      }
    ],
    "path": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## JSON Structure Explained

| Field                     | Type        | Description |
| ------------------------- | ----------- | ----------- |
| data[]                    | array       | Collection of byte-based products |
| data[].uuid               | string      | Product identifier |
| data[].measurement_type   | object      | Measurement type metadata |
| data[].measurement_type.id| string      | Enum identifier (always `byte`) |
| data[].measurement_type.name | string   | Enum name (`BYTE`) |
| data[].measurement_type.title | string  | Human readable label |
| data[].title              | string|null | Localized name of the product |
| data[].slug               | string|null | Slug derived from the title |
| data[].description        | string|null | Optional localized description |
| data[].language           | string|null | Locale tied to the title record |
| data[].price              | number|null | Default price in minor units |
| data[].currency           | string|null | Currency ISO code |
| data[].formatted_price    | string|null | Human readable price |
| data[].created_at         | string|null | Creation timestamp (ISO 8601) |
| links                     | object      | Pagination navigation links |
| meta                      | object      | Pagination metadata |

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
- Pagination link labels are localized; the example above reflects a pt-BR backend locale.

## Related

- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Updated to single-price model and immutable title.
