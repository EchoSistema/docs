<!-- markdownlint-disable MD013 -->

# User – Register User

## Endpoint

`POST /api/v1/auth/register`

Registers a new user on the platform and returns a token plus user context.

---

## Authentication

None. Still, the `X-PUBLIC-KEY` header must be sent.

---

## Request

### Required Headers

| Header | Type | Description |
| ------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Platform public key required to authenticate and receive responses. |
| **Accept-Language** | `string` | Language for error messages (e.g., `en`). Optional. |

### Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `collaborator` | `boolean` | No | Indicates if the user is a collaborator. |
| `device` | `string` | Yes | Device used for the registration. |
| `name` | `string` | Yes | User name. |
| `platform` | `mixed` | No | Associated platform. |
| `gender` | `UserGenderEnum` | Conditional | Required with `collaborator`. |
| `birth_date` | `date` | Conditional | Required with `collaborator`. |
| `language` | `LanguageEnum` | No | User preferred language. |
| `currency` | `CurrencyEnum` | No | User currency. |
| `email` | `string` | Yes | Valid email unique per platform. |
| `password` | `string` | Yes | Password (minimum 8 characters). |
| `password_confirmation` | `string` | Yes | Must match `password`. |
| `roles` | `array` | Conditional | User roles; required with `collaborator`. |
| `roles.*` | `RoleEnum` | Conditional | Each role must be valid. |
| `nationalities` | `array` | Conditional | User nationalities; required with `collaborator`. |
| `address` | `array` | Conditional | User address; required with `collaborator`. |
| `address.city` | `string` | No | City. |
| `address.state` | `string` | No | State. |
| `address.country` | `string` | No | Country. |
| `address.city_id` | `integer` | No | City ID. |
| `address.state_id` | `integer` | No | State ID. |
| `address.country_id` | `integer` | Conditional | Country ID; required with `collaborator`. |
| `address.zipcode` | `string` | No | Zip code. |
| `address.address_one` | `string` | No | Address line 1. |
| `address.address_two` | `string` | No | Address line 2. |
| `address.address_three` | `string` | No | Address line 3. |
| `address.address_four` | `string` | No | Address line 4. |
| `address.addressable` | `string` | No | Additional reference. |
| `address.type` | `AddressTypeEnum` | No | Address type. |
| `contacts` | `array` | Conditional | User contacts; required with `collaborator`. |
| `contacts.*.type` | `ContactTypeEnum` | Yes | Contact type. |
| `contacts.*.value` | `string` | No | Contact value (e.g., email). |
| `contacts.*.country_code` | `string` | No | Country code. |
| `contacts.*.number` | `string` | No | Phone number. |
| `contacts.*.contactable` | `string` | Yes | Contact reference. |

### Body Example

```json
{
  "collaborator": true,
  "device": "web",
  "name": "User Example",
  "gender": "male",
  "birth_date": "1990-01-01",
  "language": "en",
  "currency": "USD",
  "email": "user@example.com",
  "password": "StrongPass123",
  "password_confirmation": "StrongPass123",
  "roles": ["guest"],
  "nationalities": ["USA"],
  "address": {
    "city": "New York",
    "state": "NY",
    "country": "United States",
    "country_id": 840,
    "zipcode": "00000",
    "address_one": "Example Street",
    "type": "residential"
  },
  "contacts": [
    {
      "type": "email",
      "value": "user@example.com",
      "contactable": "user"
    }
  ]
}
```

---

## Responses

### Success `200 OK`

```json
{
  "message": "User Example registered successfully.",
  "token": "0000|exampleToken",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e1c1h1o1111111111111111111111111111",
      "name": "User Example",
      "email": "user@example.com",
      "avatar": {
        "url": "https://storage.example.com/avatar_thumbnail.webp",
        "usage": "avatar"
      },
      "language": "en",
      "currency": "USD",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "11111111-2222-3333-4444-555555555555",
            "name": "Example Platform",
            "public_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
          },
          "name": "guest",
          "localized_name": "Guest",
          "permissions": [
            {
              "subject": "complaint",
              "action": "store"
            }
          ]
        }
      ]
    }
  }
}
```

### Error `400 Bad Request`

Returns an object with validation error messages.

---

<!-- markdownlint-enable MD013 -->
## Notes

- The `recently_created` flag is present on registration responses.
- The `device` field is used as the token name.
- To avoid issuing a token (for diagnostics), send `no_auth=true` in the request (query or body).
