# Learn – List Learn Categories

## Endpoint

```
GET /api/v1/learn/categories
```

## Authentication

None

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | No       | `Bearer {token}` (optional). |
| X-PUBLIC-KEY    | string | Yes      | Platform public key. |
| Accept-Language | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| language  | string | No       | Language code for localized titles | Platform default |

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/categories?language=en"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "resource": "category-resource-name",
      "domain_area": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Learn"
      },
      "group": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "resource": "group-resource-name",
        "title": {
          "language": "en",
          "value": "Category Group Name"
        }
      },
      "title": {
        "language": "en",
        "value": "Category Name",
        "is_default": false
      }
    }
  ]
}
```

## JSON Structure Explained

| Field                | Type    | Description |
| -------------------- | ------- | ----------- |
| data[]               | array   | List of categories |
| data[].uuid          | string  | Category unique identifier |
| data[].resource      | string  | Category resource name |
| data[].domain_area   | object  | Domain area information |
| data[].group         | object  | Category group information |
| data[].group.title   | object  | Localized group title |
| data[].title         | object  | Localized category title |
| data[].title.value   | string  | Title text |
| data[].title.language| string  | Title language code |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Invalid platform key"
}
```

## Notes

- Returns all categories for the platform's domain area
- Categories include their associated group and domain area
- Titles are localized based on the `language` parameter or `Accept-Language` header
- If a localized title is not available, the default title is returned

## Related

- [List Learn Category Groups](./LearnCategoryGroupIndex.md)
- [Show Learn Category Group](./LearnCategoryGroupShow.md)

## Changelog

- 2025-10-16: Initial documentation
