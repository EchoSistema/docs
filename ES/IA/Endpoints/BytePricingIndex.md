# IA — Precio por Byte: Índice

## Endpoint

```
GET /ia/admin/pricing/bytes
```

## Autenticación

Requerida — Bearer {token} con Sanctum.

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}` |
| Accept-Language  | string | No        | Locale IETF (`es`, `pt-BR`, `en`, ...) |

## Parámetros

### Parámetros de ruta

Ninguno.

### Parámetros de consulta

| Parámetro | Tipo | Requerido | Descripción | Predeterminado |
| --------- | ---- | --------- | ----------- | -------------- |
| page      | int  | No        | Página      | 1              |

## Ejemplos

### Solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes?page=1"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
      "measurement_type": {
        "id": "byte",
        "name": "BYTE",
        "title": "Byte"
      },
      "title": "Price per Byte",
      "slug": "price-per-byte",
      "description": null,
      "language": "af",
      "price": 29900,
      "currency": "BRL",
      "formatted_price": "R$\u00a0299,00",
      "created_at": "2025-09-26T04:46:04-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
    "last": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "links": [
      {
        "url": null,
        "label": "\u00ab Anterior",
        "active": false
      },
      {
        "url": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Pr\u00f3ximo \u00bb",
        "active": false
      }
    ],
    "path": "https://sandbox.echosistema.online/api/v1/ia/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## Estructura JSON Explicada

| Campo                         | Tipo        | Descripción |
| ----------------------------- | ----------- | ----------- |
| data[]                        | array       | Colección de productos medidos en bytes |
| data[].uuid                   | string      | Identificador del producto |
| data[].measurement_type       | object      | Metadatos del tipo de medición |
| data[].measurement_type.id    | string      | Identificador enum (siempre `byte`) |
| data[].measurement_type.name  | string      | Nombre del enum (`BYTE`) |
| data[].measurement_type.title | string      | Etiqueta legible |
| data[].title                  | string|null | Título localizado |
| data[].slug                   | string|null | Slug derivado del título |
| data[].description            | string|null | Descripción localizada opcional |
| data[].language               | string|null | Locale asociado al título |
| data[].price                  | number|null | Precio predeterminado en unidades menores |
| data[].currency               | string|null | Código ISO de la moneda |
| data[].formatted_price        | string|null | Precio formateado |
| data[].created_at             | string|null | Marca de tiempo de creación (ISO 8601) |
| links                         | object      | Navegación de paginación |
| meta                          | object      | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Notas

- Retorna solo productos con `measurement_type = byte`.
- Las etiquetas en `links[].label` se localizan según el backend; el ejemplo muestra el locale pt-BR.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alineado al modelo de precio único y título inmutable.
