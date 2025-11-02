# Pigeons – Crear Conversación

## Endpoint
```
POST /api/v1/pigeons/chat/conversations
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
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| `participant_uuid` | string | Sí | UUID del usuario con quien iniciar la conversación |
| `taggable_type` | string | No | Tipo de entidad relacionada (ej., "App\\Models\\Article") |
| `taggable_id` | integer | No | ID de la entidad relacionada |

## Ejemplos

### Solicitud
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: es' \
  -d '{
    "participant_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123
  }'
```

### Respuesta
```json
{
  "message": "Conversación creada exitosamente",
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-11-02T10:30:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "participants": [
      {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Usuario Actual",
        "email": "usuario@example.com"
      },
      {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Juan Pérez",
        "email": "juan@example.com"
      }
    ]
  }
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |
| `data` | object | Objeto de conversación creado |
| `data.uuid` | string | Identificador único de la conversación |
| `data.taggable_type` | string | Tipo de entidad relacionada |
| `data.taggable_id` | integer | ID de la entidad relacionada |
| `data.last_message_text` | string\|null | Texto del último mensaje (null para nuevas conversaciones) |
| `data.last_message_at` | string\|null | Timestamp ISO 8601 del último mensaje |
| `data.is_archived` | boolean | Estado de archivo de la conversación |
| `data.created_at` | string | Timestamp ISO 8601 de creación |
| `data.updated_at` | string | Timestamp ISO 8601 de actualización |
| `data.participants` | array | Array de usuarios participantes |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 201 | Creado – conversación creada exitosamente |
| 400 | Solicitud incorrecta – parámetros inválidos |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – permisos insuficientes |
| 422 | Entidad no procesable – errores de validación |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Errores
```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "participant_uuid": [
      "El campo participant uuid es obligatorio."
    ]
  }
}
```

## Notas
- Si ya existe una conversación entre los dos usuarios para el mismo `taggable_type` y `taggable_id`, puede devolverse la conversación existente en lugar de crear un duplicado
- El usuario autenticado se agrega automáticamente como participante
- `taggable_type` y `taggable_id` son opcionales y permiten asociar la conversación con una entidad específica
- `participant_uuid` debe ser un UUID de usuario válido existente

## Relacionado
- [Listar Conversaciones](./ConversationsIndex.md)
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
