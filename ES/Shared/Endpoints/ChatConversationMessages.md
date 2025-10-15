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
  "data": [
    {
      "conversation_uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
      "sender": {
        "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
        "name": "Ewerton Girardi",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/ewerton-daniel-080933300-1643321172.png"
      },
      "receiver": {
        "uuid": "05e12689-1a76-3af3-8326-7028e3b8a503",
        "name": "Daniel C. S. C. Soares",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/3-daniel-c-s-c-soares.png"
      },
      "context": {
        "text": "¡Hola! ¿Cómo estás?",
        "texts": {
          "pt-BR": "Olá! Como você está?",
          "es": "¡Hola! ¿Cómo estás?",
          "en": "Hello! How are you?"
        },
        "type": "text"
      },
      "attachments": [],
      "metadata": [],
      "reactions": [],
      "status": {
        "is_read": true,
        "read_at": "2025-10-15T09:36:04-03:00",
        "delivered_at": "2025-10-15T09:35:04-03:00",
        "is_deleted": false
      },
      "timestamps": {
        "created_at": "2025-10-15T09:34:04-03:00",
        "updated_at": "2025-10-15T09:34:04-03:00"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "device": {
        "type": "mobile",
        "os": "macOS",
        "os_version": "11",
        "app_version": "1.0.0",
        "model": null,
        "browser": "Edge",
        "browser_version": "121.0"
      },
      "message_id": "68efa2cc35334e074c00abde",
      "id": "68efa2cc35334e074c00abde"
    }
  ],
  "conversation": {
    "uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
    "taggable_type": null,
    "last_message_text": "¡Gracias! Cuando termine aviso al equipo de QA.",
      "last_message_at": "2025-10-15T13:29:04.000000Z",
      "is_archived": false,
      "created_at": "2025-10-15T13:34:04.000000Z",
      "updated_at": "2025-10-15T13:34:04.000000Z",
      "taggable_info": null
  }
}
```

---

## Estructura JSON Explicada

### Cuerpo de la Respuesta

| Campo          | Tipo     | Descripción |
| -------------- | -------- | ----------- |
| `data`         | `array`  | Mensajes pertenecientes a la conversación. |
| `conversation` | `object` | Conversación asociada a los mensajes listados. |

### `conversation`

| Campo               | Tipo      | Descripción |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Identificador único de la conversación. |
| `taggable_type`     | `string`  | Tipo del modelo polimórfico asociado. |
| `last_message_text` | `string`  | Texto del último mensaje. |
| `last_message_at`   | `string`  | Fecha del último mensaje (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica si la conversación está archivada. |
| `created_at`        | `string`  | Fecha de creación de la conversación (formato ISO-8601). |
| `updated_at`        | `string`  | Fecha de la última actualización (formato ISO-8601). |
| `taggable_info`     | `object`  | Información del contexto asociado (puede ser `null`). |

### `data[]`

| Campo                | Tipo     | Descripción |
| -------------------- | -------- | ----------- |
| `message_id`         | `string` | ID único del mensaje (MongoDB ObjectId). |
| `id`                 | `string` | Alias legado para `message_id`. |
| `conversation_uuid`  | `uuid`   | UUID de la conversación. |
| `sender`             | `object` | Información del remitente. |
| `receiver`           | `object` | Información del destinatario. |
| `context`            | `object` | Contenido del mensaje. |
| `attachments`        | `array`  | Lista de archivos adjuntos (puede estar vacía). |
| `metadata`           | `array`  | Lista de metadatos adicionales. |
| `reactions`          | `array`  | Lista de reacciones (emojis). |
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
| `text`  | `string`  | Texto principal del mensaje. |
| `texts` | `object?` | Traducciones opcionales por locale disponible. |
| `type`  | `string`  | Tipo de contenido: `text`, `image`, `file`, `audio`, `video`. |

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
* El campo `context.texts` es opcional y solo aparece cuando existen traducciones almacenadas para el mensaje.

---

## Relacionados

- [Listar Conversaciones](./ChatConversationsList.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Marcar Mensaje como Leído](./ChatMessageMarkRead.md)
- [Agregar Reacción](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
