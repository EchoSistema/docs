# Shared – Actualizar Usuario (Admin)

## Endpoint

`PUT /api/v1/users/{user:uuid}`

## Autenticación

Requiere token Bearer con habilidad `backoffice` y permisos de plataforma.

Permisos
- Uno de: `update.guest`, `update.collaborator` o `update.all` en el rol de la plataforma actual.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Locale para mensajes de validación. |

## Parámetros de Ruta

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| `user` | `uuid` | Sí | Identificador del usuario destino. |

## Cuerpo (application/json)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ---------- | ----------- |
| `name` | `string` | No | Nombre completo (máx. 255). |
| `gender` | `string` | No | Acepta `m`/`f`/`o` o `male`/`female`/`other` (normalizado). |
| `birth_date` | `date` | No | Fecha de nacimiento (ISO 8601). |
| `email` | `email` | No | Correo electrónico (máx. 255). |

Notas
- El servidor normaliza `gender`: `male`→`m`, `female`→`f`, otros→`o`.
- Solo los campos enviados se actualizan; el resto permanece sin cambios.

## Ejemplo de Solicitud

```json
{
  "name": "María Admin",
  "gender": "female",
  "birth_date": "1988-09-20",
  "email": "maria.admin@example.com"
}
```

## Éxito `200 OK`

Devuelve el recurso completo de perfil del usuario (misma estructura que “Obtener Perfil de Usuario”).

```json
{
  "data": {
    "uuid": "11111111-2222-3333-4444-555555555555",
    "echo_uuid": "encoded_uuid",
    "name": "María Admin",
    "email": "maria.admin@example.com",
    "email_verified_at": null,
    "slug": "maria-admin",
    "avatar": {"url": "https://cdn.example.com/users/maria/avatar.webp", "usage": "avatar"},
    "profile_image": null,
    "age": 36,
    "gender": "f",
    "gender_name": "female",
    "birthday": "1988-09-20T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "es-ES",
    "currency": "EUR",
    "created_at": "2024-02-10T12:00:00+00:00",
    "roles": [
      {"id": 2, "uuid": "role-admin-uuid", "name": "administrator", "localized_name": "Administrador", "permissions": ["index.platform_message", "show.platform_message", "update.all"]}
    ],
    "telephone": "+34 600 000 001",
    "contacts": {"email": {"uuid": "c1", "name": "email", "email": "maria.admin@example.com", "phone": null}},
    "social_medias": {"linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/maria"}},
    "address": {"uuid": "addr-uuid", "main": true, "zipcode": "28001", "one": "Calle Mayor", "formatted": "Calle Mayor, Madrid, 28001, ES"},
    "nationalities": [{"id": 724, "country_id": 724, "country": {"name": "España", "code": "ES", "flag": "🇪🇸"}}],
    "biography": {"uuid": "bio-uuid", "name": "María Admin", "summary": "..."},
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": null,
    "identities": []
  }
}
```

## Estructura JSON (resumen)

Igual al endpoint “Actualizar Mi Perfil”. Consulte también “Obtener Perfil de Usuario”.

## Errores

- 401 Unauthorized — token ausente/inválido
- 403 Forbidden — permisos insuficientes o plataforma distinta
- 404 Not Found — usuario no encontrado
- 422 Unprocessable Entity — errores de validación
- 500 Internal Server Error

### Ejemplo 422

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "email": ["El campo email debe ser una dirección de correo válida."],
    "gender": ["El valor seleccionado para gender no es válido."],
    "birth_date": ["El campo birth date no es una fecha válida."]
  }
}
```

## Relacionados

- [Obtener Perfil de Usuario](./UserProfile.md)
- [Actualizar Mi Perfil](./UserProfileUpdate.md)
