# Usuarios de la Plataforma — Actualizar Rol

- Método: `PUT`
- Ruta: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Autenticación: `auth:sanctum`

## Resumen
Agrega o actualiza un rol de usuario dentro de la plataforma actual.

## Parámetros de ruta
- `user` — uuid
- `role` — acepta `id`, `uuid` o `name`

## Cuerpo de la solicitud
- `status` (opcional) — uno de `approved`, `disapproved`, `requested`

## Comportamiento
- Si el rol no existe para el usuario en esta plataforma, lo crea (por defecto `status=requested`).
- Si ya existe, actualiza el `status` (y asegura que `role_id` coincide con el rol indicado).

## Respuesta
`UserProfileResource` con `platformRoles` cargados.
