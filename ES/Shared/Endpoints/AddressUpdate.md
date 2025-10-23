# Compartido – Actualizar Dirección

## Endpoint

```
PUT /api/v1/addresses/{address}/type/{type}
```

## Autenticación

Obligatorio – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción |
| ---------------- | ------ | ----------- | ----------- |
| Authorization    | string | Sí          | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma. |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sí          | `application/json`. |

## Parámetros de la URL

| Parámetro | Tipo   | Obligatorio | Descripción |
| --------- | ------ | ----------- | ----------- |
| address   | string | Sí          | UUID de la dirección. |
| type      | string | Sí          | Tipo de dirección para filtrar. Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |

## Parámetros

### Parámetros del cuerpo

| Parámetro    | Tipo   | Obligatorio | Descripción | Predeterminado/Valores |
| ------------ | ------ | ----------- | ----------- | ---------------------- |
| type         | string | No          | Tipo de dirección. | Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| city         | string/object | No | Nombre de la ciudad u objeto. | - |
| city_id      | integer | No         | ID de la ciudad. | - |
| state_id     | integer | No         | ID del estado. | - |
| country_id   | integer | No         | ID del país. | - |
| zipcode      | string | No          | Código postal. | Máx: 255 caracteres |
| address_one  | string | No          | Línea de dirección principal. | Máx: 255 caracteres |
| address_two  | string | No          | Línea de dirección secundaria. | Máx: 255 caracteres |
| address_three| string | No          | Línea de dirección terciaria. | Máx: 255 caracteres |
| address_four | string | No          | Línea de dirección cuaternaria. | Máx: 255 caracteres |
| uuid         | -      | -           | Campo prohibido. No se puede actualizar. | - |

> Los nombres de los parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case` o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "address_one": "Gran Vía, 32",
    "address_two": "Piso 6",
    "zipcode": "28013"
  }' \
  "https://sandbox.your-domain.com/api/v1/addresses/00000000-0000-0000-0000-000000000001/type/BOTH"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/addresses/00000000-0000-0000-0000-000000000001/type/BOTH', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'es',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    address_one: 'Gran Vía, 32',
    address_two: 'Piso 6',
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
    "two": "Piso 6",
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
    "formatted": "Gran Vía, 32, Piso 6, Madrid, M 28013, España"
  }
}
```

## Estructura JSON Explicada

| Campo                   | Tipo    | Descripción |
| ----------------------- | ------- | ----------- |
| data                    | object  | Dirección actualizada. |
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
- 401: No Autorizado
- 403: Prohibido
- 404: No Encontrado
- 422: Entidad No Procesable (errores de validación)
- 429: Demasiadas Solicitudes
- 500: Error Interno del Servidor

## Errores

### Error de Validación

```json
{
  "message": "El campo uuid está prohibido.",
  "errors": {
    "uuid": [
      "El campo uuid está prohibido."
    ]
  }
}
```

### No Encontrado

```json
{
  "message": "No encontrado"
}
```

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

- Este endpoint actualiza una dirección existente y requiere la habilidad `backoffice`.
- Solo se actualizarán los campos proporcionados; todos los demás campos permanecen sin cambios.
- El campo `uuid` está prohibido y no se puede actualizar.
- La dirección se consulta tanto por el UUID como por el parámetro de tipo en la URL.
- La resolución de la ciudad se maneja a través de un pipeline que resuelve automáticamente la información de ciudad, estado y país cuando se actualizan campos relacionados con la ciudad.
- Puede actualizar la ciudad usando una cadena/objeto en el campo `city` o proporcionando `city_id`, `state_id` y `country_id`.

## Relacionados

- [Crear Dirección](./AddressStore.md)

## Historial de Cambios

- 2025-10-23: Documentación inicial.
