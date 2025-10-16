# Company – Crear Reseña de Empresa

## Endpoint

```
POST /api/v1/company/reviews
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Cuerpo de la solicitud

| Parámetro     | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ------------- | ------- | --------- | ----------- | ---------------------- |
| company       | string  | Sí*       | Identificador de empresa (UUID o ID). Requerido si `company_id` y `company_uuid` no se proporcionan. | |
| company_id    | integer | Sí*       | ID numérico de la empresa. Requerido si `company` y `company_uuid` no se proporcionan. | |
| company_uuid  | string  | Sí*       | UUID de la empresa. Requerido si `company` y `company_id` no se proporcionan. | |
| review        | string  | Sí        | Contenido de texto de la reseña. | |
| rating        | integer | Sí        | Valor de calificación entre 0 y 5. | 0-5 |

> *Uno de `company`, `company_id`, o `company_uuid` debe ser proporcionado.

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "company_uuid": "00000000-0000-0000-0000-000000000001",
    "review": "Excelente servicio y equipo profesional. ¡Altamente recomendado!",
    "rating": 5
  }' \
  "https://sandbox.su-dominio.com/api/v1/company/reviews"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 123,
    "author_name": "Juan Pérez",
    "profile_photo_url": "https://example.com/avatars/juanperez.jpg",
    "text": "Excelente servicio y equipo profesional. ¡Altamente recomendado!",
    "company": {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "name": "Tech Solutions Inc.",
      "logo": [
        {
          "usage": "logo",
          "url": "https://example.com/logos/techsolutions.png"
        }
      ],
      "assumed_name": "TechSol",
      "slug": "tech-solutions-inc",
      "rating": 4.7,
      "google": null
    },
    "time": "2025-10-16T15:30:00+00:00",
    "rating": 5
  }
}
```

## Estructura JSON Explicada

| Campo                       | Tipo    | Descripción |
| --------------------------- | ------- | ----------- |
| data                        | object  | Recurso de reseña. |
| data.id                     | integer | Identificador de la reseña. |
| data.author_name            | string  | Nombre del usuario que escribió la reseña. |
| data.profile_photo_url      | string  | URL de la foto de perfil del autor. |
| data.text                   | string  | Contenido de texto de la reseña. |
| data.company                | object  | Información de la empresa. |
| data.company.uuid           | string  | UUID de la empresa. |
| data.company.name           | string  | Nombre de la empresa. |
| data.company.logo[]         | array   | Array de objetos de logo de la empresa. |
| data.company.logo[].usage   | string  | Tipo de uso del logo. |
| data.company.logo[].url     | string  | URL del logo. |
| data.company.assumed_name   | string  | Nombre comercial de la empresa. |
| data.company.slug           | string  | Identificador amigable para URL de la empresa. |
| data.company.rating         | float   | Calificación promedio de la empresa. |
| data.company.google         | mixed   | Datos relacionados con Google (si disponible). |
| data.time                   | string  | Marca de tiempo de creación de la reseña (ISO 8601). |
| data.rating                 | integer | Calificación de la reseña (0-5). |

## Estados HTTP

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
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "review": ["El campo review es obligatorio."],
    "rating": ["El campo rating es obligatorio.", "El rating debe estar entre 0 y 5."],
    "company": ["Se requiere al menos un identificador de empresa."]
  }
}
```

## Notas

- El usuario autenticado se asocia automáticamente como el autor de la reseña.
- El parámetro `company` acepta tanto un UUID como un ID numérico y se resuelve automáticamente.
- Los valores de calificación deben ser enteros entre 0 y 5 (inclusive).
- Después de crear una reseña, se recalcula la calificación promedio de la empresa.
- Los usuarios pueden crear múltiples reseñas para diferentes empresas, pero las reglas de negocio pueden limitar reseñas duplicadas para la misma empresa.

## Relacionados

- [Company Review Index](./CompanyReviewIndex.md)
- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentación inicial.
