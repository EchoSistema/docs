# Shared – Get User Profile

## Endpoint

```
GET /api/v1/me/profile
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

This endpoint has no parameters.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/me/profile"
```

### Response example

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "encoded_uuid",
    "name": "John Doe",
    "email": "john@example.com",
    "slug": "john-doe",
    "avatar": "https://example.com/avatar.jpg",
    "age": 30,
    "gender": "M",
    "gender_name": "Male",
    "language": "en",
    "currency": "USD",
    "roles": [
      {
        "id": 1,
        "uuid": "role-uuid",
        "name": "admin",
        "localized_name": "Administrator",
        "permissions": ["view", "create"]
      }
    ],
    "contacts": {
      "TELEPHONE": {"phone": "+1234567890"}
    },
    "biography": {
      "uuid": "bio-uuid",
      "slug": "john-doe",
      "summary": "Software developer..."
    }
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data | object | User profile data |
| data.uuid | string | User unique identifier |
| data.echo_uuid | string | Encoded EchoSistema UUID |
| data.name | string | User full name |
| data.email | string | User email address |
| data.slug | string\|null | User slug for URL |
| data.avatar | string | Avatar image URL |
| data.profile_image | string\|null | Profile image URL |
| data.age | integer\|null | User age |
| data.gender | string | Gender code (M/F/O) |
| data.gender_name | string | Localized gender name |
| data.birthday | string\|null | Birth date (ISO 8601) |
| data.is_banned | boolean | Whether user is banned |
| data.language | string | User preferred language |
| data.currency | string | User preferred currency |
| data.roles | array\|null | User roles with permissions |
| data.telephone | string\|null | Primary phone number |
| data.contacts | object\|null | All contacts keyed by type |
| data.social_medias | object\|null | Social media links keyed by platform |
| data.address | object\|null | User address details |
| data.nationalities | array | User nationalities |
| data.biography | object\|null | Complete biography resource |
| data.job_occupation | object\|null | Current job occupation |
| data.platform | object\|null | Platform information |
| data.affiliate | object\|null | Affiliate data |
| data.identities | array\|null | User identity documents |
| data.raw | object\|null | Custom platform-specific data |

## HTTP Status

- 200: OK - Profile retrieved successfully
- 401: Unauthorized - Invalid or missing token
- 403: Forbidden - Insufficient permissions
- 404: Not Found - User not found
- 500: Internal Server Error

## Errors

### 401 Unauthorized
```json
{
  "message": "Unauthenticated."
}
```

### 403 Forbidden
```json
{
  "message": "This action is unauthorized."
}
```

## Notes

- Returns the authenticated user's complete profile
- All relationships are eagerly loaded for optimal performance
- Biography data includes nested relationships (educations, skills, certifications, etc.)
- Contacts and social medias are keyed by type/name for easy access
- Use `Accept-Language` header to get localized field values

## Related

- [Update User Language](./UserRegionalInformationLanguageUpdate.md)
- [Update User Currency](./UserRegionalInformationCurrencyUpdate.md)

## Changelog

- 2025-10-16: Expanded profile with biography, contacts, social medias, identities, and affiliate data
