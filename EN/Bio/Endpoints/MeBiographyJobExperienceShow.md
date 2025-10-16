# Bio – My Job Experiences Show

## Endpoint

```
GET /api/v1/bio/me/job-experiences/{uuid}
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Resource UUID. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found
- 500: Internal Server Error

## Related

- [My Job Experiences — Index](MyJobExperiencesIndex.md)
- [My Job Experiences — Update](MyJobExperiencesUpdate.md)
