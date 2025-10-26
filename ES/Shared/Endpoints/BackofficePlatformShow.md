# Shared – Mostrar Detalles de la Plataforma

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

## Descripción

Retorna información detallada de una plataforma específica, incluyendo datos completos de la empresa, configuraciones regionales, gateways de pago, imágenes, configuraciones IMAP y estadísticas agregadas.

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de URL

| Parámetro | Tipo | Requerido | Descripción |
| ----------- | ------- | ----------- | ----------- |
| platform    | string  | Sí          | UUID de la plataforma a recuperar |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Ejemplo de respuesta

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "total": {
    "users": 150
  },
  "is_parent": true,
  "domain": {
    "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
    "slug": "ecommerce",
    "name": "E-commerce"
  },
  "name": "Plataforma Demo",
  "email": "contacto@plataforma.com",
  "open_to_register": true,
  "public_key": "pk_test_123456789",
  "logo": {
    "main": "https://cdn.example.com/logos/main.png",
    "square": "https://cdn.example.com/logos/square.png",
    "rounded": "https://cdn.example.com/logos/rounded.png",
    "rectangular": "https://cdn.example.com/logos/rectangular.png"
  },
  "created_at": "2024-01-15T10:30:00Z",
  "company": {
    "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
    "name": "Empresa Demo Ltda",
    "slug": "empresa-demo",
    "zipcode": "01310-100",
    "country": "Brasil",
    "state": "São Paulo",
    "city": "São Paulo",
    "company_logos": [
      {
        "main_logo": "https://cdn.example.com/company/main.png"
      },
      {
        "square_logo": "https://cdn.example.com/company/square.png"
      }
    ],
    "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
    "links": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "platform": "Facebook",
        "url": "https://facebook.com/plataforma"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "platform": "Instagram",
        "url": "https://instagram.com/plataforma"
      }
    ]
  },
  "client_id": "client_abc123xyz",
  "discord": {
    "channel_id": "1234567890123456789"
  },
  "is_main": true,
  "is_banned": false,
  "platform_transactions": {
    "in": 5000,
    "out": 3000
  },
  "regional_information": {
    "uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "language": "es",
    "currency": "EUR"
  },
  "imap_setting": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "host": "imap.gmail.com",
    "port": 993,
    "encryption": "ssl",
    "username": "contacto@mitienda.com",
    "is_active": true
  },
  "payment_gateways": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "name": "Stripe",
      "is_active": true,
      "gateway_type": "credit_card"
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "name": "PayPal",
      "is_active": true,
      "gateway_type": "wallet"
    }
  ],
  "images": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "usage": "banner",
      "url": "https://cdn.example.com/images/banner.jpg",
      "created_at": "2024-01-20T14:30:00Z"
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "usage": "background",
      "url": "https://cdn.example.com/images/background.jpg",
      "created_at": "2024-01-21T09:15:00Z"
    }
  ],
  "total_counts": {
    "products": 250,
    "orders": 1500,
    "articles": 45,
    "events": 12,
    "newsletters": 8
  }
}
```

## Estructura JSON Explicada

### Campos Principales (siempre presentes)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| uuid | string  | Identificador único de la plataforma |
| total | object  | Totalizadores de la plataforma (condicional - solo si hay usuarios) |
| total.users | integer | Total de usuarios registrados |
| is_parent | boolean | Indica si la plataforma es una plataforma padre |
| domain | object  | Información del área de dominio |
| domain.uuid | string  | Identificador único del área de dominio |
| domain.slug | string  | Slug del área de dominio |
| domain.name | string  | Nombre del área de dominio |
| name | string  | Nombre de la plataforma |
| email | string  | Email de contacto de la plataforma |
| open_to_register | boolean | Indica si está abierta para nuevos registros |
| public_key | string  | Clave pública de la plataforma |
| logo | object  | URLs de los logos de la plataforma |
| logo.main | string  | URL del logo principal |
| logo.square | string  | URL del logo cuadrado |
| logo.rounded | string  | URL del logo redondeado |
| logo.rectangular | string  | URL del logo rectangular |
| created_at | string  | Fecha de creación (ISO 8601) |

### Campos de la Empresa (condicional)

Estos campos aparecen dentro del objeto `company` solo cuando la plataforma tiene una empresa asociada:

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| company | object  | Datos de la empresa (condicional) |
| company.uuid | string  | UUID de la empresa |
| company.name | string  | Nombre de la empresa |
| company.slug | string  | Slug de la empresa |
| company.zipcode | string\|null | Código postal |
| company.country | string\|null | País |
| company.state | string\|null | Estado |
| company.city | string\|null | Ciudad |
| company.company_logos | array  | Logos de la empresa |
| company.full_address | string\|null | Dirección completa formateada |
| company.links | array  | Redes sociales |
| company.links[].uuid | string  | UUID del enlace social |
| company.links[].platform | string  | Nombre de la plataforma social |
| company.links[].url | string  | URL del perfil |

### Campos Detallados (exclusivos del show)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| client_id | string  | ID del cliente OAuth |
| discord | object  | Configuraciones de Discord |
| discord.channel_id | string  | ID del canal Discord para notificaciones |

### Campos de Afiliación (condicional - platformAffiliated)

Aparecen solo cuando la plataforma tiene una relación de afiliación:

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| is_main | boolean | Si es la plataforma principal |
| is_banned | boolean | Si está prohibida |
| platform_transactions | object  | Transacciones de la plataforma |
| platform_transactions.in | integer | Transacciones entrantes |
| platform_transactions.out | integer | Transacciones salientes |

### Información Regional (condicional)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| regional_information | object  | Información regional (condicional) |
| regional_information.uuid | string  | UUID de la información regional |
| regional_information.language | string  | Código del idioma (ej.: pt-BR, en, es) |
| regional_information.currency | string  | Código de la moneda (ej.: BRL, USD, EUR) |

### Configuraciones IMAP (condicional)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| imap_setting | object  | Configuraciones IMAP (condicional) |
| imap_setting.uuid | string  | UUID de la configuración |
| imap_setting.host | string  | Host del servidor IMAP |
| imap_setting.port | integer | Puerto del servidor IMAP |
| imap_setting.encryption | string  | Tipo de encriptación (ssl, tls, none) |
| imap_setting.username | string  | Usuario de autenticación |
| imap_setting.is_active | boolean | Si la configuración está activa |

### Gateways de Pago (condicional)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| payment_gateways | array  | Lista de gateways (condicional) |
| payment_gateways[].uuid | string  | UUID del gateway |
| payment_gateways[].name | string  | Nombre del gateway |
| payment_gateways[].is_active | boolean | Si está activo |
| payment_gateways[].gateway_type | string  | Tipo del gateway |

### Imágenes (condicional)

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| images | array  | Imágenes de la plataforma (condicional) |
| images[].uuid | string  | UUID de la imagen |
| images[].usage | string  | Uso de la imagen (banner, background, etc) |
| images[].url | string  | URL completa de la imagen |
| images[].created_at | string  | Fecha de creación (ISO 8601) |

### Contadores Agregados

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| total_counts | object  | Contadores agregados |
| total_counts.products | integer | Total de productos (solo si > 0) |
| total_counts.orders | integer | Total de pedidos (solo si > 0) |
| total_counts.articles | integer | Total de artículos (solo si > 0) |
| total_counts.events | integer | Total de eventos (solo si > 0) |
| total_counts.newsletters | integer | Total de boletines (solo si > 0) |

## Relaciones Cargadas

Este endpoint carga automáticamente las siguientes relaciones:

- `domainArea` - Área de dominio
- `company` - Empresa asociada
- `company.logos` - Logos de la empresa
- `company.address` - Dirección de la empresa
- `company.socialMedias` - Redes sociales de la empresa
- `regionalInformation` - Información regional
- `imapSetting` - Configuraciones IMAP
- `userRoles` - Roles de usuarios
- `userRoles.user` - Datos de los usuarios
- `paymentGateways` - Gateways de pago
- `images` - Imágenes de la plataforma
- `affiliated` - Plataformas afiliadas

## Contadores

- `users_count` - Total de usuarios
- `products_count` - Total de productos
- `orders_count` - Total de pedidos
- `articles_count` - Total de artículos
- `events_count` - Total de eventos
- `newsletters_count` - Total de boletines

## Estado HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido (sin permiso `show.all`)
- 404: Plataforma no encontrada
- 422: Error de validación
- 429: Límite de solicitudes excedido
- 500: Error interno

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Campos Condicionales

Los siguientes bloques de campos son condicionales y solo aparecen cuando son aplicables:

- **company**: Solo cuando la plataforma tiene una empresa asociada
- **is_main, is_banned, platform_transactions**: Solo cuando tiene relación platformAffiliated
- **regional_information**: Solo cuando la relación está cargada
- **imap_setting**: Solo cuando está configurado y la relación está cargada
- **payment_gateways**: Solo cuando la relación está cargada
- **images**: Solo cuando la relación está cargada
- **Contadores en total_counts**: Solo si el valor es mayor que 0

## Notas

- Este endpoint retorna información mucho más detallada que el index
- El `client_id` solo se expone en show, no en index
- Las configuraciones de Discord incluyen el canal específico de la plataforma o el canal por defecto
- Todos los arrays retornan vacío cuando las relaciones no están disponibles
- Todas las fechas siguen el formato ISO 8601
- Las URLs de logos e imágenes son completas y listas para usar
- Los contadores agregados proporcionan una visión general de la actividad de la plataforma

## Relacionados

- [Listar Plataformas](BackofficePlatformIndex.md)
- [Listar Usuarios de la Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Actualización completa con todos los campos detallados incluyendo client_id, discord, regional_information, imap_setting, payment_gateways, images y total_counts
- 2025-10-16: Documentación inicial
