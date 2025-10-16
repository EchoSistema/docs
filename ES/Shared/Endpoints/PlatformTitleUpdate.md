# Shared – Actualizar Título de Plataforma

## Endpoint

```
PUT /api/v1/management/titles/{title}
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

Actualizar un título de plataforma

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/management/titles/{title}"
```

### Ejemplo de respuesta

```json
{
  "data": {}
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | object  | Response data |

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

- Actualizar un título de plataforma

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
