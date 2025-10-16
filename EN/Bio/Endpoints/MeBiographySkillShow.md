# Bio – My Biography Skills Show

## Endpoint

```
GET /api/v1/bio/me/biography-skills/{uuid}
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

- [My Biography Skills — Index](MyBiographySkillsIndex.md)
- [My Biography Skills — Update](MyBiographySkillsUpdate.md)
