# ReputationBook – Mis reclamaciones

## Listar mis reclamaciones

### Endpoint

```
GET /api/v1/complaint-book/my-complaints
```

Devuelve las reclamaciones creadas por el usuario autenticado en la plataforma actual.

### Autenticación

Obligatoria – `Bearer {token}` mediante `auth:sanctum`.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
| ---------- | ---- | ----------- | ----------- |
| `Authorization` | string | Sí | Token `Bearer` válido. |
| `X-PUBLIC-KEY` | string | Sí | Clave de la plataforma actual. |
| `Accept-Language` | string | Recomendado | Idioma usado para títulos/textos. |

### Parámetros de consulta

Admite los mismos filtros que `ComplaintFilterRequest`, pero la API fuerza internamente `user_id = Auth::id()`.

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `page`, `per_page`, `no_paginate`, `limit` | integer / boolean | Control de paginación. |
| `status` | string | Filtra por estado (`open`, `resolved`, ...). |
| `created_at`, `created_at_period[]` | date / array | Filtros de fecha de creación. |
| `purchase_date`, `purchase_date_period[]` | date / array | Filtros de fecha de compra. |
| `with_texts` | boolean | Incluye textos completos en la respuesta. |

### Respuesta (200)

Comparte la misma estructura que la lista pública, pero incluye campos adicionales (`texts`, `images`, `user`) de `ComplaintResource`.

### Códigos HTTP

- 200: Lista devuelta.
- 401: Token ausente o expirado.
- 500: Error interno.

---

## Ver una reclamación propia

### Endpoint

```
GET /api/v1/complaint-book/my-complaints/{complaint}
```

Devuelve detalles completos de una reclamación perteneciente al usuario autenticado. Acepta ID, UUID o slug.

### Autenticación

Obligatoria – `Bearer {token}` (`auth:sanctum`).

### Parámetros de ruta

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `complaint` | string | Identificador de la reclamación (ID/UUID/slug). |

### Códigos HTTP

- 200: Reclamación encontrada.
- 403: La reclamación pertenece a otro usuario.
- 404: Reclamación no encontrada.

### Notas

- La respuesta incluye plataforma, empresa, textos completos, imágenes y eventos de estado cuando están cargados.

