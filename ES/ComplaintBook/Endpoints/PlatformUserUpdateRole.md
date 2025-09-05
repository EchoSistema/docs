# Libro de Quejas – Usuarios (Actualizar Rol)

## Endpoint

`PUT /api/v1/complaint-book/users/{user}/roles/{role}`

Actualiza el rol del usuario para la plataforma actual y/o el estado del rol.

---

## Autenticación

Obligatoria – Token Bearer con habilidad `backoffice` y `domain:reputationbook`.

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
| `user` | `int|uuid|string` | Sí | Identificador del usuario. |
| `role` | `uuid|int|string` | Sí | Identificador del rol (UUID, ID o nombre). |

### Body (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `status` | `string` | No | Uno de: `approved`, `disapproved`, `requested`. |

---

## Notas

- Usa PUT y actúa como upsert: crea el vínculo usuario↔rol cuando no existe; actualiza el estado cuando existe. Idempotente para el mismo payload.
