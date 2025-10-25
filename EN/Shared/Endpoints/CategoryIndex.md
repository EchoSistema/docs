# Shared – List Categories

## Endpoint

```
GET /api/v1/categories
```

## Authentication

None

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). Used to filter titles. |

## Query Parameters

| Parameter | Type | Required | Description |
| ----------- | ------- | ----------- | ----------- |
| domain_area_uuid | string (UUID) | No | Domain area UUID to filter categories. If not provided, uses the current platform's domain area. |
| language | string | No | Language code to filter titles (e.g., `pt-BR`, `en`, `es`). If not provided, returns default titles. |
| noPaginate | boolean | No | If present, returns all results without pagination. |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/categories?language=en"
```

### Request example with specific domain area

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/categories?domain_area_uuid=123e4567-e89b-12d3-a456-426614174000&language=en"
```

### Request example without pagination

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/categories?noPaginate=true&language=en"
```

### Response example (paginated)

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "domain_area": {
        "uuid": "123e4567-e89b-12d3-a456-426614174001",
        "name": "Ecommerce"
      },
      "group": {
        "uuid": "123e4567-e89b-12d3-a456-426614174002",
        "domain_area": {
          "uuid": "123e4567-e89b-12d3-a456-426614174001",
          "name": "Ecommerce"
        },
        "title": "Electronics",
        "title_language": "en",
        "is_default": true,
        "slug": "electronics"
      },
      "title": "Smartphones",
      "title_is_default": true,
      "title_language": "en",
      "slug": "smartphones"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/categories?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/categories?page=3",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/categories?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "path": "https://sandbox.your-domain.com/api/v1/categories",
    "per_page": 10,
    "to": 10,
    "total": 30
  }
}
```

### Response example (without pagination)

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "domain_area": {
        "uuid": "123e4567-e89b-12d3-a456-426614174001",
        "name": "Ecommerce"
      },
      "group": {
        "uuid": "123e4567-e89b-12d3-a456-426614174002",
        "domain_area": {
          "uuid": "123e4567-e89b-12d3-a456-426614174001",
          "name": "Ecommerce"
        },
        "title": "Electronics",
        "title_language": "en",
        "is_default": true,
        "slug": "electronics"
      },
      "title": "Smartphones",
      "title_is_default": true,
      "title_language": "en",
      "slug": "smartphones"
    }
  ]
}
```

## JSON Structure Explained

### Paginated Response

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | array   | Array of category objects |
| links       | object  | Pagination navigation links |
| links.first | string  | URL to first page |
| links.last  | string  | URL to last page |
| links.prev  | string\|null | URL to previous page (null if first page) |
| links.next  | string\|null | URL to next page (null if last page) |
| meta        | object  | Pagination metadata |
| meta.current_page | integer | Current page number |
| meta.from   | integer | Index of first item on page |
| meta.last_page | integer | Last page number |
| meta.path   | string  | Base URL of route |
| meta.per_page | integer | Items per page |
| meta.to     | integer | Index of last item on page |
| meta.total  | integer | Total number of items |

### Category Object (data[])

| Field | Type | Description |
| ----------- | ------- | ----------- |
| uuid        | string  | Unique category UUID |
| domain_area | object\|null | Category's domain area (only if loaded) |
| domain_area.uuid | string | Domain area UUID |
| domain_area.name | string | Domain area name |
| group       | object\|null | Group to which the category belongs (only if loaded) |
| group.uuid  | string  | Group UUID |
| group.domain_area | object | Group's domain area |
| group.title | string  | Group title in requested or default language |
| group.title_language | string | Title language code |
| group.is_default | boolean | Whether the title is the default one |
| group.slug  | string  | Slug of group title |
| title       | string  | Category title in requested or default language |
| title_is_default | boolean | Whether the returned title is the default one |
| title_language | string | Title language code (e.g., `pt-BR`, `en`, `es`) |
| slug        | string  | Slug generated from title |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- Returns categories filtered by domain area
- Titles are filtered by language parameter or default language
- Supports pagination by default (10 items per page)
- Use `noPaginate` to get all results at once

## Related

- [Category Group Index](CategoryGroupIndex.md)
- [Category Group Store](CategoryGroupStore.md)
- See other Shared API endpoints

## Changelog

- 2025-10-25: Updated documentation with complete structure and relations
- 2025-10-16: Initial documentation
