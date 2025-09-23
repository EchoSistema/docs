# Shared – Health Endpoint

## Endpoint

`GET /api/v1/health`

Simple healthcheck used by load balancers and monitors.

---

## Authentication

No Bearer token required. Requires platform header.

### Required Headers

| Header | Type | Description |
| ------ | ---- | ----------- |
| `X-PUBLIC-KEY` | `string` | Platform public key required to authenticate and receive responses. |
| `Accept-Language` | `string` | Optional locale for response messages. |

---

## Example Response

```json
{
  "success": true
}
```

---

## Notes

* Route name: `api.v1.health`.
* Missing or invalid `X-PUBLIC-KEY` is rejected by the `platform` middleware (401/403 as configured).
* Localisation: response messages may be localised according to `Accept-Language`.
