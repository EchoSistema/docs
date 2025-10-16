# RealEstate – List Properties

## Endpoint

```
GET /api/v1/real-estate/properties
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| uuid | array | No | Filter by specific property UUIDs | - |
| language | string | No | Language code for translatable fields | - |
| city | array | No | Filter by city name(s) | - |
| country | array | No | Filter by country code(s) (ISO 3166-1 alpha-2) | - |
| region | array | No | Filter by region name(s) | - |
| rent_type | array | No | Filter by rent type(s) | - |
| property_type | array | No | Filter by property type(s) | - |
| agency_uuid | array | No | Filter by agency UUID(s) | - |
| agency | array | No | Filter by agency name(s) | - |
| agent_uuid | array | No | Filter by agent UUID(s) | - |
| agent | array | No | Filter by agent name(s) | - |
| rooms | integer | No | Filter by exact number of rooms | - |
| rooms_min | integer | No | Filter by minimum number of rooms | - |
| rooms_max | integer | No | Filter by maximum number of rooms | - |
| bed | integer | No | Filter by exact number of bedrooms | - |
| bed_min | integer | No | Filter by minimum number of bedrooms | - |
| bed_max | integer | No | Filter by maximum number of bedrooms | - |
| bath | integer | No | Filter by exact number of bathrooms | - |
| bath_min | integer | No | Filter by minimum number of bathrooms | - |
| bath_max | integer | No | Filter by maximum number of bathrooms | - |
| size_min | integer | No | Filter by minimum size (sq m) | - |
| size_max | integer | No | Filter by maximum size (sq m) | - |
| is_featured | boolean | No | Filter featured properties | - |
| direct_from_owner | boolean | No | Filter properties direct from owner | - |
| search | string | No | Full-text search across property fields | - |
| value_currency | string | No | Currency code (ISO 4217, 3 chars) | - |
| currency | string | No | Alternative currency parameter | - |
| value_min | numeric | No | Filter by minimum price | - |
| value_max | numeric | No | Filter by maximum price | - |
| created_at | date | No | Filter by creation date | - |
| created_at_period | object | No | Filter by date range (from/to or start/end) | - |
| sort_by | string | No | Field to sort by | `createdAt` |
| direction | string | No | Sort direction | `DESC` |
| per_page | integer | No | Items per page | 9 (max: 200) |
| page | integer | No | Page number | 1 |

> Parameters accept variations: `snake_case`, `camelCase`, `kebab-case`, `CapitalCase`.

**Sortable fields:** `created_at`, `createdat`, `created`, `createdAt`, `size`, `rooms`, `bed`, `beds`, `bedrooms`, `bath`, `baths`, `bathrooms`, `garage`, `is_featured`, `isFeatured`, `direct_from_owner`, `directFromOwner`, `value`, `price`.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/properties?per_page=9&city[]=Miami&bed_min=2&value_max=500000"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "title": "Modern 3-Bedroom Apartment",
      "description": "Beautiful apartment with ocean view",
      "property_type": "Apartment",
      "rent_type": "Sale",
      "value": 450000,
      "currency": "USD",
      "size": 120,
      "rooms": 3,
      "bedrooms": 3,
      "bathrooms": 2,
      "garage": 1,
      "is_featured": true,
      "direct_from_owner": false,
      "address": {
        "city": "Miami",
        "region": "Florida",
        "country": "US"
      },
      "agent": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "John Smith"
      },
      "images": [
        {
          "url": "https://example.com/property1.jpg",
          "is_primary": true
        }
      ],
      "created_at": "2025-10-16T10:30:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/real-estate/properties?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/real-estate/properties?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/real-estate/properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 9,
    "to": 9,
    "total": 42
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of properties |
| data[].uuid | string | Property unique identifier |
| data[].title | string | Property title |
| data[].description | string | Property description |
| data[].property_type | string | Type of property (Apartment, House, etc.) |
| data[].rent_type | string | Transaction type (Sale, Rent, etc.) |
| data[].value | numeric | Property price/value |
| data[].currency | string | Currency code |
| data[].size | integer | Property size in square meters |
| data[].rooms | integer | Total number of rooms |
| data[].bedrooms | integer | Number of bedrooms |
| data[].bathrooms | integer | Number of bathrooms |
| data[].garage | integer | Number of garage spaces |
| data[].is_featured | boolean | Whether property is featured |
| data[].direct_from_owner | boolean | Whether property is direct from owner |
| data[].address | object | Property address information |
| data[].agent | object | Agent information |
| data[].images[] | array | Property images |
| data[].created_at | string | Property creation timestamp (ISO 8601) |
| links | object | Pagination links |
| meta | object | Pagination metadata |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Validation error",
  "errors": {
    "per_page": ["The per page field must not be greater than 200."],
    "country": ["The country field must be 2 characters."]
  }
}
```

## Notes

- The `public_key` parameter is automatically extracted from the `X-PUBLIC-KEY` header
- Results are paginated with a default of 9 items per page (max 200)
- Array parameters (city, country, etc.) can be passed multiple times or as array notation
- Date filters support various formats: single date or period with from/to or start/end
- All translatable fields respect the `Accept-Language` header
- Default sorting is by creation date in descending order (newest first)

## Related

- [Show Property Details](PropertyShow.md)
- [List Property Types](PropertyTypeIndex.md)
- [List Rent Types](RentTypeIndex.md)
- [List Agents](AgentIndex.md)

## Changelog

- 2025-10-16: Initial documentation
