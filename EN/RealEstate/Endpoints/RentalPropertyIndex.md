# RealEstate – List Rental Properties

## Endpoint

```
GET /api/v1/real-estate/rental-properties
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
| uuid | string | No | Filter by rental property UUID | - |
| user_uuid | string | No | Filter by user UUID | - |
| title | string | No | Filter by title | - |
| description | string | No | Filter by description | - |
| type | string | No | Filter by rental property type | - |
| language | string | No | Language code for translatable fields | - |
| payment_interval | string | No | Payment interval (daily, weekly, monthly, yearly) | - |
| is_available | boolean | No | Filter by availability status | true |
| zipcode | string | No | Filter by postal code | - |
| address_one | string | No | Filter by address line 1 | - |
| address_two | string | No | Filter by address line 2 | - |
| address_three | string | No | Filter by address line 3 | - |
| address_four | string | No | Filter by address line 4 | - |
| city | mixed | No | Filter by city (ID, UUID, or name) | - |
| city_id | integer | No | Filter by city ID | - |
| city_uuid | string | No | Filter by city UUID | - |
| city_name | string | No | Filter by city name | - |
| state | mixed | No | Filter by state (ID, UUID, or name) | - |
| state_id | integer | No | Filter by state ID | - |
| state_uuid | string | No | Filter by state UUID | - |
| state_name | string | No | Filter by state name | - |
| country | mixed | No | Filter by country (ID, UUID, code, or name) | - |
| country_id | integer | No | Filter by country ID | - |
| country_uuid | string | No | Filter by country UUID | - |
| country_code | string | No | Filter by country code (ISO 3166-1 alpha-2) | - |
| country_name | string | No | Filter by country name | - |
| no_paginate | boolean | No | Disable pagination and return all results | false |
| per_page | integer | No | Items per page (when paginated) | 15 |
| page | integer | No | Page number | 1 |
| sort_by | string | No | Field to sort by | created_at |
| direction | string | No | Sort direction (ASC or DESC) | DESC |

> Parameters accept variations: `snake_case`, `camelCase`, `kebab-case`, `CapitalCase`.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/rental-properties?per_page=15&city_name=Miami"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440001",
      "title": "Luxury Beachfront Apartment",
      "description": "Beautiful 2-bedroom apartment with ocean view",
      "type": "Apartment",
      "payment_interval": "monthly",
      "price": 2500,
      "currency": "USD",
      "is_available": true,
      "image": {
        "url": "https://example.com/rental-property-1.jpg",
        "alt": "Beachfront Apartment"
      },
      "address": {
        "address_one": "123 Beach Road",
        "city": "Miami",
        "state": "Florida",
        "country": "US",
        "zipcode": "33139"
      },
      "renter": {
        "uuid": "ee0e8400-e29b-41d4-a716-446655440002",
        "name": "Premium Properties LLC"
      },
      "creator": {
        "uuid": "ff0e8400-e29b-41d4-a716-446655440003",
        "name": "John Manager"
      },
      "type_assigned": {
        "type": {
          "uuid": "110e8400-e29b-41d4-a716-446655440004",
          "name": "Residential"
        }
      },
      "created_at": "2025-10-01T10:00:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/real-estate/rental-properties?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/real-estate/rental-properties?page=3",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/real-estate/rental-properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 15,
    "to": 15,
    "total": 42
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of rental properties |
| data[].uuid | string | Rental property unique identifier |
| data[].title | string | Property title (translatable) |
| data[].description | string | Property description |
| data[].type | string | Property type |
| data[].payment_interval | string | Payment frequency (daily, weekly, monthly, yearly) |
| data[].price | numeric | Rental price |
| data[].currency | string | Currency code |
| data[].is_available | boolean | Availability status |
| data[].image | object | Primary property image |
| data[].address | object | Property address details |
| data[].renter | object | Property renter/owner information |
| data[].creator | object | User who created the listing |
| data[].type_assigned | object | Assigned property type with translations |
| data[].created_at | string | Creation timestamp (ISO 8601) |
| links | object | Pagination links (if paginated) |
| meta | object | Pagination metadata (if paginated) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Validation error",
  "errors": {
    "payment_interval": ["The payment interval field must be one of: daily, weekly, monthly, yearly."]
  }
}
```

## Notes

- Only available rental properties are returned by default (`is_available=true`)
- Results are paginated by default (15 items per page)
- Use `no_paginate=true` to retrieve all results without pagination
- The `city`, `state`, and `country` parameters accept multiple formats (ID, UUID, code, or name)
- All translatable fields respect the `Accept-Language` header
- Properties are filtered to belong to the platform's company

## Related

- [Show Rental Property Details](RentalPropertyShow.md)
- [List Rental Property Types](RentalPropertyTypeIndex.md)

## Changelog

- 2025-10-16: Initial documentation
