# RealEstate – Listar Tipos de Propiedad

## Endpoint

```
GET /api/v1/real-estate/property-types
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

No se requieren parámetros de consulta.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/real-estate/property-types"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440001",
      "name": "Apartamento",
      "slug": "apartment",
      "description": "Edificio residencial de múltiples unidades",
      "icon": "building"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440002",
      "name": "Casa",
      "slug": "house",
      "description": "Residencia unifamiliar independiente",
      "icon": "home"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440003",
      "name": "Condominio",
      "slug": "condo",
      "description": "Unidad de propiedad individual en un complejo",
      "icon": "condo"
    }
  ],
  "meta": {
    "total": 3
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de tipos de propiedad |
| data[].uuid | string | Identificador único del tipo de propiedad |
| data[].name | string | Nombre del tipo de propiedad (traducible) |
| data[].slug | string | Identificador amigable para URL del tipo de propiedad |
| data[].description | string | Descripción del tipo de propiedad |
| data[].icon | string | Identificador de icono para visualización en UI |
| meta | object | Metadatos de respuesta |
| meta.total | integer | Número total de tipos de propiedad |

## Estados HTTP

- 200: OK
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Devuelve todos los tipos de propiedad disponibles para la plataforma
- Los nombres de tipos de propiedad son traducibles y respetan el encabezado `Accept-Language`
- Use el campo `slug` para filtrar propiedades por tipo
- Los resultados no están paginados ya que la lista es típicamente pequeña

## Relacionados

- [Listar Propiedades](PropertyIndex.md)
- [Mostrar Detalles de Propiedad](PropertyShow.md)

## Changelog

- 2025-10-16: Documentación inicial
