# PaymentHub – Payment Order Index

## Endpoint

```
GET /api/v1/payment-hub/payment-orders
```

## Authentication

Required – Bearer {token}

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| per_page  | integer | No       | Number of results per page | 10 (1-100) |
| page      | integer | No       | Page number for pagination | 1 |

> Parameter names accept variants (`camelCase`, `kebab-case`, `snake_case`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/payment-hub/payment-orders?per_page=10"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "status": "completed",
      "amount": 15000,
      "currency": "EUR",
      "created_at": "2025-10-15T10:30:00Z",
      "updated_at": "2025-10-15T11:00:00Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/payment-hub/payment-orders?page=1",
    "last": "https://api.example.com/api/v1/payment-hub/payment-orders?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/payment-hub/payment-orders?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## JSON Structure Explained

| Field              | Type    | Description |
| ------------------ | ------- | ----------- |
| data[]             | array   | List of payment orders |
| data[].uuid        | string  | Unique identifier for the payment order |
| data[].status      | string  | Current status of the payment order |
| data[].amount      | integer | Payment amount in cents |
| data[].currency    | string  | ISO currency code |
| data[].created_at  | string  | ISO 8601 timestamp of creation |
| data[].updated_at  | string  | ISO 8601 timestamp of last update |
| links              | object  | Pagination links |
| meta               | object  | Pagination metadata |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard error response:

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- Payment orders are filtered by the authenticated platform
- Results are paginated with a default of 10 items per page
- Maximum per_page value is 100
- Orders are returned in descending order by creation date

## Related

- [Payment Order Show](./PaymentOrderShow.md)
- [PaymentHub Domain](../README.md)

## Changelog

- 2025-10-16: Initial documentation
