# IA — Precio por Byte: Crear

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

### Respuesta (201)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Precio por Byte" },
    "text": { "content": "Precio por byte" },
    "prices": [ { "currency_id": 2, "value": 100, "is_default": true } ]
  }
}
```

## Estados HTTP

- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Notas

- Título creado para todos los idiomas desde la clave `common.byte_price` y slug fijo `byte_price`.
- El título es inmutable para los endpoints de Precio por Byte.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alineado con entrada de precio único y título inmutable.
