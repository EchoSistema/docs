# Inteligência Artificial – Mostrar Detalhes do Preço por Byte

## Endpoint

```
GET /api/v1/ai/admin/pricing/bytes/details
```

## Autenticação

Required – Bearer {token} with ability `auth:sanctum`

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | texto | Sim      | `Bearer {token}`. |
| X-PUBLIC-KEY     | texto | Sim      | Chave pública da plataforma. |
| Accept-Language  | texto | Não       | Localidade IETF (ex., `pt-BR`, `en`, `es`). |

## Parâmetros

No parameters required.

## Exemplos

### Exemplo de solicitação (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/details"
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
    "description": "Price per byte for data processing",
    "language": "en",
    "price": "0.0010",
    "raw_price": 10,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 2,
        "currency": "EUR",
        "value": "0.0009",
        "raw_value": 9,
        "formatted_value": "€0.0009"
      },
      {
        "currency_id": 3,
        "currency": "GBP",
        "value": "0.0008",
        "raw_value": 8,
        "formatted_value": "£0.0008"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Byte pricing product details. |
| data.uuid                      | texto  | UUID do produto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | inteiro | Measurement type ID. |
| data.measurement_type.name     | texto  | Measurement type name (BYTE). |
| data.measurement_type.title    | texto  | Human-readable measurement type. |
| data.title                     | texto  | Product title. |
| data.slug                      | texto  | URL-friendly identifier (always "byte_price"). |
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
- 404: Nãot Found (when byte pricing product doesn't exist for the platform)
- 429: Too Many Requests
- 500: Internal Server Error

## Erros

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

- Este endpoint returns the byte pricing product associated with the authenticated user's platform.
- The product is identified by the slug "byte_price" and measurement type BYTE.
- Returns 404 if no byte pricing product exists for the platform.
- The price represents the cost per byte of data processing.
- The `formatted_price` uses the platform's locale for currency formatting.
- Alternative prices show all active prices in currencies different from the default currency.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)
- [Data Calculator Process](./DataCalculatorProcess.md)

## Histórico de mudanças

- 2025-10-23: Atualizado com documentação detalhada e estrutura de resposta precisa.
- 2025-10-16: Documentação inicial.
