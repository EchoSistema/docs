# Bio – My Job Experiences Batch Destroy

## Endpoint

```
DELETE /api/v1/bio/me/job-experiences
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Request body

```json
{
  "job_experiences": ["uuid1", "uuid2"]
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Related

- [My Job Experiences — Index](MyJobExperiencesIndex.md)
