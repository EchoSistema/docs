# Inteligência Artificial – Excluir Produto de Preço por Byte

## Endpoint

```
DELETE /api/v1/ai/admin/pricing/bytes/{product:uuid}
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

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| product   | texto | Sim      | UUID do produto. |

## Exemplos

### Exemplo de solicitação (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Exemplo de solicitação (JavaScript)

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

### Exemplo de resposta

```json
{
  "message": "Byte pricing successfully deleted"
}
```

## Status HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Nãot Found (product not found or wrong measurement type)
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

## Histórico de mudanças

- 2025-10-23: Atualizado com documentação detalhada e estrutura de resposta precisa.
- 2025-10-16: Documentação inicial.
