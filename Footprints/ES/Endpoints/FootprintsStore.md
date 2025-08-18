# Footprints – Crear Footprint

## Endpoint

`POST /footprints`

Registra un rastro de navegación o interacción del visitante.

---

## Autenticación

Ninguna. Aun así, se debe enviar el encabezado `X-PUBLIC-KEY`.

---

## Solicitud

### Encabezados Requeridos

| Encabezado | Tipo | Descripción |
| --------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Clave pública de la plataforma necesaria para autenticar y recibir respuestas. |
| **Accept-Language** | `string` | Idioma para mensajes de error (ej.: `es`). Opcional. |

### Cuerpo

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ---------- | ----------- |
| `session_id` | `string` | Sí | Identificador de la sesión; usa el encabezado `x-correlation-id` si falta. |
| `event` | `FootprintEventEnum` | Sí | Tipo de evento. |
| `event_time` | `date` | Sí | Momento en que ocurrió el evento. |
| `page` | `string` | Sí | URL de la página visitada. |
| `visitor_ip` | `ip` | No | IP del visitante; detectada automáticamente. |
| `echo_id` | `string` | No | Identificador de espejo. |
| `user` | `string` | No | Referencia al usuario. |
| `referrer` | `string` | No | URL de origen. |
| `title` | `string` | No | Título de la página. |
| `utm.source` | `string` | No | Fuente de la campaña. |
| `utm.medium` | `string` | No | Medio de la campaña. |
| `utm.campaign` | `string` | No | Nombre de la campaña. |
| `utm.term` | `string` | No | Término de la campaña. |
| `utm.content` | `string` | No | Contenido de la campaña. |
| `utms` | `array` | No | Datos UTM adicionales. |
| `clids` | `array` | No | Identificadores de clic. |
| `browser.name` | `string` | No | Nombre del navegador. |
| `browser.version` | `string` | No | Versión del navegador. |
| `os.name` | `string` | No | Sistema operativo. |
| `os.version` | `string` | No | Versión del sistema operativo. |
| `device.type` | `string` | No | Tipo de dispositivo. |
| `device.brand` | `string` | No | Marca del dispositivo. |
| `device.model` | `string` | No | Modelo del dispositivo. |
| `screen.w` | `integer` | No | Ancho de la pantalla. |
| `screen.h` | `integer` | No | Alto de la pantalla. |
| `screen.dpr` | `numeric` | No | Densidad de píxeles. |
| `viewport.w` | `integer` | No | Ancho del viewport. |
| `viewport.h` | `integer` | No | Alto del viewport. |
| `timezone_offset` | `integer` | No | Desfase horario en minutos. |
| `connection_type` | `string` | No | Tipo de conexión. |
| `language` | `string` | No | Idioma del navegador. |
| `public_key` | `uuid` | No | Clave pública asociada. |
| `platform_id` | `integer` | No | ID de la plataforma. |
| `platform_uuid` | `uuid` | No | UUID de la plataforma. |
| `platform_language` | `string` | No | Idioma de la plataforma. |
| `platform_currency` | `string` | No | Moneda de la plataforma. |

### Ejemplo de Cuerpo

```json
{
  "echo_id": null,
  "session_id": "ec6db9bd-c4b2-414e-b354-be31760a4979",
  "event": "page_view",
  "event_time": "2025-08-17T23:56:02.827Z",
  "page": "https://encarnacion.defensadelconsumidor.online/laws",
  "referrer": null,
  "title": "Libro de Quejas - Leis",
  "utm": {
    "source": null,
    "medium": null,
    "campaign": null,
    "term": null,
    "content": null
  },
  "browser": {
    "name": "Edge",
    "version": "140.0.0.0"
  },
  "os": {
    "name": "Linux",
    "version": null
  },
  "device": {
    "type": "desktop",
    "brand": null,
    "model": null
  },
  "screen": {
    "w": 1920,
    "h": 1080,
    "dpr": 1
  },
  "viewport": {
    "w": 1912,
    "h": 964
  },
  "timezone_offset": 180,
  "connection_type": "4g",
  "language": "pt-BR",
  "platform_id": 7,
  "platform_uuid": "649914d2-f6ba-3b3f-87f3-ad3f55e0c0f6",
  "platform_language": "pt-BR",
  "platform_currency": "BRL",
  "visitor_ip": "181.120.47.200"
}
```

---

## Respuestas

### Éxito `201 Created`

```json
["success"]
```

### Error `400 Bad Request`

Devuelve un objeto con el mensaje de error.
