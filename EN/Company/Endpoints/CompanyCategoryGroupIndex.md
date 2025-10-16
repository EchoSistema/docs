# Company – List Company Category Groups

## Endpoint

```
GET /api/v1/company/category-groups
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

| Parameter  | Type    | Required | Description | Default/Values |
| ---------- | ------- | -------- | ----------- | -------------- |
| language   | string  | No       | Language code for titles (e.g., `en`, `pt-BR`, `es`). | Platform default |
| categories | boolean | No       | Include categories within groups. | `false` |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company/category-groups?language=en&categories=true"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "title": "Technology Services",
      "group": {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "category": [
          {
            "uuid": "00000000-0000-0000-0000-000000000100",
            "title": "Software Development"
          },
          {
            "uuid": "00000000-0000-0000-0000-000000000101",
            "title": "IT Support"
          }
        ]
      },
      "domain_area": {
        "uuid": "00000000-0000-0000-0000-000000000020",
        "name": "Technology"
      }
    }
  ]
}
```

## JSON Structure Explained

| Field                        | Type     | Description |
| ---------------------------- | -------- | ----------- |
| data[]                       | array    | List of category groups. |
| data[].uuid                  | string   | Category group UUID. |
| data[].title                 | string   | Category group title in requested language. |
| data[].group                 | object   | Group details (when `categories=true`). |
| data[].group.uuid            | string   | Group UUID. |
| data[].group.category[]      | array    | List of categories in the group. |
| data[].group.category[].uuid | string   | Category UUID. |
| data[].group.category[].title| string   | Category title in requested language. |
| data[].domain_area           | object   | Domain area information. |
| data[].domain_area.uuid      | string   | Domain area UUID. |
| data[].domain_area.name      | string   | Domain area name. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Invalid platform key.",
  "errors": {}
}
```

## Notes

- Returns only category groups associated with the current platform (identified by `X-PUBLIC-KEY`).
- When `language` is not provided, the system uses the platform's default language.
- If a title in the requested language is not available, the system falls back to the default language.
- When `categories=false` or omitted, only category group information is returned without nested categories.

## Related

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Initial documentation.
