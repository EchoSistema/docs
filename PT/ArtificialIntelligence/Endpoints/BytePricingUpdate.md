# Inteligência Artificial – Atualizar Produto de Preço por Byte

## Endpoint

```
PUT /api/v1/ai/admin/pricing/bytes/{product:uuid}
```

## Autenticação

Required – Bearer {token} with ability `auth:sanctum`

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | texto | Sim      | `Bearer {token}`. |
| X-PUBLIC-KEY     | texto | Sim      | Chave pública da plataforma. |
| Accept-Language  | texto | Não       | Localidade IETF (ex., `pt-BR`, `en`, `es`). |
| Content-Type     | texto | Sim      | `application/json`. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| product   | texto | Sim      | UUID do produto. |

### Parâmetros do corpo

| Parâmetro   | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ----------- | ------- | -------- | ----------- | -------------- |
| price       | inteiro | Não       | Price per byte (raw value * 10000). | Min: 0 |
| currency    | texto  | Obrigatório with price | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | texto  | Não       | Product description. | Max: 5000 characters |

> Parâmetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Exemplos

### Exemplo de solicitação (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 15,
    "currency": "USD",
    "description": "Updated price per byte for enhanced data processing"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Exemplo de solicitação (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 15,
    currency: 'USD',
    description: 'Updated price per byte for enhanced data processing'
  })
});
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "measurement_type": {
      "id": 1,
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Byte Price",
    "slug": "byte_price",
    "description": "Updated price per byte for enhanced data processing",
    "language": "en",
    "price": "0.0015",
    "raw_price": 15,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 2,
        "currency": "EUR",
        "value": "0.0013",
        "raw_value": 13,
        "formatted_value": "€0.0013"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0015",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Updated byte pricing product. |
| data.uuid                      | texto  | UUID do produto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | inteiro | Measurement type ID. |
| data.measurement_type.name     | texto  | Measurement type name (BYTE). |
| data.measurement_type.title    | texto  | Human-readable measurement type. |
| data.title                     | texto  | Product title. |
| data.slug                      | texto  | URL-friendly identifier. |
| data.description               | texto  | Product description. |
| data.language                  | texto  | Content language code. |
| data.price                     | texto  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | inteiro | Raw price value (price * 10000). |
| data.price_precision           | inteiro | Decimal precision for price (always 4). |
| data.prices                    | matriz   | Alternative prices in other currencies. |
| data.prices[].currency_id      | inteiro | Currency ID. |
| data.prices[].currency         | texto  | Currency code (ISO 4217). |
| data.prices[].value            | texto  | Price value in this currency. |
| data.prices[].raw_value        | inteiro | Raw price value. |
| data.prices[].formatted_value  | texto  | Formatted price with currency symbol. |
| data.currency                  | texto  | Default currency code. |
| data.formatted_price           | texto  | Formatted default price with currency symbol. |
| data.created_at                | texto  | Creation timestamp (ISO 8601). |

## Status HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Nãot Found (product not found or wrong measurement type)
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Erros

### Validation Error

```json
{
  "message": "The currency field is required when price is present.",
  "errors": {
    "currency": [
      "The currency field is required when price is present."
    ]
  }
}
```

### Nãot Found

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

## Nãotas

- Este endpoint updates a byte pricing product.
- The product must have measurement type BYTE, otherwise 404 is returned.
- All parameters are optional, allowing partial updates.
- If you provide a price, you must also provide the currency.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0015, send 15).
- The description can be up to 5000 characters (increased from 255 in store).
- The currency parameter must be a valid currency name from the CurrencyEnum.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Histórico de mudanças

- 2025-10-23: Atualizado com documentação detalhada e request/estrutura de resposta precisa.
- 2025-10-16: Documentação inicial.
