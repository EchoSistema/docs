# ReputationBook – Perfil del usuario del libro de reclamaciones

## Consultar mi perfil

### Endpoint

```
GET /api/v1/complaint-book/me
```

Devuelve los datos del usuario autenticado, incluyendo estadísticas y relaciones cargadas.

### Autenticación

Obligatoria – `Bearer {token}` (`auth:sanctum`).

### Respuesta

Sigue la estructura de `ComplaintBookUserResource`, con los siguientes puntos principales:

- Identificadores (`id`, `uuid`), nombre, correo, género, fecha de nacimiento.
- Contadores `complaints_count` y `reviews_count`.
- Contactos agrupados por tipo (`public_email`, `telephone`, `whatsapp`, ...), cuando existen.
- Direcciones (colección de `AddressResource`).
- Imágenes cargadas (avatar, documentos, etc.).

### Códigos HTTP

- 200: Perfil retornado.
- 401: Token inválido o ausente.

---

## Subir imagen a mi perfil

### Endpoint

```
POST /api/v1/complaint-book/me/image/upload
```

Permite adjuntar o actualizar imágenes (avatar, documentos, etc.) vinculadas al usuario.

### Autenticación

Obligatoria – `Bearer {token}`.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
| ---------- | ---- | ----------- | ----------- |
| `Authorization` | string | Sí | Credencial `Bearer`. |
| `Content-Type` | string | Sí | `multipart/form-data` o `application/json`, según la carga. |

### Cuerpo aceptado (`ImageStoreRequest`)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `usage` | string | Sí | Propósito de la imagen (ej.: `avatar`). |
| `name` | string | Condicional | Nombre amigable (obligatorio si falta `image_url`). |
| `image_url` | string | Condicional | URL pública de la imagen. |
| `image_encoded` | string | Condicional | Contenido codificado en Base64. |
| `image_file` | file | Condicional | Archivo binario (`jpeg`, `png`, `webp`, `heic`, ...). Máx. 2 MB. |
| `unique_id` | string | No | Identificador para actualizar una imagen existente. |
| `type` | string | No | Tipo lógico (avatar, documento, ...). |
| `raw` | array | No | Metadatos adicionales. |

> Debe enviarse al menos uno entre `image_url`, `image_encoded` o `image_file`.

### Ejemplo de respuesta (201)

```json
{
  "data": {
    "uuid": "b9b16c30-0d6f-4d77-91f9-56a553d9f9a1",
    "unique_id": "avatar-main",
    "usage": "avatar",
    "url": "https://cdn.echosistema.com/avatars/user-123.png",
    "name": "Foto de perfil",
    "slug": "foto-de-perfil",
    "width": 512,
    "height": 512
  }
}
```

### Códigos HTTP

- 201: Imagen almacenada.
- 400/422: Payload o archivo inválido.
- 401: Token inválido.

