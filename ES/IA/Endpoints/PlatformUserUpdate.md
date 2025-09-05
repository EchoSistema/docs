# IA – Usuarios Admin (Actualizar)

## Endpoint

`PUT /api/v1/ia/admin/users/{user:uuid}`

Actualiza los campos del perfil del usuario: `name`, `email`, `gender`, `birth_date` y `password`.

---

## Autenticación

Obligatoria – Token Bearer con habilidad `backoffice`.

---

## Headers

| Header | Tipo | Obligatorio | Descripción |
| ------ | ---- | ----------- | ----------- |
| Authorization | `string` | Sí | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traducibles. |

---

## Parámetros

### Path

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| `user` | `uuid` | Sí | UUID del usuario. |

### Body (JSON)

Se requiere al menos uno de los siguientes campos.

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `name` | `string` | No | Nombre completo. Máx. 255. |
| `email` | `string` | No | Email único; validado con `email` y `email:dns`. |
| `gender` | `string` | No | Uno de: `m`, `f`, `o`. |
| `birth_date` | `date` | No | Fecha ISO-8601. |
| `password` | `string` | No | Mín. 8 caracteres. Requiere `password_confirmation`. |
| `password_confirmation` | `string` | No | Debe coincidir con `password`. |

