# Shared – Actualizar Mi Perfil

## Endpoint

`PUT /api/v1/me/profile`

## Autenticación

Requiere token Bearer válido.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Locale para mensajes de validación. |

## Cuerpo (application/json)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ---------- | ----------- |
| `name` | `string` | No | Nombre completo (máx. 255). |
| `gender` | `string` | No | Acepta `m`/`f`/`o` o valores normalizados `male`/`female`/`other`. |
| `birth_date` | `date` | No | Fecha de nacimiento (ISO 8601). |
| `email` | `email` | No | Correo electrónico (máx. 255). |

Notas
- El servidor normaliza `gender`: `male`→`m`, `female`→`f`, otros→`o`.
- Solo los campos enviados se actualizan; el resto permanece sin cambios.

## Ejemplo de Solicitud

```json
{
  "name": "Juan Actualizado",
  "gender": "male",
  "birth_date": "1990-05-12",
  "email": "juan.actualizado@example.com"
}
```

## Éxito `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "encoded_uuid",
    "name": "Juan Actualizado",
    "email": "juan.actualizado@example.com",
    "email_verified_at": null,
    "slug": "juan-actualizado",
    "avatar": {"url": "https://cdn.example.com/users/juan/avatar.webp", "usage": "avatar"},
    "profile_image": null,
    "age": 35,
    "gender": "m",
    "gender_name": "male",
    "birthday": "1990-05-12T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "es-ES",
    "currency": "EUR",
    "created_at": "2024-01-01T12:00:00+00:00",
    "roles": [
      {"id": 9, "uuid": "role-guest-uuid", "name": "guest", "localized_name": "Invitado", "permissions": ["store.complaint"]}
    ],
    "telephone": "+34 600 000 000",
    "contacts": {"email": {"uuid": "c1", "name": "email", "email": "juan.actualizado@example.com", "phone": null}},
    "social_medias": {"linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/juan"}},
    "address": {"uuid": "addr-uuid", "main": true, "zipcode": "28001", "one": "Calle Mayor", "formatted": "Calle Mayor, Madrid, 28001, ES"},
    "nationalities": [{"id": 724, "country_id": 724, "country": {"name": "España", "code": "ES", "flag": "🇪🇸"}}],
    "biography": {"uuid": "bio-uuid", "name": "Juan Actualizado", "summary": "Desarrollador de software..."},
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": null,
    "identities": []
  }
}
```

## Estructura JSON (resumen)

Sigue la misma estructura de “Obtener Perfil de Usuario”. Principales campos mostrados arriba.

## Errores

- 401 Unauthorized — token ausente/inválido
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
- [Actualizar Idioma del Usuario](./UserLanguageUpdate.md)
- [Actualizar Moneda del Usuario](./UserCurrencyUpdate.md)
