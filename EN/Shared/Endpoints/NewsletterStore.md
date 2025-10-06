# Newsletter — Subscribe

## Endpoint

`POST /api/v1/newsletters`

Receives public newsletter subscription requests and stores the records associated with the provided platform.

---

## Authentication

No Bearer token. Requires the platform header to identify the tenant.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| `X-PUBLIC-KEY` | `string` | Platform public identifier. |

### Optional Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| `Accept-Language` | `string` | Sets the locale; used to fill `language`. |

---

## Body (JSON)
| Field            | Type     | Req. | Description |
| ---------------- | -------- | ---- | ----------- |
| `name`           | `string` | No   | Subscriber name. Optional. |
| `email`          | `email`  | Yes  | Subscriber email address. |
| `g_recaptcha`    | `string` | Yes  | reCAPTCHA token validated by `ReCaptchaRule`. |
| `platform_uuid`  | `uuid`   | Yes  | UUID of the platform that receives the subscription. |
| `is_active`      | `bool`   | Auto | Forced to `true` by the backend; incoming values are ignored. |
| `language`       | `string` | Auto | Derived from the current locale (`Accept-Language` or app default). |

### Request Example
```json
{
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "g_recaptcha": "token-recaptcha",
  "platform_uuid": "11111111-1111-1111-1111-111111111111"
}
```

---

## Responses

### Success — 201 Created
```json
{
  "message": "Email saved successfully"
}
```

### Validation Error — 422 Unprocessable Entity
```json
{
  "message": "Email not saved",
  "errors": {
    "email": ["The email field is required."],
    "g_recaptcha": ["Failed to validate reCAPTCHA."]
  }
}
```

### Robot Block — 402 Payment Required
Returned when the identified user agent matches a bot.

---

## Notes
- Route name: `api.v1.newsletters.store`.
- Each subscription triggers a Slack notification defined at `config('backoffice.slack.channel.default')`.
- Additional attributes (`language`, `is_active`) are computed internally through `NewsletterStoreRequest`.
- Records are persisted by `Newsletter::createWithAttributes` after preparation via `PrepareNewsletterData`.
