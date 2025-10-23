# Inteligencia Artificial – Crear Dirección

## Endpoint

```
POST /api/v1/ai/admin/addresses
```

## Autenticación

Obligatorio – Bearer {token} con habilidad `auth:sanctum`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción |
| ---------------- | ------ | ----------- | ----------- |
| Authorization    | string | Sí          | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma. |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sí          | `application/json`. |

## Parámetros

### Parámetros del cuerpo

| Parámetro    | Tipo   | Obligatorio | Descripción | Predeterminado/Valores |
| ------------ | ------ | ----------- | ----------- | ---------------------- |
| type         | string | No          | Tipo de dirección. | Predeterminado: `BOTH`. Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| uuid         | string | No          | UUID personalizado para la dirección. | Generado automáticamente si no se proporciona |
| city         | string/object | Sí | Nombre de la ciudad u objeto. | - |
| state        | string/object | No | Nombre del estado u objeto. | - |
| country_code | string | Sí          | Código del país (ISO 3166-1 alpha-2). | Máx: 2 caracteres |
| address_one  | string | Sí          | Línea de dirección principal. | - |
| address_two  | string | No          | Línea de dirección secundaria. | - |
| address_three| string | No          | Línea de dirección terciaria. | - |
| address_four | string | No          | Línea de dirección cuaternaria. | - |
| zipcode      | string | No          | Código postal. | - |

> Los nombres de los parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case` o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "BOTH",
    "city": "Madrid",
    "country_code": "ES",
    "address_one": "Gran Vía, 32",
    "address_two": "Piso 5",
    "zipcode": "28013"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/addresses"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/addresses', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'es',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    type: 'BOTH',
    city: 'Madrid',
    country_code: 'ES',
    address_one: 'Gran Vía, 32',
    address_two: 'Piso 5',
    zipcode: '28013'
  })
});
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "main": true,
    "type": "BOTH",
    "zipcode": "28013",
    "one": "Gran Vía, 32",
    "two": "Piso 5",
    "three": null,
    "four": null,
    "city": {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "name": "Madrid",
      "abbreviation": "M"
    },
    "state": {
      "uuid": "00000000-0000-0000-0000-000000000003",
      "name": "Madrid",
      "abbreviation": "M",
      "region": "Centro"
    },
    "country": {
      "uuid": "00000000-0000-0000-0000-000000000004",
      "name": "España",
      "official_name": "Reino de España",
      "code": "ES"
    },
    "formatted": "Gran Vía, 32, Piso 5, Madrid, M 28013, España"
  }
}
```

## Estructura JSON Explicada

| Campo                   | Tipo    | Descripción |
| ----------------------- | ------- | ----------- |
| data                    | object  | Dirección creada. |
| data.uuid               | string  | UUID de la dirección. |
| data.main               | boolean | Si esta es la dirección principal (verdadero cuando el tipo es `BOTH`). |
| data.is_billing         | boolean | Si esta es una dirección de facturación (presente cuando el tipo no es `BOTH`). |
| data.is_shipping        | boolean | Si esta es una dirección de envío (presente cuando el tipo no es `BOTH`). |
| data.is_fiscal          | boolean | Si esta es una dirección fiscal (presente cuando el tipo no es `BOTH`). |
| data.type               | string  | Tipo de dirección. |
| data.zipcode            | string  | Código postal. |
| data.one                | string  | Línea de dirección principal. |
| data.two                | string  | Línea de dirección secundaria. |
| data.three              | string  | Línea de dirección terciaria. |
| data.four               | string  | Línea de dirección cuaternaria. |
| data.city               | object  | Información de la ciudad. |
| data.city.uuid          | string  | UUID de la ciudad. |
| data.city.name          | string  | Nombre de la ciudad. |
| data.city.abbreviation  | string  | Abreviatura de la ciudad. |
| data.state              | object  | Información del estado/provincia. |
| data.state.uuid         | string  | UUID del estado. |
| data.state.name         | string  | Nombre del estado. |
| data.state.abbreviation | string  | Abreviatura del estado. |
| data.state.region       | string  | Región del estado. |
| data.country            | object  | Información del país. |
| data.country.uuid       | string  | UUID del país. |
| data.country.name       | string  | Nombre del país. |
| data.country.official_name | string | Nombre oficial del país. |
| data.country.code       | string  | Código del país (ISO 3166-1 alpha-2). |
| data.formatted          | string  | Dirección formateada. |

## Estado HTTP

- 200: OK
- 201: Creado
- 401: No Autorizado
- 403: Prohibido
- 422: Entidad No Procesable (errores de validación)
- 429: Demasiadas Solicitudes
- 500: Error Interno del Servidor

## Errores

### Error de Validación

```json
{
  "message": "El campo city es obligatorio.",
  "errors": {
    "city": [
      "El campo city es obligatorio."
    ]
  }
}
```

### No Autenticado

```json
{
  "message": "No autenticado.",
  "errors": {}
}
```

## Notas

- Este endpoint crea una nueva dirección para la plataforma del usuario autenticado.
- El campo `type` tiene como valor predeterminado `BOTH` si no se proporciona.
- La resolución de la ciudad se maneja a través de un pipeline que resuelve automáticamente la información de ciudad, estado y país.
- Si no se proporciona `uuid`, se generará automáticamente.
- El `country_code` debe ser un código válido ISO 3166-1 alpha-2 (ej.: "US", "BR", "ES").

## Relacionados

- [Actualizar Dirección](./AddressUpdate.md)

## Historial de Cambios

- 2025-10-23: Documentación inicial.
