# Bio – Mis Enlaces Mostrar

## Endpoint

```
GET /api/v1/bio/me/links/{uuid}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | UUID del recurso. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 404: No encontrado
- 500: Error interno del servidor

## Relacionados

- [Mis Enlaces — Índice](MyLinksÍndice.md)
- [Mis Enlaces — Actualizar](MyLinksActualizar.md)
