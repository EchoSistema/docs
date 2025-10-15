# Shared – Autorizar canal conversation.{uuid}

## Endpoint

```
POST /broadcasting/auth
```

## Autenticación

Obligatoria – Bearer {token}

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Authorization   | string | Sí          | Credencial `Bearer {token}` del usuario autenticado. |
| X-PUBLIC-KEY    | string | No          | Solo necesario cuando la aplicación exige clave pública de plataforma. |
| Accept-Language | string | No          | Locale IETF (`pt-BR`, `en`, `es`) para mensajes traducibles. |

## Parámetros

### Parámetros de ruta

Ninguno.

### Parámetros de consulta

Ninguno.

### Cuerpo de la solicitud

| Campo         | Tipo   | Obligatorio | Descripción |
| ------------- | ------ | ----------- | ----------- |
| channel_name  | string | Sí          | Debe seguir `conversation.{uuid}`, donde `{uuid}` es el identificador de la conversación. |
| socket_id     | string | Sí          | Identificador de socket entregado por Pusher/Echo al abrir el WebSocket. |

```json
{
  "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
  "socket_id": "1234.5678"
}
```

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  https://sandbox.ejemplo.com/broadcasting/auth \
  -d '{
    "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
    "socket_id": "1234.5678"
  }'
```

### Ejemplo de respuesta

```json
{
  "auth": "123456:abcdef0123456789",
  "channel_data": {
    "user_id": 42,
    "user_info": {
      "uuid": "b7c1e1f2-8f20-4e4b-a6e1-0bf8f2c8c7b3"
    }
  }
}
```

## Estructura JSON explicada

| Campo                      | Tipo   | Descripción |
| -------------------------- | ------ | ----------- |
| auth                       | string | Token generado por el backend que autoriza la suscripción al canal privado. |
| channel_data               | object | Payload opcional reenviado al proveedor de broadcast. |
| channel_data.user_id       | integer| ID interno del usuario autenticado. |
| channel_data.user_info     | object | Metadatos adicionales del usuario. |
| channel_data.user_info.uuid| string | UUID del usuario autenticado. |

## Estados HTTP

- 200: Autorización concedida
- 401: Token ausente o inválido
- 403: El usuario no participa de la conversación
- 404: Conversación no encontrada
- 429: Límite de peticiones excedido
- 500: Error interno inesperado

## Errores

```json
{
  "message": "No puedes unirte a esta conversación.",
  "errors": null
}
```

## Notas

- Solo los participantes de la conversación identificada por el UUID pueden suscribirse a `conversation.{uuid}`.
- La autorización reutiliza la relación `participants` para validar la pertenencia de forma atómica.
- Asegúrate de mantener sincronizado el UUID de la conversación entre MySQL (tabla `conversations`) y el frontend.

## Relacionados

- Aún no hay endpoints relacionados documentados.

## Changelog

- 2025-10-15: Documento inicial.
