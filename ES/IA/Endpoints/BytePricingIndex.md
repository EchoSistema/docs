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
      "uuid": "...",
      "measurement_type": "byte",
      "title": { "content": "Precio por Byte" },
      "text": { "content": "Precio por byte" },
      "prices": [ { "currency_id": 2, "value": 123 } ]
    }
  ],
  "meta": { "current_page": 1, "per_page": 25 }
}
```

## Estructura JSON Explicada

| Campo                  | Tipo   | Descripción |
| ---------------------- | ------ | ----------- |
| data[]                 | array  | Lista de productos |
| data[].uuid            | string | Identificador del producto |
| data[].measurement_type| string | Tipo de medición (`byte`) |
| data[].title.content   | string | Título predeterminado localizado |
| data[].text.content    | string | Descripción predeterminada |
| data[].prices[]        | array  | Precios del producto |
| meta                   | object | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Notas

- Retorna solo productos con `measurement_type = byte`.

## Relacionados

- docs/ES/IA/Endpoints/BytePricingShow.md
- docs/ES/IA/Endpoints/BytePricingStore.md
- docs/ES/IA/Endpoints/BytePricingUpdate.md
- docs/ES/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alineado al modelo de precio único y título inmutable.
