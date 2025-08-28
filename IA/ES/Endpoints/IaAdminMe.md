<!-- markdownlint-disable MD013 -->

# IA – Perfil de Administrador

## Endpoint

`GET /api/v1/ia/admin/me`

Devuelve el perfil del usuario autenticado para la plataforma actual.

---

## Autenticación

Token Bearer vía Sanctum.

---

## Solicitud

### Encabezados Obligatorios

| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| **Authorization** | `string` | Credencial Bearer {token}. |
| **X-PUBLIC-KEY** | `string` | Clave pública que identifica la plataforma. |
| **Accept-Language** | `string` | Idioma para campos traducibles. Opcional. |

No se admiten parámetros de consulta.

---

## Ejemplo de Respuesta

```json
{
  "id": 123,
  "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
  "name": "Jane Doe",
  "gender": "female",
  "birth_date": "1990-05-12",
  "email": "jane@example.com",
  "roles": ["admin"],
  "permissions": ["store.company"],
  "contacts": {
    "public_email": {
      "email": "jane.public@example.com",
      "type": "public_email"
    },
    "whatsapp": {
      "ddi": "+55",
      "ddd": "11",
      "number": "999999999",
      "full": "+55 11 99999-9999",
      "type": "whatsapp"
    }
  },
  "platform": {"id": 1, "name": "PlatformName"},
  "images": {
    "avatar": {
      "uuid": "img-uuid",
      "unique_id": "a1b2c3",
      "usage": "avatar",
      "url": "https://cdn.example.com/avatar.jpg",
      "name": "Avatar",
      "slug": "avatar",
      "width": 512,
      "height": 512,
      "creation_date": "2025-01-01T12:00:00Z"
    }
  }
}
```

---

## Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `id` | `integer` | ID del usuario. |
| `uuid` | `uuid` | Identificador del usuario. |
| `name` | `string` | Nombre completo. |
| `gender` | `string` | Género. |
| `birth_date` | `date` | Fecha de nacimiento. |
| `email` | `string` | Correo electrónico de contacto. |
| `roles[]` | `array` | Roles asignados dentro de la plataforma. |
| `permissions[]` | `array` | Permisos concedidos. |
| `contacts` | `object` | Información de contacto pública organizada por tipo. |
| `platform` | `object` | Resumen de la plataforma (id, nombre). |
| `images` | `object` | Imágenes cargadas organizadas por uso. |

---

## Notas

* `401 Unauthorized` cuando falta o es inválido el token.
* `403 Forbidden` cuando el usuario no posee un rol distinto de invitado en la plataforma.

<!-- markdownlint-enable MD013 -->
