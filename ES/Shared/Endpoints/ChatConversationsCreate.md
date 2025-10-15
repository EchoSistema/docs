# Chat – Crear Conversación

## Endpoint

`POST /api/v1/chat/conversations`

Crea una nueva conversación entre el usuario autenticado y otro participante.

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

### Parámetros del Cuerpo de la Solicitud

| Parámetro          | Tipo      | Requerido | Descripción |
| ------------------ | --------- | --------- | ----------- |
| `participant_uuid` | `uuid`    | Sí        | UUID del participante que se agregará a la conversación. |
| `taggable_type`    | `string`  | No        | Tipo del modelo polimórfico a asociar con la conversación (ej.: `App\Models\Order`). |
| `taggable_id`      | `integer` | No        | ID del modelo polimórfico a asociar con la conversación. |

---

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Content-Type: application/json" \
  -d '{
    "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123
  }' \
  "https://sandbox.su-dominio.com/api/v1/chat/conversations"
```

### Ejemplo de Cuerpo de Solicitud

```json
{
  "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "taggable_type": "App\\Models\\Order",
  "taggable_id": 123
}
```

### Ejemplo de Respuesta

```json
{
  "message": "Conversación creada exitosamente",
  "data": {
    "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-10-15T10:30:00.000000Z",
    "updated_at": "2025-10-15T10:30:00.000000Z",
    "participants": [
      {
        "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
        "name": "Juan Silva",
        "email": "juan@example.com",
        "pivot": {
          "conversation_id": 1,
          "user_id": 10,
          "unread_count": 0,
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
        }
      },
      {
        "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
        "name": "María Santos",
        "email": "maria@example.com",
        "pivot": {
          "conversation_id": 1,
          "user_id": 11,
          "unread_count": 0,
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
        }
      }
    ]
  }
}
```

---

## Estructura JSON Explicada

### Respuesta de Éxito

| Campo     | Tipo     | Descripción |
| --------- | -------- | ----------- |
| `message` | `string` | Mensaje de confirmación. |
| `data`    | `object` | Objeto de conversación creada con participantes cargados. |

### `data` – Objeto Conversación

| Campo               | Tipo      | Descripción |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Identificador único de la conversación. |
| `taggable_type`     | `string`  | Tipo del modelo polimórfico asociado (cuando aplica). |
| `taggable_id`       | `integer` | ID del modelo polimórfico asociado (cuando aplica). |
| `last_message_text` | `string`  | Texto del último mensaje (null en conversaciones nuevas). |
| `last_message_at`   | `string`  | Fecha del último mensaje (null en conversaciones nuevas). |
| `is_archived`       | `boolean` | Indica si la conversación está archivada. |
| `created_at`        | `string`  | Fecha de creación de la conversación (formato ISO-8601). |
| `updated_at`        | `string`  | Fecha de la última actualización (formato ISO-8601). |
| `participants[]`    | `array`   | Lista de participantes de la conversación. |

---

## Estados HTTP

- 201: Creado exitosamente
- 400: Solicitud inválida
- 401: No autorizado
- 422: Error de validación (participante no encontrado o parámetros inválidos)
- 500: Error interno del servidor

---

## Errores

### Ejemplo de Error de Validación

```json
{
  "message": "The participant uuid field is required.",
  "errors": {
    "participant_uuid": [
      "The participant uuid field is required."
    ]
  }
}
```

### Ejemplo de Error Interno

```json
{
  "success": false,
  "message": "Failed to create conversation",
  "error": "Database connection error"
}
```

---

## Notas

* La conversación se crea con ambos participantes (usuario autenticado y participante especificado).
* Los campos `taggable_type` y `taggable_id` son opcionales y permiten asociar la conversación con otro modelo (ej.: pedido, reserva, etc.).
* La conversación se crea con `is_archived` establecido en `false` por defecto.
* Ambos participantes tienen `unread_count` inicializado en `0`.
* La fecha de ingreso (`joined_at`) de ambos participantes se establece en el momento de la creación.

---

## Relacionados

- [Listar Conversaciones](./ChatConversationsList.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
