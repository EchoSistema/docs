# Usuarios de la Plataforma — Crear (Registro)

- Método: `POST`
- Ruta: `/api/v1/ia/admin/users`
- Autenticación: `auth:sanctum`

## Resumen
Registra un nuevo usuario en la plataforma actual usando el pipeline de registro. Devuelve los datos del usuario y, por defecto en este flujo, sin token.

## Cuerpo de la solicitud
Requeridos:
- `device` (string)
- `name` (string)
- `email` (email)
- `password` (string, mínimo 8)
- `password_confirmation` (string)

Opcional (común):
- `language` (enum)
- `currency` (enum)

Opcional (cuando `collaborator=true`):
- `collaborator` (bool)
- `gender` (enum)
- `birth_date` (date)
- `roles[]` (array de nombres; p. ej. `guest`, `support`)
- `address{...}` (ver reglas de validación)
- `contacts[]` con `type`/`value`/`country_code`/`number`
- `nationalities[]`

Notas:
- El controlador inyecta banderas: `recently_created=true`, `no_auth=true`, `registered_by=true`.
- Cuando `roles` incluye `guest`, los campos obligatorios de dirección se relajan.

## Respuesta
Formato de `UserAuthResource` (token omitido debido a `no_auth=true`):

```json
{
  "message": "User successfully registered.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "...",
      "echo_uuid": "...",
      "name": "...",
      "email": "...",
      "avatar": "...",
      "language": "en",
      "roles": [
        {
          "id": 5,
          "platform": { "uuid": "...", "name": "...", "public_key": "..." },
          "name": "guest",
          "localized_name": "Guest",
          "permissions": [ { "subject": "users", "action": "read" } ]
        }
      ],
      "registered_by": { }
    }
  }
}
```
