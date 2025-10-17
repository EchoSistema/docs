# Shared – Add Item to Wishlist

## Endpoint

```
POST /api/v1/wishlists
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Request body

```json
{
  "type": "company",
  "uuid": "550e8400-e29b-41d4-a716-446655440000"
}
```

| Field | Type   | Required | Description |
| ----- | ------ | -------- | ----------- |
| type  | string | Yes      | Type of item to add. Valid values: `bio`, `property`, `event`, `course`, `product`, `platform`, `company`, `article`, `complaint`, `review`, `country`, `state`, `city` |
| uuid  | string | Yes      | UUID of the item to add to wishlist |

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "company",
    "uuid": "550e8400-e29b-41d4-a716-446655440000"
  }' \
  "https://sandbox.your-domain.com/api/v1/wishlists"
```

### Response example

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "user_id": 1,
    "type": "company",
    "type_id": 123,
    "created_at": "2025-10-17T00:00:00+00:00",
    "updated_at": "2025-10-17T00:00:00+00:00"
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data | object | Wishlist entry data |
| data.uuid | string | Unique identifier for the wishlist entry |
| data.user_id | integer | User ID who owns this wishlist entry |
| data.type | string | Type of wishlisted item |
| data.type_id | integer | Internal ID of the wishlisted item |
| data.created_at | string | ISO 8601 creation timestamp |
| data.updated_at | string | ISO 8601 last update timestamp |

## HTTP Status

- 200: OK - Item already in wishlist (returns existing entry)
- 201: Created - Item added to wishlist successfully
- 401: Unauthorized - Invalid or missing token
- 403: Forbidden - Insufficient permissions
- 404: Not Found - Item with provided UUID not found
- 422: Unprocessable Entity - Invalid type or validation error
- 500: Internal Server Error

## Errors

### 401 Unauthorized
```json
{
  "message": "Unauthenticated."
}
```

### 404 Not Found
```json
{
  "message": "Item not found."
}
```

### 422 Unprocessable Entity
```json
{
  "message": "Invalid type.",
  "errors": {
    "type": ["The selected type is invalid."],
    "uuid": ["The uuid field must be a valid UUID."]
  }
}
```

## Notes

- If the item is already in the user's wishlist, the existing entry is returned (no duplicate)
- The `type` field accepts enum values that map to different resource types
- The endpoint automatically resolves the class from the type and finds the item by UUID
- Both `type` and `uuid` are required fields
- The authenticated user becomes the owner of the wishlist entry

## Related

- [List User Wishlists](./WishlistIndex.md)
- [Remove Item from Wishlist](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Initial version - Add item to wishlist with type and UUID
