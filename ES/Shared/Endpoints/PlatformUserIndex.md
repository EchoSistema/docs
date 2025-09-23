# Plataforma – Documentación del Endpoint de Listado de Usuarios de la Plataforma

## Endpoint

`GET /api/v1/reputation-book/users`
`GET /api/v1/ia/admin/users`

Recupera una lista de usuarios de la plataforma con filtrado por rol, ocupación y área. Admite criterios de búsqueda detallados e internacionalización.

---

## Autenticación

**Requerida** – Token Bearer con capacidad `backoffice`.

---

## Solicitud

### Encabezados Requeridos

| Encabezado         | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| **Authorization**  | `string` | Credencial `Bearer {token}`. **Requerido**. |
| **X-PUBLIC-KEY**   | `string` | Clave pública que identifica la plataforma solicitante. **Requerido**. |
| **Accept-Language**| `string` | Etiqueta de idioma IETF (ej.: `es`, `en`, `pt-BR`). Determina la localización de campos traducibles. **Requerido**. |

### Parámetros de Filtro

| Parámetro                | Tipo           | Requerido | Descripción |
| ------------------------ | -------------- | --------- | ----------- |
| `role`                   | `string/int`   | No        | Identificador de rol (ID numérico o nombre). |
| `roles[]`                | `array`        | No        | Lista de roles (IDs numéricos o nombres). |
| `role_id`                | `integer`      | No        | ID numérico del rol. |
| `role_name`              | `string`       | No        | Nombre del rol. |
| `role_ids[]`             | `array`        | No        | Lista de IDs numéricos de roles. |
| `role_names[]`           | `array`        | No        | Lista de nombres de roles. |
| `name`                   | `string`       | No        | Nombre del usuario (parcial o completo). |
| `email`                  | `string`       | No        | Correo electrónico del usuario (coincidencia exacta, sin distinguir mayúsculas). |
| `user_name`              | `string`       | No        | Alias de `name`. |
| `user_email`             | `string`       | No        | Alias de `email`. |
| `user_uuid`              | `uuid`         | No        | UUID del usuario. |
| `job_occupation`         | `string/int`   | No        | ID numérico, UUID o título de la ocupación. |
| `job_occupation_id`      | `string`       | No        | ID numérico de la ocupación. |
| `job_occupation_uuid`    | `string`       | No        | UUID de la ocupación. |
| `job_occupation_title`   | `string`       | No        | Título de la ocupación. |
| `occupation_area`        | `string/array` | No        | Área de ocupación como UUID, como cadena `contenido:uso` o arreglo con `content` y `usage`. |
| `occupation_area.content`| `string`       | No        | Nombre o palabra clave del área de ocupación. |
| `occupation_area.usage`  | `string`       | No        | Tipo de uso del área de ocupación. Permitido: `occupation_area_title`. |
| `occupation_area_id`     | `string`       | No        | ID numérico del área de ocupación. |
| `occupation_area_uuid`   | `string`       | No        | UUID del área de ocupación. |
| `has_job_occupation`     | `boolean`      | No        | Filtra usuarios por presencia (`true`) o ausencia (`false`) de ocupación. |
| `has_occupation_area`    | `boolean`      | No        | Filtra usuarios por presencia (`true`) o ausencia (`false`) de área de ocupación. |
| `no_paginate`            | `boolean`      | No        | Cuando es `true`, deshabilita la paginación y devuelve todos los resultados. Por defecto `false`. |
| `per_page`               | `integer`      | No        | Elementos por página en la paginación. Por defecto `25`. |

> **Nota:** Todos los parámetros también aceptan variantes en camelCase (ej.: `roleId`, `userEmail`).

---

