# Inteligência Artificial – Criar Produto de Preço por Byte

## Endpoint

```
POST /api/v1/ai/admin/pricing/bytes
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

### Parâmetros do corpo

| Parâmetro | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------- | -------- | ----------- | -------------- |
| price     | inteiro | Sim      | Price per byte (raw value * 10000). For example, 10 represents $0.0010 | Min: 0 |
| currency  | texto  | Sim      | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | texto | Não     | Product description. | Max: 255 characters |
| language  | texto  | Não       | Language code for the title. | Platform default language |

> Parâmetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Exemplos

### Exemplo de solicitação (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 10,
    "currency": "USD",
    "description": "Price per byte for data processing and storage",
    "language": "en"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes"
```

### Exemplo de solicitação (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 10,
    currency: 'USD',
    description: 'Price per byte for data processing and storage',
    language: 'en'
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
    "description": "Price per byte for data processing and storage",
    "language": "en",
    "price": "0.0010",
    "raw_price": 10,
    "price_precision": 4,
    "prices": [],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-23T14:30:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Created byte pricing product. |
| data.uuid                      | texto  | UUID do produto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | inteiro | Measurement type ID. |
| data.measurement_type.name     | texto  | Measurement type name (BYTE). |
| data.measurement_type.title    | texto  | Human-readable measurement type. |
| data.title                     | texto  | Product title (always "Byte Price"). |
| data.slug                      | texto  | URL-friendly identifier (always "byte_price"). |
| data.description               | texto  | Product description. |
| data.language                  | texto  | Content language code. |
| data.price                     | texto  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | inteiro | Raw price value (price * 10000). |
| data.price_precision           | inteiro | Decimal precision for price (always 4). |
| data.prices                    | matriz   | Alternative prices (empty on creation). |
| data.currency                  | texto  | Currency code. |
| data.formatted_price           | texto  | Formatted price with currency symbol. |
| data.created_at                | texto  | Creation timestamp (ISO 8601). |

## Status HTTP

- 200: OK (product already exists, returns existing product)
- 201: Created
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Erros

### Validation Error

```json
{
  "message": "The price field is required.",
  "errors": {
    "price": [
      "The price field is required."
    ]
  }
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

- Este endpoint creates a byte pricing product for the authenticated user's platform.
- The title is automatically set to "Byte Price" and slug to "byte_price".
- The measurement type is automatically set to BYTE.
- If a byte pricing product already exists for the platform, the existing product is returned instead of creating a new one.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0010, send 10).
- The currency parameter must be a valid currency name from the CurrencyEnum (e.g., "USD", "EUR", "GBP").
- The description is optional and can be up to 255 characters.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Histórico de mudanças

- 2025-10-23: Atualizado com documentação detalhada e request/estrutura de resposta precisa.
- 2025-10-16: Documentação inicial.
