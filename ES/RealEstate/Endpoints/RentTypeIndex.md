# RealEstate – Listar Tipos de Alquiler

## Endpoint

```
GET /api/v1/real-estate/rent-types
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
  "https://sandbox.su-dominio.com/api/v1/real-estate/rent-types"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440001",
      "name": "Venta",
      "slug": "sale",
      "description": "Propiedad en venta"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440002",
      "name": "Alquiler",
      "slug": "rent",
      "description": "Propiedad en alquiler"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440003",
      "name": "Arrendamiento",
      "slug": "lease",
      "description": "Propiedad en arrendamiento"
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
| data[] | array | Lista de tipos de alquiler/transacción |
| data[].uuid | string | Identificador único del tipo de alquiler |
| data[].name | string | Nombre del tipo de alquiler (traducible) |
| data[].slug | string | Identificador amigable para URL del tipo de alquiler |
| data[].description | string | Descripción del tipo de alquiler |
| meta | object | Metadatos de respuesta |
| meta.total | integer | Número total de tipos de alquiler |

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

- Devuelve todos los tipos de transacción disponibles para la plataforma (venta, alquiler, arrendamiento, etc.)
- Los nombres de tipos de alquiler son traducibles y respetan el encabezado `Accept-Language`
- Use el campo `slug` para filtrar propiedades por tipo de transacción
- Los resultados no están paginados ya que la lista es típicamente pequeña

## Relacionados

- [Listar Propiedades](PropertyIndex.md)
- [Mostrar Detalles de Propiedad](PropertyShow.md)

## Changelog

- 2025-10-16: Documentación inicial
