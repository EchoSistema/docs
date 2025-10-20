# Shared – Update My Profile

## Endpoint

`PUT /api/v1/me/profile`

## Authentication

Requires a valid Bearer token.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for validation messages. |

## Body (application/json)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | No | Full name (max 255). |
| `gender` | `string` | No | Accepts `m`/`f`/`o` or normalized values `male`/`female`/`other`. |
| `birth_date` | `date` | No | Birth date (ISO 8601). |
| `email` | `email` | No | Email address (max 255). |

Notes
- The server normalizes `gender`: `male`→`m`, `female`→`f`, others→`o`.
- Only the provided fields are updated; unspecified fields remain unchanged.

## Example Request

```json
{
  "name": "John Updated",
  "gender": "male",
  "birth_date": "1990-05-12",
  "email": "john.updated@example.com"
}
```

## Success `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "encoded_uuid",
    "name": "John Updated",
    "email": "john.updated@example.com",
    "email_verified_at": null,
    "slug": "john-doe",
    "avatar": {
      "url": "https://cdn.example.com/users/john/avatar.webp",
      "usage": "avatar"
    },
    "profile_image": "https://cdn.example.com/users/john/profile.webp",
    "age": 35,
    "gender": "m",
    "gender_name": "male",
    "birthday": "1990-05-12T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "en",
    "currency": "USD",
    "created_at": "2024-01-01T12:00:00+00:00",
    "roles": [
      {
        "id": 9,
        "uuid": "role-guest-uuid",
        "name": "guest",
        "localized_name": "Guest",
        "permissions": ["store.complaint"]
      }
    ],
    "telephone": "+1 555-0100",
    "contacts": {
      "email": {"uuid": "c1", "name": "email", "email": "john.updated@example.com", "phone": null},
      "telephone": {"uuid": "c2", "name": "telephone", "phone": "+1 555-0100", "email": null}
    },
    "social_medias": {
      "linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/john"}
    },
    "address": {
      "uuid": "addr-uuid",
      "main": true,
      "zipcode": "10001",
      "one": "5th Avenue",
      "two": null,
      "three": null,
      "four": null,
      "city": {"uuid": "city-uuid", "name": "New York", "abbreviation": "NYC"},
      "state": {"uuid": "state-uuid", "name": "New York", "abbreviation": "NY", "region": "Northeast"},
      "country": {"uuid": "country-uuid", "name": "United States", "official_name": "United States of America", "code": "US"},
      "formatted": "5th Avenue, New York - NY, 10001, United States"
    },
    "nationalities": [
      {"id": 840, "country_id": 840, "country": {"name": "United States", "code": "US", "flag": "🇺🇸"}}
    ],
    "biography": {
      "uuid": "bio-uuid",
      "name": "John Updated",
      "gender": "male",
      "summary": "Software developer...",
      "job": "Senior Engineer",
      "education": "BSc Computer Science",
      "languages": ["en"],
      "nationalities": ["US"],
      "public_email": "john.public@example.com",
      "image": "https://cdn.example.com/users/john/profile.webp",
      "residence_country": {"name": "United States", "code": "US", "flag": "🇺🇸"},
      "slug": "john-doe",
      "birth_date": "1990-05-12",
      "age": 35,
      "created_at": "2024-01-01T12:00:00+00:00"
    },
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": {"uuid": "aff-uuid", "token": "aff-token", "is_active": true, "transactions_in": 3, "transactions_out": 1},
    "identities": [
      {"type": "CPF", "type_id": 1, "value": "***.***.***-**", "is_checked": false}
    ],
    "raw": null
  }
}
```

## JSON Structure (highlights)

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data.uuid` | `uuid` | User identifier. |
| `data.echo_uuid` | `string` | Internal encoded UUID. |
| `data.name` | `string` | Full name. |
| `data.email` | `email` | Email address. |
| `data.email_verified_at` | `datetime|null` | Email verification time. |
| `data.avatar.url` | `string|null` | Avatar URL. |
| `data.profile_image` | `string|null` | Profile image URL. |
| `data.gender` | `string` | `m`/`f`/`o`. |
| `data.gender_name` | `string` | Localized gender. |
| `data.birthday` | `datetime|null` | Birth date. |
| `data.roles[].permissions[]` | `string` | Permission keys. |
| `data.contacts` | `object` | Contacts keyed by type. |
| `data.address` | `object|null` | Address resource. |
| `data.biography` | `object|null` | Biography resource. |
| `data.platform` | `object|null` | Platform summary. |
| `data.affiliate` | `object|null` | Affiliate info. |
| `data.identities[]` | `array` | Identity documents. |

## Errors

- 401 Unauthorized — missing/invalid token
- 422 Unprocessable Entity — validation errors
- 500 Internal Server Error

### 422 Example

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": ["The email must be a valid email address."],
    "gender": ["The selected gender is invalid."],
    "birth_date": ["The birth date is not a valid date."]
  }
}
```

## Related

- [Get User Profile](./UserProfile.md)
- [Update User Language](./UserLanguageUpdate.md)
- [Update User Currency](./UserCurrencyUpdate.md)
