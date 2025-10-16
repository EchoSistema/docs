# Company – Mostrar Detalles de Oportunidad de Empleo

## Endpoint

```
GET /api/v1/company/opportunities/{jobOpportunity}
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro       | Tipo   | Requerido | Descripción |
| --------------- | ------ | --------- | ----------- |
| jobOpportunity  | string | Sí        | UUID de la oportunidad de empleo. |

### Parámetros de consulta

| Parámetro | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------ | --------- | ----------- | ---------------------- |
| language  | string | No        | Código de idioma para campos traducibles (ej.: `en`, `pt-BR`, `es`). | Predeterminado de la plataforma |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company/opportunities/00000000-0000-0000-0000-000000000001?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "company": {
      "name": "Tech Solutions Inc.",
      "country": "Paraguay",
      "state": "Central",
      "city": "Asunción"
    },
    "total_job_openings": 3,
    "title": "Desarrollador de Software Senior",
    "mode": "remote",
    "description": "Buscamos un desarrollador de software experimentado para unirse a nuestro equipo y trabajar en proyectos de vanguardia utilizando tecnologías modernas.",
    "education_required": "bachelor",
    "required_skills": [
      "PHP",
      "Laravel",
      "JavaScript",
      "Vue.js",
      "Docker"
    ],
    "required_experience": [
      {
        "area": "Desarrollo de Software",
        "years": 5
      }
    ],
    "occupation_areas_scope": [
      {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "title": "Tecnología de la Información"
      }
    ],
    "languages": [
      {
        "language": "Inglés",
        "level": "avanzado"
      },
      {
        "language": "Español",
        "level": "intermedio"
      }
    ],
    "salary_range": {
      "from": "$5,000.00",
      "to": "$8,000.00"
    },
    "country": "Paraguay",
    "state": "Central",
    "city": "Asunción",
    "starts_at": "2025-10-20T00:00:00+00:00",
    "finishes_at": "2025-11-30T23:59:59+00:00"
  }
}
```

## Estructura JSON Explicada

| Campo                               | Tipo     | Descripción |
| ----------------------------------- | -------- | ----------- |
| data                                | object   | Recurso de oportunidad de empleo. |
| data.uuid                           | string   | UUID de la oportunidad de empleo. |
| data.company                        | object   | Información de la empresa (solo cuando no es propietario). |
| data.company.name                   | string   | Nombre de la empresa. |
| data.company.country                | string   | País de la empresa. |
| data.company.state                  | string   | Estado de la empresa. |
| data.company.city                   | string   | Ciudad de la empresa. |
| data.total_job_openings             | integer  | Número de puestos abiertos. |
| data.title                          | string   | Título del trabajo en el idioma solicitado. |
| data.mode                           | string   | Modo de trabajo: `remote`, `hybrid`, `on-site`. |
| data.description                    | string   | Descripción del trabajo en el idioma solicitado. |
| data.education_required             | string   | Nivel de educación requerido. |
| data.required_skills[]              | array    | Lista de habilidades requeridas. |
| data.required_experience[]          | array    | Lista de experiencia requerida. |
| data.required_experience[].area     | string   | Área de experiencia. |
| data.required_experience[].years    | integer  | Años de experiencia requeridos. |
| data.occupation_areas_scope[]       | array    | Áreas de ocupación relevantes. |
| data.occupation_areas_scope[].uuid  | string   | UUID del área de ocupación. |
| data.occupation_areas_scope[].title | string   | Título del área de ocupación. |
| data.languages[]                    | array    | Idiomas requeridos. |
| data.languages[].language           | string   | Nombre del idioma. |
| data.languages[].level              | string   | Nivel de competencia. |
| data.salary_range                   | object   | Información del rango salarial. |
| data.salary_range.from              | string   | Salario mínimo (formateado). |
| data.salary_range.to                | string   | Salario máximo (formateado). |
| data.country                        | string   | País de ubicación del trabajo. |
| data.state                          | string   | Estado de ubicación del trabajo. |
| data.city                           | string   | Ciudad de ubicación del trabajo. |
| data.starts_at                      | datetime | Fecha de inicio de la oportunidad. |
| data.finishes_at                    | datetime | Fecha de finalización de la oportunidad. |

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
  "message": "Oportunidad de empleo no encontrada.",
  "errors": {}
}
```

## Notas

- Este endpoint devuelve información detallada sobre una oportunidad de empleo específica.
- La oportunidad de empleo debe pertenecer a la empresa asociada a la plataforma actual.
- El campo `company` solo se incluye cuando la solicitud no proviene del propietario.
- Los rangos salariales se formatean según el idioma solicitado y la moneda.
- Todos los campos traducibles (título, descripción) se devuelven en el idioma solicitado, recurriendo al idioma predeterminado si no está disponible.
- El endpoint valida que la oportunidad de empleo pertenezca a la empresa antes de devolver los datos.

## Relacionados

- [Job Opportunity Index](./MeJobOpportunityIndex.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Documentación inicial.
