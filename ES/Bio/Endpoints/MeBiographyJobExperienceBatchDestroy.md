# Bio – Mis Experiencias Laborales Batch Eliminar

## Endpoint

```
DELETE /api/v1/bio/me/job-experiences
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Content-Type     | string | Yes      | `application/json`. |

## Parámetros

### Cuerpo de la solicitud

```json
{
  "job_experiences": ["uuid1", "uuid2"]
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Relacionados

- [Mis Experiencias Laborales — Índice](MyJobExperiencesÍndice.md)
