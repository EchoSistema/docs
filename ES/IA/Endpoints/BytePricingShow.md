# IA — Precio por Byte: Detalle

Expone el producto con precio por byte de la plataforma, incluyendo metadatos localizados y precio predeterminado por byte.

## Endpoint

```
GET /ia/admin/pricing/bytes/details
```

Retorna el producto medido en BYTE configurado para la plataforma.

## Autenticación

Requerida — Bearer {token} con Sanctum.

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}` |
| Accept-Language  | string | No        | Locale IETF |

## Parámetros

### Parámetros de ruta

Ninguno.

## Ejemplos

### Solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes/details"
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
    "description": "Descripción predeterminada de precio por byte",
    "language": "es",
    "price": "0.0299",
    "raw_price": 299,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 840,
        "currency": "USD",
        "value": "0.0055",
        "raw_value": 55,
        "formatted_value": "$0.0055"
      }
    ],
    "currency": "BRL",
    "formatted_price": "R$\u00a00.0299",
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
| data.description              | string|null | Descripción predeterminada |
| data.language                 | string|null | Locale asociado al título predeterminado |
| data.price                    | string|null | Precio por byte con cuatro decimales |
| data.raw_price                | integer|null | Valor original almacenado en la unidad mínima |
| data.price_precision          | integer|null | Precisión decimal aplicada |
| data.prices                   | array       | Precios alternativos activos por moneda |
| data.prices[].currency_id     | integer     | Identificador de moneda |
| data.prices[].currency        | string      | Código ISO de la moneda |
| data.prices[].value           | string      | Precio alternativo por byte con cuatro decimales |
| data.prices[].raw_value       | integer     | Valor alternativo almacenado en la unidad mínima |
| data.prices[].formatted_value | string      | Precio alternativo formateado |
| data.currency                 | string|null | Código ISO de la moneda predeterminada |
| data.formatted_price          | string|null | Precio predeterminado formateado con cuatro decimales |
| data.created_at               | string|null | Marca temporal de creación (ISO 8601) |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 404: No encontrado (cuando el producto no es BYTE)

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Notas

- El título permanece inmutable después de la creación.

## Changelog

- 2025-09-23: Campos y códigos actualizados.
- 2025-10-03: Documentados campos de precisión por byte, precios alternativos y nuevo endpoint sin parámetro.
- 2025-09-26: Ejemplo de respuesta actualizado según la forma actual del recurso.
