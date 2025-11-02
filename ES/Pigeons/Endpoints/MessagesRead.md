# Pigeons – Marcar Mensaje como Leído

## Endpoint
```
PATCH /api/v1/pigeons/chat/messages/{messageId}/read
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
| `messageId` | string | Sí | ID del mensaje |

## Ejemplos

### Solicitud
```bash
curl -X PATCH \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/read \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: es'
```

### Respuesta
```json
{
  "message": "Mensaje marcado como leído"
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – mensaje marcado como leído |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – el usuario no es el receptor de este mensaje |
| 404 | No encontrado – el mensaje no existe |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Errores
```json
{
  "message": "Mensaje no encontrado"
}
```

```json
{
  "message": "No autorizado"
}
```

## Notas
- Solo el receptor del mensaje puede marcarlo como leído
- El `status.read` del mensaje se establece en `true`
- El `timestamps.read_at` se establece en el timestamp actual
- Se difunde un evento de confirmación de lectura en tiempo real al canal de conversación
- El `unread_count` del receptor para esta conversación se decrementa
- Si el mensaje ya estaba marcado como leído, la operación es idempotente

## Relacionado
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
- [Listar Conversaciones](./ConversationsIndex.md)
