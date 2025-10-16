# PaymentHub – Payment Order Show

## Endpoint

```
GET /api/v1/payment-hub/payment-orders/{paymentOrder}
```

## Authentication

Required – Bearer {token}

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |

## Parameters

### Path parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| paymentOrder | string | Yes      | UUID of the payment order |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/payment-hub/payment-orders/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "status": "completed",
    "amount": 15000,
    "currency": "EUR",
    "installments": [
      {
        "number": 1,
        "amount": 5000,
        "due_date": "2025-11-01",
        "status": "paid"
      },
      {
        "number": 2,
        "amount": 5000,
        "due_date": "2025-12-01",
        "status": "pending"
      },
      {
        "number": 3,
        "amount": 5000,
        "due_date": "2026-01-01",
        "status": "pending"
      }
    ],
    "created_at": "2025-10-15T10:30:00Z",
    "updated_at": "2025-10-15T11:00:00Z"
  }
}
```

## JSON Structure Explained

| Field                    | Type    | Description |
| ------------------------ | ------- | ----------- |
| data                     | object  | Payment order details |
| data.uuid                | string  | Unique identifier for the payment order |
| data.status              | string  | Current status of the payment order |
| data.amount              | integer | Total payment amount in cents |
| data.currency            | string  | ISO currency code |
| data.installments[]      | array   | List of payment installments |
| data.installments[].number | integer | Installment number |
| data.installments[].amount | integer | Installment amount in cents |
| data.installments[].due_date | string | Due date for the installment |
| data.installments[].status | string | Status of the installment |
| data.created_at          | string  | ISO 8601 timestamp of creation |
| data.updated_at          | string  | ISO 8601 timestamp of last update |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard error responses:

```json
{
  "message": "Payment order not found."
}
```

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- The payment order must belong to the authenticated platform
- Installment data is automatically loaded when viewing a specific payment order
- If the payment order doesn't exist or doesn't belong to the platform, a 404 error is returned

## Related

- [Payment Order Index](./PaymentOrderIndex.md)
- [PaymentHub Domain](../README.md)

## Changelog

- 2025-10-16: Initial documentation
