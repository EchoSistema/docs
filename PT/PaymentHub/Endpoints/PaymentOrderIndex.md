# PaymentHub – Índice de Ordens de Pagamento

## Endpoint

```
GET /api/v1/payment-hub/payment-orders
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| --------- | -------- | ----------- | --------- | -------------- |
| per_page  | integer  | Não         | Número de resultados por página | 10 (1-100) |
| page      | integer  | Não         | Número da página para paginação | 1 |

> Os nomes dos parâmetros aceitam variantes (`camelCase`, `kebab-case`, `snake_case`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/payment-hub/payment-orders?per_page=10"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "status": "completed",
      "amount": 15000,
      "currency": "EUR",
      "created_at": "2025-10-15T10:30:00Z",
      "updated_at": "2025-10-15T11:00:00Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/payment-hub/payment-orders?page=1",
    "last": "https://api.example.com/api/v1/payment-hub/payment-orders?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/payment-hub/payment-orders?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## Estrutura JSON Explicada

| Campo              | Tipo    | Descrição |
| ------------------ | ------- | --------- |
| data[]             | array   | Lista de ordens de pagamento |
| data[].uuid        | string  | Identificador único da ordem de pagamento |
| data[].status      | string  | Status atual da ordem de pagamento |
| data[].amount      | integer | Valor do pagamento em centavos |
| data[].currency    | string  | Código de moeda ISO |
| data[].created_at  | string  | Timestamp ISO 8601 de criação |
| data[].updated_at  | string  | Timestamp ISO 8601 da última atualização |
| links              | object  | Links de paginação |
| meta               | object  | Metadados de paginação |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

Resposta de erro padrão:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- As ordens de pagamento são filtradas pela plataforma autenticada
- Os resultados são paginados com um padrão de 10 itens por página
- O valor máximo de per_page é 100
- As ordens são retornadas em ordem decrescente por data de criação

## Relacionados

- [Exibir Ordem de Pagamento](./PaymentOrderShow.md)
- [Domínio PaymentHub](../README.md)

## Changelog

- 2025-10-16: Documentação inicial
