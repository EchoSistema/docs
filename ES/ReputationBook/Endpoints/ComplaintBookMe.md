# ReputationBook – Empresa vinculada a la plataforma

## Endpoint

```
GET /api/v1/complaint-book/me
```

> Esta ruta pertenece al grupo de backoffice (`ability: backoffice,domain:reputationbook`). Aunque comparte el camino con el endpoint público, la autorización garantiza que solo los colaboradores de la plataforma accedan a esta versión.

## Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

## Parámetros de consulta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| `platform_id` | integer | Sí | ID interno de la plataforma. |
| `rel` | string | No | Lista separada por comas de relaciones extra para cargar (`addresses`, `contacts`, `documents`, ...). El controller convierte los valores a camelCase. |

## Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
    "name": "Defensa del Consumidor",
    "slug": "defensa-del-consumidor",
    "is_governamental": true,
    "platform": {
      "email": "contacto@plataforma.gov",
      "favicon": "https://cdn.assets/favicon.ico",
      "logo": {
        "main": "https://cdn.assets/logo.png",
        "square": "https://cdn.assets/logo-square.png",
        "rounded": "https://cdn.assets/logo-rounded.png",
        "rectangular": "https://cdn.assets/logo-rect.png"
      },
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Defensa del Consumidor"
      }
    },
    "addresses": [
      {
        "type": "headquarters",
        "zipcode": "70000-000",
        "address_one": "Esplanada, bloque A",
        "city": { "id": 1, "name": "Brasilia" },
        "state": { "id": 7, "name": "Distrito Federal" },
        "country": { "id": 33, "name": "Brasil" }
      }
    ],
    "contacts": [
      {
        "id": 10,
        "type": "telephone",
        "country_code": "+55",
        "number": "6130000000",
        "full_number": "+556130000000"
      }
    ],
    "documents": [
      {
        "uuid": "b3d2e698-4a6e-41d3-9a50-288e7b2c506b",
        "type": "ruc",
        "value": "80012345-6",
        "expires_at": null
      }
    ],
    "coverages": [
      {
        "type": "city",
        "values": ["Encarnación", "Posadas"]
      }
    ]
  }
}
```

## Códigos HTTP

- 200: Empresa vinculada a la plataforma retornada.
- 400/422: Parámetros faltantes (por ejemplo, `platform_id`).
- 404: La plataforma no tiene empresa asociada.

