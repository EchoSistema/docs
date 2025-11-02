# Pigeons – Eliminar Token de Dispositivo

## Endpoint
```
DELETE /api/v1/pigeons/chat/devices/{token}
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
| `token` | string | Sí | Token FCM del dispositivo a eliminar |

## Ejemplos

### Solicitud
```bash
curl -X DELETE \
  https://api.example.com/api/v1/pigeons/chat/devices/fGxR8hJ3T... \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: es'
```

### Respuesta
```json
{
  "message": "Device token removido"
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 200 | Éxito – token de dispositivo eliminado |
| 401 | No autorizado – token inválido o faltante |
| 403 | Prohibido – permisos insuficientes |
| 429 | Demasiadas solicitudes – límite de tasa excedido |
| 500 | Error interno del servidor |

## Notas
- Elimina el token de dispositivo para el usuario autenticado y la plataforma actual
- Solo el propietario del token puede eliminar sus propios tokens
- Si el token no existe, la operación se completa exitosamente (idempotente)
- Después de la eliminación, ya no se enviarán notificaciones push a este dispositivo
- Los usuarios deben llamar a este endpoint cuando:
  - Cierran sesión en la aplicación
  - Desinstalan la aplicación
  - Deshabilitan las notificaciones push
- El token se compara con el UUID del usuario y la clave pública de la plataforma por seguridad

## Relacionado
- [Registrar Token de Dispositivo](./DevicesStore.md)
- [Enviar Mensaje](./MessagesStore.md)
- [Listar Conversaciones](./ConversationsIndex.md)
