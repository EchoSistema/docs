# Shared – Show User Details

## Endpoint

```
GET /api/v1/backoffice/users/{user}
```

## Authentication

Required – Bearer {token} with `backoffice` ability

## Headers

| Header        | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## URL Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| user      | string | Yes      | User UUID, Echo UUID, or ID to display |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Response example

```json
{
  "data": {
    "id": 1234,
    "echo_uuid": "echo_9d4e1c2a3b4c5d6e",
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "name": "John Doe",
    "gender": {
      "symbol": "M",
      "name": "Male"
    },
    "age": 32,
    "birth_date": "1992-05-15T00:00:00Z",
    "email": "john.doe@example.com",
    "avatar": "https://cdn.example.com/avatars/john-doe.jpg",
    "created_at": "2024-01-15T10:30:00Z",
    "roles": [
      {
        "id": 5,
        "main": true,
        "platform": "E-commerce Platform",
        "platform_uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "domain": "E-commerce",
        "role": "Admin",
        "language": "en",
        "currency": "USD",
        "status": "active",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "id": 7,
        "main": false,
        "platform": "Articles Platform",
        "platform_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "domain": "Articles",
        "role": "Editor",
        "language": "en",
        "currency": "USD",
        "status": "active",
        "created_at": "2024-02-20T14:15:00Z"
      }
    ],
    "updated_at": "2024-10-15T08:20:00Z",
    "language": "en",
    "currency": "USD",
    "telephone": "+15551234567",
    "slug": "john-doe",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "email_verified_at": "2024-01-15T11:00:00Z",
    "platform": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "E-commerce Platform",
      "domain_area": "E-commerce"
    },
    "contacts": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "mobile",
        "country_code": "+1",
        "number": "5551234567",
        "phone": "+15551234567",
        "email": null,
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "type": "email",
        "country_code": null,
        "number": null,
        "phone": null,
        "email": "john.doe.alt@example.com",
        "created_at": "2024-02-10T14:20:00Z"
      }
    ],
    "social_medias": [
      {
        "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
        "name": "LinkedIn",
        "url": "https://linkedin.com/in/johndoe",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "3a8b5c6d-7e8f-9g0h-1i2j-3k4l5m6n7o8p",
        "name": "Twitter",
        "url": "https://twitter.com/johndoe",
        "created_at": "2024-01-15T10:30:00Z"
      }
    ],
    "address": {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "zipcode": "10001",
      "street": "5th Avenue",
      "number": "123",
      "complement": "Apt 501",
      "neighborhood": "Manhattan",
      "city": "New York",
      "state": "New York",
      "country": "United States",
      "formatted": "123 5th Avenue, Apt 501 - Manhattan, New York - NY, United States, 10001"
    },
    "nationalities": [
      {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "country": "United States"
      }
    ],
    "biography": {
      "uuid": "0a5b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "summary": "Experienced e-commerce management professional with over 10 years of experience."
    },
    "job_occupation": {
      "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
      "occupation": "E-commerce Manager",
      "company": "Tech Solutions Inc",
      "is_default": true
    },
    "job_occupations": [
      {
        "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
        "occupation": "E-commerce Manager",
        "company": "Tech Solutions Inc",
        "is_default": true,
        "started_at": "2020-01-15T00:00:00Z",
        "ended_at": null
      },
      {
        "uuid": "8a3b0c1d-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "occupation": "Marketing Analyst",
        "company": "Marketing Corp",
        "is_default": false,
        "started_at": "2017-03-01T00:00:00Z",
        "ended_at": "2019-12-31T00:00:00Z"
      }
    ],
    "affiliate": {
      "uuid": "7a2b9c0d-1e2f-3g4h-5i6j-7k8l9m0n1o2p",
      "token": "aff_tkn_123456789abcdef",
      "created_at": "2024-01-15T10:30:00Z"
    },
    "identities": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "ssn",
        "number": "***-**-1234",
        "verified_at": "2024-01-15T11:00:00Z"
      }
    ]
  }
}
```

## JSON Structure Explained

### Base Fields (always present)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | object  | User data |
| data.id     | integer | Internal user ID |
| data.echo_uuid | string  | EchoSistema UUID (format: echo_*) |
| data.uuid   | string  | Unique user identifier |
| data.name   | string  | User's full name |
| data.gender | object  | Gender information |
| data.gender.symbol | string  | Gender symbol (M, F, O) |
| data.gender.name | string  | Gender name in full |
| data.age    | integer | Calculated user age |
| data.birth_date | string  | Birth date (ISO 8601) |
| data.email  | string  | User's email |
| data.avatar | string  | Avatar image URL |
| data.created_at | string  | Account creation date (ISO 8601) |
| data.roles  | array   | List of user roles/functions across different platforms |

### Additional Fields (detailed mode)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.updated_at | string  | Last update date (ISO 8601) |
| data.language | string  | User's preferred language (IETF locale) |
| data.currency | string  | Preferred currency (ISO code) |
| data.telephone | string  | Formatted primary phone number |
| data.slug   | string  | User's unique slug |
| data.is_banned | boolean | Indicates if user is banned |
| data.is_foreign | boolean | Indicates if user is foreign |
| data.is_master | boolean | Indicates if user has master privileges |
| data.email_verified_at | string  | Email verification date (ISO 8601) |

