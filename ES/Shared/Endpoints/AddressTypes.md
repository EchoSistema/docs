# Compartido – Obtener Tipos de Dirección

## Endpoint

```
GET /api/v1/addresses/types
```

## Autenticación

Obligatorio – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción |
| ---------------- | ------ | ----------- | ----------- |
| Authorization    | string | Sí          | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma. |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). Afecta la traducción del campo `title_localized`. |

## Parámetros

Este endpoint no acepta parámetros.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/addresses/types"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/addresses/types', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'es',
  }
});
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "name": "BILLING",
      "value": "billing",
      "title_localized": "Facturación"
    },
    {
      "name": "SHIPPING",
      "value": "shipping",
      "title_localized": "Envío"
    },
    {
      "name": "BOTH",
      "value": "both",
      "title_localized": "Ambos"
    },
    {
      "name": "FISCAL",
      "value": "fiscal",
      "title_localized": "Fiscal"
    },
    {
      "name": "ADDITIONAL",
      "value": "additional",
      "title_localized": "Adicional"
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                  | Tipo   | Descripción |
| ---------------------- | ------ | ----------- |
| data                   | array  | Lista de tipos de dirección disponibles. |
| data[].name            | string | Nombre de la constante enum (mayúsculas). |
| data[].value           | string | Valor del enum (minúsculas). Use este valor al crear/actualizar direcciones. |
| data[].title_localized | string | Título traducido legible basado en el encabezado `Accept-Language`. |

## Estado HTTP

- 200: OK
- 401: No Autorizado
- 403: Prohibido
- 429: Demasiadas Solicitudes
- 500: Error Interno del Servidor

## Errores

### No Autenticado

```json
{
  "message": "No autenticado.",
  "errors": {}
}
```

### Prohibido

```json
{
  "message": "Esta acción no está autorizada.",
  "errors": {}
}
```

## Notas

- Este endpoint devuelve todos los tipos de dirección disponibles del `AddressTypeEnum`.
- El campo `title_localized` se traduce automáticamente según el encabezado `Accept-Language`.
- Use el campo `value` al crear o actualizar direcciones (ej.: `"type": "billing"`).
- Idiomas soportados: `en` (Inglés), `pt-BR` (Portugués Brasileño), `es` (Español), `gn` (Guaraní).

## Traducciones de Tipos de Dirección

| Valor      | EN          | PT-BR     | ES           | GN           |
| ---------- | ----------- | --------- | ------------ | ------------ |
| billing    | Billing     | Cobrança  | Facturación  | Jehepyme'ã   |
| shipping   | Shipping    | Entrega   | Envío        | Mondo        |
| both       | Both        | Ambos     | Ambos        | Mokõive      |
| fiscal     | Fiscal      | Fiscal    | Fiscal       | Fiscal       |
| additional | Additional  | Adicional | Adicional    | Oĩmbýva      |

## Relacionados

- [Crear Dirección](./AddressStore.md)
- [Actualizar Dirección](./AddressUpdate.md)

## Historial de Cambios

- 2025-10-23: Actualizado nombre del campo de `title` a `title_localized`.
- 2025-10-23: Documentación inicial.
