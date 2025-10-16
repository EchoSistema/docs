# Company – Mostrar Detalles de Empresa

## Endpoint

```
GET /api/v1/company
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------ | --------- | ----------- | ---------------------- |
| language  | string | No        | Código de idioma para campos traducibles. | Predeterminado de la plataforma |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "name": "Tech Solutions Inc.",
    "slug": "tech-solutions-inc",
    "rating": 4.5,
    "reviews": 150,
    "country": "Paraguay",
    "state": "Central",
    "city": "Asunción",
    "email": "contacto@techsolutions.com",
    "favicon": "https://example.com/favicon.ico",
    "logo": {
      "main": "https://example.com/logo.png",
      "square": "https://example.com/logo-cuadrado.png",
      "rounded": "https://example.com/logo-redondeado.png",
      "rectangular": "https://example.com/logo-rectangular.png"
    },
    "domain_area": {
      "uuid": "00000000-0000-0000-0000-000000000010",
      "name": "Tecnología"
    },
    "open_to_register": true
  }
}
```

## Estructura JSON Explicada

| Campo                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| data                   | object  | Recurso de empresa. |
| data.uuid              | string  | UUID de la empresa. |
| data.name              | string  | Nombre legal de la empresa. |
| data.slug              | string  | Identificador amigable para URL. |
| data.rating            | float   | Calificación promedio (0-5). |
| data.reviews           | integer | Número total de reseñas. |
| data.country           | string  | Nombre del país donde se encuentra la empresa. |
| data.state             | string  | Nombre del estado/provincia. |
| data.city              | string  | Nombre de la ciudad. |
| data.email             | string  | Email de contacto de la empresa (desde plataforma). |
| data.favicon           | string  | URL del favicon de la plataforma. |
| data.logo              | object  | Variantes del logo de la plataforma. |
| data.logo.main         | string  | URL del logo principal. |
| data.logo.square       | string  | URL del logo cuadrado. |
| data.logo.rounded      | string  | URL del logo redondeado. |
| data.logo.rectangular  | string  | URL del logo rectangular. |
| data.domain_area       | object  | Información del área de dominio empresarial. |
| data.domain_area.uuid  | string  | UUID del área de dominio. |
| data.domain_area.name  | string  | Nombre del área de dominio. |
| data.open_to_register  | boolean | Si la empresa acepta nuevos registros. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "No autenticado.",
  "errors": {}
}
```

## Notas

- Este endpoint devuelve la empresa asociada a la plataforma del usuario autenticado.
- El usuario autenticado debe tener la habilidad `backoffice`.
- La calificación y las reseñas se calculan a partir de todas las reseñas de la empresa.
- La información de dirección (país, estado, ciudad) proviene de la dirección principal de la empresa.
- Los campos relacionados con la plataforma (email, logo, favicon, domain_area, open_to_register) se cargan desde la relación de plataforma asociada.

## Relacionados

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)
- [Company Address Store](./CompanyAddressStore.md)
- [Company Review Store](./CompanyReviewStore.md)

## Changelog

- 2025-10-16: Documentación inicial.
