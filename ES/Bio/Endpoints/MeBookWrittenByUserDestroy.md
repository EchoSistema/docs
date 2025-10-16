# Bio – Mis Libros Eliminar

## Endpoint

```
DELETE /api/v1/bio/me/books/{uuid}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 404: No encontrado
- 500: Error interno del servidor

## Relacionados

- [Mis Libros — Índice](MyBooksÍndice.md)
