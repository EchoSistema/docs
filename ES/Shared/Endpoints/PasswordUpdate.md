# Shared – Actualizar Contraseña

## Endpoint

```
PUT /api/v1/password
```

## Autenticación

Ninguna

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

Actualizar contraseña del usuario

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/password"
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

- Actualizar contraseña del usuario

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
