# Pigeons – Agregar Reacción a Mensaje

## Endpoint
```
POST /api/v1/pigeons/chat/messages/{messageId}/reactions
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

### Parámetros de Ruta
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| `messageId` | string | Sí | ID del mensaje |

### Cuerpo de la Solicitud
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| `emoji` | string | Sí | Reacción emoji (máx 10 caracteres) |

## Ejemplos

### Solicitud
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/reactions \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: es' \
  -d '{
    "emoji": "👍"
  }'
```

### Respuesta
```json
{
  "message": "Reacción agregada exitosamente"
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – reacción agregada |
| 400 | Solicitud incorrecta – parámetros inválidos |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – permisos insuficientes |
| 404 | No encontrado – el mensaje no existe |
| 422 | Entidad no procesable – errores de validación |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Errores
```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "emoji": [
      "El campo emoji es obligatorio."
    ]
  }
}
```

```json
{
  "message": "Mensaje no encontrado"
}
```

## Notas
- Cualquier participante de la conversación puede agregar reacciones a los mensajes
- El emoji está limitado a 10 caracteres (soporta la mayoría de emojis individuales y secuencias)
- Cada usuario puede agregar múltiples reacciones diferentes al mismo mensaje
- Agregar el mismo emoji nuevamente no creará un duplicado
- Las reacciones se almacenan en el array `reactions` del mensaje con el UUID del usuario y el timestamp
- Las actualizaciones en tiempo real pueden difundirse para notificar a los participantes de nuevas reacciones

## Relacionado
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
- [Marcar Mensaje como Leído](./MessagesRead.md)