## Ejemplo de Objeto de Respuesta

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "image": "https://cdn.example.com/avatars/johndoe.webp",
  "gender": {
    "abbr": "M",
    "name": "Male"
  },
  "birth_date": "1990-05-15T00:00:00+00:00",
  "age": 34,
  "language": "en",
  "currency": {
    "id": "USD",
    "name": "US Dollar",
    "sign": "$"
  },
  "role": {
    "id": 2,
    "name": "Admin",
    "localized_name": "Administrator",
    "created_at": "2024-05-01T12:00:00+00:00"
  },
  "telephone": "+1-202-555-0147",
  "addresses": [
    "123 Example Street, District Name, Sample City, Sample State, Sample Country"
  ],
  "platform": {
    "user_status": "active",
    "name": "PlatformName"
  },
  "occupation": {
    "uuid": "bb4a7f72-39b6-4d23-a5c5-7186f1d2bcb3",
    "title": "Software Engineer",
    "is_default": true
  },
  "created_at": "2024-05-01T12:00:00+00:00",
  "updated_at": "2024-08-20T09:30:00+00:00"
}
```

---

## Explicación de la Estructura JSON

### `data[]` – Objeto Usuario

| Campo        | Tipo     | Descripción |
| ------------ | -------- | ----------- |
| `uuid`       | `uuid`   | Identificador único del usuario. |
| `name`       | `string` | Nombre completo del usuario. |
| `email`      | `string` | Dirección de correo electrónico del usuario. |
| `image`      | `string` | URL de la imagen de avatar, si está disponible. |
| `gender`     | `object` | Información de género con abreviación y nombre. |
| `birth_date` | `string` | Fecha de nacimiento en formato ISO-8601. |
| `age`        | `int`    | Edad calculada del usuario. |
| `language`   | `string` | Idioma preferido del usuario. |
| `currency`   | `object` | Información de la moneda con `id` (código ISO), `name` y `sign`. |
| `role`       | `object` | Rol asignado al usuario en la plataforma, con ID, nombre, nombre localizado y fecha de creación. |
| `telephone`  | `string` | Número de teléfono del usuario, si está disponible. |
| `addresses[]`| `array`  | Lista de direcciones formateadas vinculadas al usuario. |
| `platform`   | `object` | Datos específicos de la plataforma, incluyendo `user_status` y `name` (solo cuando se consulta `platform`). |
| `occupation` | `object` | Información de ocupación si el usuario tiene ocupaciones cargadas. |
| `created_at` | `string` | Fecha de creación de la asignación del rol (formato ISO-8601). |
| `updated_at` | `string` | Fecha de la última actualización de los datos del usuario (formato ISO-8601). |

### `gender`

| Campo | Tipo     | Descripción                           |
| ----- | -------- | ------------------------------------- |
| `abbr`| `string` | Abreviatura de género (ej.: `M`).     |
| `name`| `string` | Nombre completo del género.           |

### `currency`

| Campo | Tipo     | Descripción            |
| ----- | -------- | ---------------------- |
| `id`  | `string` | Código ISO de la moneda.|
| `name`| `string` | Nombre de la moneda.   |
| `sign`| `string` | Símbolo de la moneda.  |

### `role`

| Campo            | Tipo     | Descripción                                           |
| ---------------- | -------- | ----------------------------------------------------- |
| `id`             | `int`    | ID numérico del rol.                                 |
| `name`           | `string` | Nombre interno del rol.                              |
| `localized_name` | `string` | Nombre localizado del rol.                           |
| `created_at`     | `string` | Fecha en que se asignó el rol (formato ISO-8601).    |

### `occupation`

| Campo       | Tipo   | Descripción                                         |
| ----------- | ------ | --------------------------------------------------- |
| `uuid`      | `uuid` | UUID de la experiencia laboral.                     |
| `title`     | `string` | Título del cargo.                                  |
| `is_default`| `bool` | Indica si es la ocupación predeterminada del usuario. |

---

## Notas

* Los usuarios solo pueden listar a otros usuarios con roles inferiores al suyo.
* Si la consulta incluye `no_paginate=true`, se omiten los metadatos de paginación y se devuelven todos los resultados.
* Los filtros de texto son **coincidencias parciales sin distinción de mayúsculas**, salvo que se indique lo contrario.
* Los parámetros `role`, `roles`, `name` y `email` se mapean internamente a `role_id`, `role_ids`, `user_name` y `user_email` antes de aplicar los filtros.
* Los filtros de ocupación y área de ocupación configuran automáticamente las banderas `has_job_occupation` y `has_occupation_area`.
* Todos los parámetros aceptan equivalentes en camelCase.
