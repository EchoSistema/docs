# Real Estate — Agencies: List

## Endpoint

```
GET /real-estate/agencies
```

## Authentication

Not required.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | Localizes textual fields when available. |

## Query Parameters

| Parameter | Type   | Required | Description | Default |
| --------- | ------ | -------- | ----------- | ------- |
| public_key | string | Yes      | Platform public key (`pbk`). |
| per_page   | int    | No       | Page size (1–200). | 9 |
| page       | int    | No       | Page index. | 1 |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/agencies?public_key=realestate-demo&per_page=9"
```

### Response (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "_id": "68f252d365a4bd9f7c0321d2",
      "uuid": "363e4540-ae9d-356e-b6a3-06988311335c",
      "pbk": "88dc019b-6856-4115-a287-5f641e45c1a6",
      "name": "Fenicia Inmobiliaria",
      "email": "contacto@fenicia-inmobiliaria.com.py",
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/fenicia-inmobiliaria.png",
      "phones": ["+595 71 201 501", "+595 981 201 701"],
      "masked_phones": ["+595 71 201 501", "+595 981 201 701"],
      "website": "https://fenicia-inmobiliaria.com.py",
      "socials": [
        "https://www.facebook.com/fenicia-inmobiliaria",
        "https://www.instagram.com/fenicia-inmobiliaria",
        "https://www.linkedin.com/company/fenicia-inmobiliaria"
      ],
      "role": "agency",
      "properties": 11,
      "agents": 4,
      "documents": {
        "ruc": "80000001-1",
        "value": "Fenicia Inmobiliaria S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "127",
        "neighborhood": "Centro",
        "city": "Encarnación",
        "state": "Itapúa",
        "country": "Paraguay"
      },
      "responsible": {
        "uuid": "06b3ad57-869b-4552-9e01-7282c57d76d3",
        "name": "Responsable Fenicia Inmobiliaria"
      },
      "created_at": "2025-10-17T14:29:39.032Z",
      "updated_at": "2025-10-17T14:29:38.968Z"
    }
  ],
  "first_page_url": "https://api.example.com/real-estate/agencies?page=1",
  "from": 1,
  "last_page": 5,
  "last_page_url": "https://api.example.com/real-estate/agencies?page=5",
  "links": [],
  "next_page_url": "https://api.example.com/real-estate/agencies?page=2",
  "path": "https://api.example.com/real-estate/agencies",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 42
}
```

## Status Codes

- 200 — Success
- 400 — Invalid pagination parameters
- 404 — Platform key not found

## Notes

- Every result belongs to the provided `public_key`.
- Fields are stored in MongoDB and may vary per deployment.
- The `responsible` field contains information about the agency's responsible person.
- The `documents` field contains legal documentation (RUC for Paraguay).
- The `address_full` object provides complete address details.

## Changelog

- 2025-10-17 — Initial documentation.