# Learn – Mostrar Grupo de Categoría Learn

## Endpoint

```
GET /api/v1/learn/category-groups/{categoryGroup}
```

## Autenticación

Ninguna

## Encabezados

| Encabezado      | Tipo   | Requerido | Descripción |
| --------------- | ------ | --------- | ----------- |
| Authorization   | string | No        | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sí        | Clave pública de la plataforma. |
| Accept-Language | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro     | Tipo   | Requerido | Descripción |
| ------------- | ------ | --------- | ----------- |
| categoryGroup | string | Sí        | Identificador de recurso del grupo de categoría |

> El parámetro `categoryGroup` usa vinculación de modelo de ruta con el campo `resource`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/category-groups/nombre-recurso-grupo"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "770e8400-e29b-41d4-a716-446655440002",
    "resource": "nombre-recurso-grupo",
    "domain_area_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "title": {
      "language": "es",
      "value": "Nombre del Grupo de Categoría",
      "is_default": false
    }
  }
}
```

## Estructura JSON Explicada

| Campo                   | Tipo    | Descripción |
| ----------------------- | ------- | ----------- |
| data                    | object  | Detalles del grupo de categoría |
| data.uuid               | string  | Identificador único del grupo de categoría |
| data.resource           | string  | Nombre del recurso del grupo de categoría |
| data.domain_area_uuid   | string  | UUID del área de dominio asociada |
| data.title              | object  | Título localizado del grupo de categoría |
| data.title.value        | string  | Texto del título |
| data.title.language     | string  | Código de idioma del título |
| data.title.is_default   | boolean | Si este es el título predeterminado |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado - Grupo de categoría no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Grupo de categoría no encontrado"
}
```

## Notas

- Retorna un único grupo de categoría por su identificador de recurso
- El título está localizado según el encabezado `Accept-Language`
- Si un título localizado no está disponible, se retorna el título predeterminado

## Relacionados

- [Listar Grupos de Categorías Learn](./LearnCategoryGroupIndex.md)
- [Listar Categorías Learn](./LearnCategoryIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
