# Learn – List Learn Category Groups

## Endpoint

```
GET /api/v1/learn/category-groups
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

| Parameter  | Type    | Required | Description | Default/Values |
| ---------- | ------- | -------- | ----------- | -------------- |
| language   | string  | No       | Language code for localized titles | Platform default |
| categories | boolean | No       | Include categories in response | false |

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/category-groups?language=en&categories=true"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "resource": "group-resource-name",
      "domain_area_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "title": {
        "language": "en",
        "value": "Category Group Name",
        "is_default": false
      },
      "group": {
        "category": [
          {
            "uuid": "550e8400-e29b-41d4-a716-446655440000",
            "resource": "category-resource-name",
            "title": {
              "language": "en",
              "value": "Category Name"
            }
          }
        ]
      }
    }
  ]
}
```

## JSON Structure Explained

| Field                     | Type    | Description |
| ------------------------- | ------- | ----------- |
| data[]                    | array   | List of category groups |
| data[].uuid               | string  | Category group unique identifier |
| data[].resource           | string  | Category group resource name |
| data[].domain_area_uuid   | string  | Associated domain area UUID |
| data[].title              | object  | Localized category group title |
| data[].title.value        | string  | Title text |
| data[].title.language     | string  | Title language code |
| data[].group              | object  | Group information (when categories=true) |
| data[].group.category[]   | array   | List of categories in this group |

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

- Returns all category groups for the platform's domain area
- Use `categories=true` to include related categories in the response
- Titles are localized based on the `language` parameter or `Accept-Language` header
- If a localized title is not available, the default title is returned

## Related

- [List Learn Categories](./LearnCategoryIndex.md)
- [Show Learn Category Group](./LearnCategoryGroupShow.md)

## Changelog

- 2025-10-16: Initial documentation
