# Pigeons – Listar Conversaciones

## Endpoint
```
GET /api/v1/pigeons/chat/conversations
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
No se requieren parámetros.

## Ejemplos

### Solicitud
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: es'
```

### Respuesta
```json
{
  "data": [
    {
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
      },
      "participants": [
        {
          "uuid": "660e8400-e29b-41d4-a716-446655440001",
          "name": "Juan Pérez",
          "email": "juan@example.com",
          "avatar": "https://example.com/avatars/juan.jpg",
          "avatar_url": "https://example.com/avatars/juan.jpg",
          "pivot": {
            "unread_count": 3,
            "last_read_at": "2025-11-02T09:00:00+00:00",
            "joined_at": "2025-11-01T08:00:00+00:00"
          }
        }
      ]
    }
  ]
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `data` | array | Array de objetos de conversación |
| `data[].uuid` | string | Identificador único de la conversación |
| `data[].taggable_type` | string | Tipo de entidad relacionada (polimórfico) |
| `data[].last_message_text` | string | Texto del último mensaje |
| `data[].last_message_at` | string | Timestamp ISO 8601 del último mensaje |
| `data[].is_archived` | boolean | Estado de archivo de la conversación |
| `data[].created_at` | string | Timestamp ISO 8601 de creación |
| `data[].updated_at` | string | Timestamp ISO 8601 de actualización |
| `data[].taggable_info` | object | Información sobre la entidad relacionada |
| `data[].participants` | array | Array de usuarios participantes |
| `data[].participants[].uuid` | string | Identificador único del usuario |
| `data[].participants[].name` | string | Nombre completo del usuario |
| `data[].participants[].email` | string | Dirección de correo electrónico del usuario |
| `data[].participants[].avatar` | string | URL del avatar del usuario |
| `data[].participants[].pivot.unread_count` | integer | Número de mensajes no leídos para este participante |
| `data[].participants[].pivot.last_read_at` | string | Timestamp ISO 8601 de última lectura |
| `data[].participants[].pivot.joined_at` | string | Timestamp ISO 8601 cuando el usuario se unió a la conversación |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – conversaciones recuperadas |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – permisos insuficientes |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Notas
- Las conversaciones se ordenan por `last_message_at` descendente (más reciente primero)
- Solo devuelve conversaciones donde el usuario autenticado es participante
- Incluye participantes con sus imágenes de avatar y recuentos de mensajes no leídos
- `taggable_type` y `taggable_id` permiten asociar conversaciones con otras entidades (artículos, propiedades, etc.)

## Relacionado
- [Crear Conversación](./ConversationsStore.md)
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
