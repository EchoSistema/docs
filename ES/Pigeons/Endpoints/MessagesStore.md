# Pigeons – Enviar Mensaje

## Endpoint
```
POST /api/v1/pigeons/chat/messages
```

## Autenticación
Requerida – Bearer token con habilidad `backoffice`.

## Encabezados
| Encabezado | Tipo | Requerido | Descripción |
|------------|------|-----------|-------------|
| `Authorization` | string | Sí | Bearer {token} |
| `X-PUBLIC-KEY` | string | Sí | Clave pública de la plataforma |
| `Accept-Language` | string | No | Locale para campos traducibles (pt-BR, en, es) |
| `Content-Type` | string | Sí | application/json |

## Parámetros

### Cuerpo de la Solicitud
| Parámetro | Tipo | Requerido | Descripción | Valores |
|-----------|------|-----------|-------------|---------|
| `conversation_uuid` | string | Sí | UUID de la conversación | UUID válido de conversación |
| `receiver_uuid` | string | Sí | UUID del destinatario del mensaje | UUID válido de usuario |
| `context_text` | string | Sí | Contenido de texto del mensaje (máx 5000 caracteres) | - |
| `context_type` | string | Sí | Tipo de mensaje | text, image, file, audio, video |
| `attachments` | array | No | Array de archivos adjuntos | - |
| `metadata` | object | No | Metadatos adicionales del mensaje | - |
| `device.type` | string | Sí | Tipo de dispositivo | mobile, web, desktop |
| `device.os` | string | Sí | Nombre del sistema operativo | - |
| `device.os_version` | string | No | Versión del sistema operativo | - |
| `device.app_version` | string | No | Versión de la aplicación | - |
| `device.model` | string | No | Modelo del dispositivo | - |
| `device.browser` | string | No | Nombre del navegador (para web/desktop) | - |
| `device.browser_version` | string | No | Versión del navegador | - |

## Ejemplos

### Solicitud
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: es' \
  -d '{
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "receiver_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "context_text": "Hola, ¿cómo estás?",
    "context_type": "text",
    "attachments": [],
    "metadata": {},
    "device": {
      "type": "mobile",
      "os": "Android",
      "os_version": "14",
      "app_version": "1.0.0",
      "model": "Samsung Galaxy S23"
    }
  }'
```

### Respuesta
```json
{
  "message": "Mensaje enviado exitosamente",
  "data": {
    "message_id": "msg_123456789",
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "sender": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Usuario Actual",
      "avatar": "https://example.com/avatars/usuario.jpg"
    },
    "receiver": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "Juan Pérez",
      "avatar": "https://example.com/avatars/juan.jpg"
    },
    "context": {
      "text": "Hola, ¿cómo estás?",
      "type": "text"
    },
    "attachments": [],
    "metadata": {},
    "reactions": [],
    "status": {
      "sent": true,
      "delivered": false,
      "read": false
    },
    "timestamps": {
      "sent_at": "2025-11-02T10:30:00+00:00"
    },
    "pbk": "platform_public_key",
    "device": {
      "type": "mobile",
      "os": "Android",
      "os_version": "14",
      "app_version": "1.0.0",
      "model": "Samsung Galaxy S23"
    }
  }
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |
| `data` | object | Objeto de mensaje creado |
| `data.message_id` | string | Identificador único del mensaje |
| `data.conversation_uuid` | string | UUID de la conversación |
| `data.sender` | object | Información del usuario remitente (usuario autenticado) |
| `data.receiver` | object | Información del usuario receptor |
| `data.context` | object | Contenido y tipo del mensaje |
| `data.attachments` | array | Array de archivos adjuntos |
| `data.metadata` | object | Metadatos adicionales del mensaje |
| `data.reactions` | array | Array de reacciones emoji (vacío para mensajes nuevos) |
| `data.status` | object | Estado de entrega del mensaje |
| `data.timestamps` | object | Timestamps del mensaje |
| `data.pbk` | string | Clave pública de la plataforma |
| `data.device` | object | Información del dispositivo |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 201 | Creado – mensaje enviado exitosamente |
| 400 | Solicitud incorrecta – parámetros inválidos |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – el usuario no es participante de esta conversación |
| 404 | No encontrado – conversación o receptor no encontrado |
| 422 | Entidad no procesable – errores de validación |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Errores
```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "context_text": [
      "El campo context text es obligatorio."
    ],
    "device.type": [
      "El campo device.type es obligatorio."
    ]
  }
}
```

## Notas
- Los mensajes se almacenan en MongoDB para alto rendimiento
- Se envían notificaciones en tiempo real a través de WebSockets/broadcasting
- Se envían notificaciones push a los tokens de dispositivo registrados
- `last_message_at` y `last_message_text` de la conversación se actualizan automáticamente
- El `unread_count` del participante se incrementa automáticamente para el receptor
- La longitud máxima del mensaje es de 5000 caracteres
- Se requiere información del dispositivo para rastrear las fuentes de los mensajes

## Relacionado
- [Listar Conversaciones](./ConversationsIndex.md)
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Marcar Mensaje como Leído](./MessagesRead.md)
- [Agregar Reacción](./MessagesReactionsStore.md)
