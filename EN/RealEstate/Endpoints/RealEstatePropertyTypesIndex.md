# Real Estate — Property Types: List

## Endpoint

```
GET /real-estate/property-types
```

## Authentication

Not required.

## Query Parameters

| Parameter  | Type   | Required | Description |
| ---------- | ------ | -------- | ----------- |
| public_key | string | Yes      | Platform public key (`pbk`). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/property-types?public_key=realestate-demo"
```

### Response (200)

```json
{
  "data": [
    {
      "uuid": "7cc944ef-3b77-41a1-9f36-3f0a3c1f9001",
      "slug": "apartment",
      "title": "Apartment",
      "pbk": "realestate-demo"
    },
    {
      "uuid": "e1840e5b-485d-4ea5-8ea6-64cef0f30102",
      "slug": "house",
      "title": "House",
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Status Codes

- 200 — Success
- 404 — Platform key not found

## Notes

- Data lives in MongoDB collection `property_types`.
- The endpoint returns every type for the provided platform without pagination.

## Changelog

- 2025-09-25 — Initial documentation.
