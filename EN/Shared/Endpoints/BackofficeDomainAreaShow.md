# Shared – Show Domain Area

## Endpoint

```
GET /api/v1/backoffice/domain-areas/{domainArea}
```

## Authentication

Required – Bearer {token} with `backoffice` ability

## Headers

| Header           | Type   | Required | Description                                  |
| ---------------- | ------ | -------- | -------------------------------------------- |
| Authorization    | string | Yes      | Bearer token credential.                     |
| X-PUBLIC-KEY     | string | Yes      | Platform public key.                         |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`).    |

## Route Parameters

| Parameter    | Type   | Required | Description                      |
| ------------ | ------ | -------- | -------------------------------- |
| domainArea   | string | Yes      | Domain area UUID.                |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas/efa68660-4870-51c2-84be-736f87792f1d"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
    "name": "Article",
    "slug": "article",
    "is_active": true,
    "platforms": [
      {
        "uuid": "550e8400-e29b-41d4-a716-446655440000",
        "domain": {
          "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
          "name": "Article"
        },
        "name": "My Platform",
        "email": "platform@example.com",
        "open_to_register": true,
        "public_key": "pk_test_123456",
        "logo": {
          "main": "https://cdn.example.com/logo.png",
          "square": "https://cdn.example.com/logo-square.png",
          "rounded": "https://cdn.example.com/logo-rounded.png",
          "rectangular": "https://cdn.example.com/logo-rect.png"
        },
        "created_at": "2024-01-01T00:00:00+00:00"
      }
    ],
    "categories": [
      {
        "uuid": "123e4567-e89b-12d3-a456-426614174000",
        "title": "Technology",
        "title_is_default": true,
        "title_language": "en",
        "slug": "technology"
      }
    ],
    "item_types": [
      {
        "uuid": "987fcdeb-51a2-43f7-8543-123456789012",
        "default_title": true,
        "language": "en",
        "title": "Blog Post",
        "slug": "blog-post"
      }
    ]
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description                                         |
| ------------------------------ | ------- | --------------------------------------------------- |
| data                           | object  | Domain area data                                    |
| data.id                        | integer | Internal domain area ID                             |
| data.uuid                      | string  | Unique domain area UUID                             |
| data.name                      | string  | Domain area name (formatted)                        |
| data.slug                      | string  | Domain area identifier slug                         |
| data.is_active                 | boolean | Indicates if the domain area is active              |
| data.platforms                 | array   | List of associated platforms                        |
| data.platforms[].uuid          | string  | Unique platform UUID                                |
| data.platforms[].domain        | object  | Platform domain data                                |
| data.platforms[].name          | string  | Platform name                                       |
| data.platforms[].email         | string  | Platform email                                      |
| data.platforms[].open_to_register | boolean | Indicates if open for registration              |
| data.platforms[].public_key    | string  | Platform public key                                 |
| data.platforms[].logo          | object  | Logo URLs in different formats                      |
| data.platforms[].created_at    | string  | Creation date (ISO 8601)                            |
| data.categories                | array   | List of domain area categories                      |
| data.categories[].uuid         | string  | Unique category UUID                                |
| data.categories[].title        | string  | Category title                                      |
| data.categories[].title_is_default | boolean | Indicates if it's the default title            |
| data.categories[].title_language | string | Title language                                     |
| data.categories[].slug         | string  | Category slug                                       |
| data.item_types                | array   | List of domain area item types                      |
| data.item_types[].uuid         | string  | Unique item type UUID                               |
| data.item_types[].default_title | boolean | Indicates if it's the default title                |
| data.item_types[].language     | string  | Title language                                      |
| data.item_types[].title        | string  | Item type title                                     |
| data.item_types[].slug         | string  | Item type slug                                      |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden (insufficient permissions)
- 404: Domain area not found
- 429: Too many requests
- 500: Internal error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- Shows a specific domain area with all associated platforms, categories and item types
- Requires `show.all` permission in backoffice context
- The `{domainArea}` parameter must be the domain area UUID
- Categories and item types include their localized titles

## Related

- [List Domain Areas](BackofficeDomainAreaIndex.md)
- [List Platforms](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Initial documentation
