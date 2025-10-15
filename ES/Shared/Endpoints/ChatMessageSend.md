# Chat – Enviar Mensaje

## Endpoint

`POST /api/v1/chat/messages`

Envía un nuevo mensaje en una conversación existente, con soporte para diferentes tipos de contenido, archivos adjuntos e información del dispositivo.

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

### Parámetros del Cuerpo de la Solicitud

| Parámetro               | Tipo     | Requerido | Descripción |
| ----------------------- | -------- | --------- | ----------- |
| `conversation_uuid`     | `uuid`   | Sí        | UUID de la conversación. |
| `receiver_uuid`         | `uuid`   | Sí        | UUID del destinatario del mensaje. |
| `context_text`          | `string` | Sí        | Texto del mensaje (máx. 5000 caracteres). |
| `context_type`          | `string` | Sí        | Tipo de contenido: `text`, `image`, `file`, `audio`, `video`. |
| `attachments[]`         | `array`  | No        | Lista de archivos adjuntos (estructura libre). |
| `metadata`              | `object` | No        | Metadatos adicionales (estructura libre). |
| `device.type`           | `string` | Sí        | Tipo de dispositivo: `mobile`, `web`, `desktop`. |
| `device.os`             | `string` | Sí        | Sistema operativo (ej.: `Windows`, `iOS`, `Android`). |
| `device.os_version`     | `string` | No        | Versión del sistema operativo. |
| `device.app_version`    | `string` | No        | Versión de la aplicación. |
| `device.model`          | `string` | No        | Modelo del dispositivo. |
| `device.browser`        | `string` | No        | Nombre del navegador. |
| `device.browser_version`| `string` | No        | Versión del navegador. |

---

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Content-Type: application/json" \
  -d '{
    "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "context_text": "Hola, ¿cómo estás?",
    "context_type": "text",
    "attachments": [],
    "metadata": {
      "source": "web_app"
    },
    "device": {
      "type": "web",
      "os": "Windows",
      "os_version": "10",
      "app_version": "1.0.0",
      "browser": "Chrome",
      "browser_version": "120.0"
    }
  }' \
  "https://sandbox.su-dominio.com/api/v1/chat/messages"
```

### Ejemplo de Cuerpo de Solicitud

```json
{
  "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "context_text": "Hola, ¿cómo estás?",
  "context_type": "text",
  "attachments": [],
  "metadata": {
    "source": "web_app"
  },
  "device": {
    "type": "web",
    "os": "Windows",
    "os_version": "10",
    "app_version": "1.0.0",
    "browser": "Chrome",
    "browser_version": "120.0"
  }
}
```

### Ejemplo de Respuesta

```json
{
  "message": "Mensaje enviado exitosamente",
  "data": {
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
    "metadata": {
      "source": "web_app"
    },
    "reactions": [],
    "status": {
      "is_read": false,
      "read_at": null,
      "delivered_at": "2025-10-15T10:30:00+00:00",
      "is_deleted": false
    },
    "timestamps": {
      "created_at": "2025-10-15T10:30:00+00:00",
      "updated_at": "2025-10-15T10:30:00+00:00"
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
}
```

---

## Estructura JSON Explicada

### Respuesta de Éxito

| Campo     | Tipo     | Descripción |
| --------- | -------- | ----------- |
| `message` | `string` | Mensaje de confirmación. |
| `data`    | `object` | Objeto del mensaje creado. |

### `data` – Objeto Mensaje

Ver estructura completa en [Listar Mensajes de una Conversación](./ChatConversationMessages.md).

---

## Estados HTTP

- 201: Creado exitosamente
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido (el usuario no es participante de la conversación)
- 404: Conversación o destinatario no encontrado
- 422: Error de validación
- 500: Error interno del servidor

---

## Errores

### Ejemplo de Error de Validación

```json
{
  "message": "The conversation uuid field is required.",
  "errors": {
    "conversation_uuid": [
      "The conversation uuid field is required."
    ],
    "context_text": [
      "The context text must not be greater than 5000 characters."
    ]
  }
}
```

### Ejemplo de Error Interno

```json
{
  "success": false,
  "message": "Failed to send message",
  "error": "Database connection error"
}
```

---

## Notas

* El usuario autenticado debe ser participante de la conversación especificada.
* El texto del mensaje está limitado a 5000 caracteres.
* El mensaje se crea con estado `is_read: false` y `read_at: null` por defecto.
* El campo `delivered_at` se completa automáticamente con la fecha/hora actual.
* La conversación se actualiza con `last_message_text` y `last_message_at` después del envío.
* Se dispara un evento `MessageSent` mediante broadcast para notificar a otros participantes en tiempo real.
* Los campos del dispositivo (`device`) se almacenan para análisis y estadísticas.

---

## Relacionados

- [Listar Conversaciones](./ChatConversationsList.md)
- [Crear Conversación](./ChatConversationsCreate.md)
- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)
- [Marcar Mensaje como Leído](./ChatMessageMarkRead.md)
- [Estadísticas de Dispositivos](./ChatDeviceStats.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
