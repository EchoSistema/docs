# Usuarios de la Plataforma — Actualizar

- Método: `PUT`
- Ruta: `/api/v1/ia/admin/users/{user:uuid}`
- Autenticación: `auth:sanctum`

## Resumen
Actualiza campos básicos del usuario.

## Cuerpo de la solicitud (cualquiera de)
- `name` (string)
- `email` (email, único)
- `gender` (enum)
- `birth_date` (date)
- `password` + `password_confirmation` (string, mínimo 8)

## Respuesta
`UserProfileResource` — mismo formato que Mostrar.
