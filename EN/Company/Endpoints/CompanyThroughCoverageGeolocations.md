# Company – List Companies Through Coverage with Geolocations

## Endpoint

```
GET /api/v1/company/through-coverage/geolocations
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
| id | integer | No | Filter by company ID. | - |
| uuids | array | No | Filter by company UUIDs. | - |
| name | string | No | Filter by company name (partial match). | max: 255 |
| slug | string | No | Filter by company slug. | max: 255 |
| fiscal_document_type | string | No | Fiscal document type. Required with `fiscal_document_value`. | enum values |
| fiscal_document_value | string | No | Fiscal document value. | max: 255 |
| sort_by | string | No | Field to sort by. | name (max: 255) |
| order_by | string | No | Sort direction. | ASC (max: 4) |
| no_complaints | boolean | No | Filter companies without complaints. | false |
| has_complaints | boolean | No | Filter companies with complaints. | false |
| no_reviews | boolean | No | Filter companies without reviews. | false |
| has_reviews | boolean | No | Filter companies with reviews. | false |
| rating | numeric | No | Filter by rating. | 0-5 |
| good_rating | boolean | No | Filter companies with good rating. | false |
| bad_rating | boolean | No | Filter companies with bad rating. | false |

> Parameter names are documented in `snake_case`. The endpoint accepts variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company/through-coverage/geolocations?name=Sample"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "name": "Sample Company",
      "slug": "sample-company",
      "geolocation": {
        "id": 1,
        "latitude": "-25.4284",
        "longitude": "-49.2733"
      }
    }
  ]
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of companies with geolocations. |
| data[].uuid | string | Company UUID. |
| data[].name | string | Company name. |
| data[].slug | string | Company URL-friendly identifier. |
| data[].geolocation | object | Geolocation data (null if not available). |
| data[].geolocation.id | integer | Geolocation ID. |
| data[].geolocation.latitude | string | Latitude coordinate. |
| data[].geolocation.longitude | string | Longitude coordinate. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard validation errors:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "rating": ["The rating must be between 0 and 5."]
  }
}
```

## Notes

- This endpoint returns only companies that have geolocation data in their address.
- Companies without geolocation data are automatically excluded.
- The response is not paginated and returns all matching results.
- Useful for displaying companies on maps or location-based features.
- All filter parameters from the main index endpoint are supported.

## Related

- [List Companies Through Coverage](CompanyThroughCoverageIndex.md)
- [Count Companies Through Coverage](CompanyThroughCoverageCounter.md)
- [Show Company](CompanyShow.md)
