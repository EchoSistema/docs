# Plataforma – Endpoint de Listado de Correos (IMAP)

## Endpoint

`GET /platform/app/emails`

Devuelve correos IMAP del buzón configurado en la plataforma. Soporta paginación, ordenación y proyección de campos para mejorar el rendimiento.

---

## Autenticación

Obligatoria — `auth:sanctum` (token Bearer para el backoffice de la plataforma).

---

## Solicitud

### Encabezados Requeridos

| Encabezado       | Tipo     | Descripción |
| ---------------- | -------- | ----------- |
| Authorization    | `string` | `Bearer {token}`. |
| X-PUBLIC-KEY     | `string` | Clave pública de la plataforma. |
| Accept-Language  | `string` | Idioma IETF (p.ej., `es`, `pt-BR`, `en`). |

### Parámetros de Consulta

| Parámetro                     | Tipo       | Predet. | Descripción |
| ---------------------------- | ---------- | ------- | ----------- |
| `page`                       | `integer`  | `1`     | Página actual. |
| `per_page`                   | `integer`  | `10`    | Elementos por página (1–100). |
| `order`                      | `string`   | `desc`  | Orden cronológico: `asc` o `desc`. |
| `fields`                     | `string`   | —       | Lista separada por comas para proyección (p.ej., `subject,sender_name,sender_full,date,uid,seen`). Recomendado para rendimiento. |
| `include_flags`              | `boolean`  | `true`  | Incluye flags IMAP cuando sea necesario. Se infiere automáticamente cuando algún flag es solicitado vía `fields` (p.ej., `seen`). |
| `include_body`               | `boolean`  | `false` | Incluye el cuerpo del correo (costoso; solicitar solo si es necesario). |
| `include_body_preview`       | `boolean`  | `false` | Incluye una vista previa de 300 caracteres en texto plano. |
| `include_attachment_count`   | `boolean`  | `false` | Cuenta los adjuntos (puede ser costoso según el servidor). |
| `include_headers`            | `boolean`  | `false` | Incluye encabezados en bruto (costo extra). |

---

## Respuesta

La respuesta sigue el formato estándar con `data` y `meta`.

### Ejemplo (proyección ligera)

Recomendado para el listado: `?fields=subject,sender_name,sender_full,date,uid,seen`.

```json
{
  "data": [
    {
      "subject": "Bienvenido",
      "sender_name": "Equipo de Soporte",
      "sender_full": "Equipo de Soporte <soporte@example.com>",
      "date": "2025-09-10T15:43:12+00:00",
      "uid": 12345,
      "seen": false
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 10,
    "has_more": true,
    "order": "desc"
  }
}
```

### Ejemplo (payload completo)

Sin `fields`, se pueden devolver más campos (to/cc/bcc/reply_to, tamaño, flags, etc.). Úselo solo si es necesario debido al costo de análisis.

---

## Campos Comunes

- Identidad
  - `subject` `string`
  - `sender` `string|null` – correo del remitente (cuando se solicite)
  - `sender_full` `string|null` – nombre + correo
  - `sender_name` `string|null` – nombre del remitente
  - `from`/`from_full`/`from_name` – variantes del encabezado From (cuando se soliciten)
- Metadatos
  - `date` `string|null` – ISO‑8601
  - `uid` `int|null`
  - `msgno`, `message_id`, `size` (cuando se soliciten)
- Flags (cuando `include_flags` o solicitadas vía `fields`)
  - `flags` `array|null`
  - `seen`, `answered`, `flagged`, `draft`, `recent`, `deleted` `boolean|null`
- Contenido (costoso; solicitar solo cuando sea necesario)
  - `body_preview` `string|null`
  - `body_raw` `string|null`
  - `body` `string|null`
  - `headers` `object|null`
- Adjuntos (costo variable)
  - `has_attachments` `boolean|null`
  - `attachments_count` `int|null`

---

## Notas

- Use `fields` para reducir la latencia y la carga sobre IMAP.
- `seen` y otros flags solo se obtienen cuando se solicitan, ahorrando llamadas IMAP.
- `attachments_count` puede ser costoso; actívelo solo cuando sea necesario.
- Las fechas se normalizan a ISO‑8601 cuando es posible.
- Localización: los campos de texto pueden reflejar el encabezado `Accept-Language` cuando aplique; de lo contrario, se devuelven valores por defecto.
