# Shared – List User Wishlists

## Endpoint

```
GET /api/v1/wishlists
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| per_page  | integer | No       | Number of items per page | 25 |
| page      | integer | No       | Page number | 1 |

> The endpoint accepts parameter name variants: `per_page`, `perPage`, `per-page`.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/wishlists?per_page=25&page=1"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "user_id": 1,
      "type": "company",
      "type_id": 123,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "user_id": 1,
      "type": "product",
      "type_id": 456,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/wishlists?page=1",
    "last": "https://api.example.com/api/v1/wishlists?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/wishlists?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 120
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data | array | Array of wishlist items |
| data[].uuid | string | Unique identifier for the wishlist entry |
| data[].user_id | integer | User ID who owns this wishlist entry |
| data[].type | string | Type of wishlist item (bio, property, event, course, product, platform, company, article, complaint, review, country, state, city) |
| data[].type_id | integer | Internal ID of the wishlisted item |
| data[].created_at | string | ISO 8601 creation timestamp |
| data[].updated_at | string | ISO 8601 last update timestamp |
| links | object | Pagination links |
| links.first | string | URL to first page |
| links.last | string | URL to last page |
| links.prev | string\|null | URL to previous page |
| links.next | string\|null | URL to next page |
| meta | object | Pagination metadata |
| meta.current_page | integer | Current page number |
| meta.from | integer | First item number on current page |
| meta.last_page | integer | Total number of pages |
| meta.per_page | integer | Items per page |
| meta.to | integer | Last item number on current page |
| meta.total | integer | Total number of wishlist items |

## HTTP Status

- 200: OK - Wishlists retrieved successfully
- 401: Unauthorized - Invalid or missing token
- 403: Forbidden - Insufficient permissions
- 500: Internal Server Error

## Errors

### 401 Unauthorized
```json
{
  "message": "Unauthenticated."
}
```

### 403 Forbidden
```json
{
  "message": "This action is unauthorized."
}
```

## Notes

- Returns only wishlists belonging to the authenticated user
- Results are ordered by most recent first (latest)
- Default pagination is 25 items per page
- Soft-deleted items are not included in the results

## Related

- [Add Item to Wishlist](./WishlistStore.md)
- [Remove Item from Wishlist](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Initial version - List user wishlists with pagination
