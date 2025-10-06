# Shared — Listar Usuarios de Plataformas (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/users
```

## Autenticación

Obligatoria – Bearer {token} con permiso `index.all`

## Encabezados

| Encabezado        | Tipo     | Obligatorio | Descripción |
| ----------------- | -------- | ----------- | ----------- |
| Authorization     | string   | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY      | string   | Sí          | Clave pública de la plataforma. |
| Accept-Language   | string   | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro     | Tipo    | Obligatorio | Descripción | Predeterminado/Valores |
| ------------- | ------- | ----------- | ----------- | ---------------------- |
| per_page      | integer | No          | Número de elementos por página. | `25` |
| page          | integer | No          | Página actual (cuando está paginado). | `1` |
| no_paginate   | boolean | No          | Si es `true`, devuelve todos los registros sin paginación. | `false` |

> Los parámetros aceptan variantes en `snake_case`, `camelCase` y `kebab-case`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.tu-dominio.com/api/v1/backoffice/users?per_page=25&page=1"
```

### Ejemplo de respuesta (200 - Paginado)

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo-550e8400-e29b-41d4-a716",
      "name": "Maria Silva",
      "gender": {
        "symbol": "F",
        "name": "Femenino"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00+00:00",
      "email": "maria.silva@example.com",
      "avatar": "https://cdn.example.com/avatars/maria.webp",
      "created_at": "2024-01-15T10:30:00+00:00",
      "roles": [
        {
          "id": 2,
          "main": true,
          "platform": "Echo Educación",
          "platform_uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
          "domain": "Educación",
          "role": "Admin",
          "language": "es",
          "currency": "EUR",
          "status": "active",
          "created_at": "2024-01-15T10:35:00+00:00"
        }
      ]
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/backoffice/users?page=1",
    "last": "https://api.example.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://api.example.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "path": "https://api.example.com/api/v1/backoffice/users",
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

### Ejemplo de respuesta (200 - Sin paginación)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.tu-dominio.com/api/v1/backoffice/users?no_paginate=true"
```

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo-550e8400-e29b-41d4-a716",
      "name": "Maria Silva",
      "gender": {
        "symbol": "F",
        "name": "Femenino"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00+00:00",
      "email": "maria.silva@example.com",
      "avatar": "https://cdn.example.com/avatars/maria.webp",
      "created_at": "2024-01-15T10:30:00+00:00",
      "roles": [...]
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                   | Tipo     | Descripción |
| ----------------------- | -------- | ----------- |
| data[]                  | array    | Lista de usuarios |
| data[].id               | integer  | ID interno del usuario |
| data[].echo_uuid        | string   | Identificador único Echo del usuario |
| data[].name             | string   | Nombre completo del usuario |
| data[].gender           | object   | Objeto con información de género |
| data[].gender.symbol    | string   | Símbolo del género (ej: `M`, `F`) |
| data[].gender.name      | string   | Nombre del género traducido |
| data[].age              | integer  | Edad calculada del usuario |
| data[].birth_date       | string   | Fecha de nacimiento (ISO 8601) |
| data[].email            | string   | Correo electrónico del usuario |
| data[].avatar           | string   | URL del avatar del usuario |
| data[].created_at       | string   | Fecha de creación (ISO 8601) |
| data[].roles[]          | array    | Lista de roles del usuario en plataformas |
| data[].roles[].id       | integer  | ID del rol |
| data[].roles[].main     | boolean  | Si es la plataforma principal del usuario |
| data[].roles[].platform | string   | Nombre de la plataforma |
| data[].roles[].platform_uuid | string | UUID de la plataforma |
| data[].roles[].domain   | string   | Área de dominio de la plataforma |
| data[].roles[].role     | string   | Nombre del rol/cargo |
| data[].roles[].language | string   | Idioma de la plataforma (ej: `pt-BR`) |
| data[].roles[].currency | string   | Moneda de la plataforma (ej: `BRL`, `USD`) |
| data[].roles[].status   | string   | Estado del rol (ej: `active`, `inactive`) |
| data[].roles[].created_at | string | Fecha de creación del rol (ISO 8601) |
| links                   | object   | Enlaces de paginación (cuando está paginado) |
| meta                    | object   | Metadatos de paginación (cuando está paginado) |

**Nota sobre error tipográfico:** Existe un error tipográfico en el código (`staus` en lugar de `status`) en `BackofficeUserResource.php:38`. El campo devuelto es `staus`.

## Estado HTTP

- 200: Éxito
- 401: No autenticado
- 403: Prohibido (sin permiso `index.all`)
- 429: Demasiadas solicitudes
- 500: Error interno

## Errores

### 403 Forbidden
El usuario no tiene el permiso necesario:

```json
{
  "message": "Forbidden"
}
```

### 401 Unauthorized
Token ausente o inválido:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Este endpoint devuelve usuarios con carga anticipada de: `regionalInformation`, `identities`, `telephone`, `avatarImage`, `profileImage`, `platform`, `platformRoles`, `platformRoles.platform`
- La respuesta usa `BackofficeUserCollection` y `BackofficeUserResource` para el formato
- Cuando `is_index=true` (predeterminado para este endpoint), campos extra como `updated_at`, `language`, `currency` y `telephone` del usuario **no** están incluidos en la respuesta (ver `BackofficeUserResource.php:41-48`)
- El parámetro `no_paginate=true` debe usarse con cuidado en entornos con muchos usuarios
- El campo `main` en `roles[]` indica si esa plataforma es la plataforma principal del usuario
- Existe un error tipográfico en el Resource: el campo devuelto es `staus` en lugar de `status` (línea 38 de BackofficeUserResource.php)

## Relacionados

- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Listar Áreas de Dominio](./BackofficeDomainAreaIndex.md)
- [Listar Roles](./RoleIndex.md)

## Changelog

- 2025-10-05: Refactorización completa de la documentación basada en el código real (BackofficeUserResource y BackofficeUserCollection)
- 2025-10-04: Documento inicial
