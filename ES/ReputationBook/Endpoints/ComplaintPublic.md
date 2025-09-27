# ReputationBook – Reclamaciones públicas

## Listar reclamaciones (público)

### Endpoint

```
GET /api/v1/complaints
```

Devuelve las reclamaciones publicadas para la plataforma actual (forzada por el middleware `platform`). Los resultados pueden paginarse o limitarse mediante parámetros de consulta.

### Autenticación

Ninguna. Solo se exige el encabezado `X-PUBLIC-KEY` para identificar la plataforma.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción | Valores |
| ---------- | ---- | ----------- | ----------- | ------- |
| `X-PUBLIC-KEY` | string | Sí | Clave pública de la plataforma consumidora. | — |
| `Accept-Language` | string | Recomendado | Locale (`pt-BR`, `en`, `es`) usado para recuperar títulos/textos cuando estén disponibles. | — |

### Parámetros de consulta

| Parámetro | Tipo | Obligatorio | Descripción | Valores |
| --------- | ---- | ----------- | ----------- | ------- |
| `page` | integer | No | Página actual al paginar. | `1` |
| `per_page` | integer | No | Tamaño de la página. | `15` |
| `no_paginate` | boolean | No | Cuando es `true`, devuelve una lista simple limitada por `limit`. | `false` |
| `limit` | integer | No | Máximo de elementos cuando `no_paginate=true`. | `15` |
| `status` | string | No | Filtra por estado (`open`, `in_progress`, etc.). | — |
| `company` | string | No | Acepta ID, UUID, slug o los prefijos `email:` / `url:`. | — |
| `title` | string | No | Busca por título de la reclamación. | — |
| `complaint` | string | No | Busca dentro del cuerpo de la reclamación. | — |
| `with_texts` | boolean | No | Incluye textos completos en la respuesta. | `false` |
| `created_at`, `created_at_period[]` | date / array | No | Filtra por fecha de creación (exacta o intervalo). | — |
| `purchase_date`, `purchase_date_period[]` | date / array | No | Filtra por la fecha de compra informada. | — |
| `month`, `year` | integer | No | Filtra por mes/año (ambos requeridos cuando `month` está presente). | — |

> Los nombres de parámetros son canónicos en `snake_case`, pero también se aceptan `camelCase`, `kebab-case` y `CapitalCase`.

### Ejemplo de respuesta (200)

```json
{
  "data": [
    {
      "uuid": "0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5",
      "is_public": true,
      "title": {
        "title": "Entrega atrasada",
        "slug": "entrega-atrasada",
        "language": "es",
        "is_default": true
      },
      "text": "El pedido llegó 7 días tarde...",
      "company": {
        "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
        "name": "Tienda Ejemplo",
        "slug": "tienda-ejemplo"
      },
      "status": "open",
      "purchase_date": "2025-08-12",
      "created_at": "2025-08-13T17:01:22+00:00"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 42
  }
}
```

### Estructura JSON explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data[]` | array | Lista de reclamaciones públicas. |
| `data[].uuid` | uuid | Identificador de la reclamación. |
| `data[].title` | objeto | Título localizado de la reclamación. |
| `data[].text` | string | Fragmento del texto principal (truncado públicamente). |
| `data[].company` | objeto | Empresa asociada. |
| `data[].status` | string | Estado actual (`open`, `resolved`, ...). |
| `meta` | objeto | Metadatos de paginación cuando aplica. |

### Códigos HTTP

- 200: Lista devuelta correctamente.
- 400: Filtros inválidos.
- 429: Límite de peticiones excedido.
- 500: Error interno.

---

## Crear reclamación

### Endpoint

```
POST /api/v1/complaints
```

Crea una nueva reclamación en nombre del usuario autenticado.

### Autenticación

Obligatoria – `Bearer {token}` vía `auth:sanctum` con las habilidades `backoffice` **y** `domain:reputationbook`.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
| ---------- | ---- | ----------- | ----------- |
| `Authorization` | string | Sí | `Bearer {token}` válido. |
| `X-PUBLIC-KEY` | string | Sí | Identifica la plataforma. |
| `Accept-Language` | string | Recomendado | Idioma preferido para título/texto. |
| `Content-Type` | string | Sí | `application/json`. |

### Cuerpo (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `company_uuid` | uuid | Condicional | UUID de la empresa. Obligatorio si `company_id` está ausente. |
| `company_id` | integer | Condicional | ID interno de la empresa (mutuamente excluyente con `company_uuid`). |
| `purchase_date` | date | Sí | Fecha de compra (<= hoy). |
| `title` | string | Sí | Título de la reclamación. |
| `complaint` | string | Sí | Texto completo de la reclamación. |
| `is_public` | boolean | No | Marca la reclamación como pública. |
| `invoice_number` | string | No | Referencia de la factura. |
| `attendants_name` | string | No | Nombres de los atendientes involucrados. |
| `language` | string | No | Idioma del contenido (`pt-BR`, `en`, etc.). |

