# Shared – Subir Imagen de Mensaje de Contacto

## Endpoint

`POST /api/v1/contact/{message}/images`

## Autenticación

Ninguna (público). Requiere cabecera de plataforma.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| X-PUBLIC-KEY | `string` | Sí | Clave pública de la plataforma. |
| Content-Type | `multipart/form-data` | Condicional | Requerido al enviar `image_file`; puede ser `application/json` al usar URLs/Base64. |

## Parámetros

Subir una imagen adjunta a un mensaje de contacto.

Parámetro de ruta:
- `message` (uuid) — identificador del mensaje retornado en la creación.

### Cuerpo

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ---------- | ----------- |
| `usage` | `string` | Sí | Finalidad de la imagen (ej.: `platform_contact`). |
| `name` | `string` | Obligatorio sin `image_url` | Nombre del archivo. |
| `image_url` | `url` | Obligatorio sin `image_file`/`image_encoded` | URL pública (máx. 500). |
| `image_encoded` | `string` | Obligatorio sin `image_file`/`image_url` | Imagen codificada en Base64. |
| `image_file` | `file` | Obligatorio sin `image_url`/`image_encoded` | `jpeg,png,jpg,gif,svg,webp,heic,heif` hasta 2 MB. |
| `unique_id` | `string` | No | Clave de deduplicación opcional. |
| `type` | `string` | No | Categoría libre.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/contact/{message}/images"
```

### Éxito `200 OK`

```json
{
  "data": {
    "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
    "unique_id": "5f2c19f4",
    "usage": "platform_contact",
    "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
    "name": "recibo.png",
    "slug": "recibo-png",
    "width": 1280,
    "height": 720,
    "creation_date": "2024-06-01T13:45:00+00:00"
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data  | `object` | Datos del recurso de imagen. |

## Estados HTTP

- 200: OK
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
  "message": "Mensaje de error"
}
```

## Notas

- Subir una imagen adjunta a un mensaje de contacto

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
