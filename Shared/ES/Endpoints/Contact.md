# Contactos — Crear/Actualizar (IA Admin)

## Endpoints

- `POST /ia/admin/contacts`
- `PUT /ia/admin/contacts/{contact}`

Crea o actualiza un contacto del usuario (email o telefónico: telephone, whatsapp, telegram).

---

## Autenticación

Obligatoria — `auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma para campos traducibles (p. ej., `en`, `pt-BR`). Opcional. |

---

## POST /ia/admin/contacts

Crea un contacto vinculado a un usuario por UUID.

### Cuerpo (JSON)
| Campo          | Tipo              | Req. | Descripción |
| -------------- | ----------------- | ---- | ----------- |
| `uuid`         | `uuid`            | Sí   | UUID del usuario al que se vincula el contacto. |
| `type`         | `ContactTypeEnum` | Sí   | Uno de `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | Cond | Requerido si `type = email` (o usar `value`). |
| `value`        | `email`           | Cond | Alternativa a `email` cuando `type = email`. |
| `country_code` | `string`          | Cond | Requerido con `number` cuando `type` no es `email`. Acepta `+` y dígitos (otros ignorados). |
| `number`       | `string`          | Cond | Número (solo dígitos) cuando `type` no es `email`. |

Se exige campos de email (para `email`) o de teléfono (para otros tipos).

### Ejemplos de Solicitud
Contacto email:
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "email",
  "email": "john.doe@example.com"
}
```

Contacto WhatsApp:
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "whatsapp",
  "country_code": "+55",
  "number": "11 91234-5678"
}
```

### Respuestas
- `200 OK` con el recurso creado.

### Ejemplos de Respuesta
Email:
```json
{ "email": "john.doe@example.com" }
```

WhatsApp/Telephone/Telegram:
```json
{
  "whatsapp": {
    "country_code": "+55",
    "number": "11912345678",
    "full": "+55 11912345678"
  }
}
```

### Error de Validación (422)
Ejemplo cuando faltan campos obligatorios o son inválidos para el `type` elegido.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "email": ["The email field is required when type is email."],
    "country_code": ["The country code field is required when type is not email."],
    "number": ["The number field is required when type is not email."]
  }
}
```

### No Encontrado (404)
Cuando el UUID de usuario proporcionado no existe.
```json
{ "message": "Usuario no encontrado." }
```

### No Autenticado (401)
Cuando falta un token válido de Sanctum.
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
Cuando el usuario autenticado no tiene permiso para esta acción.
```json
{ "message": "This action is unauthorized." }
```

---

## PUT /ia/admin/contacts/{contact}

Actualiza un contacto existente. Los campos son opcionales y solo se actualizan los enviados. Al cambiar `type`, la estructura se adapta.

### Cuerpo (JSON)
| Campo          | Tipo              | Req. | Descripción |
| -------------- | ----------------- | ---- | ----------- |
| `type`         | `ContactTypeEnum` | No   | Cambiar tipo (`email`, `telephone`, `whatsapp`, `telegram`). |
| `email`        | `email`           | No   | Email (para `email`) o usar `value`. |
| `value`        | `email`           | No   | Alternativa a `email` cuando `type = email`. |
| `country_code` | `string`          | No   | `+` y dígitos; se ignoran no dígitos. |
| `number`       | `string`          | No   | Solo dígitos; se ignoran no dígitos. |
| `uuid`         | —                 | —    | Prohibido (no se puede cambiar el usuario). |

### Respuestas
- `200 OK` con el recurso actualizado.

### Error de Validación (422)
Ejemplo al enviar campos inválidos o intentar cambiar un atributo prohibido.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": ["The email must be a valid email address."],
    "uuid": ["The uuid field is prohibited."]
  }
}
```

### No Encontrado (404)
Cuando el contacto no existe.
```json
{ "message": "Not Found" }
```

### No Autenticado (401)
Token ausente o inválido.
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
Autenticado pero sin permiso para actualizar.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Para contactos telefónicos, `country_code` y `number` se normalizan; `country_code` se guarda con `+`.
- La forma de retorno depende de `type`: `{ "email": "..." }` o `{ "<type>": { country_code, number, full } }`.
- Localización: si existen campos traducibles en datos relacionados, sus valores siguen el encabezado `Accept-Language` cuando esté disponible; en caso contrario, se devuelven valores por defecto.
