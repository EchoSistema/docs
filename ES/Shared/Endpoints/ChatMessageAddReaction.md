# Chat – Agregar Reacción a Mensaje

## Endpoint

`POST /api/v1/chat/messages/{messageId}/reactions`

Agrega una reacción (emoji) a un mensaje existente.

---

## Autenticación

**Requerida** – Token Bearer.

---

## Solicitud

### Encabezados Requeridos

| Encabezado        | Tipo     | Descripción |
| ----------------- | -------- | ----------- |
| **Authorization** | `string` | Credencial `Bearer {token}`. **Requerido**. |
| **X-PUBLIC-KEY**  | `string` | Clave pública que identifica la plataforma solicitante. **Requerido**. |

### Parámetros de Ruta

| Parámetro   | Tipo     | Requerido | Descripción |
| ----------- | -------- | --------- | ----------- |
| `messageId` | `string` | Sí        | ID del mensaje (MongoDB ObjectId). |

### Parámetros del Cuerpo de la Solicitud

| Parámetro | Tipo     | Requerido | Descripción |
| --------- | -------- | --------- | ----------- |
| `emoji`   | `string` | Sí        | Emoji de la reacción (máx. 10 caracteres). |

---

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Content-Type: application/json" \
  -d '{
    "emoji": "👍"
  }' \
  "https://sandbox.su-dominio.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/reactions"
```

### Ejemplo de Cuerpo de Solicitud

```json
{
  "emoji": "👍"
}
```

### Ejemplo de Respuesta

```json
{
  "message": "Reacción agregada exitosamente"
}
```

---

## Estructura JSON Explicada

### Respuesta de Éxito

| Campo     | Tipo     | Descripción |
| --------- | -------- | ----------- |
| `message` | `string` | Mensaje de confirmación. |

---

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 404: Mensaje no encontrado
- 422: Error de validación (emoji inválido o demasiado largo)
- 500: Error interno del servidor

---

## Errores

### Ejemplo de Error de Validación

```json
{
  "message": "The emoji field is required.",
  "errors": {
    "emoji": [
      "The emoji field is required."
    ]
  }
}
```

---

## Notas

* Cualquier usuario autenticado puede agregar una reacción a un mensaje.
* El mismo usuario puede agregar múltiples reacciones (emojis diferentes) al mismo mensaje.
* La reacción se agrega al array `reactions` del mensaje con el UUID del usuario, el emoji y la fecha/hora actual.
* El campo `timestamps.updated_at` del mensaje se actualiza.
* No hay validación si el usuario ya agregó el mismo emoji anteriormente (permitiendo duplicados).
* El emoji está limitado a 10 caracteres para soportar emojis compuestos.

---

## Relacionados

- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Marcar Mensaje como Leído](./ChatMessageMarkRead.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
