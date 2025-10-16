# Learn – Show Event

## Endpoint

```
GET /api/v1/learn/events/{event}
```

## Authentication

None

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | No       | `Bearer {token}` (optional). |
| X-PUBLIC-KEY    | string | Yes      | Platform public key. |
| Accept-Language | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| event     | string | Yes      | Event resource identifier |

### Query parameters

| Parameter   | Type   | Required | Description | Default/Values |
| ----------- | ------ | -------- | ----------- | -------------- |
| platform_id | string | No       | Platform UUID for ownership validation | Derived from X-PUBLIC-KEY |

> The `event` parameter uses route model binding with the `resource` field. The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/event-resource-name"
```

### Response example

```json
{
  "data": {
    "uuid": "880e8400-e29b-41d4-a716-446655440003",
    "resource": "event-resource-name",
    "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "contents_count": 12,
    "team_coordinators": [
      {
        "user_basic_data": {
          "uuid": "990e8400-e29b-41d4-a716-446655440004",
          "name": "John Doe",
          "email": "john@example.com",
          "images": [
            {
              "usage": "profile",
              "url": "https://example.com/profile.jpg"
            }
          ],
          "biography": {
            "job_title": {
              "title": "Senior Instructor"
            },
            "user": {
              "social_medias": [
                {
                  "platform": "linkedin",
                  "url": "https://linkedin.com/in/johndoe"
                }
              ]
            }
          }
        }
      }
    ],
    "regional_information": {
      "country": "US",
      "state": "CA",
      "city": "San Francisco"
    },
    "card": {
      "title": "Event Card Title",
      "description": "Event card description",
      "image_url": "https://example.com/card.jpg"
    },
    "titles": [
      {
        "language": "en",
        "value": "Complete Web Development Course",
        "is_default": false
      }
    ],
    "prices": [
      {
        "currency_id": "USD",
        "amount": 99.99,
        "is_default": false
      }
    ],
    "offer": {
      "is_active": true,
      "discount_percent": 20,
      "valid_until": "2025-12-31T23:59:59Z",
      "titles": [
        {
          "language": "en",
          "value": "Limited Time Offer"
        }
      ]
    }
  }
}
```

## JSON Structure Explained

| Field                               | Type    | Description |
| ----------------------------------- | ------- | ----------- |
| data                                | object  | Event details |
| data.uuid                           | string  | Event unique identifier |
| data.resource                       | string  | Event resource name |
| data.platform_uuid                  | string  | Associated platform UUID |
| data.contents_count                 | integer | Number of contents in the event |
| data.team_coordinators[]            | array   | List of team coordinators with full details |
| data.team_coordinators[].user_basic_data | object | Coordinator user information |
| data.regional_information           | object  | Regional information |
| data.card                           | object  | Card display information |
| data.titles[]                       | array   | Localized titles |
| data.prices[]                       | array   | Prices in different currencies |
| data.offer                          | object  | Active offer information (when available) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden - Platform does not own this event
- 404: Not Found - Event not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Event not found"
}
```

```json
{
  "message": "This event does not belong to the specified platform"
}
```

## Notes

- Returns detailed information for a specific event
- The event must belong to the platform making the request
- Platform ownership is validated using the `platform_id` parameter or derived from `X-PUBLIC-KEY`
- Includes complete team coordinator information with images and biographies
- Localized content is returned based on the `Accept-Language` header
- The response includes active offers when available

## Related

- [List Events](./EventIndex.md)
- [Get Event Counter](./EventCounter.md)
- [Get Event Text](./EventText.md)
- [List Event Contents](./EventContentIndex.md)

## Changelog

- 2025-10-16: Initial documentation
