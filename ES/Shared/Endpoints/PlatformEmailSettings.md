# Plataforma — Endpoint de Configuración de Correo (IMAP)

## Endpoints

- `GET /platform/app/emails/settings`
- `POST /platform/app/emails/settings`
- `PUT /platform/app/emails/settings`

Gestiona la configuración IMAP de la plataforma (host, puerto, cifrado, validación de certificado, credenciales, carpeta por defecto).

---

## Autenticación

Obligatoria — `auth:sanctum` (token Bearer para el backoffice de la plataforma).

---

## GET /platform/app/emails/settings

Devuelve la configuración IMAP actual de la plataforma.

- Respuestas
  - `200 OK` con el objeto de configuración (sin la contraseña)
  - `204 No Content` cuando no existe configuración

### Ejemplo de Respuesta
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "default_folder": "INBOX",
  "timeout": 30
}
```

---

## POST /platform/app/emails/settings

Crea la configuración IMAP para la plataforma (si no existe).

- Reglas
  - Falla con `422 Unprocessable Entity` si ya existe configuración
  - Campos obligatorios según validación

### Cuerpo (JSON)
| Campo            | Tipo         | Oblig. | Descripción |
| ---------------- | ------------ | ------ | ----------- |
| `host`           | `string`     | Sí     | Host IMAP. |
| `port`           | `integer`    | Sí     | Puerto (1–65535). |
| `encryption`     | `string|null`| No     | `ssl`, `tls` o `null`. |
| `validate_cert`  | `boolean`    | Sí     | Validar certificado TLS. |
| `username`       | `string`     | Sí     | Usuario IMAP. |
| `password`       | `string`     | Sí     | Contraseña IMAP. |
| `default_folder` | `string`     | Sí     | Carpeta por defecto (p.ej., `INBOX`). |
| `timeout`        | `integer`    | No     | Timeout en segundos (1–600). Por defecto: 30. |

### Ejemplo de Solicitud
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "password": "secret",
  "default_folder": "INBOX",
  "timeout": 45
}
```

### Respuestas
- `201 Created` con mensaje de éxito
- `422 Unprocessable Entity` si ya existe

---

## PUT /platform/app/emails/settings

Actualiza (o crea si no existe) la configuración IMAP de la plataforma.

### Cuerpo (JSON)
Mismos campos que POST; `password` es opcional (solo se actualiza si se proporciona).

### Respuestas
- `200 OK` con mensaje de éxito
- `422 Unprocessable Entity` si hay errores de validación

---

## Notas

- Por seguridad, la contraseña nunca es devuelta.
- `timeout` por defecto es 30s cuando se omite.
- Para rendimiento y seguridad, prefiera `INBOX` o carpetas específicas; evite escanear archivos grandes salvo necesidad.
- Valores permitidos para `encryption`: `ssl`, `tls` o `null` (sin cifrado — no recomendado).
