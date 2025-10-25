# Shared – Listar Todos los Usuarios

## Endpoint

```
GET /api/v1/backoffice/users
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado    | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de Query

| Parámetro   | Tipo    | Requerido | Descripción |
| ----------- | ------- | --------- | ----------- |
| no_paginate | boolean | No        | Si es `true`, retorna todos los resultados sin paginación. |
| per_page    | integer | No        | Número de registros por página (predeterminado: 25). |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users?per_page=25"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
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
      ]
    },
    {
      "id": 1235,
      "echo_uuid": "echo_8c3d0b1a2e3f4g5h",
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "María Santos",
      "gender": {
        "symbol": "F",
        "name": "Femenino"
      },
      "age": 28,
      "birth_date": "1996-08-22T00:00:00Z",
      "email": "maria.santos@example.com",
      "avatar": "https://cdn.example.com/avatars/maria-santos.jpg",
      "created_at": "2024-01-20T15:45:00Z",
      "roles": [
        {
          "id": 3,
          "main": true,
          "platform": "Plataforma Inmobiliaria",
          "platform_uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "domain": "RealEstate",
          "role": "Agente",
          "language": "es",
          "currency": "EUR",
          "status": "active",
          "created_at": "2024-01-20T15:45:00Z"
        }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | array   | Lista de usuarios |
| data[].id   | integer | ID interno del usuario |
| data[].echo_uuid | string  | UUID de EchoSistema (formato: echo_*) |
| data[].uuid | string  | Identificador único del usuario |
| data[].name | string  | Nombre completo del usuario |
| data[].gender | object  | Información de género |
| data[].gender.symbol | string  | Símbolo del género (M, F, O) |
| data[].gender.name | string  | Nombre del género completo |
| data[].age  | integer | Edad calculada del usuario |
| data[].birth_date | string  | Fecha de nacimiento (ISO 8601) |
| data[].email | string  | Email del usuario |
| data[].avatar | string  | URL de la imagen de avatar |
| data[].created_at | string  | Fecha de creación de cuenta (ISO 8601) |
| data[].roles | array   | Lista de roles/funciones del usuario en diferentes plataformas |
| data[].roles[].id | integer | ID del rol |
| data[].roles[].main | boolean | Indica si es la plataforma principal del usuario |
| data[].roles[].platform | string  | Nombre de la plataforma |
| data[].roles[].platform_uuid | string  | UUID de la plataforma |
| data[].roles[].domain | string  | Área de dominio de la plataforma |
| data[].roles[].role | string  | Nombre del rol/función |
| data[].roles[].language | string  | Idioma configurado (locale IETF) |
| data[].roles[].currency | string  | Moneda configurada (código ISO) |
| data[].roles[].status | string  | Estado del rol (active, inactive, etc.) |
| data[].roles[].created_at | string  | Fecha de asignación del rol (ISO 8601) |
| links       | object  | Enlaces de paginación |
| meta        | object  | Metadatos de la paginación |

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

- Este endpoint retorna todos los usuarios de todas las plataformas del sistema
- Solo usuarios con permiso `index.all` de backoffice pueden acceder
- La respuesta incluye las siguientes relaciones cargadas:
  - `regionalInformation`: Información regional del usuario (idioma, moneda)
  - `identities`: Documentos de identidad del usuario
  - `telephone`: Teléfono principal
  - `avatarImage`: Imagen del avatar
  - `profileImage`: Imagen del perfil
  - `platform`: Plataforma principal del usuario
  - `platformRoles`: Todos los roles del usuario en diferentes plataformas
  - `platformRoles.platform`: Datos de las plataformas asociadas a los roles
- El campo `avatar` se genera dinámicamente a través del método `getAvatar()`
- El campo `age` se calcula automáticamente a partir de la fecha de nacimiento
- El campo `roles` consolida todos los roles del usuario con indicación de la plataforma principal
- Ordenado por defecto por fecha de creación

## Relacionados

- [Mostrar Detalles del Usuario](BackofficePlatformUserShow.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Actualización completa de documentación con estructura detallada y todos los campos
- 2025-10-16: Documentación inicial
