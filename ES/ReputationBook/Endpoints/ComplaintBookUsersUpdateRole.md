# ReputationBook – Usuarios del Libro de Reclamaciones (Actualizar Rol)

## Endpoint

```
PUT /api/v1/complaint-book/users/{user}/roles/{role}
```

Crea o actualiza la asignación de rol del usuario en la plataforma actual y, opcionalmente, cambia su estado.

---

## Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

---

## Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
| ---------- | ---- | ----------- | ----------- |
| `Authorization` | string | Sí | `Bearer {token}` válido. |
| `X-PUBLIC-KEY` | string | Sí | Identificador público de la plataforma. |
| `Accept-Language` | string | Recomendado | Idioma preferido para campos traducibles. |

---

## Parámetros

### Ruta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| `user` | uuid | Sí | UUID del usuario. |
| `role` | uuid \| integer \| string | Sí | Identificador del rol (UUID, ID interno o nombre canónico). |

### Cuerpo (JSON)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `status` | string | No | Nuevo estado de la asignación (`approved`, `disapproved`, `requested`). |

---

## Notas

- El método `PUT` actúa como upsert: crea el vínculo usuario↔rol cuando no existe y solo actualiza el estado cuando ya está presente.
- Es idempotente: enviar el mismo payload varias veces mantiene el mismo resultado.

