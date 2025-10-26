# ReputationBook – Ver Detalles de la Queja (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints/{complaint}
```

## Descripción

Devuelve información detallada sobre una queja específica, incluyendo textos completos, imágenes, respuestas, historial de cambios de estado y todos los datos relacionados. Endpoint exclusivo para administradores del backoffice.

## Autenticación

Obligatoria – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción |
| ---------------- | ------ | ----------- | ----------- |
| Authorization    | string | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma. |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de URL

| Parámetro  | Tipo   | Obligatorio | Descripción |
| ---------- | ------ | ----------- | ----------- |
| complaint  | string | Sí          | UUID de la queja a visualizar |

## Ejemplo de Respuesta

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "is_public": true,
  "status": "replied",
  "purchase_date": "2024-01-10T00:00:00Z",
  "invoice_number": "INV-2024-001",
  "created_at": "2024-01-15T10:30:00Z",
  "user": {
    "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
    "name": "Juan Pérez",
    "email": "juan.perez@example.com",
    "gender": "Masculino"
  },
  "texts": [
    {
      "content": "<p>Compré el producto el 10/01/2024 y noté que estaba defectuoso...</p>",
      "usage": "complaint_text",
      "language": "es",
      "is_default": true,
      "created_at": "2024-01-15T10:30:00Z"
    }
  ],
  "images": [
    {
      "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
      "usage": "evidence",
      "url": "https://cdn.example.com/complaints/evidence1.jpg",
      "created_at": "2024-01-15T10:40:00Z"
    }
  ],
  "replies": [
    {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "is_support": false,
      "index_number": 1,
      "created_at": "2024-01-16T14:20:00Z",
      "user": {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "name": "Servicio al Cliente",
        "email": "soporte@empresa.com"
      },
      "text": {
        "content": "Hola, lamentamos lo sucedido. Gestionaremos el reemplazo del producto.",
        "language": "es",
        "is_default": true,
        "usage": "reply_text"
      }
    }
  ],
  "status_change_history": [
    {
      "previous_status": null,
      "status": "initiated",
      "updated_by": null,
      "changed_at": "2024-01-15T10:30:00Z"
    },
    {
      "previous_status": "initiated",
      "status": "pending",
      "updated_by": "Admin Juan",
      "changed_at": "2024-01-15T11:00:00Z"
    }
  ]
}
```

## Relaciones Cargadas

Este endpoint carga automáticamente las siguientes relaciones según `Complaint::RESOURCE_RELATIONS`:

- `user` - Usuario que creó la queja
- `platform` - Plataforma donde se registró
- `company` - Empresa reclamada con detalles completos
- `company.addresses` - Direcciones de la empresa
- `company.contacts` - Contactos de la empresa
- `company.socialMedias` - Redes sociales
- `company.rating` - Calificación de la empresa
- `titles` - Títulos multilingües
- `description` - Descripción detallada
- `images` - Imágenes adjuntas
- `replies` - Respuestas a la queja
- `statusChangeEvents` - Historial de cambios

## Estado HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido (sin habilidad `backoffice`)
- 404: Queja no encontrada
- 422: Error de validación
- 429: Demasiadas solicitudes
- 500: Error interno

## Campos Condicionales

Los siguientes bloques de campos son condicionales:

- **user**: Cuando el usuario está cargado
- **platform**: Cuando la plataforma está cargada
- **company**: Cuando está asociada a una empresa
- **texts**: Cuando hay textos cargados
- **images**: Cuando existen imágenes
- **replies**: Cuando hay respuestas
- **status_change_history**: Cuando existe historial de estado

## Notas

- Devuelve información mucho más detallada que el index
- Todos los datos sensibles están expuestos (endpoint administrativo)
- Los correos de usuarios siempre son visibles
- El historial de estado muestra la evolución completa de la queja
- Las respuestas están ordenadas por `index_number`
- `updated_by` en el historial es `null` cuando el cambio lo hizo el propietario de la queja
- Los textos y descripciones pueden contener HTML
- Todas las URL de imágenes son completas y listas para usar
- Las fechas siguen el formato ISO 8601

## Relacionados

- [Listar Quejas](BackofficeComplaintIndex.md)

## Changelog

- 2025-10-26: Documentación inicial del endpoint de backoffice
