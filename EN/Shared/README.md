# Shared Domain API Documentation

The **Shared** domain provides common API endpoints used across the EchoSistema platform. These endpoints handle core functionalities like user management, authentication helpers, platform resources, chat messaging, contact forms, and content management.

## Overview

Common resources across platforms. Each platform must include the `X-PUBLIC-KEY` header to authenticate and receive responses. The frontend should send the current application language via the `Accept-Language` header (e.g., `en`, `pt-BR`).

This domain contains endpoints for:

- **Chat System**: Real-time messaging, conversations, reactions, and device statistics
- **Contact Management**: Platform contact forms and message handling
- **Orders**: Order listing and details
- **Images**: Image upload and management
- **User Profile**: User profile retrieval and regional settings (language/currency)
- **Addresses**: Address creation and management
- **Categories**: Category and category group management
- **Resource Lists**: Available currencies, languages, and roles
- **Newsletter**: Newsletter subscription management
- **Password**: Password update functionality
- **Health Check**: API health status monitoring
- **Platform Users**: Platform user management and role assignment
- **Backoffice**: Administrative endpoints for domain areas, platforms, and users
- **Platform Management**: Email configuration, images, texts, titles, and popular content

## Authentication

Most endpoints require authentication using Bearer tokens:

\`\`\`
Authorization: Bearer {token}
\`\`\`

Some endpoints also require specific abilities (e.g., \`backoffice\`) for access control.

## Common Headers

| Header          | Required | Description |
| --------------- | -------- | ----------- |
| Authorization   | Varies   | Bearer token for authenticated requests |
| X-PUBLIC-KEY    | Yes      | Platform public key for all requests |
| Accept-Language | No       | IETF locale (e.g., \`pt-BR\`, \`en\`, \`es\`) for localized content |

## Endpoint Documentation

For detailed endpoint documentation, see the [Endpoints](./Endpoints/README.md) directory.

## Quick Links

- [Chat Endpoints](./Endpoints/README.md#chat-endpoints)
- [Contact Endpoints](./Endpoints/README.md#contact-endpoints)
- [User Management](./Endpoints/README.md#user-endpoints)
- [Platform Management](./Endpoints/README.md#platform-management-endpoints)

## Support

For API support or documentation updates:
- Documentation Repository: https://github.com/EchoSistema/docs
- Support Email: api-support@echosistema.com
