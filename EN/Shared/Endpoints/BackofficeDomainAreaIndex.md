# Shared – List Domain Areas

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No additional parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas"
```

### Response example

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
      "name": "Article",
      "slug": "article",
      "is_active": true,
      "platforms_total": 5,
      "categories_total": 12,
      "item_types_total": 8
    },
    {
      "id": 2,
      "uuid": "70f8067c-7d68-50ef-b847-c43e3d6c0878",
      "name": "Bio",
      "slug": "bio",
      "is_active": true,
      "platforms_total": 3,
      "categories_total": 0,
      "item_types_total": 0
    }
  ]
}
```

## JSON Structure Explained

| Field                      | Type    | Description                                         |
| -------------------------- | ------- | --------------------------------------------------- |
| data                       | array   | List of domain areas                                |
| data[].id                  | integer | Internal domain area ID                             |
| data[].uuid                | string  | Unique domain area UUID                             |
| data[].name                | string  | Domain area name (formatted)                        |
| data[].slug                | string  | Domain area identifier slug                         |
| data[].is_active           | boolean | Indicates if the domain area is active              |
| data[].platforms_total     | integer | Total number of associated platforms                |
| data[].categories_total    | integer | Total number of associated categories               |
| data[].item_types_total    | integer | Total number of associated item types               |

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

- Lists all domain areas with counters for platforms, categories and item types
- Backoffice-only endpoint requiring `index.all` permission
- Returns all domain areas at once (not paginated)

## Related

- [Show Domain Area](BackofficeDomainAreaShow.md)
- [List Platforms](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Updated documentation with complete response structure
- 2025-10-16: Initial documentation
