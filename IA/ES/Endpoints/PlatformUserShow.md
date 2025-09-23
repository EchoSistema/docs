# Usuarios de la Plataforma — Mostrar

- Método: `GET`
- Ruta: `/api/v1/ia/admin/users/{user:uuid}`
- Autenticación: `auth:sanctum`

## Resumen
Devuelve el perfil del usuario. Opcionalmente incluye los roles del usuario dentro de la plataforma actual.

## Parámetros de consulta
- `role` (booleano) — cuando es verdadero, carga `platformRoles` limitados a la plataforma actual, ordenados por `role_id`.

## Respuesta
Campos de `UserProfileResource`:

```json
{
  "uuid": "...",
  "name": "...",
  "email": "...",
  "avatar": "...",
  "roles": [
    { "id": 5, "uuid": "...", "name": "guest", "localized_name": "Guest", "permissions": ["..."] }
  ],
  "telephone": "+1 ...",
  "gender": "...",
  "birthday": "2021-01-02T00:00:00Z",
  "address": { "city": "...", "state": "...", "country": { "name": "...", "code": "...", "flag": "..." } },
  "nationalities": [ { "uuid": "...", "country": { "name": "...", "code": "...", "flag": "..." } } ]
}
```

Notas:
- `roles` solo se incluye cuando `role=true`.
- Dirección y nacionalidades se incluyen cuando están presentes en el usuario.
