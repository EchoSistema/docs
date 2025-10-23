# Inteligencia Artificial – Eliminar Producto de Precio por Byte

## Endpoint

```
DELETE /api/v1/ai/admin/pricing/bytes/{product:uuid}
```

## Autenticación

Required – Bearer {token} with ability `auth:sanctum`

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | cadena | Sí      | `Bearer {token}`. |
| X-PUBLIC-KEY     | cadena | Sí      | Clave pública de la plataforma. |
| Accept-Language  | cadena | No       | Configuración regional IETF (ej., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| product   | cadena | Sí      | UUID del producto. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001', {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
  }
});
```

### Ejemplo de respuesta

```json
{
  "message": "Byte pricing successfully deleted"
}
```

## Estado HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (product not found or wrong measurement type)
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

### Not Found

```json
{
  "message": "The requested resource was not found.",
  "errors": {}
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notas

- Este endpoint permanently deletes a byte pricing product.
- The product must have measurement type BYTE, otherwise 404 is returned.
- This action is irreversible - deleted products cannot be recovered.
- Make sure you have proper backups before deleting pricing data.
- After deletion, you can create a new byte pricing product using the store endpoint.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
