# Usuarios de la Plataforma — Índice

- Método: `GET`
- Ruta: `/api/v1/ia/admin/users`
- Autenticación: `auth:sanctum`

## Resumen
Lista los usuarios de la plataforma actual, devolviendo registros de roles de plataforma con el usuario relacionado. Admite filtrado avanzado y paginación opcional.

## Parámetros de consulta
- Paginación: `page` (entero), `per_page` (entero, por defecto 25), `no_paginate` (booleano)
- Relaciones: `relations[]` — por defecto incluye `user`, `user.avatarImage`, `user.telephone`
- Localización: `language` — usado al solicitar relaciones de ocupación
- Filtros de rol:
  - `role` (id o nombre)
  - `roles[]` (array de ids o nombres)
  - o explícitos: `role_id`, `role_name`, `role_ids[]`, `role_names[]`
- Filtros de usuario: `name` o `user_name` (contiene), `email` o `user_email` (contiene), `user_uuid`
- Filtros de ocupación:
  - `job_occupation` (id | uuid | título) → normalizado en `job_occupation_id` | `job_occupation_uuid` | `job_occupation_title`
  - `occupation_area` (uuid | `content:usage`) o estructurado `occupation_area[content]`, `occupation_area[usage]`
  - Flags de conveniencia (derivadas automáticamente): `has_job_occupation`, `has_occupation_area`

## Comportamiento
- Excluye roles por debajo del rol mínimo del operador actual en la plataforma.
- Cuando `has_job_occupation` o `has_occupation_area` son verdaderos, carga `user.jobOccupations` (solo la predeterminada) con títulos localizados según `language`.

## Respuesta
Devuelve una colección en el formato estándar:

```json
{
  "data": [
    {
      "platform_id": 1,
      "user_id": 10,
      "role_id": 5,
      "status": "approved",
      "user": { "uuid": "...", "name": "...", "email": "...", "avatar": "..." }
    }
  ],
  "links": { "first": "...", "last": "...", "prev": null, "next": "..." },
  "meta": { "current_page": 1, "per_page": 25, "total": 123 }
}
```

Notas:
- `links`/`meta` aparecen cuando hay paginación; se omiten cuando `no_paginate=true`.
- Cada elemento es un registro `platform_user_roles`, posiblemente con las relaciones solicitadas.

## Ejemplos
- Básico: `GET /api/v1/ia/admin/users?per_page=25`
- Filtrar por nombre de rol: `GET ...?role=guest`
- Filtrar por múltiples ids de rol: `GET ...?roles[]=1&roles[]=2`
- Filtrar por email del usuario (contiene): `GET ...?email=@domain.com`
- Incluir ocupación y títulos localizados: `GET ...?job_occupation=Some Title&language=en`
