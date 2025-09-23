# Contactos — Actualizar (IA Admin)

## Endpoint

`PUT /ia/admin/contacts/{contact}`

Actualiza un contacto existente.

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
| `type`         | `ContactTypeEnum` | No   | `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | No   | Para tipo `email` (o usar `value`). |
| `value`        | `email`           | No   | Alternativa a `email` para tipo `email`. |
| `country_code` | `string`          | No   | `+` y dígitos; se ignoran no dígitos. |
| `number`       | `string`          | No   | Solo dígitos. |
| `uuid`         | —                 | —    | Prohibido. |

### Éxito (200)
Devuelve el recurso actualizado con el mismo formato que la creación.

### Error de Validación (422)
```json
{ "message": "The given data was invalid.", "errors": { "email": ["The email must be a valid email address."], "uuid": ["The uuid field is prohibited."] } }
```

### No Encontrado (404)
```json
{ "message": "Not Found" }
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
- Localización: sigue `Accept-Language` cuando aplique.

