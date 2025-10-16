# Bio – My Job Experiences Store

## Endpoint

```
POST /api/v1/bio/me/job-experiences
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Content-Type     | string | Yes      | `application/json`. |

## HTTP Status

- 201: Created
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Related

- [My Job Experiences — Index](MyJobExperiencesIndex.md)