### Platform (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.platform | object  | Main platform data |
| data.platform.uuid | string  | Platform UUID |
| data.platform.name | string  | Platform name |
| data.platform.domain_area | string  | Platform domain area |

### Contacts (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.contacts | array   | List of user contacts |
| data.contacts[].uuid | string  | Contact UUID |
| data.contacts[].type | string  | Contact type (mobile, email, phone, etc.) |
| data.contacts[].country_code | string  | Country code (e.g., +1) |
| data.contacts[].number | string  | Number without country code |
| data.contacts[].phone | string  | Complete formatted phone |
| data.contacts[].email | string  | Email (when type is email) |
| data.contacts[].created_at | string  | Creation date (ISO 8601) |

### Social Media (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.social_medias | array   | List of social media profiles |
| data.social_medias[].uuid | string  | Social media UUID |
| data.social_medias[].name | string  | Social media platform name |
| data.social_medias[].url | string  | Profile URL |
| data.social_medias[].created_at | string  | Creation date (ISO 8601) |

### Address (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.address | object  | User's address |
| data.address.uuid | string  | Address UUID |
| data.address.zipcode | string  | Postal/ZIP code |
| data.address.street | string  | Street name |
| data.address.number | string  | Street number |
| data.address.complement | string  | Complement |
| data.address.neighborhood | string  | Neighborhood |
| data.address.city | string  | City |
| data.address.state | string  | State/Province |
| data.address.country | string  | Country |
| data.address.formatted | string  | Complete formatted address |

### Nationalities (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.nationalities | array   | List of nationalities |
| data.nationalities[].uuid | string  | Nationality UUID |
| data.nationalities[].country | string  | Country name |

### Ban Information (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.banned_info | object  | Ban information (only if user is banned) |
| data.banned_info.reason | string  | Ban reason |
| data.banned_info.banned_at | string  | Ban date (ISO 8601) |
| data.banned_info.until_date | string  | Ban end date (ISO 8601), null if permanent |

### Biography (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.biography | object  | User's biography |
| data.biography.uuid | string  | Biography UUID |
| data.biography.summary | string  | Biography text |

### Job Occupation (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.job_occupation | object  | Current default job occupation |
| data.job_occupation.uuid | string  | Occupation UUID |
| data.job_occupation.occupation | string  | Occupation name |
| data.job_occupation.company | string  | Company name |
| data.job_occupation.is_default | boolean | Indicates if this is the default occupation |

### Job History (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.job_occupations | array   | Complete job occupation history |
| data.job_occupations[].uuid | string  | Occupation UUID |
| data.job_occupations[].occupation | string  | Occupation name |
| data.job_occupations[].company | string  | Company name |
| data.job_occupations[].is_default | boolean | Indicates if this is the default occupation |
| data.job_occupations[].started_at | string  | Start date (ISO 8601) |
| data.job_occupations[].ended_at | string  | End date (ISO 8601), null if current |

### Affiliate (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.affiliate | object  | Affiliate information (only if user is an affiliate) |
| data.affiliate.uuid | string  | Affiliate UUID |
| data.affiliate.token | string  | Affiliate referral token |
| data.affiliate.created_at | string  | Affiliate registration date (ISO 8601) |

### Identity Documents (when available)

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data.identities | array   | List of identity documents |
| data.identities[].uuid | string  | Document UUID |
| data.identities[].type | string  | Document type (cpf, rg, passport, ssn, etc.) |
| data.identities[].number | string  | Document number |
| data.identities[].verified_at | string  | Verification date (ISO 8601) |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad request
- 401: Unauthorized
- 403: Forbidden
- 404: Not found
- 422: Validation error
- 429: Too many requests
- 500: Internal error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- This endpoint returns complete details of a specific user
- Only users with `show.all` backoffice permission can access
- The `{user}` parameter accepts UUID, Echo UUID, or user ID
- The response includes the following loaded relationships:
  - `regionalInformation`: Regional information (language, currency)
  - `identities`: Identity documents
  - `telephone`: Primary phone
  - `avatarImage`: Avatar image
  - `profileImage`: Profile image
  - `platform`: Main platform
  - `platform.domainArea`: Main platform's domain area
  - `platformRoles`: All platform roles
  - `platformRoles.platform`: Role platforms
  - `platformRoles.platform.domainArea`: Platform domain areas
  - `platformRoles.role`: Role details
  - `contacts`: User contacts
  - `socialMedias`: Social media profiles
  - `address`: Complete address with city, state, and country
  - `nationalities`: Nationalities
  - `banned`: Ban information (if applicable)
  - `biography`: Biography
  - `jobOccupation`: Current default job occupation
  - `jobOccupations`: Complete job history
  - `affiliate`: Affiliate data (if applicable)
- Conditional fields only appear when relationships are loaded and have data
- The `avatar` field is dynamically generated through the `getAvatar()` method
- The `age` field is automatically calculated from birth date

## Related

- [List All Users](BackofficePlatformUserIndex.md)
- [List Platforms](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Complete documentation with all fields and detailed relationships
- 2025-10-16: Initial documentation
