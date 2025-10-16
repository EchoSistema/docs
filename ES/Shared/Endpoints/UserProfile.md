# Shared – Obtener Perfil de Usuario

## Endpoint

```
GET /api/v1/me/profile
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

Este endpoint no tiene parámetros.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/me/profile"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "uuid_codificado",
    "name": "Juan Pérez",
    "email": "juan@ejemplo.com",
    "slug": "juan-perez",
    "avatar": "https://ejemplo.com/avatar.jpg",
    "age": 30,
    "gender": "M",
    "gender_name": "Masculino",
    "language": "es",
    "currency": "EUR",
    "roles": [
      {
        "id": 1,
        "uuid": "role-uuid",
        "name": "admin",
        "localized_name": "Administrador",
        "permissions": ["view", "create"]
      }
    ],
    "contacts": {
      "TELEPHONE": {"phone": "+1234567890"}
    },
    "biography": {
      "uuid": "bio-uuid",
      "slug": "juan-perez",
      "summary": "Desarrollador de software..."
    }
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data | object | Datos del perfil del usuario |
| data.uuid | string | Identificador único del usuario |
| data.echo_uuid | string | UUID codificado de EchoSistema |
| data.name | string | Nombre completo del usuario |
| data.email | string | Dirección de correo electrónico |
| data.slug | string\|null | Slug del usuario para URL |
| data.avatar | string | URL de la imagen de avatar |
| data.profile_image | string\|null | URL de la imagen de perfil |
| data.age | integer\|null | Edad del usuario |
| data.gender | string | Código de género (M/F/O) |
| data.gender_name | string | Nombre de género localizado |
| data.birthday | string\|null | Fecha de nacimiento (ISO 8601) |
| data.is_banned | boolean | Si el usuario está baneado |
| data.language | string | Idioma preferido del usuario |
| data.currency | string | Moneda preferida del usuario |
| data.roles | array\|null | Roles del usuario con permisos |
| data.telephone | string\|null | Número de teléfono principal |
| data.contacts | object\|null | Todos los contactos indexados por tipo |
| data.social_medias | object\|null | Enlaces de redes sociales indexados por plataforma |
| data.address | object\|null | Detalles de dirección del usuario |
| data.nationalities | array | Nacionalidades del usuario |
| data.biography | object\|null | Recurso completo de biografía |
| data.job_occupation | object\|null | Ocupación laboral actual |
| data.platform | object\|null | Información de la plataforma |
| data.affiliate | object\|null | Datos de afiliado |
| data.identities | array\|null | Documentos de identidad del usuario |
| data.raw | object\|null | Datos personalizados específicos de la plataforma |

## Estados HTTP

- 200: OK - Perfil recuperado exitosamente
- 401: No autorizado - Token inválido o faltante
- 403: Prohibido - Permisos insuficientes
- 404: No encontrado - Usuario no encontrado
- 500: Error interno del servidor

## Errores

### 401 No autorizado
```json
{
  "message": "No autenticado."
}
```

### 403 Prohibido
```json
{
  "message": "Esta acción no está autorizada."
}
```

## Notas

- Devuelve el perfil completo del usuario autenticado
- Todas las relaciones se cargan anticipadamente para un rendimiento óptimo
- Los datos de biografía incluyen relaciones anidadas (educaciones, habilidades, certificaciones, etc.)
- Los contactos y redes sociales están indexados por tipo/nombre para fácil acceso
- Use el encabezado `Accept-Language` para obtener valores de campos localizados

## Relacionados

- [Actualizar Idioma de Usuario](./UserRegionalInformationLanguageUpdate.md)
- [Actualizar Moneda de Usuario](./UserRegionalInformationCurrencyUpdate.md)

## Changelog

- 2025-10-16: Perfil expandido con biografía, contactos, redes sociales, identidades y datos de afiliado
