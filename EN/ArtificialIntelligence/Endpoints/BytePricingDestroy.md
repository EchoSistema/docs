# Artificial Intelligence – Delete Byte Pricing Product

## Endpoint

```
DELETE /api/v1/ai/admin/pricing/bytes/{product:uuid}
```

## Authentication

Required – Bearer {token} with ability `auth:sanctum`

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
| product   | string | Yes      | Product UUID. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001', {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
  }
});
```

### Response example

```json
{
  "message": "Byte pricing successfully deleted"
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (product not found or wrong measurement type)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Not Found

```json
{
  "message": "The requested resource was not found.",
  "errors": {}
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notes

- This endpoint permanently deletes a byte pricing product.
- The product must have measurement type BYTE, otherwise 404 is returned.
- This action is irreversible - deleted products cannot be recovered.
- Make sure you have proper backups before deleting pricing data.
- After deletion, you can create a new byte pricing product using the store endpoint.

## Related

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)

## Changelog

- 2025-10-23: Updated with detailed documentation and accurate response structure.
- 2025-10-16: Initial documentation.
