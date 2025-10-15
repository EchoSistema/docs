# Chat – Estadísticas de Dispositivos

## Endpoint

`GET /api/v1/chat/stats/devices`

Recupera estadísticas agregadas sobre los tipos de dispositivos utilizados para enviar mensajes en las conversaciones del usuario autenticado.

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

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/chat/stats/devices"
```

### Ejemplo de Respuesta

```json
{
  "data": [
    {
      "_id": "web",
      "count": 125
    },
    {
      "_id": "mobile",
      "count": 87
    },
    {
      "_id": "desktop",
      "count": 43
    }
  ]
}
```

---

## Estructura JSON Explicada

### Respuesta de Éxito

| Campo    | Tipo    | Descripción |
| -------- | ------- | ----------- |
| `data[]` | `array` | Lista de estadísticas agregadas por tipo de dispositivo. |

### `data[]` – Estadística de Dispositivo

| Campo   | Tipo      | Descripción |
| ------- | --------- | ----------- |
| `_id`   | `string`  | Tipo de dispositivo: `web`, `mobile`, `desktop`. |
| `count` | `integer` | Cantidad total de mensajes enviados desde este tipo de dispositivo. |

---

## Estados HTTP

- 200: OK
- 401: No autorizado
- 500: Error interno del servidor

---

## Notas

* Las estadísticas incluyen solo mensajes de conversaciones en las que el usuario autenticado es participante.
* La agregación se realiza utilizando el MongoDB Aggregation Framework.
* El campo `_id` corresponde al campo `device.type` almacenado en cada mensaje.
* Si el usuario no tiene conversaciones, se devuelve un array vacío.
* Esta API es útil para análisis de uso y comportamiento, permitiendo identificar qué dispositivos son más utilizados.
* Los datos pueden utilizarse para optimizar la experiencia del usuario en diferentes plataformas.

---

## Relacionados

- [Listar Conversaciones](./ChatConversationsList.md)
- [Enviar Mensaje](./ChatMessageSend.md)
- [Listar Mensajes de una Conversación](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Creación inicial de la documentación.
