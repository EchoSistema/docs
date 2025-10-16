# RealEstate – List Property Types

## Endpoint

```
GET /api/v1/real-estate/property-types
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No query parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/property-types"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440001",
      "name": "Apartment",
      "slug": "apartment",
      "description": "Multi-unit residential building",
      "icon": "building"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440002",
      "name": "House",
      "slug": "house",
      "description": "Single-family detached residence",
      "icon": "home"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440003",
      "name": "Condo",
      "slug": "condo",
      "description": "Individually owned unit in a complex",
      "icon": "condo"
    }
  ],
  "meta": {
    "total": 3
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of property types |
| data[].uuid | string | Property type unique identifier |
| data[].name | string | Property type name (translatable) |
| data[].slug | string | Property type URL-friendly identifier |
| data[].description | string | Property type description |
| data[].icon | string | Icon identifier for UI display |
| meta | object | Response metadata |
| meta.total | integer | Total number of property types |

## HTTP Status

- 200: OK
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- Returns all property types available for the platform
- Property type names are translatable and respect the `Accept-Language` header
- Use the `slug` field for filtering properties by type
- Results are not paginated as the list is typically small

## Related

- [List Properties](PropertyIndex.md)
- [Show Property Details](PropertyShow.md)

## Changelog

- 2025-10-16: Initial documentation
