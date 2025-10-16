# Learn – Listar Categorías Learn

## Endpoint

```
GET /api/v1/learn/categories
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

### Parámetros de consulta

| Parámetro | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------ | --------- | ----------- | ---------------------- |
| language  | string | No        | Código de idioma para títulos localizados | Predeterminado de la plataforma |

> El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/categories?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "resource": "nombre-recurso-categoria",
      "domain_area": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Learn"
      },
      "group": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "resource": "nombre-recurso-grupo",
        "title": {
          "language": "es",
          "value": "Nombre del Grupo de Categoría"
        }
      },
      "title": {
        "language": "es",
        "value": "Nombre de la Categoría",
        "is_default": false
      }
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                | Tipo    | Descripción |
| -------------------- | ------- | ----------- |
| data[]               | array   | Lista de categorías |
| data[].uuid          | string  | Identificador único de categoría |
| data[].resource      | string  | Nombre del recurso de categoría |
| data[].domain_area   | object  | Información del área de dominio |
| data[].group         | object  | Información del grupo de categoría |
| data[].group.title   | object  | Título localizado del grupo |
| data[].title         | object  | Título localizado de categoría |
| data[].title.value   | string  | Texto del título |
| data[].title.language| string  | Código de idioma del título |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Clave de plataforma inválida"
}
```

## Notas

- Retorna todas las categorías para el área de dominio de la plataforma
- Las categorías incluyen su grupo asociado y área de dominio
- Los títulos están localizados según el parámetro `language` o encabezado `Accept-Language`
- Si un título localizado no está disponible, se retorna el título predeterminado

## Relacionados

- [Listar Grupos de Categorías Learn](./LearnCategoryGroupIndex.md)
- [Mostrar Grupo de Categoría Learn](./LearnCategoryGroupShow.md)

## Changelog

- 2025-10-16: Documentación inicial
