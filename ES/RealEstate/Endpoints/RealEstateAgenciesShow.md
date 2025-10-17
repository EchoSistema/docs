# Bienes Raíces — Inmobiliarias: Detalle

## Endpoint

```
GET /real-estate/agencies/{agency}
```

## Autenticación

No es necesaria.

## Parámetros de Ruta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| agency    | uuid | Sí          | UUID de la inmobiliaria (`agencies.uuid`). |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/agencies/363e4540-ae9d-356e-b6a3-06988311335c"
```

### Respuesta (200)

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

## Códigos de Estado

- 200 — Correcto
- 404 — Inmobiliaria no encontrada

## Notas

- El documento se recupera de MongoDB (`agencies`).
- No se requiere clave de plataforma porque el UUID es único por tenant.
- El campo `responsible` contiene información sobre el responsable de la inmobiliaria.
- El campo `documents` contiene documentación legal (RUC para Paraguay).
- El objeto `address_full` proporciona los detalles completos de la dirección.
- El campo `properties` indica el número de propiedades gestionadas por la inmobiliaria.
- El campo `agents` indica el número de agentes que trabajan para la inmobiliaria.

## Changelog

- 2025-10-17 — Documentación inicial.
