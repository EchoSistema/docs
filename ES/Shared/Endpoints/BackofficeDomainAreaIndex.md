# Shared – Listar Áreas de Dominio

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

No se requieren parámetros adicionales.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
      "name": "Article",
      "slug": "article",
      "is_active": true,
      "platforms_total": 5,
      "categories_total": 12,
      "item_types_total": 8
    },
    {
      "id": 2,
      "uuid": "70f8067c-7d68-50ef-b847-c43e3d6c0878",
      "name": "Bio",
      "slug": "bio",
      "is_active": true,
      "platforms_total": 3,
      "categories_total": 0,
      "item_types_total": 0
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                      | Tipo    | Descripción                                         |
| -------------------------- | ------- | --------------------------------------------------- |
| data                       | array   | Lista de áreas de dominio                           |
| data[].id                  | integer | ID interno del área de dominio                      |
| data[].uuid                | string  | UUID único del área de dominio                      |
| data[].name                | string  | Nombre del área de dominio (formateado)             |
| data[].slug                | string  | Slug identificador del área de dominio              |
| data[].is_active           | boolean | Indica si el área de dominio está activa            |
| data[].platforms_total     | integer | Cantidad total de plataformas asociadas             |
| data[].categories_total    | integer | Cantidad total de categorías asociadas              |
| data[].item_types_total    | integer | Cantidad total de tipos de ítem asociados           |

## Estados HTTP

- 200: OK
- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Lista todas las áreas de dominio con contadores de plataformas, categorías y tipos de ítem
- Endpoint exclusivo de backoffice que requiere permiso `index.all`
- Retorna todas las áreas de dominio de una vez (sin paginación)

## Relacionados

- [Mostrar Área de Dominio](BackofficeDomainAreaShow.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Actualización de la documentación con estructura completa de respuesta
- 2025-10-16: Documentación inicial
