# IA — Precio por Byte: Crear

Crea un producto con precio por byte, agregando contenido localizado y un único precio activo.

## Endpoint

```
POST /ia/admin/pricing/bytes
```

Crea un producto medido en BYTE con título localizado, descripción opcional y un único precio.

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

### Parámetros de consulta

Ninguno.

### Cuerpo de la solicitud

```json
{
  "price": 100,
  "currency": "USD",
  "description": "Precio por byte"
}
```

| Campo       | Tipo    | Requerido | Descripción |
| ----------- | ------- | --------- | ----------- |
| price       | integer | Sí        | Valor del precio (unidad más pequeña; ej.: centavos). |
| currency    | string  | Sí        | Código de moneda (USD, EUR, BRL, PYG). |
| description | string  | No        | Descripción predeterminada. |

## Ejemplos

### Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{"price":100,"description":"Precio por byte"}' \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes"
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
    "description": "Precio por byte",
    "language": "es",
    "price": 100,
    "currency": "USD",
    "formatted_price": "US$\u00a01,00",
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
| data.price                    | integer     | Precio predeterminado en la unidad mínima |
| data.currency                 | string      | Código ISO de la moneda |
| data.formatted_price          | string|null | Precio formateado |
| data.created_at               | string|null | Marca temporal de creación (ISO 8601) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Notas

- Título creado a partir de la clave `common.byte_price`; el slug se mantiene `byte_price` para integrarse con el pipeline de Data Calculator.
- El título es inmutable para los endpoints de Precio por Byte.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alineado con entrada de precio único y título inmutable.
- 2025-09-26: Respuesta y estado actualizados para reflejar la implementación.
