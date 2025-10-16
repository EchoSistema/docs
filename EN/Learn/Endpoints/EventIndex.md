# Learn – List Events

## Endpoint

```
GET /api/v1/learn/events
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

### Query parameters

| Parameter    | Type    | Required | Description | Default/Values |
| ------------ | ------- | -------- | ----------- | -------------- |
| language     | string  | No       | Language code for localized titles | Platform default |
| currency     | string  | No       | Currency code for prices (e.g., USD, BRL, EUR) | Platform default |
| per_page     | integer | No       | Items per page (pagination) | 10 |
| page         | integer | No       | Page number | 1 |
| no_paginate  | boolean | No       | Return all results without pagination | false |
| limit        | integer | No       | Maximum items to return (when no_paginate=true) | No limit |

> Additional filter parameters are available through EventFilter. The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events?language=en&currency=USD&per_page=10&page=1"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "880e8400-e29b-41d4-a716-446655440003",
      "resource": "event-resource-name",
      "contents_count": 12,
      "team_coordinators": [
        {
          "user_basic_data": {
            "uuid": "990e8400-e29b-41d4-a716-446655440004",
            "name": "John Doe",
            "images": [
              {
                "usage": "profile",
                "url": "https://example.com/profile.jpg"
              }
            ]
          }
        }
      ],
      "regional_information": {
        "country": "US",
        "state": "CA"
      },
      "card": {
        "title": "Event Card Title",
        "description": "Event card description"
      },
      "titles": [
        {
          "language": "en",
          "value": "Event Title",
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
        "titles": [
          {
            "language": "en",
            "value": "Special Offer"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/learn/events?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/learn/events?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/learn/events?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.your-domain.com/api/v1/learn/events",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description |
| ------------------------------ | ------- | ----------- |
| data[]                         | array   | List of events |
| data[].uuid                    | string  | Event unique identifier |
| data[].resource                | string  | Event resource name |
| data[].contents_count          | integer | Number of contents in the event |
| data[].team_coordinators[]     | array   | List of team coordinators |
| data[].regional_information    | object  | Regional information |
| data[].card                    | object  | Card display information |
| data[].titles[]                | array   | Localized titles |
| data[].prices[]                | array   | Prices in different currencies |
| data[].offer                   | object  | Active offer information |
| links                          | object  | Pagination links |
| meta                           | object  | Pagination metadata |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Invalid platform key",
  "errors": {
    "per_page": ["The per page must be an integer."]
  }
}
```

## Notes

- Returns all events for the authenticated platform
- Results are paginated by default with 10 items per page
- Use `no_paginate=true` to retrieve all results at once (use with caution)
- Titles and prices are localized based on the `language` and `currency` parameters
- If localized content is not available, default values are returned
- The response includes team coordinators with their profile images and biographies
- Active offers are included in the response when available

## Related

- [Show Event](./EventShow.md)
- [Get Event Counter](./EventCounter.md)
- [List Event Contents](./EventContentIndex.md)

## Changelog

- 2025-10-16: Initial documentation
