# IA — Precio por Byte: Actualizar

Actualiza la descripción predeterminada y/o el precio único de un producto con precio por byte existente.

## Endpoint

```
PUT /ia/admin/pricing/bytes/{product}
```

Actualiza la descripción y/o el precio único de un producto BYTE. El título no es actualizable.

## Autenticación

Requerida — Bearer {token} con Sanctum.

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}` |
| Accept-Language  | string | No        | Locale IETF |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| product   | string | Sí        | Clave de ruta del producto |

### Cuerpo de la solicitud

```json
{
  "price": 150,
  "currency": "EUR",
  "description": "Nueva descripción"
}
```

| Campo       | Tipo    | Requerido | Descripción |
| ----------- | ------- | --------- | ----------- |
| price       | integer | No        | Nuevo valor del precio (unidad más pequeña; ej.: centavos). |
| currency    | string  | No (requerido con price) | Código de moneda (USD, EUR, BRL, PYG). |
| description | string  | No        | Nueva descripción predeterminada. |

## Ejemplos

### Solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{"price":150,"description":"Nueva descripción"}' \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes/{product}"
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
    "measurement_type": {
      "id": "byte",
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Precio por Byte",
    "slug": "byte_price",
    "description": "Nueva descripción",
    "language": "es",
    "price": 150,
    "currency": "EUR",
    "formatted_price": "€1,50",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
```

## Estructura JSON Explicada

| Campo                         | Tipo        | Descripción |
| ----------------------------- | ----------- | ----------- |
| data.uuid                     | string      | Identificador del producto |
| data.measurement_type         | object      | Metadatos del tipo de medida |
| data.measurement_type.id      | string      | Identificador enum (`byte`) |
| data.measurement_type.name    | string      | Nombre del enum (`BYTE`) |
| data.measurement_type.title   | string      | Etiqueta legible |
| data.title                    | string|null | Título localizado del producto |
| data.slug                     | string|null | Slug usado internamente |
| data.description              | string|null | Descripción predeterminada actualizada |
| data.language                 | string|null | Locale asociado al título predeterminado |
| data.price                    | integer|null | Precio actualizado en la unidad mínima |
| data.currency                 | string|null | Código ISO de la moneda |
| data.formatted_price          | string|null | Precio formateado |
| data.created_at               | string|null | Marca temporal de creación (ISO 8601) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 404: No encontrado (cuando el producto no es BYTE)
- 422: Entidad no procesable

## Notas

- El título para Precio por Byte es inmutable y no será actualizado por este endpoint.
- Precio y moneda deben enviarse juntos; omitir ambos conserva los valores actuales.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Actualizado para precio único y título inmutable.
- 2025-09-26: Ejemplo de respuesta y referencias de campos actualizados.
