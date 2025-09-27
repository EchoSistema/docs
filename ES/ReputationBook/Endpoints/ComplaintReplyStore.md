# ReputationBook – Crear respuesta de reclamación

## Endpoint

```
POST /api/v1/complaints/{complaint}/replies
```

Crea una nueva respuesta para la reclamación identificada por el parámetro de ruta `complaint`.

---

## Autenticación

Obligatoria – `Bearer {token}` autenticado por `auth:sanctum`. No se requiere habilidad adicional.

---

## Encabezados

| Encabezado | Tipo | Obligatorio | Descripción | Valores |
| ---------- | ---- | ----------- | ----------- | ------- |
| `Authorization` | string | Sí | Credencial `Bearer {token}`. | — |
| `X-PUBLIC-KEY` | string | Sí | Identifica la plataforma emisora. | — |
| `Accept-Language` | string | Recomendado | Locale del cliente (`pt-BR`, `en`, `es`). | — |

---

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo | Obligatorio | Descripción | Valores |
| --------- | ---- | ----------- | ----------- | ------- |
| `complaint` | string | Sí | Identificador de la reclamación (acepta ID numérico o UUID). | — |

### Cuerpo de la solicitud (JSON)

| Campo | Tipo | Obligatorio | Descripción | Valores |
| ----- | ---- | ----------- | ----------- | ------- |
| `user` | string | Sí | Referencia al autor de la respuesta. Acepta ID numérico, UUID o correo registrado. | — |
| `company` | string | No | Identificador de la empresa cuando emite la respuesta. Acepta ID, UUID o slug. | — |
| `text` | string | Sí | Contenido principal de la respuesta (5–5000 caracteres). | — |
| `language` | string | Sí | Idioma de la respuesta en formato IETF (ej.: `es`). | `app.locale` |
| `is_platform_support` | boolean | No | Cuando es `true`, indica que la respuesta fue creada por el soporte de la plataforma. | `false` |

> Los nombres de los campos son canónicos en `snake_case`, pero el endpoint acepta `camelCase`, `kebab-case` y `CapitalCase`.

---

## Ejemplos

### Solicitud (curl)

```bash
curl -X POST "https://api.sandbox.echosistema.com/api/v1/complaints/0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5/replies" \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-client-id>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "user": "cliente@example.com",
    "text": "Estamos revisando tu caso y te contactaremos en breve.",
    "language": "es",
    "is_platform_support": false
  }'
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "4f0d5906-8aa5-3f44-8eb8-0b2936ccaf8c",
    "index": 2,
    "is_support": false,
    "content": {
      "content": "Estamos revisando tu caso y te contactaremos en breve.",
      "language": "es"
    },
    "user": {
      "uuid": "b02527ac-47f4-4c0e-9157-1f917bf923c7",
      "name": "María Silva"
    },
    "company": {
      "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
      "name": "Tienda Ejemplo"
    },
    "platform": {
      "uuid": "8b3dd5bb-1d6b-48ce-84b4-3097f3d5343f",
      "name": "EchoSistema"
    }
  }
}
```

---

## Estructura JSON explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data.uuid` | uuid | Identificador de la respuesta creada. |
| `data.index` | integer | Orden de la respuesta dentro de la reclamación. |
| `data.is_support` | boolean | Indica si la respuesta fue marcada como soporte de la plataforma. |
| `data.content.content` | string | Texto de la respuesta. |
| `data.content.language` | string | Idioma del texto principal. |
| `data.user.uuid` | uuid | Identificador del autor. |
| `data.user.name` | string | Nombre del autor. |
| `data.company.uuid` | uuid | (Opcional) Empresa asociada a la respuesta. |
| `data.company.name` | string | (Opcional) Nombre de la empresa. |
| `data.platform.uuid` | uuid | (Opcional) Plataforma asociada a la reclamación. |
| `data.platform.name` | string | (Opcional) Nombre de la plataforma. |

---

## Códigos HTTP

- 200: Respuesta creada correctamente.
- 400: No se encontró la reclamación, el usuario u otro recurso relacionado.
- 401: Token ausente o inválido.
- 404: Reclamación no encontrada para el identificador proporcionado.
- 422: Error de validación en los campos enviados.
- 500: Error interno inesperado.

---

## Errores

```json
{
  "message": "usuario no encontrado"
}
```

---

## Notas

- El identificador de la reclamación admite IDs numéricos y UUID gracias al binding dinámico.
- Cuando `is_platform_support=false` y la reclamación tiene plataforma asociada, la respuesta se marca automáticamente como soporte (`is_support=true`).
- El índice se calcula automáticamente considerando las respuestas existentes de la reclamación.
- Campos opcionales como `company` y `platform` solo aparecen cuando las relaciones están cargadas y son válidas.

---

## Relacionados

- [Visión general del dominio](../README.md)
- [Índice de endpoints de ReputationBook](README.md)

---

## Changelog

- 2025-09-26: Documento inicial del endpoint para crear respuestas de reclamación.

