# Compartido – Actualizar Mi Imagen

## Endpoint

`POST /api/v1/me/image`

## Autenticación

Requiere un token Bearer válido.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Idioma para mensajes de validación. |
| Content-Type | `string` | Sí | `multipart/form-data` para carga de archivo o `application/json` para URL/codificado. |

## Cuerpo

La solicitud admite tres métodos diferentes para proporcionar la imagen:

### Método 1: Carga de Archivo (multipart/form-data)

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `name` | `string` | Sí | Nombre de la imagen (máx 255). |
| `usage` | `string` | Sí | Tipo de uso de imagen (ej., "avatar"). |
| `image_file` | `file` | Sí | Archivo de imagen (jpeg, png, jpg, gif, svg, webp, heic, heif). Máx 2MB. |
| `type` | `string` | No | Identificador del tipo de imagen (máx 255). |
| `raw` | `array` | No | Metadatos adicionales. |

### Método 2: URL de Imagen (application/json)

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `name` | `string` | Sí | Nombre de la imagen (máx 255). |
| `usage` | `string` | Sí | Tipo de uso de imagen (ej., "avatar"). |
| `image_url` | `url` | Sí | URL de la imagen (máx 500). |
| `type` | `string` | No | Identificador del tipo de imagen (máx 255). |
| `raw` | `array` | No | Metadatos adicionales. |

### Método 3: Codificado en Base64 (application/json)

| Campo | Tipo | Requerido | Descripción |
| ----- | ---- | --------- | ----------- |
| `name` | `string` | Sí | Nombre de la imagen (máx 255). |
| `usage` | `string` | Sí | Tipo de uso de imagen (ej., "avatar"). |
| `image_encoded` | `string` | Sí | Datos de imagen codificados en Base64. |
| `type` | `string` | No | Identificador del tipo de imagen (máx 255). |
| `raw` | `array` | No | Metadatos adicionales. |

## Ejemplo de Solicitud (Carga de Archivo)

```bash
curl -X POST https://api.example.com/api/v1/me/image \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=Mi Avatar" \
  -F "usage=avatar" \
  -F "image_file=@/ruta/al/avatar.jpg"
```

## Ejemplo de Solicitud (JSON con URL)

```json
{
  "name": "Mi Avatar",
  "usage": "avatar",
  "image_url": "https://ejemplo.com/imagenes/avatar.jpg"
}
```

## Éxito `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "unique_id": "abc123def456",
    "usage": "avatar",
    "url": "https://cdn.ejemplo.com/usuarios/juan/avatar-abc123def456.webp",
    "name": "Mi Avatar",
    "slug": "mi-avatar",
    "width": 512,
    "height": 512,
    "creation_date": "2024-01-15T10:30:00+00:00"
  }
}
```

## Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data.uuid` | `uuid` | Identificador de imagen. |
| `data.unique_id` | `string` | Identificador único hexadecimal. |
| `data.usage` | `string` | Tipo de uso de imagen. |
| `data.url` | `string` | URL completa para acceder a la imagen. |
| `data.name` | `string` | Nombre de la imagen. |
| `data.slug` | `string` | Versión del nombre compatible con URL. |
| `data.width` | `integer|null` | Ancho de la imagen en píxeles. |
| `data.height` | `integer|null` | Alto de la imagen en píxeles. |
| `data.creation_date` | `datetime` | Cuándo se creó la imagen. |

## Errores

- 401 No autorizado — token faltante/inválido
- 422 Entidad no procesable — errores de validación
- 500 Error interno del servidor

### Ejemplo 422

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "name": ["El campo nombre es obligatorio cuando la URL de imagen no está presente."],
    "usage": ["El campo uso es obligatorio."],
    "image_file": ["El archivo de imagen debe ser un archivo de tipo: jpeg, png, jpg, gif, svg, webp, heic, heif."]
  }
}
```

## Notas

- Debe proporcionar **exactamente uno** de: `image_file`, `image_url` o `image_encoded`.
- El campo `usage` debe establecerse en `"avatar"` para avatares de usuario.
- Las imágenes cargadas se optimizan automáticamente y se convierten al formato WebP.
- La imagen de avatar anterior se reemplazará con la nueva.

## Relacionado

- [Obtener Perfil de Usuario](./UserProfile.md)
- [Actualizar Perfil de Usuario](./UserProfileUpdate.md)
- [Admin Actualizar Imagen de Usuario](./AdminUserImageUpdate.md)
