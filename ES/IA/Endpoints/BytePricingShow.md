# IA — Precio por Byte: Detalle

## Endpoint

```
GET /ia/admin/pricing/bytes/{product}
```

Retorna un producto medido en BYTE.

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

## Ejemplos

### Solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes/{product}"
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Precio por Byte" },
    "text": { "content": "Precio por byte" },
    "prices": [ { "currency_id": 2, "value": 123 } ]
  }
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 404: No encontrado (cuando el producto no es BYTE)

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Campos y códigos actualizados.
