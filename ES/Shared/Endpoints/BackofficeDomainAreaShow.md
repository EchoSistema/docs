# Shared – Mostrar Área de Dominio

## Endpoint

```
GET /api/v1/backoffice/domain-areas/{domainArea}
```

## Autenticación

Obligatoria – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción                                  |
| ---------------- | ------ | ----------- | -------------------------------------------- |
| Authorization    | string | Sí          | Credencial `Bearer {token}`.                 |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma.              |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`).     |

## Parámetros de Ruta

| Parámetro    | Tipo   | Obligatorio | Descripción                      |
| ------------ | ------ | ----------- | -------------------------------- |
| domainArea   | string | Sí          | UUID del área de dominio.        |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas/efa68660-4870-51c2-84be-736f87792f1d"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
    "name": "Article",
    "slug": "article",
    "is_active": true,
    "platforms": [
      {
        "uuid": "550e8400-e29b-41d4-a716-446655440000",
        "domain": {
          "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
          "name": "Article"
        },
        "name": "My Platform",
        "email": "platform@example.com",
        "open_to_register": true,
        "public_key": "pk_test_123456",
        "logo": {
          "main": "https://cdn.example.com/logo.png",
          "square": "https://cdn.example.com/logo-square.png",
          "rounded": "https://cdn.example.com/logo-rounded.png",
          "rectangular": "https://cdn.example.com/logo-rect.png"
        },
        "created_at": "2024-01-01T00:00:00+00:00"
      }
    ],
    "categories": [
      {
        "uuid": "123e4567-e89b-12d3-a456-426614174000",
        "title": "Technology",
        "title_is_default": true,
        "title_language": "en",
        "slug": "technology"
      }
    ],
    "item_types": [
      {
        "uuid": "987fcdeb-51a2-43f7-8543-123456789012",
        "default_title": true,
        "language": "en",
        "title": "Blog Post",
        "slug": "blog-post"
      }
    ]
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción                                         |
| ------------------------------ | ------- | --------------------------------------------------- |
| data                           | object  | Datos del área de dominio                           |
| data.id                        | integer | ID interno del área de dominio                      |
| data.uuid                      | string  | UUID único del área de dominio                      |
| data.name                      | string  | Nombre del área de dominio (formateado)             |
| data.slug                      | string  | Slug identificador del área de dominio              |
| data.is_active                 | boolean | Indica si el área de dominio está activa            |
| data.platforms                 | array   | Lista de plataformas asociadas                      |
| data.platforms[].uuid          | string  | UUID único de la plataforma                         |
| data.platforms[].domain        | object  | Datos del dominio de la plataforma                  |
| data.platforms[].name          | string  | Nombre de la plataforma                             |
| data.platforms[].email         | string  | Email de la plataforma                              |
| data.platforms[].open_to_register | boolean | Indica si está abierta para registro            |
| data.platforms[].public_key    | string  | Clave pública de la plataforma                      |
| data.platforms[].logo          | object  | URLs de los logos en diferentes formatos            |
| data.platforms[].created_at    | string  | Fecha de creación (ISO 8601)                        |
| data.categories                | array   | Lista de categorías del área de dominio             |
| data.categories[].uuid         | string  | UUID único de la categoría                          |
| data.categories[].title        | string  | Título de la categoría                              |
| data.categories[].title_is_default | boolean | Indica si es el título por defecto             |
| data.categories[].title_language | string | Idioma del título                                  |
| data.categories[].slug         | string  | Slug de la categoría                                |
| data.item_types                | array   | Lista de tipos de ítem del área de dominio          |
| data.item_types[].uuid         | string  | UUID único del tipo de ítem                         |
| data.item_types[].default_title | boolean | Indica si es el título por defecto                 |
| data.item_types[].language     | string  | Idioma del título                                   |
| data.item_types[].title        | string  | Título del tipo de ítem                             |
| data.item_types[].slug         | string  | Slug del tipo de ítem                               |

## Estados HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido (sin permisos adecuados)
- 404: Área de dominio no encontrada
- 429: Límite de solicitudes excedido
- 500: Error interno

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Muestra un área de dominio específica con todas las plataformas, categorías y tipos de ítem asociados
- Requiere permiso `show.all` en contexto de backoffice
- El parámetro `{domainArea}` debe ser el UUID del área de dominio
- Las categorías y tipos de ítem incluyen sus títulos localizados

## Relacionados

- [Listar Áreas de Dominio](BackofficeDomainAreaIndex.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Documentación inicial
