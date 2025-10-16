# Learn – Show Learn Category Group

## Endpoint

```
GET /api/v1/learn/category-groups/{categoryGroup}
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

### Path parameters

| Parameter     | Type   | Required | Description |
| ------------- | ------ | -------- | ----------- |
| categoryGroup | string | Yes      | Category group resource identifier |

> The `categoryGroup` parameter uses route model binding with the `resource` field.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/category-groups/group-resource-name"
```

### Response example

```json
{
  "data": {
    "uuid": "770e8400-e29b-41d4-a716-446655440002",
    "resource": "group-resource-name",
    "domain_area_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "title": {
      "language": "en",
      "value": "Category Group Name",
      "is_default": false
    }
  }
}
```

## JSON Structure Explained

| Field                   | Type    | Description |
| ----------------------- | ------- | ----------- |
| data                    | object  | Category group details |
| data.uuid               | string  | Category group unique identifier |
| data.resource           | string  | Category group resource name |
| data.domain_area_uuid   | string  | Associated domain area UUID |
| data.title              | object  | Localized category group title |
| data.title.value        | string  | Title text |
| data.title.language     | string  | Title language code |
| data.title.is_default   | boolean | Whether this is the default title |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found - Category group not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Category group not found"
}
```

## Notes

- Returns a single category group by its resource identifier
- The title is localized based on the `Accept-Language` header
- If a localized title is not available, the default title is returned

## Related

- [List Learn Category Groups](./LearnCategoryGroupIndex.md)
- [List Learn Categories](./LearnCategoryIndex.md)

## Changelog

- 2025-10-16: Initial documentation
