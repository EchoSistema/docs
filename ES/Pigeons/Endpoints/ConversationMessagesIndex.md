# Pigeons – Obtener Mensajes de Conversación

## Endpoint
```
GET /api/v1/pigeons/chat/conversations/{uuid}/messages
```

## Autenticación
Requerida – Bearer token con habilidad `backoffice`.

## Encabezados
| Encabezado | Tipo | Requerido | Descripción |
|------------|------|-----------|-------------|
| `Authorization` | string | Sí | Bearer {token} |
| `X-PUBLIC-KEY` | string | Sí | Clave pública de la plataforma |
| `Accept-Language` | string | No | Locale para campos traducibles (pt-BR, en, es) |

## Parámetros

### Parámetros de Ruta
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| `uuid` | string | Sí | UUID de la conversación |

## Ejemplos

### Solicitud
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations/550e8400-e29b-41d4-a716-446655440000/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: es'
```

### Respuesta
```json
{
  "data": [
    {
      "message_id": "msg_123456789",
      "id": "msg_123456789",
      "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "sender": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Juan Pérez",
        "avatar": "https://example.com/avatars/juan.jpg"
      },
      "receiver": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Usuario Actual",
        "avatar": "https://example.com/avatars/usuario.jpg"
      },
      "context": {
        "text": "Hola, ¿cómo estás?",
        "type": "text"
      },
      "attachments": [],
      "metadata": {},
      "reactions": [
        {
          "user_uuid": "770e8400-e29b-41d4-a716-446655440002",
          "emoji": "👍",
          "created_at": "2025-11-02T10:35:00+00:00"
        }
      ],
      "status": {
        "sent": true,
        "delivered": true,
        "read": true
      },
      "timestamps": {
        "sent_at": "2025-11-02T10:30:00+00:00",
        "delivered_at": "2025-11-02T10:30:05+00:00",
        "read_at": "2025-11-02T10:31:00+00:00"
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
  ],
  "conversation": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "last_message_text": "Hola, ¿cómo estás?",
    "last_message_at": "2025-11-02T10:30:00+00:00",
    "is_archived": false,
    "created_at": "2025-11-01T08:00:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "taggable_info": {
      "id": 123,
      "title": "Título del Artículo"
    }
  }
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `data` | array | Array de objetos de mensaje |
| `data[].message_id` | string | Identificador único del mensaje |
| `data[].conversation_uuid` | string | UUID de la conversación |
| `data[].sender` | object | Información del usuario remitente |
| `data[].receiver` | object | Información del usuario receptor |
| `data[].context` | object | Contenido y tipo del mensaje |
| `data[].context.text` | string | Contenido de texto del mensaje |
| `data[].context.type` | string | Tipo de mensaje (text, image, file, audio, video) |
| `data[].attachments` | array | Array de archivos adjuntos |
| `data[].metadata` | object | Metadatos adicionales del mensaje |
| `data[].reactions` | array | Array de reacciones emoji |
| `data[].status` | object | Estado de entrega del mensaje |
| `data[].timestamps` | object | Timestamps del mensaje (enviado, entregado, leído) |
| `data[].pbk` | string | Clave pública de la plataforma |
| `data[].device` | object | Información del dispositivo desde el que se envió el mensaje |
| `conversation` | object | Metadatos de la conversación |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – mensajes recuperados |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – el usuario no es participante de esta conversación |
| 404 | No encontrado – la conversación no existe |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Errores
```json
{
  "message": "Conversación no encontrada"
}
```

## Notas
- Solo los participantes de la conversación pueden recuperar mensajes
- Los mensajes se almacenan en MongoDB para alto rendimiento
- La respuesta incluye tanto mensajes como metadatos de la conversación
- Los mensajes incluyen información del dispositivo para rastrear desde qué dispositivo se envió cada mensaje
- Las reacciones se almacenan como un array y pueden ser agregadas por cualquier participante

## Relacionado
- [Listar Conversaciones](./ConversationsIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
- [Marcar Mensaje como Leído](./MessagesRead.md)
- [Agregar Reacción](./MessagesReactionsStore.md)
