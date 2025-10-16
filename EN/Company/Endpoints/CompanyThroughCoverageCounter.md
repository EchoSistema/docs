# Company – Count Companies Through Coverage

## Endpoint

```
GET /api/v1/company/through-coverage/counter
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
  "https://sandbox.your-domain.com/api/v1/company/through-coverage/counter?has_reviews=true"
```

### Response example

```json
{
  "data": {
    "total": 42
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data | object | Response data container. |
| data.total | integer | Total count of companies matching the filters. |

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

- This endpoint returns the total count of companies matching the provided filters.
- All filter parameters from the main index endpoint are supported.
- The count respects the platform's coverage area restrictions.
- Useful for displaying statistics or pagination information without fetching full data.
- More efficient than fetching all results when only the count is needed.

## Related

- [List Companies Through Coverage](CompanyThroughCoverageIndex.md)
- [List Companies Through Coverage - Geolocations](CompanyThroughCoverageGeolocations.md)
- [Show Company](CompanyShow.md)
