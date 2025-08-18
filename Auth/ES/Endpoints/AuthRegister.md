# Auth – Registrar Usuario

## Endpoint

`POST auth/register`

Registra un nuevo usuario en la plataforma.

---

## Autenticación

Ninguna. Aun así, se debe enviar el encabezado `X-PUBLIC-KEY`.

---

## Solicitud

### Encabezados Requeridos

| Encabezado | Tipo | Descripción |
| --------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Clave pública de la plataforma necesaria para autenticar y recibir respuestas. |
| **Accept-Language** | `string` | Idioma para mensajes de error (ej.: `es`). Opcional. |

### Cuerpo

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `collaborator` | `boolean` | No | Indica si el usuario es colaborador. |
| `device` | `string` | Sí | Dispositivo utilizado para el registro. |
| `name` | `string` | Sí | Nombre del usuario. |
| `platform` | `mixed` | No | Plataforma asociada. |
| `gender` | `UserGenderEnum` | Condicional | Obligatorio con `collaborator`. |
| `birth_date` | `date` | Condicional | Obligatorio con `collaborator`. |
| `language` | `LanguageEnum` | No | Idioma preferido del usuario. |
| `currency` | `CurrencyEnum` | No | Moneda del usuario. |
| `email` | `string` | Sí | Correo válido y único en la plataforma. |
| `password` | `string` | Sí | Contraseña (mínimo 8 caracteres). |
| `password_confirmation` | `string` | Sí | Debe coincidir con `password`. |
| `roles` | `array` | Condicional | Roles del usuario; obligatorio con `collaborator`. |
| `roles.*` | `RoleEnum` | Condicional | Cada rol debe ser válido. |
| `nationalities` | `array` | Condicional | Nacionalidades del usuario; obligatorio con `collaborator`. |
| `address` | `array` | Condicional | Dirección del usuario; obligatorio con `collaborator`. |
| `address.city` | `string` | No | Ciudad. |
| `address.state` | `string` | No | Estado. |
| `address.country` | `string` | No | País. |
| `address.city_id` | `integer` | No | ID de la ciudad. |
| `address.state_id` | `integer` | No | ID del estado. |
| `address.country_id` | `integer` | Condicional | ID del país; obligatorio con `collaborator`. |
| `address.zipcode` | `string` | No | Código postal. |
| `address.address_one` | `string` | No | Dirección línea 1. |
| `address.address_two` | `string` | No | Dirección línea 2. |
| `address.address_three` | `string` | No | Dirección línea 3. |
| `address.address_four` | `string` | No | Dirección línea 4. |
| `address.addressable` | `string` | No | Referencia adicional. |
| `address.type` | `AddressTypeEnum` | No | Tipo de dirección. |
| `contacts` | `array` | Condicional | Contactos del usuario; obligatorio con `collaborator`. |
| `contacts.*.type` | `ContactTypeEnum` | Sí | Tipo de contacto. |
| `contacts.*.value` | `string` | No | Valor del contacto (ej.: correo). |
| `contacts.*.country_code` | `string` | No | Código de país. |
| `contacts.*.number` | `string` | No | Número de teléfono. |
| `contacts.*.contactable` | `string` | Sí | Referencia del contacto. |

### Ejemplo de Cuerpo

```json
{
  "collaborator": true,
  "device": "web",
  "name": "Usuario Ejemplo",
  "gender": "male",
  "birth_date": "1990-01-01",
  "language": "es",
  "currency": "USD",
  "email": "usuario@example.com",
  "password": "ContrasenaFuerte123",
  "password_confirmation": "ContrasenaFuerte123",
  "roles": ["guest"],
  "nationalities": ["USA"],
  "address": {
    "city": "Madrid",
    "state": "MD",
    "country": "España",
    "country_id": 724,
    "zipcode": "00000",
    "address_one": "Calle Ejemplo",
    "type": "residential"
  },
  "contacts": [
    {
      "type": "email",
      "value": "usuario@example.com",
      "contactable": "user"
    }
  ]
}
```

---

## Respuestas

### Éxito `201 Created`

```json
{
  "message": "Usuario Ejemplo registrado con éxito.",
  "token": "0000|tokenEjemplo",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e000000000000000000000000000000000000",
      "name": "Usuario Ejemplo",
      "email": "usuario@example.com",
      "avatar": {
        "url": "https://storage.example.com/avatar_thumbnail.webp",
        "usage": "avatar"
      },
      "language": "es",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "11111111-2222-3333-4444-555555555555",
            "name": "Plataforma Ejemplo",
            "public_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
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

### Error `400 Bad Request`

Retorna un objeto con mensajes de error de validación.

---
