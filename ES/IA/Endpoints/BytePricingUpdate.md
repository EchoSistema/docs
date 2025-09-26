# IA — Precio por Byte: Actualizar

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
{ "data": { "uuid": "...", "measurement_type": "byte" } }
```

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 404: No encontrado (cuando el producto no es BYTE)
- 422: Entidad no procesable

## Notas

- El título para Precio por Byte es inmutable y no será actualizado por este endpoint.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Actualizado para precio único y título inmutable.
