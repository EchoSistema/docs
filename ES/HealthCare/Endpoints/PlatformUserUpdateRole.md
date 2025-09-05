# Health-Care – Usuarios (Actualizar Rol)

## Endpoint

`PUT /api/v1/health-care/users/{user}/roles/{role}`

Actualiza el rol del usuario para la plataforma actual y/o el estado del rol.

---

## Autenticación

Obligatoria – Token Bearer con habilidad `backoffice` y `domain:healthcare`.

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
| `role` | `uuid, int, string` | Sí | Identificador del rol (UUID, ID o nombre). |

### Body (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `status` | `string` | No | Uno de: `approved`, `disapproved`, `requested`. |

---

## Ejemplo

```bash
curl -X PUT \
  "/api/v1/health-care/users/123/roles/admin" \
  -H "Authorization: Bearer {token}" \
  -H "X-PUBLIC-KEY: {public_key}" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{"status": "approved"}'
```

---

## Códigos HTTP

- 200, 401, 403, 404, 409, 422

---

## Notas

- Usa PUT y actúa como upsert: crea el vínculo usuario↔rol cuando no existe; actualiza el estado cuando existe. Idempotente para el mismo payload.
