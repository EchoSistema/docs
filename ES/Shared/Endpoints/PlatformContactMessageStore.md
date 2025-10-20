# Shared – Crear Mensaje de Contacto

## Endpoint

`POST /api/v1/contact`

## Autenticación

Ninguna (público). Requiere cabecera de plataforma.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| X-PUBLIC-KEY | `string` | Sí | Clave pública de la plataforma (también acepta `public_key` en query). |
| X-APP-NAME | `string` | No | Si coincide con el nombre de una plataforma, no se requiere reCAPTCHA. |
| Accept-Language | `string` | No | Locale para mensajes de validación (`pt-BR`, `en`, `es`, `gn`). |

## Parámetros

Enviar un mensaje de formulario de contacto

### Cuerpo (application/json)

| Campo | Tipo | Obligatorio | Descripción |
| ----- | ---- | ---------- | ----------- |
| `type` | `enum` | Sí | `default`, `denunciation` o `report`. |
| `language` | `enum` | Sí | `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Obligatorio salvo `X-APP-NAME` | Token reCAPTCHA validado en servidor. |
| `email` | `email` | Obligatorio sin `phone` | Correo del contacto. |
| `phone` | `string` | Obligatorio sin `email` | Teléfono del contacto. |
| `content` | `string` | Sí | Mensaje (máx. 5000). Alias de `message` si enviado. |
| `name` | `string` | No | Nombre del remitente. |
| `user_uuid` | `uuid` | No | Asocia a un usuario existente. |
| `anonymous` | `boolean` | No | Si `true`, usa `anonymous@pigeons.email`. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  "https://sandbox.your-domain.com/api/v1/contact" \
  -d '{
    "type": "default",
    "language": "es",
    "g_recaptcha": "<token>",
    "email": "maria@example.com",
    "content": "Me gustaría conocer más sobre los planes.",
    "name": "Maria Silva"
  }'
```

### Éxito `201 Created`

```json
{
  "success": true,
  "data": { "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e" }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| success | `boolean` | Siempre `true` en éxito. |
| data    | `object`  | Datos de respuesta. |

## Estados HTTP

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

- Enviar un mensaje de formulario de contacto

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
