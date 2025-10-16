# Company – Create Company Review

## Endpoint

```
POST /api/v1/company/reviews
```

## Authentication

Required – Bearer {token}

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Request body

| Parameter     | Type    | Required | Description | Default/Values |
| ------------- | ------- | -------- | ----------- | -------------- |
| company       | string  | Yes*     | Company identifier (UUID or ID). Required if `company_id` and `company_uuid` are not provided. | |
| company_id    | integer | Yes*     | Company numeric ID. Required if `company` and `company_uuid` are not provided. | |
| company_uuid  | string  | Yes*     | Company UUID. Required if `company` and `company_id` are not provided. | |
| review        | string  | Yes      | Review text content. | |
| rating        | integer | Yes      | Rating value between 0 and 5. | 0-5 |

> *One of `company`, `company_id`, or `company_uuid` must be provided.

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "company_uuid": "00000000-0000-0000-0000-000000000001",
    "review": "Excellent service and professional team. Highly recommended!",
    "rating": 5
  }' \
  "https://sandbox.your-domain.com/api/v1/company/reviews"
```

### Response example

```json
{
  "data": {
    "id": 123,
    "author_name": "John Doe",
    "profile_photo_url": "https://example.com/avatars/johndoe.jpg",
    "text": "Excellent service and professional team. Highly recommended!",
    "company": {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "name": "Tech Solutions Inc.",
      "logo": [
        {
          "usage": "logo",
          "url": "https://example.com/logos/techsolutions.png"
        }
      ],
      "assumed_name": "TechSol",
      "slug": "tech-solutions-inc",
      "rating": 4.7,
      "google": null
    },
    "time": "2025-10-16T15:30:00+00:00",
    "rating": 5
  }
}
```

## JSON Structure Explained

| Field                       | Type    | Description |
| --------------------------- | ------- | ----------- |
| data                        | object  | Review resource. |
| data.id                     | integer | Review identifier. |
| data.author_name            | string  | Name of the user who wrote the review. |
| data.profile_photo_url      | string  | Author's profile photo URL. |
| data.text                   | string  | Review text content. |
| data.company                | object  | Company information. |
| data.company.uuid           | string  | Company UUID. |
| data.company.name           | string  | Company name. |
| data.company.logo[]         | array   | Array of company logo objects. |
| data.company.logo[].usage   | string  | Logo usage type. |
| data.company.logo[].url     | string  | Logo URL. |
| data.company.assumed_name   | string  | Company trade name. |
| data.company.slug           | string  | Company URL-friendly identifier. |
| data.company.rating         | float   | Company average rating. |
| data.company.google         | mixed   | Google-related data (if available). |
| data.time                   | string  | Review creation timestamp (ISO 8601). |
| data.rating                 | integer | Review rating (0-5). |

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "review": ["The review field is required."],
    "rating": ["The rating field is required.", "The rating must be between 0 and 5."],
    "company": ["At least one company identifier is required."]
  }
}
```

## Notes

- The authenticated user is automatically associated as the review author.
- The `company` parameter accepts either a UUID or numeric ID and is automatically resolved.
- Rating values must be integers between 0 and 5 (inclusive).
- After creating a review, the company's average rating is recalculated.
- Users can create multiple reviews for different companies but business rules may limit duplicate reviews for the same company.

## Related

- [Company Review Index](./CompanyReviewIndex.md)
- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Initial documentation.