### Ejemplo de solicitud

```json
{
  "company_uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
  "purchase_date": "2025-08-12",
  "title": "Entrega atrasada",
  "complaint": "El pedido debía llegar en 48 horas...",
  "is_public": true,
  "invoice_number": "NF-12345",
  "language": "es"
}
```

### Ejemplo de respuesta (201)

```json
{
  "data": {
    "uuid": "31db8d4a-0d0f-4d38-9c6a-c4c0ad9c8d41",
    "is_public": true,
    "platform": {
      "uuid": "c0aec1f4-d5ae-4ffd-8d24-8e55e9a3aca7",
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Defensa del Consumidor",
        "slug": "defensa-consumidor"
      }
    },
    "title": {
      "title": "Entrega atrasada",
      "language": "es",
      "is_default": true
    },
    "texts": [
      {
        "content": "El pedido debía llegar en 48 horas...",
        "usage": "complaint_text",
        "language": "es",
        "is_default": true
      }
    ],
    "company": {
      "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
      "name": "Tienda Ejemplo",
      "slug": "tienda-ejemplo"
    },
    "status": "open",
    "purchase_date": "2025-08-12",
    "invoice_number": "NF-12345",
    "created_at": "2025-08-13T17:01:22+00:00"
  }
}
```

### Códigos HTTP

- 201: Reclamación creada.
- 400: Campos obligatorios faltantes o conflicto entre `company_uuid` / `company_id`.
- 401: Token inválido.
- 403: Usuario sin habilidades requeridas.
- 422: Errores de validación.

### Notas

- La política de autorización exige permiso `create` sobre `Complaint`.
- Los textos adicionales se gestionan a través del endpoint `add-text`.

---

## Consultar reclamación por UUID

### Endpoint

```
GET /api/v1/complaints/{complaint}
```

Devuelve una reclamación por identificador. El parámetro acepta ID, UUID o slug según la heurística de binding.

### Autenticación

Opcional. Los usuarios autenticados reciben campos adicionales (texto completo, datos del usuario, factura, imágenes).

### Parámetros de ruta

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `complaint` | string | Identificador de la reclamación (ID/UUID/slug). |

### Códigos HTTP

- 200: Reclamación encontrada.
- 404: Reclamación no encontrada.

---

## Actualizar reclamación

### Endpoint

```
PUT /api/v1/complaints/{complaint}
```

Actualiza los datos básicos de la reclamación.

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Cuerpo (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `company_uuid` | uuid | No | Actualiza la empresa (mutuamente excluyente con `company_id`). |
| `company_id` | integer | No | ID de la empresa. |
| `purchase_date` | date | No | Nueva fecha de compra. |
| `title` | string | No | Nuevo título (requiere `language`). |
| `complaint` | string | No | Nuevo texto principal (requiere `language`). |
| `language` | string | Condicional | Obligatorio cuando se envía `title` o `complaint`. |
| `is_public` | boolean | No | Actualiza la visibilidad. |
| `invoice_number` | string | No | Actualiza la factura. |

> La solicitud debe contener al menos un campo mutable; de lo contrario se devuelve `400` (`validation.empty_request`).

### Códigos HTTP

- 200: Actualización aplicada.
- 400: Cuerpo vacío.
- 401/403: Falta de permisos.
- 404: Reclamación inexistente.
- 422: Datos inválidos.

---

## Actualizar estado de la reclamación

### Endpoint

```
PATCH /api/v1/complaints/{complaint}/status
```

### Cuerpo

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `status` | string | Sí | Nuevo estado (enum `ComplaintStatusEnum`). |

### Notas

- Requiere la política `update` sobre la reclamación.
- El usuario autenticado queda registrado como responsable del cambio.

### Códigos HTTP

- 200: Estado actualizado.
- 400: Estado inválido.
- 401/403: Usuario no autorizado.
- 404: Reclamación inexistente.

---

## Eliminar reclamación

### Endpoint

```
DELETE /api/v1/complaints/{complaint}
```

Elimina la reclamación y registra al usuario autenticado como responsable.

### Autenticación

Obligatoria – `Bearer {token}` (`auth:sanctum`) con habilidades `backoffice` y `domain:reputationbook`.

### Códigos HTTP

- 204: Eliminación completada.
- 401/403: Usuario sin permiso `delete`.
- 404: Reclamación no encontrada.

---

## Agregar texto complementario

### Endpoint

```
POST /api/v1/complaints/{complaint}/add-text
```

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Cuerpo (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `content` | string | Sí | Texto adicional (hasta 5000 caracteres). |
| `language` | string | No | Idioma del texto (`LanguageEnum`). |
| `usage` | string | No | Uso específico (por defecto `complaint_text`). |

### Códigos HTTP

- 200: Texto agregado y recurso retornado.
- 401/403: Usuario no autorizado.
- 404: Reclamación no encontrada.
- 422: Contenido inválido.

### Notas

- El texto se agrega a la relación `texts`; usa `all_texts=true` para recuperar todas las versiones.

