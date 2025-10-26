# Shared – Show Platform Details

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

## Description

Returns detailed information about a specific platform, including complete company data, regional settings, payment gateways, images, IMAP settings, and aggregated statistics.

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## URL Parameters

| Parameter | Type | Required | Description |
| ----------- | ------- | ----------- | ----------- |
| platform    | string  | Yes         | Platform UUID to retrieve |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Response example

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "total": {
    "users": 150
  },
  "is_parent": true,
  "domain": {
    "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
    "slug": "ecommerce",
    "name": "E-commerce"
  },
  "name": "Demo Platform",
  "email": "contact@platform.com",
  "open_to_register": true,
  "public_key": "pk_test_123456789",
  "logo": {
    "main": "https://cdn.example.com/logos/main.png",
    "square": "https://cdn.example.com/logos/square.png",
    "rounded": "https://cdn.example.com/logos/rounded.png",
    "rectangular": "https://cdn.example.com/logos/rectangular.png"
  },
  "created_at": "2024-01-15T10:30:00Z",
  "company": {
    "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
    "name": "Demo Company Ltd",
    "slug": "demo-company",
    "zipcode": "01310-100",
    "country": "Brazil",
    "state": "São Paulo",
    "city": "São Paulo",
    "company_logos": [
      {
        "main_logo": "https://cdn.example.com/company/main.png"
      },
      {
        "square_logo": "https://cdn.example.com/company/square.png"
      }
    ],
    "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
    "links": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "platform": "Facebook",
        "url": "https://facebook.com/platform"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "platform": "Instagram",
        "url": "https://instagram.com/platform"
      }
    ]
  },
  "client_id": "client_abc123xyz",
  "discord": {
    "channel_id": "1234567890123456789"
  },
  "is_main": true,
  "is_banned": false,
  "platform_transactions": {
    "in": 5000,
    "out": 3000
  },
  "regional_information": {
    "uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "language": "en",
    "currency": "USD"
  },
  "imap_setting": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "host": "imap.gmail.com",
    "port": 993,
    "encryption": "ssl",
    "username": "contact@mystore.com",
    "is_active": true
  },
  "payment_gateways": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "name": "Stripe",
      "is_active": true,
      "gateway_type": "credit_card"
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "name": "PayPal",
      "is_active": true,
      "gateway_type": "wallet"
    }
  ],
  "images": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "usage": "banner",
      "url": "https://cdn.example.com/images/banner.jpg",
      "created_at": "2024-01-20T14:30:00Z"
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "usage": "background",
      "url": "https://cdn.example.com/images/background.jpg",
      "created_at": "2024-01-21T09:15:00Z"
    }
  ],
  "total_counts": {
    "products": 250,
    "orders": 1500,
    "articles": 45,
    "events": 12,
    "newsletters": 8
  }
}
```

## JSON Structure Explained

### Main Fields (always present)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| uuid | string  | Platform unique identifier |
| total | object  | Platform totals (conditional - only if there are users) |
| total.users | integer | Total registered users |
| is_parent | boolean | Indicates if the platform is a parent platform |
| domain | object  | Domain area information |
| domain.uuid | string  | Domain area unique identifier |
| domain.slug | string  | Domain area slug |
| domain.name | string  | Domain area name |
| name | string  | Platform name |
| email | string  | Platform contact email |
| open_to_register | boolean | Indicates if open for new registrations |
| public_key | string  | Platform public key |
| logo | object  | Platform logo URLs |
| logo.main | string  | Main logo URL |
| logo.square | string  | Square logo URL |
| logo.rounded | string  | Rounded logo URL |
| logo.rectangular | string  | Rectangular logo URL |
| created_at | string  | Creation date (ISO 8601) |

### Company Fields (conditional)

These fields appear within the `company` object only when the platform has an associated company:

| Field | Type | Description |
| ----------- | ------- | ----------- |
| company | object  | Company data (conditional) |
| company.uuid | string  | Company UUID |
| company.name | string  | Company name |
| company.slug | string  | Company slug |
| company.zipcode | string\|null | Zip code |
| company.country | string\|null | Country |
| company.state | string\|null | State |
| company.city | string\|null | City |
| company.company_logos | array  | Company logos |
| company.full_address | string\|null | Full formatted address |
| company.links | array  | Social media |
| company.links[].uuid | string  | Social link UUID |
| company.links[].platform | string  | Social platform name |
| company.links[].url | string  | Profile URL |

### Detailed Fields (show exclusive)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| client_id | string  | OAuth client ID |
| discord | object  | Discord settings |
| discord.channel_id | string  | Discord channel ID for notifications |

### Affiliation Fields (conditional - platformAffiliated)

Appear only when the platform has an affiliation relationship:

| Field | Type | Description |
| ----------- | ------- | ----------- |
| is_main | boolean | If it's the main platform |
| is_banned | boolean | If it's banned |
| platform_transactions | object  | Platform transactions |
| platform_transactions.in | integer | Incoming transactions |
| platform_transactions.out | integer | Outgoing transactions |

### Regional Information (conditional)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| regional_information | object  | Regional information (conditional) |
| regional_information.uuid | string  | Regional information UUID |
| regional_information.language | string  | Language code (e.g.: pt-BR, en, es) |
| regional_information.currency | string  | Currency code (e.g.: BRL, USD, EUR) |

### IMAP Settings (conditional)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| imap_setting | object  | IMAP settings (conditional) |
| imap_setting.uuid | string  | Configuration UUID |
| imap_setting.host | string  | IMAP server host |
| imap_setting.port | integer | IMAP server port |
| imap_setting.encryption | string  | Encryption type (ssl, tls, none) |
| imap_setting.username | string  | Authentication username |
| imap_setting.is_active | boolean | If the configuration is active |

### Payment Gateways (conditional)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| payment_gateways | array  | Gateway list (conditional) |
| payment_gateways[].uuid | string  | Gateway UUID |
| payment_gateways[].name | string  | Gateway name |
| payment_gateways[].is_active | boolean | If it's active |
| payment_gateways[].gateway_type | string  | Gateway type |

### Images (conditional)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| images | array  | Platform images (conditional) |
| images[].uuid | string  | Image UUID |
| images[].usage | string  | Image usage (banner, background, etc) |
| images[].url | string  | Full image URL |
| images[].created_at | string  | Creation date (ISO 8601) |

### Aggregated Counters

| Field | Type | Description |
| ----------- | ------- | ----------- |
| total_counts | object  | Aggregated counters |
| total_counts.products | integer | Total products (only if > 0) |
| total_counts.orders | integer | Total orders (only if > 0) |
| total_counts.articles | integer | Total articles (only if > 0) |
| total_counts.events | integer | Total events (only if > 0) |
| total_counts.newsletters | integer | Total newsletters (only if > 0) |

## Loaded Relationships

This endpoint automatically loads the following relationships:

- `domainArea` - Domain area
- `company` - Associated company
- `company.logos` - Company logos
- `company.address` - Company address
- `company.socialMedias` - Company social media
- `regionalInformation` - Regional information
- `imapSetting` - IMAP settings
- `userRoles` - User roles
- `userRoles.user` - User data
- `paymentGateways` - Payment gateways
- `images` - Platform images
- `affiliated` - Affiliated platforms

## Counters

- `users_count` - Total users
- `products_count` - Total products
- `orders_count` - Total orders
- `articles_count` - Total articles
- `events_count` - Total events
- `newsletters_count` - Total newsletters

## HTTP Status

- 200: OK
- 401: Unauthenticated
- 403: Forbidden (without `show.all` permission)
- 404: Platform not found
- 422: Validation error
- 429: Rate limit exceeded
- 500: Internal error

## Errors

```json
{
  "message": "Error message"
}
```

## Conditional Fields

The following field blocks are conditional and only appear when applicable:

- **company**: Only when the platform has an associated company
- **is_main, is_banned, platform_transactions**: Only when it has platformAffiliated relationship
- **regional_information**: Only when the relationship is loaded
- **imap_setting**: Only when configured and the relationship is loaded
- **payment_gateways**: Only when the relationship is loaded
- **images**: Only when the relationship is loaded
- **Counters in total_counts**: Only if the value is greater than 0

## Notes

- This endpoint returns much more detailed information than the index
- The `client_id` is only exposed in show, not in index
- Discord settings include the platform-specific channel or the default channel
- All arrays return empty when relationships are not available
- All dates follow ISO 8601 format
- Logo and image URLs are complete and ready to use
- Aggregated counters provide an overview of platform activity

## Related

- [List Platforms](BackofficePlatformIndex.md)
- [List Platform Users](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Complete update with all detailed fields including client_id, discord, regional_information, imap_setting, payment_gateways, images and total_counts
- 2025-10-16: Initial documentation
