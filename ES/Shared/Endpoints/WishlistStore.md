# Shared – Agregar Elemento a Lista de Deseos

## Endpoint

```
POST /api/v1/wishlists
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sí        | `application/json`. |

## Parámetros

### Cuerpo de la solicitud

```json
{
  "type": "company",
  "uuid": "550e8400-e29b-41d4-a716-446655440000"
}
```

| Campo | Tipo   | Requerido | Descripción |
| ----- | ------ | --------- | ----------- |
| type  | string | Sí        | Tipo de elemento a agregar. Valores válidos: `bio`, `property`, `event`, `course`, `product`, `platform`, `company`, `article`, `complaint`, `review`, `country`, `state`, `city` |
| uuid  | string | Sí        | UUID del elemento a agregar a la lista de deseos |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "company",
    "uuid": "550e8400-e29b-41d4-a716-446655440000"
  }' \
  "https://sandbox.su-dominio.com/api/v1/wishlists"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "user_id": 1,
    "type": "company",
    "type_id": 123,
    "created_at": "2025-10-17T00:00:00+00:00",
    "updated_at": "2025-10-17T00:00:00+00:00"
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data | object | Datos de la entrada de lista de deseos |
| data.uuid | string | Identificador único de la entrada |
| data.user_id | integer | ID del usuario propietario de esta entrada |
| data.type | string | Tipo del elemento guardado |
| data.type_id | integer | ID interno del elemento guardado |
| data.created_at | string | Fecha de creación ISO 8601 |
| data.updated_at | string | Fecha de última actualización ISO 8601 |

## Estados HTTP

- 200: OK - El elemento ya está en la lista de deseos (devuelve la entrada existente)
- 201: Creado - Elemento agregado a la lista de deseos exitosamente
- 401: No autorizado - Token inválido o faltante
- 403: Prohibido - Permisos insuficientes
- 404: No encontrado - Elemento con el UUID proporcionado no encontrado
- 422: Entidad no procesable - Tipo inválido o error de validación
- 500: Error interno del servidor

## Errores

### 401 No autorizado
```json
{
  "message": "No autenticado."
}
```

### 404 No encontrado
```json
{
  "message": "Elemento no encontrado."
}
```

### 422 Entidad no procesable
```json
{
  "message": "Tipo inválido.",
  "errors": {
    "type": ["El tipo seleccionado es inválido."],
    "uuid": ["El campo uuid debe ser un UUID válido."]
  }
}
```

## Notas

- Si el elemento ya está en la lista de deseos del usuario, se devuelve la entrada existente (sin duplicados)
- El campo `type` acepta valores enum que se mapean a diferentes tipos de recursos
- El endpoint resuelve automáticamente la clase desde el tipo y encuentra el elemento por UUID
- Tanto `type` como `uuid` son campos obligatorios
- El usuario autenticado se convierte en el propietario de la entrada de lista de deseos

## Relacionados

- [Listar Lista de Deseos del Usuario](./WishlistIndex.md)
- [Eliminar Elemento de Lista de Deseos](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Versión inicial - Agregar elemento a lista de deseos con tipo y UUID
