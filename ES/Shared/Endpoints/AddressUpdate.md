# Direcciones — Actualizar

## Endpoint

`PUT /ia/admin/addresses/{address}` (IA Admin)

Actualiza una dirección existente. Solo se modifican los campos enviados; `uuid` está prohibido.

---

## Autenticación

`auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma (p. ej., `es`, `pt-BR`). Opcional. |

---

## Cuerpo (JSON)
| Campo           | Tipo              | Req. | Descripción |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Tipo de dirección. |
| `city`          | `string|object`   | No   | Referencia de ciudad o usar IDs. |
| `city_id`       | `integer`         | No   | ID de ciudad. |
| `state_id`      | `integer`         | No   | ID de estado. |
| `country_id`    | `integer`         | No   | ID de país. |
| `zipcode`       | `string`          | No   | Código postal. |
| `address_one`   | `string`          | No   | Línea 1. |
| `address_two`   | `string`          | No   | Línea 2. |
| `address_three` | `string`          | No   | Línea 3. |
| `address_four`  | `string`          | No   | Línea 4. |
| `uuid`          | —                 | —    | Prohibido. |

### Éxito (200)
Devuelve el recurso actualizado.

### Error de Validación (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field is prohibited."], "city_id": ["The city id must be an integer."], "zipcode": ["The zipcode must be a string."] } }
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
- Localización: nombres de ciudad/estado/país pueden seguir `Accept-Language`.

