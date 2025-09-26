# Real Estate — Rental Properties: Detail

## Endpoint

```
GET /real-estate/rental-properties/{rentalProperty}
```

## Authentication

Requires the active platform to own the requested rental property. No bearer token enforced.

## Path Parameters

| Parameter       | Type | Required | Description |
| --------------- | ---- | -------- | ----------- |
| rentalProperty  | uuid | Yes      | UUID or slug handled by route-model binding (`RentableItem::resolveRouteBinding`). |

## Query Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| language  | string | No       | Locale used to resolve translated fields (`pt-BR`, `en`, `es`, ...). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/rental-properties/c51823c1-8d41-477e-a3b7-7108faae0001?language=en"
```

### Response (200)

```json
{
  "data": {
    "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
    "payment_interval": "monthly",
    "type": {
      "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
      "name": "Furnished Apartment"
    },
    "creator": {
      "uuid": "49287c4a-32aa-483c-8d4f-98cfd8300101",
      "name": "Rental Manager"
    },
    "title": "Studio next to the subway",
    "description": "Fully furnished studio, utilities included.",
    "renter": "Skyline Rentals",
    "renter_type": "company",
    "image": {
      "usage": "card",
      "url": "https://cdn.example.com/rental-properties/c51823c1/card.jpg",
      "name": "cover"
    },
    "images": [
      {
        "usage": "gallery",
        "url": "https://cdn.example.com/rental-properties/c51823c1/gallery-1.jpg",
        "name": "living-room",
        "width": 1920,
        "height": 1080
      }
    ],
    "address": {
      "street": "Av. Paulista, 1000",
      "number": null,
      "district": "Bela Vista",
      "city": "São Paulo",
      "state": "SP",
      "country": "BR",
      "zipcode": "01310-100",
      "formatted": "Av. Paulista, 1000 — Bela Vista, São Paulo/SP"
    },
    "is_available": true
  }
}
```

## Status Codes

- 200 — Success
- 403 — Rental property does not belong to the active platform
- 404 — Rental property not found

## Notes

- The controller ensures the renter matches the platform company (`App::platform()->company`).
- Translated titles/texts respect the `language` query; default content is returned otherwise.
- Images include both the card image and the complete gallery when available.

## Changelog

- 2025-09-25 — Initial documentation.
