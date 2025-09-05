# IA – Usuarios Admin (Crear)

## Endpoint

`POST /api/v1/ia/admin/users`

Crea un nuevo usuario en la plataforma (área administrativa de IA).

---

## Autenticación

Requerida – Token Bearer con habilidad `backoffice`.

---

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traducibles (ej.: `pt-BR`, `en`, `es`). |

---

## Cuerpo de la Solicitud

Campos mínimos requeridos:

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `device` | `string` | Sí | Identificador del dispositivo/cliente. |
| `name` | `string` | Sí | Nombre del usuario. |
| `email` | `string` | Sí | Correo único en la plataforma. |
| `password` | `string` | Sí | Contraseña (mín. 8). |
| `password_confirmation` | `string` | Sí | Confirmación de contraseña. |

Campos adicionales (cuando `collaborator=true`, pasan a ser requeridos en conjunto):

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `collaborator` | `boolean` | No | Habilita el flujo de colaborador. |
| `gender` | `string` | Condicional | Género (enum). |
| `birth_date` | `date` | Condicional | Fecha de nacimiento. |
| `language` | `string` | No | Idioma del usuario (enum). |
| `currency` | `string` | No | Moneda (código ISO soportado). |
| `roles[]` | `array<string>` | Condicional | Roles (nombres válidos). |
| `nationalities[]` | `array` | Condicional | Nacionalidades. |
| `address` | `object` | Condicional | Dirección del usuario. |
| `contacts[]` | `array<object>` | Condicional | Contactos (correo/teléfono/whatsapp, etc.). |

Notas de normalización:

- `contacts[*].type` acepta nombre o `type`; se normaliza y `contactable` por defecto es `user`.
- `address.addressable` por defecto es `user`.

### Ejemplo de Solicitud

```json
{
  "device": "admin-panel",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "secret123",
  "password_confirmation": "secret123",
  "collaborator": true,
  "gender": "female",
  "birth_date": "1990-05-12",
  "roles": ["admin"],
  "address": {"country_id": 76},
  "contacts": [
    {"type": "public_email", "value": "jane.public@example.com"},
    {"type": "whatsapp", "country_code": "+55", "number": "11999999999"}
  ]
}
```

---

## Ejemplo de Respuesta

Nota: este endpoint no devuelve `token` (uso interno/admin); incluye `recently_created` y `registered_by`.

```json
{
  "message": "Usuario Jane Doe creado con éxito.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
      "echo_uuid": "echo-uuid",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "avatar": null,
      "language": "es",
      "roles": [
        {
          "id": 2,
          "platform": {"uuid": "plat-uuid", "name": "Platform", "public_key": "..."},
          "name": "admin",
          "localized_name": "Administrador",
          "permissions": [{"subject": "company", "action": "store"}]
        }
      ],
      "registered_by": {"uuid": "admin-uuid", "name": "Admin"}
    }
  }
}
```

---

## Estados HTTP

- 200: OK (creado)
- 401: No autorizado
- 403: Prohibido
- 422: Entidad no procesable

---

## Notas

- No se devuelve `token` ya que es un flujo de creación de panel/admin.
- El cuerpo es procesado por un pipeline de registro que normaliza contactos y dirección.

---

## Relacionados

- docs/ES/IA/Endpoints/PlatformUserIndex.md
- docs/ES/IA/Endpoints/PlatformUserShow.md
