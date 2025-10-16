# ReputationBook – Crear Queja

## Endpoint

```
POST /api/v1/complaints
```

Crea una nueva queja contra una empresa.

## Autenticación

**Requerida** – Token Bearer con habilidad `backoffice` y alcance `domain:reputationbook`.

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sí        | Debe ser `application/json`. |

## Parámetros

### Cuerpo de la solicitud

```json
{
  "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
  "purchase_date": "2025-10-01",
  "title": "Problema de calidad del producto",
  "complaint": "El producto recibido estaba defectuoso y no coincidía con la descripción.",
  "is_public": true,
  "invoice_number": "INV-2025-001",
  "attendants_name": "Jane Doe",
  "language": "es"
}
```

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| company_uuid | uuid | Sí* | UUID de la empresa. Requerido si `company_id` no se proporciona. |
| company_id | integer | Sí* | ID de la empresa. Requerido si `company_uuid` no se proporciona. |
| purchase_date | date | Sí | Fecha de compra (formato: YYYY-MM-DD). Debe ser hoy o anterior. |
| title | string | Sí | Título de la queja. |
| complaint | string | Sí | Descripción detallada de la queja. |
| is_public | boolean | No | Si la queja debe ser visible públicamente. Por defecto `false`. |
| invoice_number | string | No | Número de factura o recibo. |
| attendants_name | string | No | Nombre del empleado que atendió. |
| language | string | No | Código de idioma para la queja (ej.: `en`, `es`, `pt-BR`). |

> Se debe proporcionar `company_uuid` o `company_id`, pero no ambos.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Content-Type: application/json" \
  -H "Accept-Language: es" \
  -d '{
    "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
    "purchase_date": "2025-10-01",
    "title": "Problema de calidad del producto",
    "complaint": "El producto recibido estaba defectuoso.",
    "is_public": true
  }' \
  "https://sandbox.su-dominio.com/api/v1/complaints"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
    "user": {
      "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
      "name": "Juan Pérez",
      "gender": "male"
    },
    "company": {
      "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
      "name": "Empresa ABC",
      "slug": "empresa-abc"
    },
    "titles": [
      {
        "content": "Problema de calidad del producto",
        "language": "es",
        "is_default": true
      }
    ],
    "texts": [
      {
        "content": "El producto recibido estaba defectuoso.",
        "language": "es",
        "is_default": true
      }
    ],
    "images": [],
    "status": "pending",
    "is_public": true,
    "purchase_date": "2025-10-01",
    "invoice_number": "INV-2025-001",
    "attendants_name": "Jane Doe",
    "created_at": "2025-10-15T14:30:00.000000Z",
    "updated_at": "2025-10-15T14:30:00.000000Z"
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data        | object  | El objeto de queja creado. |
| data.uuid   | uuid    | Identificador único de la queja. |
| data.user   | object  | Usuario que creó la queja. |
| data.company | object | Empresa sobre la que se queja. |
| data.titles[] | array | Títulos de queja localizados. |
| data.texts[] | array  | Contenido de texto de queja localizado. |
| data.images[] | array | Imágenes adjuntas (vacío en creación). |
| data.status | string  | Estado inicial (siempre `pending`). |
| data.is_public | boolean | Configuración de visibilidad pública. |
| data.purchase_date | date | Fecha de compra. |
| data.invoice_number | string | Número de factura (si se proporciona). |
| data.attendants_name | string | Nombre del empleado (si se proporciona). |
| data.created_at | string | Marca de tiempo de creación (ISO-8601). |
| data.updated_at | string | Marca de tiempo de última actualización (ISO-8601). |

## Estados HTTP

- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado (empresa no encontrada)
- 422: Entidad no procesable (errores de validación)
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Error de validación",
  "errors": {
    "company_uuid": ["El campo company uuid es requerido cuando company id no está presente."],
    "purchase_date": ["El campo purchase date es requerido."],
    "title": ["El campo title es requerido."],
    "complaint": ["El campo complaint es requerido."]
  }
}
```

## Notas

- Requiere autenticación con habilidad `backoffice` y alcance `domain:reputationbook`.
- La queja se crea con estado `pending` por defecto.
- Se verifica la autorización del usuario antes de crear la queja.
- El campo `language` determina el idioma del título y el contenido del texto.
- Las imágenes se pueden agregar por separado después de la creación de la queja.

## Relacionados

- [Índice de Quejas](ComplaintIndex.md)
- [Mostrar Queja](ComplaintShow.md)
- [Actualizar Queja](ComplaintUpdate.md)
- [Agregar Texto a Queja](ComplaintAddText.md)
