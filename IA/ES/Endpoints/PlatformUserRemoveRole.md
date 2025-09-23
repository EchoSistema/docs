# Usuarios de la Plataforma — Eliminar Rol

- Método: `DELETE`
- Ruta: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Autenticación: `auth:sanctum`

## Resumen
Elimina un rol del usuario dentro de la plataforma actual. Si es el único rol que tiene, se reemplaza por `GUEST` con `status=approved` (nunca se deja al usuario sin rol).

## Parámetros de ruta
- `user` — uuid
- `role` — acepta `id`, `uuid` o `name`

## Respuesta
`UserProfileResource` con `platformRoles` cargados.

## Notas
- Sin efecto si el usuario no posee el rol especificado en esta plataforma.
