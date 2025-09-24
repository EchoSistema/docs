# IA — Byte Pricing: Show

## Endpoint

```
GET /ia/admin/pricing/bytes/{product}
```

Returns a single BYTE-measured product.

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
| product   | string | Yes      | Product route key (implicit model binding) |

### Query parameters

None.

## Examples

### Request (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes/{product}"
```

### Response (200)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Price per Byte" },
    "text": { "content": "Price per byte" },
    "prices": [ { "currency_id": 2, "value": 123 } ]
  }
}
```

## JSON Structure Explained

| Field                | Type    | Description |
| -------------------- | ------- | ----------- |
| data.uuid            | string  | Product identifier |
| data.measurement_type| string  | Measurement type (`byte`) |
| data.title.content   | string  | Localized default title |
| data.text.content    | string  | Default description |
| data.prices[]        | array   | Prices attached to product |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found (when the bound product is not BYTE)

## Errors

```json
{ "message": "Not Found" }
```

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Clarified response fields and status codes.
