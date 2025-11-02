# Pigeons – Obtener Estadísticas de Dispositivos

## Endpoint
```
GET /api/v1/pigeons/chat/stats/devices
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
  https://api.example.com/api/v1/pigeons/chat/stats/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: es'
```

### Respuesta
```json
{
  "data": [
    {
      "_id": "mobile",
      "count": 150
    },
    {
      "_id": "web",
      "count": 85
    },
    {
      "_id": "desktop",
      "count": 23
    }
  ]
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `data` | array | Array de estadísticas por tipo de dispositivo |
| `data[]._id` | string | Tipo de dispositivo (mobile, web, desktop) |
| `data[].count` | integer | Número de mensajes enviados desde este tipo de dispositivo |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – estadísticas recuperadas |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – permisos insuficientes |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Notas
- Devuelve estadísticas agregadas de mensajes por tipo de dispositivo
- Solo incluye estadísticas de conversaciones donde el usuario autenticado es participante
- Los datos se agregan desde MongoDB utilizando el pipeline de agregación `$group`
- Si el usuario no tiene conversaciones, devuelve un array vacío
- Los tipos de dispositivo son: `mobile`, `web` o `desktop`
- El conteo representa el número total de mensajes enviados desde cada tipo de dispositivo en todas las conversaciones del usuario

## Relacionado
- [Listar Conversaciones](./ConversationsIndex.md)
- [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md)
- [Enviar Mensaje](./MessagesStore.md)
