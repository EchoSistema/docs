# Chat – Listado de Conversaciones

## Endpoint

`GET /api/v1/chat/conversations`

Recupera una lista de conversaciones del usuario autenticado, ordenadas por la fecha del último mensaje.

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

---

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Example",
      "last_message_text": "Hola, ¿cómo estás?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Example",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Ejemplo de Contexto"
      },
      "participants": [
        {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "Juan Silva",
          "email": "juan@example.com",
          "pivot": {
            "conversation_id": 1,
            "user_id": 10,
            "unread_count": 2,
            "last_read_at": "2025-10-15T09:00:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T09:00:00.000000Z"
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
            "last_read_at": "2025-10-15T10:30:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T10:30:00.000000Z"
          }
        }
      ]
    }
  ]
}
```

---

## Estructura JSON Explicada

### `data[]` – Array de Conversaciones

| Campo               | Tipo      | Descripción |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Identificador único de la conversación. |
| `taggable_type`     | `string`  | Tipo del modelo polimórfico asociado a la conversación (cuando aplica). |
| `last_message_text` | `string`  | Texto del último mensaje enviado en la conversación. |
| `last_message_at`   | `string`  | Fecha y hora del último mensaje (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica si la conversación está archivada. |
| `created_at`        | `string`  | Fecha de creación de la conversación (formato ISO-8601). |
| `updated_at`        | `string`  | Fecha de la última actualización de la conversación (formato ISO-8601). |
| `taggable_info`     | `object`  | Información del contexto asociado a la conversación. |
| `participants[]`    | `array`   | Lista de participantes de la conversación. |

### `taggable_info`

| Campo   | Tipo     | Descripción |
| ------- | -------- | ----------- |
| `type`  | `string` | Nombre de la clase del modelo asociado. |
| `uuid`  | `uuid`   | UUID del modelo asociado. |
| `title` | `string` | Título o nombre del modelo asociado. |

### `participants[]` – Participante

| Campo   | Tipo     | Descripción |
| ------- | -------- | ----------- |
| `uuid`  | `uuid`   | UUID del participante. |
| `name`  | `string` | Nombre del participante. |
| `email` | `string` | Correo electrónico del participante. |
| `pivot` | `object` | Información de la relación muchos a muchos. |

### `pivot` – Información del Participante en la Conversación

| Campo             | Tipo      | Descripción |
| ----------------- | --------- | ----------- |
| `conversation_id` | `integer` | ID interno de la conversación. |
| `user_id`         | `integer` | ID interno del usuario. |
| `unread_count`    | `integer` | Cantidad de mensajes no leídos. |
| `last_read_at`    | `string`  | Fecha y hora de la última lectura (formato ISO-8601). |
| `joined_at`       | `string`  | Fecha y hora en que el participante se unió a la conversación (formato ISO-8601). |
| `created_at`      | `string`  | Fecha de creación de la relación (formato ISO-8601). |
| `updated_at`      | `string`  | Fecha de la última actualización de la relación (formato ISO-8601). |

---

## Estados HTTP

- 200: OK
- 401: No autorizado
- 500: Error interno del servidor

---

## Notas

* Las conversaciones se ordenan por la fecha del último mensaje (`last_message_at`) en orden descendente.
* Solo se devuelven conversaciones en las que el usuario autenticado es participante.
* El campo `taggable_info` puede ser `null` si no hay contexto asociado a la conversación.
* El contador `unread_count` indica cuántos mensajes aún no ha leído el participante.

---

## Relacionados

- [Crear Conversación](./ChatConversationsCreate.md)
- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
