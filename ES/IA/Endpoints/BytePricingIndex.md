# IA — Precio por Byte: Índice

Lista productos con precio por byte, mostrando contenido localizado y precio predeterminado.

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

| Parámetro | Tipo | Requerido | Descripción      | Predeterminado |
| --------- | ---- | --------- | ---------------- | -------------- |
| page      | int  | No        | Número de página | 1              |

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
      "title": "Precio por Byte",
      "slug": "byte_price",
      "description": null,
      "language": "es",
      "price": 29900,
      "currency": "BRL",
      "formatted_price": "R$\u00a0299,00",
      "created_at": "2025-09-26T04:46:04-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/ia/admin/pricing/bytes?page=1",
    "last": "https://sandbox.su-dominio.com/ia/admin/pricing/bytes?page=1",
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
        "url": "https://sandbox.su-dominio.com/ia/admin/pricing/bytes?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Siguiente \u00bb",
        "active": false
      }
    ],
    "path": "https://sandbox.su-dominio.com/ia/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## Estructura JSON Explicada

| Campo                              | Tipo        | Descripción |
| ---------------------------------- | ----------- | ----------- |
| data[]                             | array       | Lista paginada de productos con precio por byte |
| data[].uuid                        | string      | Identificador del producto |
| data[].measurement_type            | object      | Metadatos del tipo de medida |
| data[].measurement_type.id         | string      | Identificador enum (`byte`) |
| data[].measurement_type.name       | string      | Nombre del enum (`BYTE`) |
| data[].measurement_type.title      | string      | Etiqueta legible |
| data[].title                       | string|null | Título localizado del producto |
| data[].slug                        | string|null | Slug usado internamente (esperado `byte_price`) |
| data[].description                 | string|null | Descripción localizada opcional |
| data[].language                    | string|null | Locale asociado al título predeterminado |
| data[].price                       | integer|null | Precio predeterminado en la unidad mínima (p. ej., centavos) |
| data[].currency                    | string|null | Código ISO de la moneda |
| data[].formatted_price             | string|null | Precio formateado |
| data[].created_at                  | string|null | Marca temporal de creación (ISO 8601) |
| links                              | object      | Navegación de paginación |
| meta                               | object      | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Notas

- Retorna solo productos con `measurement_type = byte`.
- Las etiquetas de paginación respetan el locale configurado en el backend.
- El tamaño de página es fijo en 25 registros.
- La colección puede incluir claves adicionales cuando el atributo `raw` del producto esté poblado.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alineado al modelo de precio único y título inmutable.
- 2025-09-26: Ejemplo de respuesta y descripción de campos actualizados.
