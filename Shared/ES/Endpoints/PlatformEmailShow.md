# Plataforma – Endpoint de Visualización de Correo (IMAP)

## Endpoint

`GET /platform/app/emails/{uid}`

Devuelve un mensaje IMAP por `uid`, enfocado en los campos usados por la interfaz. Los encabezados y el conteo de adjuntos son opcionales para preservar el rendimiento.

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

| Parámetro                     | Tipo      | Predet. | Descripción |
| ---------------------------- | --------- | ------- | ----------- |
| `include_headers`            | `boolean` | `false` | Cuando es `true`, incluye encabezados en bruto (costo extra). |
| `include_attachment_count`   | `boolean` | `false` | Cuando es `true`, devuelve el conteo de adjuntos (puede ser costoso según el servidor). |

---

## Respuesta

La respuesta sigue el formato estándar con `data`.

### Ejemplo (básico)

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Equipo de Soporte <soporte@example.com>",
    "from": "soporte@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": null,
    "body_raw": "<html>...contenido...</html>",
    "body": "...contenido en texto...",
    "attachments_count": null
  }
}
```

### Ejemplo (con encabezados y adjuntos)

Solicitud: `GET /platform/app/emails/{uid}?include_headers=1&include_attachment_count=1`

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Equipo de Soporte <soporte@example.com>",
    "from": "soporte@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": {
      "subject": "Bienvenido",
      "date": "Tue, 10 Sep 2025 15:43:12 +0000",
      "message_id": "<abc@example.com>",
      "from": "Equipo de Soporte <soporte@example.com>",
      "to": "usuario@cliente.com",
      "cc": "",
      "bcc": "",
      "reply_to": "",
      "sender": "Equipo de Soporte <soporte@example.com>",
      "content_type": "text/html; charset=UTF-8"
    },
    "body_raw": "<html>...contenido...</html>",
    "body": "...contenido en texto...",
    "attachments_count": 2
  }
}
```

---

## Campos

- `uid` `int` — Identificador de mensaje IMAP.
- `sender_full` `string|null` — Nombre y correo del remitente (prefiere `Sender`; fallback a `From`).
- `from` `string|null` — Correo mostrado en la UI y usado como fallback.
- `date` `string|null` — Fecha en ISO‑8601.
- `headers` `object|null` — Encabezados en bruto solo cuando `include_headers=1`.
- `body_raw` `string|null` — HTML (preferido) o texto plano cuando no hay HTML.
- `body` `string|null` — Fallback de texto plano.
- `attachments_count` `int|null` — Conteo de adjuntos solo cuando `include_attachment_count=1`.

---

## Notas

- No se consultan flags IMAP en show para reducir la latencia.
- Evite solicitar encabezados y conteo de adjuntos cuando no sean necesarios.
- La normalización de fechas intenta mantener ISO‑8601; algunos servidores pueden devolver formatos distintos.
