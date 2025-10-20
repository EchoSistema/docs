# Plataforma - Mensajes de Contacto

## Visión general

Endpoints responsables del flujo de mensajes enviados mediante el formulario de contacto de la plataforma. Permiten registrar el contacto público (con validación de reCAPTCHA), adjuntar imágenes al ticket recién creado y, dentro del backoffice autenticado, listar, visualizar y marcar mensajes como leídos.

## Endpoints

| Método | Ruta | Descripción | Autenticación |
| ------ | ---- | ----------- | ------------- |
| `POST` | `/api/v1/contact` | Registra un nuevo mensaje de contacto. | No requiere token; cabecera `X-PUBLIC-KEY` obligatoria. |
| `POST` | `/api/v1/contact/{message}/images` | Añade una imagen vinculada al mensaje. | No requiere token; cabecera `X-PUBLIC-KEY` obligatoria. |
| `GET` | `/api/v1/contacts` | Lista mensajes con filtros/paginación. | Requiere `auth:sanctum` + `X-PUBLIC-KEY`. |
| `GET` | `/api/v1/contacts/{message:uuid}` | Muestra los detalles completos del mensaje. | Requiere `auth:sanctum` + `X-PUBLIC-KEY`. |
| `PATCH` | `/api/v1/contacts/{message}/toggle-read` | Alterna el estado de lectura del mensaje. | Requiere `auth:sanctum` + `X-PUBLIC-KEY`. |

> **Permisos Backoffice**
> - La lista requiere `index.platform_message` o `index.all` en el rol de la plataforma.
> - Mostrar/alternar requiere `show.platform_message` o `show.all`, excepto cuando el usuario es el autor.
> - Los usuarios que no son backoffice solo acceden a los mensajes de su propia plataforma; los invitados visualizan únicamente sus envíos.

---

## Crear mensaje (`POST /api/v1/contact`)

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
|------------|------|-------------|-------------|
| `X-PUBLIC-KEY` | `string` | Sí | Clave pública de la plataforma (también acepta `public_key` en la query). |
| `X-APP-NAME` | `string` | No | Cuando presente (y coincidiendo con el nombre de una plataforma), no se requiere reCAPTCHA. |
| `Content-Type` | `application/json` | Sí | Cuerpo JSON. |
| `Accept-Language` | `string` | No | Locale para mensajes de validación (`pt-BR`, `en`, `es`, `gn`). |

### Cuerpo (`application/json`)

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|-------------|-------------|
| `type` | `enum` | Sí | `default`, `denunciation` o `report`. |
| `language` | `enum` | Sí | Idioma del formulario: `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Sí | Token validado mediante `ReCaptchaRule`. |
| `email` | `email` | Condicional | Obligatorio cuando no se envía `phone`. |
| `phone` | `string` | Condicional | Obligatorio cuando no se envía `email`. |
| `content` | `string` | Sí | Mensaje (máx. 5000 caracteres). |
| `name` | `string` | No | Nombre de quien envía. |
| `user_uuid` | `uuid` | No | Asocia el mensaje a un usuario existente. |

> La IP del visitante se infiere automáticamente (`ip_address`). Si se informa `anonymous=true`, el servicio usa `anonymous@pigeons.email` como correo predeterminado.

### Ejemplo de solicitud

```json
{
    "type": "default",
    "language": "es",
    "g_recaptcha": "token_recaptcha",
    "email": "maria.silva@example.com",
    "content": "Me gustaría saber más sobre los planes.",
    "name": "Maria Silva"
}
```

### Respuestas

- **201 Created**
  ```json
  {
      "data": {
          "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e"
      }
  }
  ```
- **422 Unprocessable Entity** - falló la validación (campos obligatorios o reCAPTCHA).
- **403 Forbidden** - `X-PUBLIC-KEY` inválida o inexistente.

---

## Adjuntar imagen (`POST /api/v1/contact/{message}/images`)

Permite adjuntar archivos al contacto justo después de crearlo. El identificador `{message}` corresponde al `uuid` devuelto en la creación.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
|------------|------|-------------|-------------|
| `X-PUBLIC-KEY` | `string` | Sí | Clave pública de la plataforma. |
| `Content-Type` | `multipart/form-data` | Condicional | Necesario al enviar `image_file`; puede ser `application/json` al usar URLs/Base64. |

### Cuerpo (campos principales)

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|-------------|-------------|
| `usage` | `string` | Sí | Finalidad de la imagen (ej.: `platform_contact`). |
| `name` | `string` | Condicional | Requerido cuando no se informa `image_url`. |
| `image_url` | `url` | Condicional | URL pública de la imagen (`<= 500` caracteres). |
| `image_encoded` | `string` | Condicional | Contenido en Base64 de la imagen. |
| `image_file` | `file` | Condicional | Upload (`jpeg,png,jpg,gif,svg,webp,heic,heif` hasta 2 MB). |
| `unique_id` | `string` | No | Identificador opcional para deduplicación. |
| `type` | `string` | No | Categoría libre de la imagen. |

> Es obligatorio enviar **exactamente uno** de los campos `image_url`, `image_encoded` o `image_file`.

### Respuesta exitosa (200 OK)

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

- **404 Not Found** - cuando el `uuid` no pertenece a la plataforma.
- **422 Unprocessable Entity** - payload inválido o tipo de archivo no compatible.

---

## Listar mensajes (`GET /api/v1/contacts`)

Devuelve una colección paginada limitada a los permisos del usuario autenticado.

### Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
|------------|------|-------------|-------------|
| `Authorization` | `Bearer <token>` | Sí | Token Sanctum válido. |
| `X-PUBLIC-KEY` | `string` | Sí | Plataforma actual. |
| `Accept-Language` | `string` | No | Locale de los mensajes. |

### Parámetros de query

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `page` | `integer` | Página actual (por defecto `1`). |
| `per_page` | `integer` | Elementos por página (por defecto `25`). |
| `no_paginate` | `boolean` | Cuando es `true`, devuelve una colección simple. |
| `limit` | `integer` | Límite de elementos cuando `no_paginate=true`. |
| `sort_by` | `string` | Columna de ordenación (por defecto `created_at`). |
| `direction` | `string` | Dirección `ASC`/`DESC` (por defecto `DESC`). |
| `uuid` | `uuid` | Filtra por identificador exacto. |
| `email` | `email` | Correo del contacto. |
| `phone` | `string` | Teléfono informado. |
| `name` | `string` | Nombre (búsqueda parcial). |
| `type` | `string` | Tipo (`default`, `denunciation`, `report`). |
| `message_lang` | `string` | Idioma del mensaje (`pt-BR`, `en`, `es`, `gn`). |
| `created_at` | `date` | Fecha exacta (`YYYY-MM-DD`). |
| `interval` | `string` | Rangos rápidos: `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Rango personalizado por fecha. |
| `user` | `string` | ID, UUID, correo o parte del nombre del autor. |
| `platform` | `string` | ID, UUID, slug o parte del nombre de la plataforma. |
| `ip_address` | `ipv4` | IP del remitente. |
| `ip_banned` | `boolean` | Solo IP marcadas como bloqueadas. |
| `is_robot` | `boolean` | Solo IP identificadas como robots. |
| `is_read` | `boolean` | Filtra por estado de lectura. |
| `first_reader` | `string` | Identificador del primer lector. |
| `content` | `string` | Búsqueda de texto en el contenido. |
| `with_images` | `boolean` | Solo mensajes con adjuntos. |

