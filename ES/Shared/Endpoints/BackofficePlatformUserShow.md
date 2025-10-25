# Shared – Mostrar Detalles del Usuario

## Endpoint

```
GET /api/v1/backoffice/users/{user}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado    | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de URL

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| user      | string | Sí        | UUID, Echo UUID o ID del usuario a mostrar |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1234,
    "echo_uuid": "echo_9d4e1c2a3b4c5d6e",
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "name": "Juan Silva",
    "gender": {
      "symbol": "M",
      "name": "Masculino"
    },
    "age": 32,
    "birth_date": "1992-05-15T00:00:00Z",
    "email": "juan.silva@example.com",
    "avatar": "https://cdn.example.com/avatars/juan-silva.jpg",
    "currency": "EUR",
    "language": "es",
    "created_at": "2024-01-15T10:30:00Z",
    "roles": [
      {
        "id": 5,
        "main": true,
        "platform": "Plataforma E-commerce",
        "platform_uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "domain": "E-commerce",
        "role": "Admin",
        "language": "es",
        "currency": "EUR",
        "status": "active",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "id": 7,
        "main": false,
        "platform": "Plataforma Artículos",
        "platform_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "domain": "Articles",
        "role": "Editor",
        "language": "es",
        "currency": "EUR",
        "status": "active",
        "created_at": "2024-02-20T14:15:00Z"
      }
    ],
    "updated_at": "2024-10-15T08:20:00Z",
    "language": "es",
    "currency": "EUR",
    "telephone": "+34912345678",
    "slug": "juan-silva",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "email_verified_at": "2024-01-15T11:00:00Z",
    "platform": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "Plataforma E-commerce",
      "domain_area": "E-commerce"
    },
    "contacts": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "mobile",
        "country_code": "+34",
        "number": "912345678",
        "phone": "+34912345678",
        "email": null,
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "type": "email",
        "country_code": null,
        "number": null,
        "phone": null,
        "email": "juan.silva.alt@example.com",
        "created_at": "2024-02-10T14:20:00Z"
      }
    ],
    "social_medias": [
      {
        "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
        "name": "LinkedIn",
        "url": "https://linkedin.com/in/juansilva",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "3a8b5c6d-7e8f-9g0h-1i2j-3k4l5m6n7o8p",
        "name": "Twitter",
        "url": "https://twitter.com/juansilva",
        "created_at": "2024-01-15T10:30:00Z"
      }
    ],
    "address": {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "zipcode": "28001",
      "street": "Gran Vía",
      "number": "123",
      "complement": "Piso 5",
      "neighborhood": "Centro",
      "city": "Madrid",
      "state": "Madrid",
      "country": "España",
      "formatted": "Gran Vía, 123, Piso 5 - Centro, Madrid - Madrid, España, 28001"
    },
    "nationalities": [
      {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "country": "España"
      }
    ],
    "biography": {
      "uuid": "0a5b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "summary": "Profesional experimentado en gestión de e-commerce con más de 10 años de experiencia."
    },
    "job_occupation": {
      "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
      "occupation": "Gerente de E-commerce",
      "company": "Tech Solutions SL",
      "is_default": true
    },
    "job_occupations": [
      {
        "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
        "occupation": "Gerente de E-commerce",
        "company": "Tech Solutions SL",
        "is_default": true,
        "started_at": "2020-01-15T00:00:00Z",
        "ended_at": null
      },
      {
        "uuid": "8a3b0c1d-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "occupation": "Analista de Marketing",
        "company": "Marketing Corp",
        "is_default": false,
        "started_at": "2017-03-01T00:00:00Z",
        "ended_at": "2019-12-31T00:00:00Z"
      }
    ],
    "affiliate": {
      "uuid": "7a2b9c0d-1e2f-3g4h-5i6j-7k8l9m0n1o2p",
      "token": "aff_tkn_123456789abcdef",
      "created_at": "2024-01-15T10:30:00Z"
    },
    "identities": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "dni",
        "number": "12345678A",
        "verified_at": "2024-01-15T11:00:00Z"
      }
    ]
  }
}
```

