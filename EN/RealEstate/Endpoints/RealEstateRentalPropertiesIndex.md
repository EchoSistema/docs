# Real Estate — Rental Properties: List

## Endpoint

```
GET /real-estate/rental-properties
```

## Authentication

Requires the platform context only; no bearer token is enforced.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | Sets the preferred locale for translated titles and descriptions. |

## Query Parameters

### Pagination & Toggle

| Parameter   | Type    | Required | Description | Default |
| ----------- | ------- | -------- | ----------- | ------- |
| per_page    | int     | No       | Items per page when pagination is active. | 15 |
| page        | int     | No       | Page number. | 1 |
| no_paginate | boolean | No       | When `true`, returns the full collection without pagination (still wrapped in `data`). | false |

### Localization & Filtering

| Parameter        | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| language         | string | No       | Locale used to resolve titles/texts (`pt-BR`, `en`, ...). |
| payment_interval | string | No       | Must match `PaymentIntervalEnum` (`daily`, `weekly`, `monthly`, ...). |
| is_available     | boolean| No       | Availability flag; forced to `true` by default. |

### Identification & Ownership

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| uuid      | uuid | Filters by rental property UUID. |
| user_uuid | uuid | Filters by the creator UUID. |
| type      | string | Filters by item type slug. |

### Address Helpers

| Parameter    | Type   | Description |
| ------------ | ------ | ----------- |
| city / city_id / city_uuid / city_name | mixed | Multi-format city lookup (string name, numeric ID, UUID). |
| state / state_id / state_uuid / state_name | mixed | State lookup (string, ID, UUID). |
| country / country_id / country_uuid / country_code / country_name | mixed | Country lookup by ID, UUID or ISO code. |
| zipcode      | string | Postal code filter. |

### Text Search

| Parameter   | Type   | Description |
| ----------- | ------ | ----------- |
| title       | string | Filter by translated title. |
| description | string | Filter by text content. |

## Example

### Request

```bash
curl -X GET \
  "https://api.example.com/real-estate/rental-properties?language=pt-BR&per_page=12"
```

### Response (200)

```json
{
  "data": [
    {
      "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
      "payment_interval": "monthly",
      "type": {
        "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
        "name": "Apartamento mobiliado"
      },
      "title": "Estúdio próximo ao metrô",
      "renter": "Skyline Rentals",
      "renter_type": "company",
      "image": {
        "usage": "card",
        "url": "https://cdn.example.com/rental-properties/c51823c1/card.jpg",
        "name": "cover"
      },
      "address": "Av. Paulista, 1000 — Bela Vista, São Paulo/SP",
      "is_available": true
    }
  ],
  "links": {
    "first": "https://api.example.com/real-estate/rental-properties?page=1",
    "last": "https://api.example.com/real-estate/rental-properties?page=8",
    "prev": null,
    "next": "https://api.example.com/real-estate/rental-properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 8,
    "path": "https://api.example.com/real-estate/rental-properties",
    "per_page": 12,
    "to": 12,
    "total": 92
  }
}
```

## Status Codes

- 200 — Success
- 400 — Invalid filters
- 403 — Rental property does not belong to the active platform
- 500 — Unexpected error

## Notes

- Availability is enforced internally (`is_available = true`).
- Type names, titles and descriptions honor the `language` query parameter and fall back to defaults when no translation exists.
- When `no_paginate=true`, paginator `links/meta` are omitted but the payload still wraps the collection under `data`.

## Changelog

- 2025-09-25 — Initial documentation.
