# Bio – Mis Enlaces Índice

## Endpoint

```
GET /api/v1/bio/me/links
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (e.g., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro  | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ---------- | ------- | -------- | ----------- | -------------- |
| noPaginate | boolean | No       | Devolver todos los registros sin paginación. | false |
| perPage    | integer | No       | Número de elementos por página. | 25 |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/links"
```

### Ejemplo de respuesta

```json
{
  "data": ["uuid": "550e8400", "type": "linkedin", "url": "https://linkedin.com/in/user"]
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 500: Error interno del servidor

## Notas

- Devuelve elementos solo para el usuario autenticado.

## Relacionados

- [Mis Enlaces — Crear](MyLinksCrear.md)
