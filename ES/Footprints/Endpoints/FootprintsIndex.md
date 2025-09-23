# Footprints – Endpoint de Listado

## Endpoint

`GET /api/v1/footprints`

Recupera una lista de footprints con filtrado y paginación opcionales.

---

## Autenticación

Requiere un token válido de Sanctum.

### Encabezados Requeridos

| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| **Authorization** | `string` | Se requiere `Bearer <token>`. |
| **X-PUBLIC-KEY** | `string` | Clave pública para identificar la plataforma. Obligatoria. |
| **Accept-Language** | `string` | Locale para campos traducibles. Opcional. |

---

## Solicitud

### Parámetros de Filtro

#### Footprint IP

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `ip_address` | `ip` | Dirección IP del visitante. |
| `is_banned` | `boolean` | Filtra IPs baneadas. |
| `is_robot` | `boolean` | Filtra banderas de robots. |

#### Plataforma

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `platform_identifier` | `integer` | Identificador de la plataforma. |
| `platform_name` | `string` | Nombre de la plataforma. |
| `platform_email` | `email` | Correo de la plataforma. |
| `platform_open` | `boolean` | Plataforma abierta. |
| `platform_is_parent` | `boolean` | La plataforma es parent. |
| `domain_area_id` | `integer` | ID del área de dominio. |
| `platform_currency_id` | `integer` | ID de moneda de la plataforma. |
| `platform_language` | `string` | Idioma de la plataforma. |
| `platform_has_company` | `boolean` | La plataforma posee empresa. |
| `platform_country` | `string` | País de la plataforma. |
| `platform_region` | `string` | Región de la plataforma. |
| `platform_city` | `string` | Ciudad de la plataforma. |
| `platform_user` | `string` | Usuario de la plataforma. |

#### Usuario

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `has_user` | `boolean` | Cuando `true`, devuelve solo footprints con relación de usuario; cuando `false`, devuelve solo los que no tienen usuario. |
| `user_id` | `integer` | ID del usuario. |
| `user_uuid` | `uuid` | UUID del usuario. |
| `user_email` | `email` | Correo del usuario. |
| `user_name` | `string` | Nombre del usuario. |
| `user_gender` | `string` | Género del usuario. |
| `user_birth_date` | `date` | Fecha de nacimiento. |
| `user_birth_from` | `date` | Nacimiento desde. |
| `user_birth_to` | `date` | Nacimiento hasta. |
| `user_email_verified` | `boolean` | Correo verificado. |

#### Footprint

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `footprint_ip_id` | `integer` | ID del IP del footprint. |
| `session_id` | `string` | ID de la sesión. |
| `event` | `string` | Evento único. |
| `events` | `array` | Lista de eventos. |
| `events[]` | `string` | Valor del evento. |
| `event_time` | `date` | Fecha/hora del evento. |
| `event_from` | `date` | Eventos desde. |
| `event_to` | `date` | Eventos hasta. |
| `interval` | `string` | Intervalo (`hour`, `day`, etc.). |
| `period` | `array` | Período con `from`/`to`. |
| `period.from` | `date` | Fecha inicial. |
| `period.to` | `date` | Fecha final. |
| `page_url` | `string` | URL de la página. |
| `referrer` | `string` | URL de referencia. |
| `title` | `string` | Título de la página. |
| `timezone_offset` | `integer` | Zona horaria. |
| `connection_type` | `string` | Tipo de conexión. |
| `language` | `string` | Idioma. |
| `is_hidden` | `boolean` | Footprint oculto. |

#### Parámetros de Query (UTM y similares)

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `utm_source` | `string` | Fuente UTM. |
| `utm_medium` | `string` | Medio UTM. |
| `utm_campaign` | `string` | Campaña UTM. |
| `utm_term` | `string` | Término UTM. |
| `utm_content` | `string` | Contenido UTM. |
| `utms` | `string` | UTMs agrupados. |
| `clids` | `string` | IDs de campaña. |

