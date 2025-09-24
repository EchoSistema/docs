# IA — Precio por Byte: Eliminar

## Endpoint

```
DELETE /ia/admin/pricing/bytes/{product}
```

Elimina (soft-delete) un producto BYTE.

## Autenticación

Requerida — Bearer {token} con Sanctum.

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}` |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| product   | string | Sí        | Clave de ruta del producto |

## Ejemplos

### Solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.su-dominio.com/ia/admin/pricing/bytes/{product}"
```

### Respuesta (200)

```json
{ "message": "Eliminado correctamente." }
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 404: No encontrado (cuando el producto no es BYTE)

## Relacionados

- docs/ES/IA/Endpoints/BytePricingIndex.md
- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md

## Changelog

- 2025-09-23: Añadidos encabezados y parámetros según template.
