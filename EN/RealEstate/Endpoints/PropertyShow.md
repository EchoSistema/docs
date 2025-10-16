# RealEstate – Show Property Details

## Endpoint

```
GET /api/v1/real-estate/properties/{uuid}
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Property unique identifier (UUID format) |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/properties/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "title": "Modern 3-Bedroom Apartment",
    "description": "Beautiful apartment with ocean view located in the heart of Miami. This stunning property features modern finishes, open floor plan, and breathtaking views of the Atlantic Ocean.",
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
      "street": "1234 Ocean Drive",
      "city": "Miami",
      "region": "Florida",
      "country": "US",
      "postal_code": "33139",
      "latitude": 25.7617,
      "longitude": -80.1918
    },
    "agent": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "agency": "Premium Real Estate"
    },
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "website": "https://premiumrealestate.com"
    },
    "images": [
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440003",
        "url": "https://example.com/property1.jpg",
        "is_primary": true,
        "order": 1
      },
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440004",
        "url": "https://example.com/property2.jpg",
        "is_primary": false,
        "order": 2
      }
    ],
    "amenities": [
      "Air Conditioning",
      "Pool",
      "Gym",
      "24/7 Security",
      "Parking"
    ],
    "created_at": "2025-10-16T10:30:00Z",
    "updated_at": "2025-10-16T15:45:00Z"
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data | object | Property details |
| data.uuid | string | Property unique identifier |
| data.title | string | Property title |
| data.description | string | Detailed property description |
| data.property_type | string | Type of property (Apartment, House, etc.) |
| data.rent_type | string | Transaction type (Sale, Rent, etc.) |
| data.value | numeric | Property price/value |
| data.currency | string | Currency code (ISO 4217) |
| data.size | integer | Property size in square meters |
| data.rooms | integer | Total number of rooms |
| data.bedrooms | integer | Number of bedrooms |
| data.bathrooms | integer | Number of bathrooms |
| data.garage | integer | Number of garage spaces |
| data.is_featured | boolean | Whether property is featured |
| data.direct_from_owner | boolean | Whether property is direct from owner |
| data.address | object | Complete address information |
| data.address.street | string | Street address |
| data.address.city | string | City name |
| data.address.region | string | Region/State name |
| data.address.country | string | Country code (ISO 3166-1 alpha-2) |
| data.address.postal_code | string | Postal/ZIP code |
| data.address.latitude | numeric | Geographic latitude |
| data.address.longitude | numeric | Geographic longitude |
| data.agent | object | Agent information |
| data.agent.uuid | string | Agent unique identifier |
| data.agent.name | string | Agent full name |
| data.agent.email | string | Agent email address |
| data.agent.phone | string | Agent phone number |
| data.agent.agency | string | Agency name |
| data.agency | object | Agency information |
| data.agency.uuid | string | Agency unique identifier |
| data.agency.name | string | Agency name |
| data.agency.website | string | Agency website URL |
| data.images[] | array | Property images |
| data.images[].uuid | string | Image unique identifier |
| data.images[].url | string | Image URL |
| data.images[].is_primary | boolean | Whether image is the primary photo |
| data.images[].order | integer | Image display order |
| data.amenities[] | array | List of property amenities |
| data.created_at | string | Property creation timestamp (ISO 8601) |
| data.updated_at | string | Property last update timestamp (ISO 8601) |

## HTTP Status

- 200: OK
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Property not found"
}
```

## Notes

- The property must belong to the platform specified by the `X-PUBLIC-KEY` header
- All translatable fields respect the `Accept-Language` header
- Geographic coordinates can be used for map integration
- Images are returned sorted by the `order` field
- The primary image (is_primary: true) should be displayed as the main property photo

## Related

- [List Properties](PropertyIndex.md)
- [List Property Types](PropertyTypeIndex.md)
- [Show Agent Details](AgentShow.md)

## Changelog

- 2025-10-16: Initial documentation
