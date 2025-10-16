# RealEstate – Listar Imágenes de la Plataforma

## Endpoint

```
GET /api/v1/real-estate/platform-images
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
  "https://sandbox.su-dominio.com/api/v1/real-estate/platform-images"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440001",
      "key": "hero_banner",
      "url": "https://example.com/images/hero-banner.jpg",
      "alt": "Banner Principal de la Plataforma Inmobiliaria",
      "usage": "homepage",
      "width": 1920,
      "height": 1080
    },
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440002",
      "key": "logo",
      "url": "https://example.com/images/logo.png",
      "alt": "Logotipo de la Plataforma",
      "usage": "branding",
      "width": 200,
      "height": 80
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de imágenes de la plataforma |
| data[].uuid | string | Identificador único de la imagen |
| data[].key | string | Clave identificadora de la imagen para referencia |
| data[].url | string | URL de la imagen |
| data[].alt | string | Texto alternativo para accesibilidad |
| data[].usage | string | Contexto de uso de la imagen (homepage, branding, etc.) |
| data[].width | integer | Ancho de la imagen en píxeles |
| data[].height | integer | Alto de la imagen en píxeles |
| meta | object | Metadatos de respuesta |
| meta.total | integer | Número total de imágenes |

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

- Devuelve todas las imágenes específicas de la plataforma para personalización
- Las imágenes se filtran por el encabezado `X-PUBLIC-KEY`
- Use el campo `key` para identificar imágenes específicas en su aplicación
- Los resultados no están paginados ya que la lista es típicamente pequeña
- Las imágenes están optimizadas y servidas a través de CDN

## Relacionados

- [Listar Textos de la Plataforma](PlatformTextIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
