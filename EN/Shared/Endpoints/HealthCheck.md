# Shared – Health Check

## Endpoints

Each domain has its own health check route:

```
GET /api/v1/advertising/health
GET /api/v1/affiliate/health
GET /api/v1/articles/health
GET /api/v1/ai/health
GET /api/v1/bio/health
GET /api/v1/service/health              # Brick
GET /api/v1/cms/health
GET /api/v1/company/health
GET /api/v1/ecommerce/health
GET /api/v1/find-a-job/health
GET /api/v1/footprints/health
GET /api/v1/health-care/health
GET /api/v1/its-checked/health
GET /api/v1/learn/health
GET /api/v1/link-list/health
GET /api/v1/public/health               # Microservices
GET /api/v1/news/health
GET /api/v1/payment-hub/health
GET /api/v1/pigeons/health
GET /api/v1/real-estate/health
GET /api/v1/rent/health
GET /api/v1/complaints/health           # ReputationBook
GET /api/v1/university/health
```

## Authentication

None

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No required parameters. Optionally, provide `pbk=<uuid>` in the query string if unable to send headers.

## Examples

### Request example (curl)

```bash
# Check health of a specific domain
curl -X GET \
  -H "X-PUBLIC-KEY: 123e4567-e89b-12d3-a456-426614174000" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/health"
```

### Response example

```json
{
  "data": {
    "domain": "RealEstate",
    "slug": "real-estate",
    "status": "ok",
    "checked_at": "2025-11-19T18:43:00+00:00",
    "routes": {
      "file": "src/Domain/RealEstate/routes/api.php",
      "exists": true,
      "last_modified": "2025-11-18T20:01:00+00:00"
    }
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data | object | Health information for the domain. |
| data.domain | string | Domain name (folder in `src/Domain`). |
| data.slug | string | Slug used in the route (`Str::kebab(domain)`). |
| data.status | string | `ok` when route file exists, `missing-routes` otherwise. |
| data.checked_at | string | Current check timestamp (ISO 8601). |
| data.routes.file | string | Relative path to the `routes/api.php` file. |
| data.routes.exists | boolean | Indicates if the file exists. |
| data.routes.last_modified | string\|null | Last update timestamp (ISO 8601). |

## HTTP Status

- 200: OK
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
  "message": "Error message"
}
```

## Notes

- Each domain has its own health check route registered directly in its respective `routes/api.php`.
- The endpoint checks if the domain's route file exists and returns the last modification time.
- There is no longer a global aggregated `/health` endpoint. Each domain must be checked individually.
- Endpoint does not require login, but requires the public key (`X-PUBLIC-KEY` or `pbk`) to identify the tenant.

## Related

- See other Shared API endpoints

## Changelog

- 2025-11-19: Updated to reflect individual domain health check routes and removed global aggregator.
- 2025-10-16: Initial documentation
