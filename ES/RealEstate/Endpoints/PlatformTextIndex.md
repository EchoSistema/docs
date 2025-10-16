# RealEstate – Listar Textos de la Plataforma

## Endpoint

```
GET /api/v1/real-estate/platform-texts
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
  "https://sandbox.su-dominio.com/api/v1/real-estate/platform-texts"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440001",
      "key": "welcome_message",
      "content": "Encuentra la casa de tus sueños con nosotros",
      "usage": "homepage",
      "language": "es"
    },
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440002",
      "key": "about_us",
      "content": "Somos una plataforma inmobiliaria líder...",
      "usage": "about",
      "language": "es"
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
| data[] | array | Lista de textos de la plataforma |
| data[].uuid | string | Identificador único del texto |
| data[].key | string | Clave identificadora del texto para referencia |
| data[].content | string | Contenido del texto |
| data[].usage | string | Contexto de uso del texto (homepage, about, footer, etc.) |
| data[].language | string | Código de idioma del texto |
| meta | object | Metadatos de respuesta |
| meta.total | integer | Número total de textos |

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

- Devuelve todos los textos específicos de la plataforma para personalización
- Los textos se filtran por el encabezado `X-PUBLIC-KEY`
- El contenido respeta el encabezado `Accept-Language` para traducciones
- Use el campo `key` para identificar textos específicos en su aplicación
- Los resultados no están paginados ya que la lista es típicamente pequeña

## Relacionados

- [Listar Imágenes de la Plataforma](PlatformImageIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
