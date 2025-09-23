# IA – Usuarios Admin (Detalle)

## Endpoint

`GET /api/v1/ia/admin/users/{user:uuid}`

Devuelve el perfil detallado de un usuario de la plataforma.

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

## Parámetros

### Ruta

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| `user` | `uuid` | Sí | UUID del usuario. |

### Consulta

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| `role` | `boolean` | No | Cuando `true`, incluye los roles del usuario en la plataforma actual. |

---

## Ejemplo de Respuesta

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "avatar": "https://cdn.example.com/avatars/johndoe.webp",
  "roles": [
    {
      "id": 2,
      "uuid": "role-uuid",
      "name": "admin",
      "localized_name": "Administrador",
      "permissions": ["store.company"]
    }
  ],
  "telephone": "+1 202 555 0147",
  "gender": "male",
  "birthday": "1990-05-15T00:00:00+00:00",
  "address": {
    "country": {"name": "Estados Unidos", "code": "US", "flag": "🇺🇸"},
    "state": "CA",
    "city": "San Francisco",
    "zipcode": "94103"
  },
  "nationalities": [
    {
      "uuid": "natio-uuid",
      "country": {"name": "Estados Unidos", "code": "US", "flag": "🇺🇸"}
    }
  ]
}
```

---

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `uuid` | `uuid` | Identificador único del usuario. |
| `name` | `string` | Nombre completo. |
| `email` | `string` | Correo electrónico. |
| `avatar` | `string` | URL del avatar cuando está disponible. |
| `roles[]` | `array` | Presente cuando `role=true`; roles del usuario en la plataforma actual. |
| `telephone` | `string` | Teléfono cuando se carga. |
| `gender` | `string` | Género. |
| `birthday` | `string` | Fecha de nacimiento en ISO-8601. |
| `address` | `object` | Dirección cuando se carga. |
| `nationalities[]` | `array` | Nacionalidades con país. |

---

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado

---

## Notas

- Use `role=true` para incluir los roles y permisos del usuario en la plataforma actual.
- Respeta `Accept-Language` para campos traducibles.

---

## Relacionados

- docs/ES/IA/Endpoints/PlatformUserIndex.md
- docs/ES/IA/Endpoints/PlatformUserStore.md
