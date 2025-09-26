# Real Estate — Properties: Detail

Returns the raw property document stored in MongoDB while preserving the
standard `{ data: ... }` response envelope used across RealEstate endpoints.

## Endpoint

```
GET /real-estate/properties/{property}
```

## Authentication

Not required.

## Path Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| property  | uuid | Yes      | Property UUID (route model binding uses `uuid`). |

## Query Parameters

| Parameter   | Type   | Required | Description |
| ----------- | ------ | -------- | ----------- |
| public_key  | string | No       | Optional platform public key filter. |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/properties/e2c9c1c6-4cac-4c41-8f8e-6fd79d303001"
```

### Response (200)

```json
{
  "data": {
    "uuid": "e2c9c1c6-4cac-4c41-8f8e-6fd79d303001",
    "title": "Penthouse with River View",
    "city": "Lisbon",
    "region": "Lisbon",
    "country": "PT",
    "value": {
      "amount": 875000,
      "currency": "EUR"
    },
    "rentType": "sale",
    "propertyType": "penthouse",
    "rooms": 7,
    "bed": 4,
    "bath": 3,
    "garage": 2,
    "size": 215,
    "isFeatured": true,
    "directFromOwner": false,
    "createdAt": "2025-08-13T12:45:11Z",
    "details": {
      "highlights": ["Rooftop deck", "River view"],
      "tags": ["luxury", "exclusive"]
    }
  }
}
```

## Status Codes

- 200 — Success
- 404 — Property not found
- 500 — Unexpected error

## Notes

- The payload returns the MongoDB document stored in `properties`, wrapped in
  the `{ data: ... }` envelope.
- `public_key` is optional but helps avoid leaking cross-platform listings.

## Changelog

- 2025-09-25 — Initial documentation.
