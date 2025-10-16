# RealEstate – List Rent Types

## Endpoint

```
GET /api/v1/real-estate/rent-types
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
  "https://sandbox.your-domain.com/api/v1/real-estate/rent-types"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440001",
      "name": "Sale",
      "slug": "sale",
      "description": "Property for sale"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440002",
      "name": "Rent",
      "slug": "rent",
      "description": "Property for rent"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440003",
      "name": "Lease",
      "slug": "lease",
      "description": "Property for lease"
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
| data[] | array | List of rent/transaction types |
| data[].uuid | string | Rent type unique identifier |
| data[].name | string | Rent type name (translatable) |
| data[].slug | string | Rent type URL-friendly identifier |
| data[].description | string | Rent type description |
| meta | object | Response metadata |
| meta.total | integer | Total number of rent types |

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

- Returns all transaction types available for the platform (sale, rent, lease, etc.)
- Rent type names are translatable and respect the `Accept-Language` header
- Use the `slug` field for filtering properties by transaction type
- Results are not paginated as the list is typically small

## Related

- [List Properties](PropertyIndex.md)
- [Show Property Details](PropertyShow.md)

## Changelog

- 2025-10-16: Initial documentation
