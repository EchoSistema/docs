# PaymentHub Domain

The PaymentHub domain handles payment order management and retrieval for the platform.

## Overview

This domain provides endpoints for:
- Listing payment orders associated with a platform
- Retrieving detailed information about specific payment orders
- Managing payment transactions and installments

## Authentication

All endpoints require authentication via Bearer token with the `X-PUBLIC-KEY` header.

## Base Path

```
/api/v1/payment-hub
```

## Available Endpoints

- [Payment Order Index](./Endpoints/PaymentOrderIndex.md) - List all payment orders for the platform
- [Payment Order Show](./Endpoints/PaymentOrderShow.md) - Get details of a specific payment order

## Resources

### PaymentOrder

A payment order represents a transaction or payment intent created by a platform user.

**Key attributes:**
- `uuid`: Unique identifier
- `status`: Current status of the payment
- `amount`: Payment amount
- `currency`: Payment currency
- `installments`: Payment installment details (when applicable)

## Related Domains

- **Shared**: Common resources and authentication
- **Ecommerce**: Product purchases that generate payment orders

## Notes

- Payment orders are scoped to platforms
- Installment data is only loaded when viewing a specific payment order
- Pagination is available with configurable page size (default: 10)

## Support

For questions or issues related to PaymentHub endpoints, refer to the individual endpoint documentation or contact the development team.
