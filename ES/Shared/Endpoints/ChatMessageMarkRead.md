# Chat – Marcar Mensaje como Leído

## Endpoint

`PATCH /api/v1/chat/messages/{messageId}/read`

Marca un mensaje como leído, actualizando su estado y registrando la fecha/hora de lectura.

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

---

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/read"
```

### Ejemplo de Respuesta

```json
{
  "message": "Mensaje marcado como leído"
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
- 401: No autorizado
- 403: Prohibido (el usuario no es el destinatario del mensaje)
- 404: Mensaje no encontrado
- 500: Error interno del servidor

---

## Errores

### Ejemplo de Error de Autorización

```json
{
  "message": "No autorizado"
}
```

---

## Notas

* Solo el destinatario del mensaje (`receiver`) puede marcarlo como leído.
* La operación actualiza el campo `status.is_read` a `true` y `status.read_at` con la fecha/hora actual.
* El campo `timestamps.updated_at` también se actualiza.
* Si el mensaje ya está marcado como leído, la operación aún devolverá éxito.
* Esta acción puede usarse para actualizar contadores de mensajes no leídos en la interfaz de usuario.

---

## Relacionados

- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Agregar Reacción](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
