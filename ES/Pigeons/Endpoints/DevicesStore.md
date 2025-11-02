# Pigeons – Registrar Token de Dispositivo

## Endpoint
```
POST /api/v1/pigeons/chat/devices
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
| `token` | string | Sí | Token de Firebase Cloud Messaging (FCM) del dispositivo (máx 4096 caracteres) | - |
| `platform` | string | Sí | Plataforma del dispositivo | android, ios, web |
| `device.type` | string | No | Tipo de dispositivo | - |
| `device.os` | string | No | Nombre del sistema operativo | - |
| `device.os_version` | string | No | Versión del sistema operativo | - |
| `device.app_version` | string | No | Versión de la aplicación | - |
| `device.model` | string | No | Modelo del dispositivo | - |
| `device.browser` | string | No | Nombre del navegador (para web) | - |
| `device.browser_version` | string | No | Versión del navegador | - |

## Ejemplos

### Solicitud
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: es' \
  -d '{
    "token": "fGxR8hJ3T...",
    "platform": "android",
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
  "message": "Device token registrado",
  "data": {
    "id": "67890abcdef12345",
    "token": "fGxR8hJ3T...",
    "platform": "android"
  }
}
```

## Estructura JSON Explicada
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `message` | string | Mensaje de éxito |
| `data` | object | Información del token de dispositivo registrado |
| `data.id` | string | ID del documento del token de dispositivo |
| `data.token` | string | Token FCM del dispositivo |
| `data.platform` | string | Plataforma del dispositivo |

## Estado HTTP
| Código | Descripción |
|--------|-------------|
| 201 | Creado – token de dispositivo registrado exitosamente |
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
    "token": [
      "El campo token es obligatorio."
    ],
    "platform": [
      "El campo platform es obligatorio."
    ]
  }
}
```

## Notas
- Los tokens de dispositivo se utilizan para notificaciones push de Firebase Cloud Messaging (FCM)
- Si ya existe un token para la misma plataforma y usuario, será eliminado y recreado con metadatos actualizados
- Los tokens están asociados con el usuario autenticado y la plataforma actual (vía `X-PUBLIC-KEY`)
- La longitud máxima del token es de 4096 caracteres (límite del token FCM)
- Las plataformas soportadas son: `android`, `ios` y `web`
- Los metadatos del dispositivo son opcionales pero recomendados para análisis y depuración
- Los tokens se almacenan en MongoDB para consultas eficientes

## Relacionado
- [Eliminar Token de Dispositivo](./DevicesDestroy.md)
- [Enviar Mensaje](./MessagesStore.md)
- [Listar Conversaciones](./ConversationsIndex.md)