### Ejemplo de respuesta (200 OK)

```json
{
    "data": [
        {
            "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
            "name": "Maria Silva",
            "is_read": false,
            "email": "maria.silva@example.com",
            "created_at": "2024-06-06T18:12:45+00:00",
            "user": {
                "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
                "name": "Maria Silva",
                "email": "maria.silva@example.com"
            },
            "content": "Me gustaría saber más sobre los planes..."
        }
    ],
    "links": {
        "first": "https://api.example.com/api/v1/contacts?page=1",
        "last": "https://api.example.com/api/v1/contacts?page=5",
        "prev": null,
        "next": "https://api.example.com/api/v1/contacts?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 5,
        "path": "https://api.example.com/api/v1/contacts",
        "per_page": 25,
        "to": 25,
        "total": 112
    }
}
```

> El campo `content` se trunca (220 caracteres) en la lista. Usa el endpoint de detalle para obtener el contenido completo.

- **403 Forbidden** - usuario sin permiso en la plataforma.

---

## Detallar mensaje (`GET /api/v1/contacts/{message}`)

Devuelve todos los campos del mensaje especificado.

### Respuesta exitosa (200 OK)

```json
{
    "data": {
        "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
        "name": "Maria Silva",
        "is_read": true,
        "email": "maria.silva@example.com",
        "created_at": "2024-06-06T18:12:45+00:00",
        "type": "default",
        "language": "es",
        "phone": "+55 11912345678",
        "content": "Me gustaría saber más sobre los planes e integraciones disponibles...",
        "ip_address": "200.200.200.5",
        "images": [
            {
                "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
                "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
                "usage": "platform_contact"
            }
        ],
        "user": {
            "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
            "name": "Maria Silva",
            "email": "maria.silva@example.com"
        },
        "platform": {
            "uuid": "7a3b6d12-4d9f-4f5c-b41b-6ce39e04d57f",
            "name": "Echosistema"
        },
        "first_reader": {
            "uuid": "f39af7fa-6622-4c46-ba0d-fefb699b10f8",
            "name": "Juan Admin",
            "email": "juan.admin@example.com"
        }
    }
}
```

- **403 Forbidden** - usuario sin permiso y que no es el autor.
- **404 Not Found** - mensaje inexistente en la plataforma.

---

## Alternar lectura (`PATCH /api/v1/contacts/{message}/toggle-read`)

Invierte el estado `is_read` del mensaje y registra al primer lector (`first_reader`) cuando corresponde. Devuelve el mismo payload que el endpoint de detalle.

- **200 OK** - mensaje actualizado.
- **403 Forbidden** - sin permiso o plataforma diferente.

---

## Buenas prácticas y consideraciones

- Envía siempre `X-PUBLIC-KEY` o `public_key` con el valor correcto de la plataforma.
- Después de crear el mensaje, espera el `uuid` de la respuesta antes de adjuntar imágenes.
- Los adjuntos están limitados a 2 MB por archivo. Utiliza `image_url` para archivos más grandes ya hospedados.
- Usa filtros (`is_read=false`, `with_images=true`, etc.) para priorizar las colas importantes en el backoffice.
- Trata las respuestas 403 como falta de permiso en el rol actual; revisa los permisos asignados al usuario.
