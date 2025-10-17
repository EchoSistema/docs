# Real Estate — Agencies: Detail

## Endpoint

```
GET /real-estate/agencies/{agency}
```

## Authentication

Not required.

## Path Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| agency    | uuid | Yes      | Agency UUID (`agencies.uuid`). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/agencies/363e4540-ae9d-356e-b6a3-06988311335c"
```

### Response (200)

```json
{
  "data": {
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
}
```

## Status Codes

- 200 — Success
- 404 — Agency not found

## Notes

- The document is retrieved from MongoDB (`agencies`).
- No platform key is required because the UUID is unique per tenant.
- The `responsible` field contains information about the agency's responsible person.
- The `documents` field contains legal documentation (RUC for Paraguay).
- The `address_full` object provides complete address details.
- The `properties` field indicates the number of properties managed by the agency.
- The `agents` field indicates the number of agents working for the agency.

## Changelog

- 2025-10-17 — Initial documentation.