## Estructura JSON Explicada

### Campos Base (siempre presentes)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | object  | Datos del usuario |
| data.id     | integer | ID interno del usuario |
| data.echo_uuid | string  | UUID de EchoSistema (formato: echo_*) |
| data.uuid   | string  | Identificador único del usuario |
| data.name   | string  | Nombre completo del usuario |
| data.gender | object  | Información de género |
| data.gender.symbol | string  | Símbolo del género (M, F, O) |
| data.gender.name | string  | Nombre del género completo |
| data.age    | integer | Edad calculada del usuario |
| data.birth_date | string  | Fecha de nacimiento (ISO 8601) |
| data.email  | string  | Email del usuario |
| data.avatar | string  | URL de la imagen de avatar |
| data.currency | string  | Moneda preferencial del usuario (código ISO) |
| data.language | string  | Idioma preferencial del usuario (locale IETF) |
| data.created_at | string  | Fecha de creación de cuenta (ISO 8601) |
| data.roles  | array   | Lista de roles/funciones del usuario en diferentes plataformas |

### Campos Adicionales (modo detallado)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.updated_at | string  | Fecha de última actualización (ISO 8601) |
| data.language | string  | Idioma preferencial del usuario (locale IETF) |
| data.currency | string  | Moneda preferencial (código ISO) |
| data.telephone | string  | Número de teléfono principal formateado |
| data.slug   | string  | Slug único del usuario |
| data.is_banned | boolean | Indica si el usuario está baneado |
| data.is_foreign | boolean | Indica si el usuario es extranjero |
| data.is_master | boolean | Indica si el usuario tiene privilegios master |
| data.email_verified_at | string  | Fecha de verificación del email (ISO 8601) |

### Plataforma (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.platform | object  | Datos de la plataforma principal |
| data.platform.uuid | string  | UUID de la plataforma |
| data.platform.name | string  | Nombre de la plataforma |
| data.platform.domain_area | string  | Área de dominio de la plataforma |

### Contactos (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.contacts | array   | Lista de contactos del usuario |
| data.contacts[].uuid | string  | UUID del contacto |
| data.contacts[].type | string  | Tipo de contacto (mobile, email, phone, etc.) |
| data.contacts[].country_code | string  | Código del país (ej: +34) |
| data.contacts[].number | string  | Número sin código del país |
| data.contacts[].phone | string  | Teléfono completo formateado |
| data.contacts[].email | string  | Email (cuando tipo es email) |
| data.contacts[].created_at | string  | Fecha de creación (ISO 8601) |

### Redes Sociales (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.social_medias | array   | Lista de redes sociales |
| data.social_medias[].uuid | string  | UUID de la red social |
| data.social_medias[].name | string  | Nombre de la red social |
| data.social_medias[].url | string  | URL del perfil |
| data.social_medias[].created_at | string  | Fecha de creación (ISO 8601) |

### Dirección (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.address | object  | Dirección del usuario |
| data.address.uuid | string  | UUID de la dirección |
| data.address.zipcode | string  | Código postal |
| data.address.street | string  | Nombre de la calle/avenida |
| data.address.number | string  | Número |
| data.address.complement | string  | Complemento |
| data.address.neighborhood | string  | Barrio |
| data.address.city | string  | Ciudad |
| data.address.state | string  | Estado/Provincia |
| data.address.country | string  | País |
| data.address.formatted | string  | Dirección completa formateada |

### Nacionalidades (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.nationalities | array   | Lista de nacionalidades |
| data.nationalities[].uuid | string  | UUID de la nacionalidad |
| data.nationalities[].country | string  | Nombre del país |

### Información de Baneo (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.banned_info | object  | Información sobre el baneo (solo si el usuario está baneado) |
| data.banned_info.reason | string  | Motivo del baneo |
| data.banned_info.banned_at | string  | Fecha del baneo (ISO 8601) |
| data.banned_info.until_date | string  | Fecha final del baneo (ISO 8601), null si es permanente |

