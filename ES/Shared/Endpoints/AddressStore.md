# Direcciones — Crear

## Endpoint

`POST /addresses` (Backoffice)  |  `POST /ia/admin/addresses` (IA Admin)

Crea una dirección. Cuando no se usan rutas anidadas, `uuid` es obligatorio.

---

## Autenticación

- `POST /addresses`: `auth:sanctum` con `ability:backoffice`.
- `POST /ia/admin/addresses`: `auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma (p. ej., `es`, `pt-BR`). Opcional. |

---

## Cuerpo (JSON)
| Campo           | Tipo              | Req. | Descripción |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Por defecto `both`. `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Requerido cuando no hay ruta `addressable` anidada. |
| `city`          | `string|object`   | Sí   | Referencia de ciudad; resuelta por pipeline. |
| `address_one`   | `string`          | Sí   | Línea 1. |
| `address_two`   | `string`          | No   | Línea 2. |
| `address_three` | `string`          | No   | Línea 3. |
| `address_four`  | `string`          | No   | Línea 4. |
| `zipcode`       | `string`          | No   | Código postal. |

### Éxito (200)
Devuelve el recurso creado.

### Error de Validación (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "city": ["The city field is required."], "address_one": ["The address one field is required."] } }
```

### No Autenticado (401)
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
Solo para `POST /addresses` cuando falte la habilidad `backoffice`.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Resolución de ciudad por pipeline.
- `type = both` responde `main: true`; si no, flags específicas.
- Localización: nombres de ciudad/estado/país pueden seguir `Accept-Language`.

