# Chat – Listar Mensajes de una Conversación

## Endpoint

`GET /api/v1/chat/conversations/{uuid}/messages`

Recupera todos los mensajes de una conversación específica, incluyendo información de la conversación y su contexto asociado.

---

## Autenticación

**Requerida** – Token Bearer.

---

## Solicitud

### Encabezados Requeridos

| Encabezado        | Tipo     | Descripción |
| ----------------- | -------- | ----------- |
| **Authorization** | `string` | Credencial `Bearer {token}`. **Requerido**. |
| **X-PUBLIC-KEY**  | `string` | Clave pública que identifica la plataforma solicitante. **Requerido**. |

### Parámetros de Ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| `uuid`    | `uuid` | Sí        | UUID de la conversación. |

---

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/chat/conversations/9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28/messages"
```

### Ejemplo de Respuesta

```json
{
  "data": {
    "conversation": {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Order",
      "last_message_text": "Hola, ¿cómo estás?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Order",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Pedido #1234"
      }
    },
    "messages": [
      {
        "message_id": "670e3a1b8f9c2d001e4f5a6b",
        "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
        "sender": {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "Juan Silva",
          "avatar": "https://cdn.example.com/avatars/juan.jpg"
        },
        "receiver": {
          "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
          "name": "María Santos",
          "avatar": "https://cdn.example.com/avatars/maria.jpg"
        },
        "context": {
          "text": "Hola, ¿cómo estás?",
          "type": "text"
        },
        "attachments": [],
        "metadata": {},
        "reactions": [
          {
            "user_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
            "emoji": "👍",
            "created_at": "2025-10-15T10:31:00+00:00"
          }
        ],
        "status": {
          "is_read": true,
          "read_at": "2025-10-15T10:31:00+00:00",
          "delivered_at": "2025-10-15T10:30:00+00:00",
          "is_deleted": false
        },
        "timestamps": {
          "created_at": "2025-10-15T10:30:00+00:00",
          "updated_at": "2025-10-15T10:31:00+00:00"
        },
        "pbk": "platform_public_key",
        "device": {
          "type": "web",
          "os": "Windows",
          "os_version": "10",
          "app_version": "1.0.0",
          "model": null,
          "browser": "Chrome",
          "browser_version": "120.0"
        }
      }
    ]
  }
}
```

---

## Estructura JSON Explicada

### Respuesta de Éxito

| Campo  | Tipo     | Descripción |
| ------ | -------- | ----------- |
| `data` | `object` | Contiene la conversación y sus mensajes. |

### `data.conversation` – Objeto Conversación

| Campo               | Tipo      | Descripción |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Identificador único de la conversación. |
| `taggable_type`     | `string`  | Tipo del modelo polimórfico asociado. |
| `last_message_text` | `string`  | Texto del último mensaje. |
| `last_message_at`   | `string`  | Fecha del último mensaje (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica si la conversación está archivada. |
| `created_at`        | `string`  | Fecha de creación de la conversación (formato ISO-8601). |
| `updated_at`        | `string`  | Fecha de la última actualización (formato ISO-8601). |
| `taggable_info`     | `object`  | Información del contexto asociado. |

### `data.messages[]` – Array de Mensajes

| Campo                | Tipo     | Descripción |
| -------------------- | -------- | ----------- |
| `message_id`         | `string` | ID único del mensaje (MongoDB ObjectId). |
| `conversation_uuid`  | `uuid`   | UUID de la conversación. |
| `sender`             | `object` | Información del remitente. |
| `receiver`           | `object` | Información del destinatario. |
| `context`            | `object` | Contenido del mensaje. |
| `attachments[]`      | `array`  | Lista de archivos adjuntos (archivos, imágenes, etc.). |
| `metadata`           | `object` | Metadatos adicionales. |
| `reactions[]`        | `array`  | Lista de reacciones (emojis). |
| `status`             | `object` | Estado de entrega y lectura. |
| `timestamps`         | `object` | Marcas de tiempo de creación y actualización. |
| `pbk`                | `string` | Clave pública de la plataforma. |
| `device`             | `object` | Información del dispositivo de envío. |

### `sender` / `receiver` – Información del Usuario

| Campo    | Tipo     | Descripción |
| -------- | -------- | ----------- |
| `uuid`   | `uuid`   | UUID del usuario. |
| `name`   | `string` | Nombre del usuario. |
| `avatar` | `string` | URL del avatar (puede ser null). |

### `context` – Contenido del Mensaje

| Campo  | Tipo     | Descripción |
| ------ | -------- | ----------- |
| `text` | `string` | Texto del mensaje. |
| `type` | `string` | Tipo de contenido: `text`, `image`, `file`, `audio`, `video`. |

### `reactions[]` – Reacciones

| Campo        | Tipo     | Descripción |
| ------------ | -------- | ----------- |
| `user_uuid`  | `uuid`   | UUID del usuario que reaccionó. |
| `emoji`      | `string` | Emoji de la reacción. |
| `created_at` | `string` | Fecha de la reacción (formato ISO-8601). |

### `status` – Estado del Mensaje

| Campo          | Tipo      | Descripción |
| -------------- | --------- | ----------- |
| `is_read`      | `boolean` | Indica si el mensaje fue leído. |
| `read_at`      | `string`  | Fecha de lectura (formato ISO-8601, null si no leído). |
| `delivered_at` | `string`  | Fecha de entrega (formato ISO-8601). |
| `is_deleted`   | `boolean` | Indica si el mensaje fue eliminado. |

### `device` – Información del Dispositivo

| Campo             | Tipo     | Descripción |
| ----------------- | -------- | ----------- |
| `type`            | `string` | Tipo de dispositivo: `mobile`, `web`, `desktop`. |
| `os`              | `string` | Sistema operativo. |
| `os_version`      | `string` | Versión del sistema operativo (puede ser null). |
| `app_version`     | `string` | Versión de la aplicación (puede ser null). |
| `model`           | `string` | Modelo del dispositivo (puede ser null). |
| `browser`         | `string` | Navegador (puede ser null). |
| `browser_version` | `string` | Versión del navegador (puede ser null). |

---

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido (el usuario no es participante de la conversación)
- 404: Conversación no encontrada
- 500: Error interno del servidor

---

## Notas

* Solo los participantes de la conversación pueden ver sus mensajes.
* Los mensajes se ordenan por fecha de creación en orden ascendente (más antiguos primero).
* El campo `taggable_info` proporciona contexto sobre qué originó la conversación (ej.: un pedido, una reserva, etc.).
* Los mensajes eliminados (`is_deleted: true`) aún se devuelven, pero pueden ocultarse en el frontend.

---

## Relacionados

- [Listar Conversaciones](./ChatConversationsList.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Marcar Mensaje como Leído](./ChatMessageMarkRead.md)
- [Agregar Reacción](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
