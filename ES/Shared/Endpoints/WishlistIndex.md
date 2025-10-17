# Shared – Listar Lista de Deseos del Usuario

## Endpoint

```
GET /api/v1/wishlists
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| per_page  | integer | No        | Número de elementos por página | 25 |
| page      | integer | No        | Número de página | 1 |

> El endpoint acepta variantes del nombre del parámetro: `per_page`, `perPage`, `per-page`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/wishlists?per_page=25&page=1"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "user_id": 1,
      "type": "company",
      "type_id": 123,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "user_id": 1,
      "type": "product",
      "type_id": 456,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/wishlists?page=1",
    "last": "https://api.example.com/api/v1/wishlists?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/wishlists?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 120
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data | array | Array de elementos de lista de deseos |
| data[].uuid | string | Identificador único de la entrada de lista de deseos |
| data[].user_id | integer | ID del usuario propietario de esta entrada |
| data[].type | string | Tipo de elemento en lista de deseos (bio, property, event, course, product, platform, company, article, complaint, review, country, state, city) |
| data[].type_id | integer | ID interno del elemento guardado |
| data[].created_at | string | Fecha de creación ISO 8601 |
| data[].updated_at | string | Fecha de última actualización ISO 8601 |
| links | object | Enlaces de paginación |
| links.first | string | URL a la primera página |
| links.last | string | URL a la última página |
| links.prev | string\|null | URL a la página anterior |
| links.next | string\|null | URL a la página siguiente |
| meta | object | Metadatos de paginación |
| meta.current_page | integer | Número de página actual |
| meta.from | integer | Número del primer elemento en la página actual |
| meta.last_page | integer | Número total de páginas |
| meta.per_page | integer | Elementos por página |
| meta.to | integer | Número del último elemento en la página actual |
| meta.total | integer | Número total de elementos en lista de deseos |

## Estados HTTP

- 200: OK - Lista de deseos recuperada exitosamente
- 401: No autorizado - Token inválido o faltante
- 403: Prohibido - Permisos insuficientes
- 500: Error interno del servidor

## Errores

### 401 No autorizado
```json
{
  "message": "No autenticado."
}
```

### 403 Prohibido
```json
{
  "message": "Esta acción no está autorizada."
}
```

## Notas

- Devuelve solo las listas de deseos del usuario autenticado
- Los resultados están ordenados por más recientes primero (latest)
- La paginación predeterminada es de 25 elementos por página
- Los elementos eliminados (soft-deleted) no se incluyen en los resultados
- La relación `taggable` se carga anticipadamente para un rendimiento óptimo

## Relacionados

- [Agregar Elemento a Lista de Deseos](./WishlistStore.md)
- [Eliminar Elemento de Lista de Deseos](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Versión inicial - Listar lista de deseos del usuario con paginación
