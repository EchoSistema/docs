# Company – Listar Grupos de Categorías de Empresa

## Endpoint

```
GET /api/v1/company/category-groups
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro  | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ---------- | ------- | --------- | ----------- | ---------------------- |
| language   | string  | No        | Código de idioma para títulos (ej.: `en`, `pt-BR`, `es`). | Predeterminado de la plataforma |
| categories | boolean | No        | Incluir categorías dentro de los grupos. | `false` |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company/category-groups?language=es&categories=true"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "title": "Servicios de Tecnología",
      "group": {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "category": [
          {
            "uuid": "00000000-0000-0000-0000-000000000100",
            "title": "Desarrollo de Software"
          },
          {
            "uuid": "00000000-0000-0000-0000-000000000101",
            "title": "Soporte IT"
          }
        ]
      },
      "domain_area": {
        "uuid": "00000000-0000-0000-0000-000000000020",
        "name": "Tecnología"
      }
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                        | Tipo     | Descripción |
| ---------------------------- | -------- | ----------- |
| data[]                       | array    | Lista de grupos de categorías. |
| data[].uuid                  | string   | UUID del grupo de categorías. |
| data[].title                 | string   | Título del grupo en el idioma solicitado. |
| data[].group                 | object   | Detalles del grupo (cuando `categories=true`). |
| data[].group.uuid            | string   | UUID del grupo. |
| data[].group.category[]      | array    | Lista de categorías en el grupo. |
| data[].group.category[].uuid | string   | UUID de la categoría. |
| data[].group.category[].title| string   | Título de la categoría en el idioma solicitado. |
| data[].domain_area           | object   | Información del área de dominio. |
| data[].domain_area.uuid      | string   | UUID del área de dominio. |
| data[].domain_area.name      | string   | Nombre del área de dominio. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Clave de plataforma inválida.",
  "errors": {}
}
```

## Notas

- Devuelve solo grupos de categorías asociados a la plataforma actual (identificada por `X-PUBLIC-KEY`).
- Cuando `language` no se proporciona, el sistema utiliza el idioma predeterminado de la plataforma.
- Si un título en el idioma solicitado no está disponible, el sistema recurre al idioma predeterminado.
- Cuando `categories=false` u omitido, solo se devuelve información del grupo de categorías sin categorías anidadas.

## Relacionados

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentación inicial.
