# PaymentHub Endpoints

This directory contains detailed documentation for all PaymentHub domain endpoints.

## Endpoint List

### Payment Orders

| Endpoint | Method | Description |
| -------- | ------ | ----------- |
| [PaymentOrderIndex](./PaymentOrderIndex.md) | GET | List all payment orders for the authenticated platform |
| [PaymentOrderShow](./PaymentOrderShow.md) | GET | Retrieve detailed information about a specific payment order |

## Common Features

### Authentication

All endpoints require:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {platform_key}`

### Pagination

List endpoints support pagination:
- `per_page`: Number of results per page (default: 10)

### Response Format

All responses follow the standard JSON format with `data` wrapper and pagination metadata when applicable.

## Related Documentation

- [PaymentHub Domain Overview](../README.md)
- [Ecommerce Domain](../../Ecommerce/README.md)
