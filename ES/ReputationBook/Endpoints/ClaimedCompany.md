# ReputationBook – Empresa reivindicada

## Consultar empresa (interno)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}
```

Devuelve los detalles completos de la empresa reivindicada (incluye reclamaciones, documentos y medios), respetando los permisos del colaborador autenticado.

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Parámetros de ruta

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `company` | string | Identificador de la empresa: ID, UUID o slug. |

### Respuesta

Basada en `ClaimedCompanyResource`, incluyendo:

- Datos generales (`uuid`, `name`, `assumed_name`, `rating`).
- Información complementaria (`addresses`, `contacts`, `social_medias`, `images`, `videos`).
- Documentos fiscales (`fiscal_documents`) cuando `public=false`.
- Reclamaciones relacionadas (`complaints`).
- Metadatos del creador cuando el usuario está autenticado.

### Códigos HTTP

- 200: Empresa encontrada.
- 403: Plataforma fuera del área de cobertura o colaborador sin permisos.
- 404: Empresa no encontrada / fuera de cobertura.

---

## Consultar empresa (público)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}/info
```

Devuelve un resumen público de la empresa.

### Autenticación

Ninguna. El middleware de plataforma sigue exigiendo `X-PUBLIC-KEY`.

### Notas

- Usa el mismo recurso pero con la bandera `public=true`, omitiendo documentos fiscales y reclamaciones internas.

---

## Verificar RUC

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/ruc/{param}
```

Consulta el documento fiscal tipo RUC. Si la empresa ya existe, devuelve sus datos; en caso contrario, consulta el servicio externo.

### Parámetro de ruta

`param` debe contener el RUC (números con o sin DV). El controller normaliza el valor antes de la búsqueda.

### Respuesta

- Existente: devuelve `ClaimedCompanyResource` con el extra `{ existent: true }`.
- No encontrado: ejecuta el scraper (`http://ruc.local:3000`) y retorna `data` con los campos recopilados y `existent: false`.

### Códigos HTTP

- 200: Resultado encontrado (cache o servicio externo).
- 404: Sin datos para el RUC informado.
- 503: Servicio externo no disponible.

---

## Crear empresa

### Endpoint

```
POST /api/v1/complaint-book/claimed-company
```

Crea una empresa asociada a la plataforma actual.

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Cuerpo (resumen de `CompanyStoreRequest`)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `name` | string | Sí | Razón social o nombre comercial. |
| `assumed_name` | string | No | Nombre comercial alternativo. |
| `raw.country` / `raw.state` / `raw.city` | string | Sí | Ubicación básica. |
| `raw.address.*` | objeto | No | Componentes de la dirección (líneas, código postal, etc.). |
| `raw.email` | string | Condicional | Correo de contacto (obligatorio si falta `raw.phone`). |
| `raw.phone.country_code`, `raw.phone.number` | string | Condicional | Teléfono principal (obligatorio si falta `raw.email`). |
| `raw.document.type` | string | Condicional | Tipo de documento fiscal (`FiscalDocumentTypeEnum`). |
| `raw.document.value` | string | Condicional | Número del documento. |
| `contacts[]` | array | No | Contactos adicionales (tipo + valor). |
| `social_medias[]` | array | No | Redes sociales oficiales. |
| `raw` | objeto | No | Puede incluir `geolocation`, `website`, `google.place_id`, etc. |

### Respuesta

Devuelve `ClaimedCompanyResource` con todos los datos.

### Códigos HTTP

- 201: Empresa creada.
- 400/422: Payload inválido o incompleto.
- 401/403: Sin permiso.

---

## Actualizar empresa

### Endpoint

```
PUT /api/v1/complaint-book/claimed-company/{company}
```

Actualiza datos de la empresa, contactos o dirección.

### Cuerpo (resumen de `CompanyUpdateRequest`)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ----------- | ----------- |
| `type` | string | Sí | `company` (datos generales) o `address` (dirección). Se define automáticamente. |
| `name`, `assumed_name`, `email`, `website` | string | Condicional | Campos generales (requeridos cuando `type=company`). |
| `document.*` / `documents[]` | array | No | Actualización de documentos fiscales. |
| `contacts[]` | array | No | Actualiza contactos (tipo + teléfono/código de país). |
| `social_medias[]`, `videos[]` | array | No | Canales oficiales (plataforma + URL). |
| `address.*` | objeto | Condicional | Datos de dirección (obligatorios cuando `type=address`), incluyendo IDs de país/estado/ciudad. |

### Códigos HTTP

- 200: Actualización aplicada.
- 400/422: Datos inválidos o tipo no soportado.
- 404: Empresa no encontrada.

---

## Subir imagen de la empresa

### Endpoint

```
POST /api/v1/complaint-book/claimed-company/{company}/images
```

Almacena o reemplaza imágenes de la empresa (logos, materiales, etc.).

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Cuerpo

Sigue `ImageStoreRequest` (ver [perfil del usuario](ComplaintBookUsers.md)). Define `usage` (`logo`, `banner`, ...) y envía uno de `image_url`, `image_encoded` o `image_file`.

### Respuesta

Devuelve `ImageResource` con los metadatos de la imagen almacenada.

### Códigos HTTP

- 201: Imagen guardada.
- 400/422: Carga inválida.
- 404: Empresa no encontrada.

