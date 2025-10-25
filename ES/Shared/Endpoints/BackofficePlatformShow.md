# Shared – Mostrar Detalles de Plataforma

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

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
  "data": {
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "total": {
      "users": 150
    },
    "domain": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
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
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | object  | Datos de la plataforma |
| data.uuid   | string  | Identificador único de la plataforma |
| data.total  | object  | Totalizadores de la plataforma |
| data.total.users | integer | Total de usuarios en la plataforma |
| data.domain | object  | Información del área de dominio |
| data.domain.uuid | string  | Identificador único del área de dominio |
| data.domain.name | string  | Nombre del área de dominio |
| data.name   | string  | Nombre de la plataforma |
| data.email  | string  | Email de contacto de la plataforma |
| data.open_to_register | boolean | Indica si la plataforma está abierta para nuevos registros |
| data.public_key | string  | Clave pública de la plataforma |
| data.logo   | object  | URLs de los logos de la plataforma |
| data.logo.main | string  | URL del logo principal |
| data.logo.square | string  | URL del logo cuadrado |
| data.logo.rounded | string  | URL del logo redondeado |
| data.logo.rectangular | string  | URL del logo rectangular |
| data.created_at | string  | Fecha de creación (ISO 8601) |

## Estados HTTP

- 200: OK
- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Este endpoint retorna los detalles de una plataforma específica
- Solo usuarios con permiso de backoffice pueden acceder
- Incluye conteo de usuarios de la plataforma (users_count)
- El parámetro {platform} acepta el UUID de la plataforma

## Relacionados

- [Listar Plataformas](BackofficePlatformIndex.md)
- [Listar Usuarios de la Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Actualización completa de la documentación con estructura detallada
- 2025-10-16: Documentación inicial
