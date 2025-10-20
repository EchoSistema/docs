# Compartido – Admin Actualizar Imagen de Usuario

## Endpoint

`POST /api/v1/users/{uuid}/image`

## Autenticación

Requiere un token Bearer válido con habilidad `backoffice` y permisos apropiados (`update.guest`, `update.collaborator` o `update.all`).

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer <token>` con habilidad backoffice. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Idioma para mensajes de validación. |
| Content-Type | `string` | Sí | `multipart/form-data` para carga de archivo o `application/json` para URL/codificado. |

## Parámetros de URL

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| `uuid` | `uuid` | Sí | UUID del usuario cuyo avatar se actualizará. |

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
curl -X POST https://api.example.com/api/v1/users/550e8400-e29b-41d4-a716-446655440000/image \
  -H "Authorization: Bearer <admin-token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=Avatar de Usuario" \
  -F "usage=avatar" \
  -F "image_file=@/ruta/al/avatar.jpg"
```

## Éxito `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "unique_id": "abc123def456",
    "usage": "avatar",
    "url": "https://cdn.ejemplo.com/usuarios/juan/avatar-abc123def456.webp",
    "name": "Avatar de Usuario",
    "slug": "avatar-de-usuario",
    "width": 512,
    "height": 512,
    "creation_date": "2024-01-15T10:30:00+00:00"
  }
}
```

## Permisos

El usuario autenticado debe tener uno de los siguientes permisos:
- `update.guest` — Puede actualizar usuarios invitados
- `update.collaborator` — Puede actualizar usuarios colaboradores
- `update.all` — Puede actualizar todos los usuarios

## Errores

- 401 No autorizado — token faltante/inválido o permisos insuficientes
- 403 Prohibido — el usuario no tiene los permisos necesarios para actualizar este usuario
- 404 No encontrado — usuario con UUID especificado no encontrado
- 422 Entidad no procesable — errores de validación
- 500 Error interno del servidor

## Notas

- Debe proporcionar **exactamente uno** de: `image_file`, `image_url` o `image_encoded`.
- El campo `usage` debe establecerse en `"avatar"` para avatares de usuario.
- Las imágenes cargadas se optimizan automáticamente y se convierten al formato WebP.
- Este endpoint requiere la habilidad de token `backoffice`.

## Relacionado

- [Obtener Perfil de Usuario](./UserProfile.md)
- [Admin Actualizar Usuario](./AdminUserUpdate.md)
- [Actualizar Mi Imagen](./UserImageUpdate.md)