#### Dispositivo

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `device_os_name` | `string` | Nombre del sistema operativo. |
| `device_os_version` | `string` | Versión del sistema operativo. |
| `device_browser_name` | `string` | Nombre del navegador. |
| `device_browser_version` | `string` | Versión del navegador. |
| `device_type` | `string` | Tipo de dispositivo. |
| `device_brand` | `string` | Marca del dispositivo. |
| `device_model` | `string` | Modelo del dispositivo. |
| `screen_w` | `integer` | Ancho de pantalla. |
| `screen_h` | `integer` | Alto de pantalla. |
| `screen_dpr` | `numeric` | Densidad de píxeles. |
| `viewport_w` | `integer` | Ancho del viewport. |
| `viewport_h` | `integer` | Alto del viewport. |

#### Paginación y Ordenación

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `page` | `integer` | Número de página (por defecto 1). |
| `per_page` | `integer` | Ítems por página (1-200, por defecto 25). |
| `sort` | `string` | Columna para ordenar (`id`, `event_time`, etc.). |
| `direction` | `string` | Dirección de orden (`ASC` o `DESC`). |
| `no_paginate` | `boolean` | Desactiva la paginación. |
| `limit` | `integer` | Máximo de ítems cuando `no_paginate` es verdadero. |

> Los parámetros aceptan variantes camelCase, snake_case, kebab-case o CapitalCase.

---

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "id": 813,
      "event_time": "2025-08-17T00:44:24-03:00",
      "event": "page_view",
      "page": "https://example.com/my-messages",
      "title": "Messages",
      "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
      "ip_address": "203.0.113.10",
      "is_banned": 0,
      "is_robot": 0,
      "platform": {
        "uuid": "00000000-0000-0000-0000-000000000000",
        "name": "Example Platform"
      },
      "user": {
        "uuid": "11111111-1111-1111-1111-111111111111",
        "name": "John Doe"
      },
      "created_at": "2025-08-16T21:44:24-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.echosistema.online/api/v1/footprints?page=1",
    "last": "https://sandbox.echosistema.online/api/v1/footprints?page=32",
    "prev": null,
    "next": "https://sandbox.echosistema.online/api/v1/footprints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 32,
    "path": "https://sandbox.echosistema.online/api/v1/footprints",
    "per_page": 25,
    "to": 1,
    "total": 798
  }
}
```

---

## Explicación de la Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data[]` | `array` | Lista de footprints. |
| `data[].id` | `integer` | ID del footprint. |
| `data[].event_time` | `datetime` | Fecha y hora del evento. |
| `data[].event` | `string` | Tipo de evento. |
| `data[].page` | `string` | URL de la página. |
| `data[].title` | `string` | Título de la página. |
| `data[].session_id` | `string` | Identificador de sesión. |
| `data[].ip_address` | `ip` | Dirección IP del visitante. |
| `data[].is_banned` | `boolean` | Indica IP baneada. |
| `data[].is_robot` | `boolean` | Indica acceso de robot. |
| `data[].platform` | `object` | Resumen de la plataforma. |
| `data[].platform.uuid` | `uuid` | UUID de la plataforma. |
| `data[].platform.name` | `string` | Nombre de la plataforma. |
| `data[].user` | `object` | Resumen del usuario. |
| `data[].user.uuid` | `uuid` | UUID del usuario. |
| `data[].user.name` | `string` | Nombre del usuario. |
| `data[].created_at` | `datetime` | Fecha de creación del registro. |
| `links` | `object` | Enlaces de paginación. |
| `meta` | `object` | Metadatos de paginación. |

---

## Notas

* Los footprints ocultos solo se devuelven a usuarios master.
* Los metadatos de paginación se omiten cuando `no_paginate` es verdadero.
* Localización: los campos traducibles siguen `Accept-Language` cuando esté disponible; de lo contrario se usan valores por defecto.
