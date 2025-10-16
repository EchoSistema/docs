# Company – Registrar Dirección de Empresa

## Endpoint

```
POST /api/v1/company/{addressable}/addresses
```

## Autenticación

Requerida – Bearer {token} con habilidad `store.company.address`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro    | Tipo   | Requerido | Descripción |
| ------------ | ------ | --------- | ----------- |
| addressable  | string | Sí        | UUID de la empresa. |

### Cuerpo de la solicitud

| Parámetro       | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| --------------- | ------ | --------- | ----------- | ---------------------- |
| type            | string | No        | Tipo de dirección: `billing`, `shipping`, o `both`. | `both` |
| city            | string | Sí        | Nombre o identificador de la ciudad. | |
| state           | string | No        | Nombre o identificador del estado. | |
| country_code    | string | Sí        | Código de país ISO 3166-1 alfa-2 (2 caracteres). | |
| address_one     | string | Sí        | Línea de dirección principal. | |
| address_two     | string | No        | Línea de dirección secundaria. | |
| address_three   | string | No        | Línea de dirección terciaria. | |
| address_four    | string | No        | Línea de dirección cuaternaria. | |
| zipcode         | string | No        | Código postal. | |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "both",
    "country_code": "PY",
    "state": "Central",
    "city": "Asunción",
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "zipcode": "1234"
  }' \
  "https://sandbox.su-dominio.com/api/v1/company/{company-uuid}/addresses"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "type": "both",
    "country": {
      "id": 1,
      "name": "Paraguay",
      "code": "PY"
    },
    "state": {
      "id": 15,
      "name": "Central"
    },
    "city": {
      "id": 234,
      "name": "Asunción"
    },
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "address_three": null,
    "address_four": null,
    "zipcode": "1234",
    "created_at": "2025-10-16T12:00:00-03:00"
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| data               | object   | Recurso de dirección. |
| data.id            | integer  | Identificador de dirección. |
| data.type          | string   | Tipo de dirección: `billing`, `shipping`, o `both`. |
| data.country       | object   | Información del país. |
| data.country.id    | integer  | Identificador del país. |
| data.country.name  | string   | Nombre del país. |
| data.country.code  | string   | Código de país ISO 3166-1 alfa-2. |
| data.state         | object   | Información del estado/provincia (cuando disponible). |
| data.state.id      | integer  | Identificador del estado. |
| data.state.name    | string   | Nombre del estado. |
| data.city          | object   | Información de la ciudad. |
| data.city.id       | integer  | Identificador de la ciudad. |
| data.city.name     | string   | Nombre de la ciudad. |
| data.address_one   | string   | Línea de dirección principal. |
| data.address_two   | string   | Línea de dirección secundaria. |
| data.address_three | string   | Línea de dirección terciaria. |
| data.address_four  | string   | Línea de dirección cuaternaria. |
| data.zipcode       | string   | Código postal. |
| data.created_at    | datetime | Marca de tiempo de creación. |

## Estados HTTP

- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "city": ["El campo city es obligatorio."],
    "country_code": ["El campo country code es obligatorio."],
    "address_one": ["El campo address one es obligatorio."]
  }
}
```

## Notas

- El parámetro `type` por defecto es `both` si no se proporciona.
- El sistema resuelve automáticamente la información de la ciudad a partir de los datos proporcionados.
- El usuario autenticado debe tener la habilidad `store.company.address`.
- El parámetro `addressable` en la ruta debe ser un UUID válido de empresa.

## Relacionados

- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentación inicial.
