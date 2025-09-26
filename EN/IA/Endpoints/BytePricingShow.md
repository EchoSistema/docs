# IA — Byte Pricing: Show

Returns a single byte-priced product with its localized metadata and default price.

## Endpoint

```
GET /ia/admin/pricing/bytes/{product}
```

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
    "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
    "measurement_type": {
      "id": "byte",
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Price per Byte",
    "slug": "byte_price",
    "description": "Default byte pricing description",
    "language": "en",
    "price": 29900,
    "currency": "BRL",
    "formatted_price": "R$\u00a0299,00",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
```

## JSON Structure Explained

| Field                          | Type        | Description |
| ------------------------------ | ----------- | ----------- |
| data.uuid                      | string      | Product identifier |
| data.measurement_type          | object      | Measurement type metadata |
| data.measurement_type.id       | string      | Enum identifier (`byte`) |
| data.measurement_type.name     | string      | Enum name (`BYTE`) |
| data.measurement_type.title    | string      | Human readable label |
| data.title                     | string|null | Localized product title |
| data.slug                      | string|null | Slug used internally to locate the product |
| data.description               | string|null | Default description content |
| data.language                  | string|null | Locale tied to the default title |
| data.price                     | integer|null | Default price in minor units (e.g., cents) |
| data.currency                  | string|null | ISO currency code |
| data.formatted_price           | string|null | Human-friendly currency string |
| data.created_at                | string|null | Creation timestamp (ISO 8601) |

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
- 2025-09-26: Updated response payload example to match current resource shape.
