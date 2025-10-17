# Shared – Eliminar Elemento de Lista de Deseos

## Endpoint

```
DELETE /api/v1/wishlists/{wishlist}
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

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| wishlist  | string | Sí        | UUID de la entrada de lista de deseos a eliminar |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/wishlists/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "message": "Eliminado exitosamente."
}
```

## Estructura JSON Explicada

| Campo   | Tipo   | Descripción |
| ------- | ------ | ----------- |
| message | string | Mensaje de éxito confirmando la eliminación |

## Estados HTTP

- 200: OK - Elemento eliminado de la lista de deseos exitosamente
- 401: No autorizado - Token inválido o faltante
- 403: Prohibido - El usuario no es propietario de esta entrada
- 404: No encontrado - Entrada de lista de deseos no encontrada
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

### 404 No encontrado
```json
{
  "message": "Lista de deseos no encontrada."
}
```

## Notas

- Solo el propietario de la entrada puede eliminarla
- El endpoint usa UUID para identificación (no ID interno)
- La eliminación es soft delete - el registro se marca como eliminado pero permanece en la base de datos
- Después de la eliminación exitosa, el elemento ya no aparecerá en la lista de deseos del usuario
- Si la entrada no existe, se devuelve un error 404
- El route binding encuentra automáticamente la lista de deseos por UUID

## Relacionados

- [Listar Lista de Deseos del Usuario](./WishlistIndex.md)
- [Agregar Elemento a Lista de Deseos](./WishlistStore.md)

## Changelog

- 2025-10-17: Versión inicial - Eliminar elemento de lista de deseos con validación de propiedad
