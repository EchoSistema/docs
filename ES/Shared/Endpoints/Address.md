# Direcciones — Crear/Actualizar

## Endpoints

- `POST /addresses` (Backoffice)
- `POST /ia/admin/addresses` (IA Admin)
- `PUT /ia/admin/addresses/{address}` (IA Admin)

Crea o actualiza direcciones. Los endpoints comparten las mismas reglas; `PUT` actualiza un recurso existente.

---

## Autenticación

- `POST /addresses`: `auth:sanctum` con `ability:backoffice`.
- `POST|PUT /ia/admin/addresses`: `auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma para campos traducibles (p. ej., `en`, `pt-BR`). Opcional. |

---

## POST /addresses y POST /ia/admin/addresses

Crea una dirección. Cuando se usan sin rutas anidadas, `uuid` es obligatorio para vincular a una entidad.

### Cuerpo (JSON)
| Campo           | Tipo              | Req. | Descripción |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Por defecto `both`. Uno de `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Requerido cuando no hay ruta `addressable` anidada. |
| `city`          | `string|object`   | Sí   | Referencia de ciudad; resuelta por pipeline. Puede ser nombre u objeto. |
| `address_one`   | `string`          | Sí   | Línea 1. |
| `address_two`   | `string`          | No   | Línea 2. |
| `address_three` | `string`          | No   | Línea 3. |
| `address_four`  | `string`          | No   | Línea 4. |
| `zipcode`       | `string`          | No   | Código postal. |

### Respuestas
- `200 OK` con el recurso creado.

### Ejemplo de Respuesta
```json
{
  "uuid": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "main": true,
  "zipcode": "01000-000",
  "one": "Av. Paulista, 1000",
  "two": null,
  "three": null,
  "four": null,
  "city": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP" },
  "state": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP", "region": "Sudeste" },
  "country": { "uuid": "...", "name": "Brasil", "official_name": "República Federativa de Brasil", "code": "BR" },
  "formatted": "Av. Paulista, 1000 - São Paulo/SP, 01000-000, Brasil"
}
```

### Error de Validación (422)
Ejemplo cuando faltan campos obligatorios o son inválidos.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "city": ["The city field is required."],
    "address_one": ["The address one field is required."]
  }
}
```

### No Autenticado (401)
Cuando falta un token válido de Sanctum.
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
Solo para `POST /addresses` cuando el usuario no tiene la habilidad `backoffice`.
```json
{ "message": "This action is unauthorized." }
```

Si `type` no es `both`, la respuesta incluye flags:
```json
{
  "uuid": "...",
  "main": false,
  "is_billing": true,
  "is_shipping": false,
  "is_fiscal": false,
  "zipcode": "..."
}
```

---

## PUT /ia/admin/addresses/{address}

Actualiza una dirección existente. Solo se cambian los campos enviados; no se permite modificar `uuid`.

### Cuerpo (JSON)
| Campo           | Tipo              | Req. | Descripción |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Tipo de dirección. |
| `city`          | `string|object`   | No   | Referencia de ciudad; también puede usar los IDs abajo. |
| `city_id`       | `integer`         | No   | ID de ciudad. |
| `state_id`      | `integer`         | No   | ID de estado. |
| `country_id`    | `integer`         | No   | ID de país. |
| `zipcode`       | `string`          | No   | Código postal. |
| `address_one`   | `string`          | No   | Línea 1. |
| `address_two`   | `string`          | No   | Línea 2. |
| `address_three` | `string`          | No   | Línea 3. |
| `address_four`  | `string`          | No   | Línea 4. |
| `uuid`          | —                 | —    | Prohibido (no cambia relación). |

### Respuestas
- `200 OK` con el recurso actualizado.

### Error de Validación (422)
Ejemplo al enviar campos inválidos o intentar modificar atributos prohibidos.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field is prohibited."],
    "city_id": ["The city id must be an integer."],
    "zipcode": ["The zipcode must be a string."]
  }
}
```

### No Encontrado (404)
Cuando la dirección no existe.
```json
{ "message": "Not Found" }
```

### No Autenticado (401)
Token ausente o inválido.
```json
{ "message": "Unauthenticated." }
```

### Prohibido (403)
Autenticado pero sin permiso para actualizar el recurso.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- La resolución de ciudad es por pipeline; se puede informar nombre/objeto o IDs directos.
- Cuando `type = both`, el recurso marca `main: true`; de lo contrario, flags indican roles de facturación/envío/fiscal.
- Localización: los nombres de ciudad, estado y país pueden mostrarse traducidos según el encabezado `Accept-Language` cuando existan traducciones; en caso contrario, se devuelven etiquetas por defecto.
