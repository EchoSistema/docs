<!-- markdownlint-disable MD013 -->

# Authentication – Autenticar Usuario

## Endpoint

`POST /api/v1/auth`

Realiza la autenticación de un usuario.

---

## Autenticación

Basic Auth. Envíe el encabezado `Authorization: Basic <credenciales>`, donde `<credenciales>` es el resultado de `base64(usuario:contraseña)`.

---

## Solicitud

### Encabezados Obligatorios

| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Clave pública de la plataforma. |
| **Accept-Language** | `string` | Idioma de la aplicación (ej.: `en`, `pt-BR`). Opcional. |
| **Authorization** | `string` | Credenciales en Basic Auth. |

### Cuerpo

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `device` | `string` | Sí | Identificador del dispositivo de acceso. |

---

## Ejemplo de Solicitud

```http
POST /api/v1/auth HTTP/1.1
Authorization: Basic am9hbzpzZW5oYQ==
X-PUBLIC-KEY: 00000000-0000-0000-0000-000000000000
Accept-Language: es
Content-Type: application/json

{
  "device": "Mozilla/5.0 (Windows NT 10.0)"
}
```

---

## Ejemplo de Respuesta

```json
{
  "message": "¡Usuario autenticado con éxito!",
  "token": "0001|tokenEjemplo",
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e1c1h1o1111111111111111111111111111",
      "name": "Usuario Ejemplo",
      "email": "usuario.ejemplo@dominio.com",
      "avatar": {
        "url": "https://example.com/avatar.png",
        "usage": "avatar"
      },
      "language": "es",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "22222222-2222-2222-2222-222222222222",
            "name": "Plataforma Ejemplo",
            "public_key": "33333333-3333-3333-3333-333333333333"
          },
          "name": "guest",
          "localized_name": "Invitado",
          "permissions": [
            {
              "subject": "complaint",
              "action": "store"
            }
          ]
        }
      ]
    }
  }
}
```

---

## Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `message` | `string` | Mensaje de éxito. |
| `token` | `string` | Token de acceso. |
| `data.user.uuid` | `uuid` | Identificador del usuario. |
| `data.user.echo_uuid` | `uuid` | Identificador interno. |
| `data.user.name` | `string` | Nombre del usuario. |
| `data.user.email` | `string` | Correo electrónico del usuario. |
| `data.user.avatar.url` | `string` | URL del avatar. |
| `data.user.avatar.usage` | `string` | Uso de la imagen. |
| `data.user.language` | `string` | Idioma preferido. |
| `data.user.roles[]` | `array` | Roles del usuario. |
| `data.user.roles[].name` | `string` | Nombre del rol. |
| `data.user.roles[].localized_name` | `string` | Nombre localizado del rol. |
| `data.user.roles[].permissions[]` | `array` | Permisos del rol. |
| `data.user.roles[].permissions[].subject` | `string` | Dominio del permiso. |
| `data.user.roles[].permissions[].action` | `string` | Acción permitida. |

---

## Notas

* El token debe enviarse en futuras solicitudes autenticadas.
* El campo `device` ayuda a identificar el dispositivo de acceso.

<!-- markdownlint-enable MD013 -->