### Biografía (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.biography | object  | Biografía del usuario |
| data.biography.uuid | string  | UUID de la biografía |
| data.biography.summary | string  | Texto de la biografía |

### Ocupación Profesional (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.job_occupation | object  | Ocupación profesional predeterminada actual |
| data.job_occupation.uuid | string  | UUID de la ocupación |
| data.job_occupation.occupation | string  | Nombre de la ocupación |
| data.job_occupation.company | string  | Nombre de la empresa |
| data.job_occupation.is_default | boolean | Indica si es la ocupación predeterminada |

### Historial de Ocupaciones (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.job_occupations | array   | Historial completo de ocupaciones |
| data.job_occupations[].uuid | string  | UUID de la ocupación |
| data.job_occupations[].occupation | string  | Nombre de la ocupación |
| data.job_occupations[].company | string  | Nombre de la empresa |
| data.job_occupations[].is_default | boolean | Indica si es la ocupación predeterminada |
| data.job_occupations[].started_at | string  | Fecha de inicio (ISO 8601) |
| data.job_occupations[].ended_at | string  | Fecha de término (ISO 8601), null si es actual |

### Afiliado (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.affiliate | object  | Información de afiliado (solo si el usuario es afiliado) |
| data.affiliate.uuid | string  | UUID del afiliado |
| data.affiliate.token | string  | Token de referencia del afiliado |
| data.affiliate.created_at | string  | Fecha de registro como afiliado (ISO 8601) |

### Documentos de Identidad (cuando disponible)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data.identities | array   | Lista de documentos de identidad |
| data.identities[].uuid | string  | UUID del documento |
| data.identities[].type | string  | Tipo de documento (dni, nie, passport, etc.) |
| data.identities[].number | string  | Número del documento |
| data.identities[].verified_at | string  | Fecha de verificación (ISO 8601) |

## Estados HTTP

- 200: OK
- 201: Creado
- 400: Solicitud incorrecta
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Error de validación
- 429: Demasiadas solicitudes
- 500: Error interno

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Este endpoint retorna todos los detalles de un usuario específico
- Solo usuarios con permiso `show.all` de backoffice pueden acceder
- El parámetro `{user}` acepta UUID, Echo UUID o ID del usuario
- La respuesta incluye las siguientes relaciones cargadas:
  - `regionalInformation`: Información regional (idioma, moneda)
  - `identities`: Documentos de identidad
  - `telephone`: Teléfono principal
  - `avatarImage`: Imagen del avatar
  - `profileImage`: Imagen del perfil
  - `platform`: Plataforma principal
  - `platform.domainArea`: Área de dominio de la plataforma principal
  - `platformRoles`: Todos los roles en plataformas
  - `platformRoles.platform`: Plataformas de los roles
  - `platformRoles.platform.domainArea`: Áreas de dominio de las plataformas
  - `platformRoles.role`: Detalles de los roles
  - `contacts`: Contactos del usuario
  - `socialMedias`: Redes sociales
  - `address`: Dirección completa con ciudad, estado y país
  - `nationalities`: Nacionalidades
  - `banned`: Información de baneo (si aplica)
  - `biography`: Biografía
  - `jobOccupation`: Ocupación profesional actual predeterminada
  - `jobOccupations`: Historial completo de ocupaciones
  - `affiliate`: Datos de afiliado (si aplica)
- Los campos condicionales aparecen solo cuando las relaciones están cargadas y tienen datos
- El campo `avatar` se genera dinámicamente a través del método `getAvatar()`
- El campo `age` se calcula automáticamente a partir de la fecha de nacimiento
- Los campos `currency` y `language` en la raíz son accesores que retornan datos de `regionalInformation`
- Los campos `currency` y `language` en "Campos Adicionales" sobrescriben los de la raíz con datos más específicos

## Relacionados

- [Listar Todos los Usuarios](BackofficePlatformUserIndex.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Documentación completa con todos los campos y relaciones detalladas
- 2025-10-16: Documentación inicial
