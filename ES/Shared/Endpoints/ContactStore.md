# Contactos — Crear (IA Admin)

## Endpoint

`POST /ia/admin/contacts`

Crea un contacto de usuario (email o telefónico: telephone, whatsapp, telegram).

---

## Autenticación

Obligatoria — `auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma (p. ej., `es`, `pt-BR`). Opcional. |

---

## Cuerpo (JSON)
| Campo          | Tipo              | Req. | Descripción |
| -------------- | ----------------- | ---- | ----------- |
| `uuid`         | `uuid`            | Sí   | UUID del usuario a vincular. |
| `type`         | `ContactTypeEnum` | Sí   | `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | Cond | Requerido si `type = email` (o usar `value`). |
| `value`        | `email`           | Cond | Alternativa a `email` cuando `type = email`. |
| `country_code` | `string`          | Cond | Con `number` cuando `type` no es `email`. Acepta `+` y dígitos. |
| `number`       | `string`          | Cond | Dígitos cuando `type` no es `email`. |

### Ejemplos
Email:
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "email", "email": "john.doe@example.com" }
```
WhatsApp:
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "whatsapp", "country_code": "+55", "number": "11 91234-5678" }
```

### Éxito (200)
```json
{ "email": "john.doe@example.com" }
```
o
```json
{ "whatsapp": { "country_code": "+55", "number": "11912345678", "full": "+55 11912345678" } }
```

### Error de Validación (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "email": ["The email field is required when type is email."], "country_code": ["The country code field is required when type is not email."], "number": ["The number field is required when type is not email."] } }
```

### No Encontrado (404)
```json
{ "message": "Usuario no encontrado." }
```

### No Autenticado (401)
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Normalización de `country_code`/`number`; `country_code` se guarda con `+`.
- Localización vía `Accept-Language` cuando aplique.

