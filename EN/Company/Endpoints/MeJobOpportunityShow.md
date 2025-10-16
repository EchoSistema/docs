# Company – Show Job Opportunity Details

## Endpoint

```
GET /api/v1/company/opportunities/{jobOpportunity}
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter       | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| jobOpportunity  | string | Yes      | Job opportunity UUID. |

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| language  | string | No       | Language code for translatable fields (e.g., `en`, `pt-BR`, `es`). | Platform default |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company/opportunities/00000000-0000-0000-0000-000000000001?language=en"
```

### Response example

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "company": {
      "name": "Tech Solutions Inc.",
      "country": "Paraguay",
      "state": "Central",
      "city": "Asunción"
    },
    "total_job_openings": 3,
    "title": "Senior Software Developer",
    "mode": "remote",
    "description": "We are looking for an experienced software developer to join our team and work on cutting-edge projects using modern technologies.",
    "education_required": "bachelor",
    "required_skills": [
      "PHP",
      "Laravel",
      "JavaScript",
      "Vue.js",
      "Docker"
    ],
    "required_experience": [
      {
        "area": "Software Development",
        "years": 5
      }
    ],
    "occupation_areas_scope": [
      {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "title": "Information Technology"
      }
    ],
    "languages": [
      {
        "language": "English",
        "level": "advanced"
      },
      {
        "language": "Spanish",
        "level": "intermediate"
      }
    ],
    "salary_range": {
      "from": "$5,000.00",
      "to": "$8,000.00"
    },
    "country": "Paraguay",
    "state": "Central",
    "city": "Asunción",
    "starts_at": "2025-10-20T00:00:00+00:00",
    "finishes_at": "2025-11-30T23:59:59+00:00"
  }
}
```

## JSON Structure Explained

| Field                               | Type     | Description |
| ----------------------------------- | -------- | ----------- |
| data                                | object   | Job opportunity resource. |
| data.uuid                           | string   | Job opportunity UUID. |
| data.company                        | object   | Company information (only when not owner). |
| data.company.name                   | string   | Company name. |
| data.company.country                | string   | Company country. |
| data.company.state                  | string   | Company state. |
| data.company.city                   | string   | Company city. |
| data.total_job_openings             | integer  | Number of open positions. |
| data.title                          | string   | Job title in requested language. |
| data.mode                           | string   | Work mode: `remote`, `hybrid`, `on-site`. |
| data.description                    | string   | Job description in requested language. |
| data.education_required             | string   | Required education level. |
| data.required_skills[]              | array    | List of required skills. |
| data.required_experience[]          | array    | List of required experience. |
| data.required_experience[].area     | string   | Experience area. |
| data.required_experience[].years    | integer  | Years of experience required. |
| data.occupation_areas_scope[]       | array    | Relevant occupation areas. |
| data.occupation_areas_scope[].uuid  | string   | Occupation area UUID. |
| data.occupation_areas_scope[].title | string   | Occupation area title. |
| data.languages[]                    | array    | Required languages. |
| data.languages[].language           | string   | Language name. |
| data.languages[].level              | string   | Proficiency level. |
| data.salary_range                   | object   | Salary range information. |
| data.salary_range.from              | string   | Minimum salary (formatted). |
| data.salary_range.to                | string   | Maximum salary (formatted). |
| data.country                        | string   | Job location country. |
| data.state                          | string   | Job location state. |
| data.city                           | string   | Job location city. |
| data.starts_at                      | datetime | Opportunity start date. |
| data.finishes_at                    | datetime | Opportunity end date. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Job opportunity not found.",
  "errors": {}
}
```

## Notes

- This endpoint returns detailed information about a specific job opportunity.
- The job opportunity must belong to the company associated with the current platform.
- The `company` field is only included when the request is not from the owner.
- Salary ranges are formatted according to the requested language and currency.
- All translatable fields (title, description) are returned in the requested language, falling back to the default language if unavailable.
- The endpoint validates that the job opportunity belongs to the company before returning the data.

## Related

- [Job Opportunity Index](./MeJobOpportunityIndex.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Initial documentation.
