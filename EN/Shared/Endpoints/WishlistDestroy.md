# Shared – Remove Item from Wishlist

## Endpoint

```
DELETE /api/v1/wishlists/{wishlist}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| wishlist  | string | Yes      | UUID of the wishlist entry to remove |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/wishlists/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "message": "Deleted successfully."
}
```

## JSON Structure Explained

| Field   | Type   | Description |
| ------- | ------ | ----------- |
| message | string | Success message confirming deletion |

## HTTP Status

- 200: OK - Item removed from wishlist successfully
- 401: Unauthorized - Invalid or missing token
- 403: Forbidden - User does not own this wishlist entry
- 404: Not Found - Wishlist entry not found
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

### 404 Not Found
```json
{
  "message": "No query results for model [Domain\\Shared\\Models\\Wishlist]."
}
```

## Notes

- Only the owner of the wishlist entry can delete it
- The endpoint uses UUID for identification (not internal ID)
- Deletion is soft delete - the record is marked as deleted but remains in the database
- After successful deletion, the item will no longer appear in the user's wishlist
- If the wishlist entry doesn't exist, a 404 error is returned

## Related

- [List User Wishlists](./WishlistIndex.md)
- [Add Item to Wishlist](./WishlistStore.md)

## Changelog

- 2025-10-17: Initial version - Remove item from wishlist with ownership validation
