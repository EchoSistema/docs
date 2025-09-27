# ReputationBook – Create Complaint Reply

## Endpoint

```
POST /api/v1/complaints/{complaint}/replies
```

Creates a new reply for the complaint identified by the `complaint` path parameter.

---

## Authentication

Required – `Bearer {token}` authenticated by `auth:sanctum`. No additional ability is enforced.

---

## Headers

| Header | Type | Required | Description | Values |
| ------ | ---- | -------- | ----------- | ------- |
| `Authorization` | string | Yes | `Bearer {token}` credential. | — |
| `X-PUBLIC-KEY` | string | Yes | Identifies the issuing platform. | — |
| `Accept-Language` | string | Recommended | Client locale (`pt-BR`, `en`, `es`). | — |

---

## Parameters

### Path Parameters

| Parameter | Type | Required | Description | Values |
| --------- | ---- | -------- | ----------- | ------- |
| `complaint` | string | Yes | Complaint identifier (accepts numeric ID or UUID). | — |

### Request Body (JSON)

| Field | Type | Required | Description | Default/Values |
| ----- | ---- | -------- | ----------- | -------------- |
| `user` | string | Yes | Reference to the author of the reply. Accepts numeric ID, UUID, or registered email. | — |
| `company` | string | No | Company identifier when the reply is issued by it. Accepts ID, UUID, or slug. | — |
| `text` | string | Yes | Main reply content (5–5000 characters). | — |
| `language` | string | Yes | Reply language in IETF format (e.g., `pt-BR`). | `app.locale` |
| `is_platform_support` | boolean | No | When `true`, marks the reply as created by platform support. | `false` |

> Field names are canonical in `snake_case`, but `camelCase`, `kebab-case`, and `CapitalCase` are also accepted.

---

## Examples

### Request (curl)

```bash
curl -X POST "https://api.sandbox.echosistema.com/api/v1/complaints/0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5/replies" \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-client-id>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "user": "customer@example.com",
    "text": "We are reviewing your case and will follow up shortly.",
    "language": "en",
    "is_platform_support": false
  }'
```

### Response (200)

```json
{
  "data": {
    "uuid": "4f0d5906-8aa5-3f44-8eb8-0b2936ccaf8c",
    "index": 2,
    "is_support": false,
    "content": {
      "content": "We are reviewing your case and will follow up shortly.",
      "language": "en"
    },
    "user": {
      "uuid": "b02527ac-47f4-4c0e-9157-1f917bf923c7",
      "name": "Mary Smith"
    },
    "company": {
      "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
      "name": "Sample Store"
    },
    "platform": {
      "uuid": "8b3dd5bb-1d6b-48ce-84b4-3097f3d5343f",
      "name": "EchoSistema"
    }
  }
}
```

---

## Explained JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data.uuid` | uuid | Identifier of the created reply. |
| `data.index` | integer | Reply order within the complaint. |
| `data.is_support` | boolean | Indicates whether the reply is flagged as platform support. |
| `data.content.content` | string | Reply text. |
| `data.content.language` | string | Language associated with the default text. |
| `data.user.uuid` | uuid | Identifier of the author. |
| `data.user.name` | string | Name of the author. |
| `data.company.uuid` | uuid | (Optional) Company linked to the reply. |
| `data.company.name` | string | (Optional) Company name. |
| `data.platform.uuid` | uuid | (Optional) Platform tied to the complaint. |
| `data.platform.name` | string | (Optional) Platform name. |

---

## HTTP Status Codes

- 200: Reply created successfully.
- 400: Complaint, user, or related resource could not be found.
- 401: Token missing or invalid.
- 404: Complaint not found for the supplied identifier.
- 422: Validation error in the submitted fields.
- 500: Unexpected server error.

---

## Errors

```json
{
  "message": "user not found"
}
```

---

## Notes

- The complaint identifier accepts numeric IDs and UUIDs thanks to dynamic binding.
- When `is_platform_support=false` and the complaint has an associated platform, the reply is automatically marked as support (`is_support=true`).
- The index is calculated automatically based on existing replies for the complaint.
- Optional relationships such as `company` and `platform` are only present when the data is loaded and valid.

---

## Related

- [Domain overview](../README.md)
- [ReputationBook endpoint index](README.md)

---

## Changelog

- 2025-09-26: Initial documentation for the complaint reply creation endpoint.

