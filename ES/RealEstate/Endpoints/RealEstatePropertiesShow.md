# Bienes Raíces — Propiedades: Detalle

Devuelve el documento bruto de la propiedad almacenado en MongoDB y conserva el
envoltorio estándar `{ data: ... }` utilizado por los endpoints de RealEstate.

## Endpoint

```
GET /real-estate/properties/{property}
```

## Autenticación

No es necesaria.

## Parámetros de Ruta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| property  | uuid | Sí          | UUID de la propiedad (binding por `uuid`). |

## Parámetros de Consulta

| Parámetro  | Tipo   | Obligatorio | Descripción |
| ---------- | ------ | ----------- | ----------- |
| public_key | string | No          | Delimita los datos a la plataforma correcta. |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/properties/e2c9c1c6-4cac-4c41-8f8e-6fd79d303001"
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "e2c9c1c6-4cac-4c41-8f8e-6fd79d303001",
    "title": "Ático con vista al mar",
    "city": "Barcelona",
    "region": "Cataluña",
    "country": "ES",
    "value": {
      "amount": 875000,
      "currency": "EUR"
    },
    "rentType": "sale",
    "propertyType": "penthouse",
    "rooms": 7,
    "bed": 4,
    "bath": 3,
    "garage": 2,
    "size": 215,
    "isFeatured": true,
    "directFromOwner": false,
    "createdAt": "2025-08-13T12:45:11Z",
    "details": {
      "highlights": ["Terraza panorámica", "Vista al mar"],
      "tags": ["lujo", "exclusivo"]
    }
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 404 — Propiedad no encontrada
- 500 — Error interno

## Notas

- Se devuelve el documento de MongoDB (`properties`) encapsulado dentro de
  `data`.
- Incluir `public_key` evita mostrar datos de otras plataformas.

## Changelog

- 2025-09-25 — Documentación inicial.
