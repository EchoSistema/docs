# Newsletter — Suscripción

## Endpoint

`POST /api/v1/newsletters`

Recibe solicitudes públicas de suscripción a la newsletter y almacena los registros asociados a la plataforma informada.

---

## Autenticación

Sin token Bearer. Requiere el encabezado de plataforma para identificar el tenant.

### Encabezados Obligatorios
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| `X-PUBLIC-KEY` | `string` | Identificador público de la plataforma. |

### Encabezados Opcionales
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| `Accept-Language` | `string` | Define el locale; se usa para completar `language`. |

---

## Cuerpo (JSON)
| Campo            | Tipo     | Req. | Descripción |
| ---------------- | -------- | ---- | ----------- |
| `name`           | `string` | No   | Nombre de la persona suscriptora. Opcional. |
| `email`          | `email`  | Sí   | Dirección de correo de la persona suscriptora. |
| `g_recaptcha`    | `string` | Sí   | Token reCAPTCHA validado por `ReCaptchaRule`. |
| `platform_uuid`  | `uuid`   | Sí   | UUID de la plataforma que recibirá la suscripción. |
| `is_active`      | `bool`   | Auto | Forzado a `true` por el backend; los valores enviados se ignoran. |
| `language`       | `string` | Auto | Definido a partir del locale actual (`Accept-Language` o el predeterminado de la aplicación). |

### Ejemplo de Solicitud
```json
{
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "g_recaptcha": "token-recaptcha",
  "platform_uuid": "11111111-1111-1111-1111-111111111111"
}
```

---

## Respuestas

### Éxito — 201 Created
```json
{
  "message": "El correo se guardó correctamente"
}
```

### Error de Validación — 422 Unprocessable Entity
```json
{
  "message": "El correo no se guardó",
  "errors": {
    "email": ["El campo email es obligatorio."],
    "g_recaptcha": ["Error al validar el reCAPTCHA."]
  }
}
```

### Bloqueo por Robot — 402 Payment Required
Se devuelve cuando el user agent identificado corresponde a un bot.

---

## Notas
- Nombre de la ruta: `api.v1.newsletters.store`.
- Cada suscripción dispara una notificación en Slack definida en `config('backoffice.slack.channel.default')`.
- Los atributos adicionales (`language`, `is_active`) se calculan internamente mediante `NewsletterStoreRequest`.
- Los registros se persisten con `Newsletter::createWithAttributes` después de la preparación realizada por `PrepareNewsletterData`.
