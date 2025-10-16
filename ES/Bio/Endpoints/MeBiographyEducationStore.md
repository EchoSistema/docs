# Bio – Mis Educaciones Crear

## Endpoint

```
POST /api/v1/bio/me/educations
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Content-Type     | string | Yes      | `application/json`. |

## Estados HTTP

- 201: Creado
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Relacionados

- [Mis Educaciones — Índice](MyEducationsÍndice.md)
