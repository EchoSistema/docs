# Company – Show Company Details

## Endpoint

```
GET /api/v1/company
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

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| language  | string | No       | Language code for translatable fields. | Platform default |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company?language=en"
```

### Response example

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "name": "Tech Solutions Inc.",
    "slug": "tech-solutions-inc",
    "rating": 4.5,
    "reviews": 150,
    "country": "Paraguay",
    "state": "Central",
    "city": "Asunción",
    "email": "contact@techsolutions.com",
    "favicon": "https://example.com/favicon.ico",
    "logo": {
      "main": "https://example.com/logo.png",
      "square": "https://example.com/logo-square.png",
      "rounded": "https://example.com/logo-rounded.png",
      "rectangular": "https://example.com/logo-rectangular.png"
    },
    "domain_area": {
      "uuid": "00000000-0000-0000-0000-000000000010",
      "name": "Technology"
    },
    "open_to_register": true
  }
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data                   | object  | Company resource. |
| data.uuid              | string  | Company UUID. |
| data.name              | string  | Company legal name. |
| data.slug              | string  | URL-friendly identifier. |
| data.rating            | float   | Average rating (0-5). |
| data.reviews           | integer | Total number of reviews. |
| data.country           | string  | Country name where company is located. |
| data.state             | string  | State/province name. |
| data.city              | string  | City name. |
| data.email             | string  | Company contact email (from platform). |
| data.favicon           | string  | Platform favicon URL. |
| data.logo              | object  | Platform logo variants. |
| data.logo.main         | string  | Main logo URL. |
| data.logo.square       | string  | Square logo URL. |
| data.logo.rounded      | string  | Rounded logo URL. |
| data.logo.rectangular  | string  | Rectangular logo URL. |
| data.domain_area       | object  | Business domain area information. |
| data.domain_area.uuid  | string  | Domain area UUID. |
| data.domain_area.name  | string  | Domain area name. |
| data.open_to_register  | boolean | Whether the company accepts new registrations. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notes

- This endpoint returns the company associated with the authenticated user's platform.
- The authenticated user must have the `backoffice` ability.
- Rating and reviews are calculated from all company reviews.
- Address information (country, state, city) comes from the company's primary address.
- Platform-related fields (email, logo, favicon, domain_area, open_to_register) are loaded from the associated platform relationship.

## Related

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)
- [Company Address Store](./CompanyAddressStore.md)
- [Company Review Store](./CompanyReviewStore.md)

## Changelog

- 2025-10-16: Initial documentation.
