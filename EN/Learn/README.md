# Learn Domain API Documentation

The **Learn** domain provides API endpoints for educational and event management within the EchoSistema platform. These endpoints handle event listings, content management, categories, and event viewing features.

## Overview

Educational and event content management. Each platform must include the `X-PUBLIC-KEY` header to authenticate and receive responses. The frontend should send the current application language via the `Accept-Language` header (e.g., `en`, `pt-BR`).

This domain contains endpoints for:

- **Events**: Event listing, details, counters, and text content retrieval
- **Event Contents**: Content and sub-content management for events
- **Event Content Types**: Available content types for events
- **Event Views**: Materialized views for event data
- **Learn Categories**: Category management for the Learn domain
- **Learn Category Groups**: Category group listing and details

## Authentication

Most endpoints require authentication using Bearer tokens:

```
Authorization: Bearer {token}
```

Some endpoints may work without authentication but require the platform identification through `X-PUBLIC-KEY`.

## Common Headers

| Header          | Required | Description |
| --------------- | -------- | ----------- |
| Authorization   | Varies   | Bearer token for authenticated requests |
| X-PUBLIC-KEY    | Yes      | Platform public key for all requests |
| Accept-Language | No       | IETF locale (e.g., `pt-BR`, `en`, `es`) for localized content |

## Endpoint Documentation

For detailed endpoint documentation, see the [Endpoints](./Endpoints/README.md) directory.

## Quick Links

- [Event Endpoints](./Endpoints/README.md#event-endpoints)
- [Event Content Endpoints](./Endpoints/README.md#event-content-endpoints)
- [Category Endpoints](./Endpoints/README.md#category-endpoints)

## Support

For API support or documentation updates:
- Documentation Repository: https://github.com/EchoSistema/docs
- Support Email: api-support@echosistema.com
