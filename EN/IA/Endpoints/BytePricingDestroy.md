# IA — Byte Pricing: Destroy

## Endpoint

```
DELETE /ia/admin/pricing/bytes/{product}
```

Soft-deletes a BYTE product.

## Authentication

Required — Bearer {token} with Sanctum.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}` |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| product   | string | Yes      | Product route key |

## Examples

### Request (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes/{product}"
```

### Response (200)

```json
{ "message": "Deleted successfully." }
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found (when product is not BYTE)

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md

## Changelog

- 2025-09-23: Added headers and parameters per template.
