# PaymentHub – Exibir Ordem de Pagamento

## Endpoint

```
GET /api/v1/payment-hub/payment-orders/{paymentOrder}
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro    | Tipo   | Obrigatório | Descrição |
| ------------ | ------ | ----------- | --------- |
| paymentOrder | string | Sim         | UUID da ordem de pagamento |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/payment-hub/payment-orders/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "status": "completed",
    "amount": 15000,
    "currency": "EUR",
    "installments": [
      {
        "number": 1,
        "amount": 5000,
        "due_date": "2025-11-01",
        "status": "paid"
      },
      {
        "number": 2,
        "amount": 5000,
        "due_date": "2025-12-01",
        "status": "pending"
      },
      {
        "number": 3,
        "amount": 5000,
        "due_date": "2026-01-01",
        "status": "pending"
      }
    ],
    "created_at": "2025-10-15T10:30:00Z",
    "updated_at": "2025-10-15T11:00:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo                    | Tipo    | Descrição |
| ------------------------ | ------- | --------- |
| data                     | object  | Detalhes da ordem de pagamento |
| data.uuid                | string  | Identificador único da ordem de pagamento |
| data.status              | string  | Status atual da ordem de pagamento |
| data.amount              | integer | Valor total do pagamento em centavos |
| data.currency            | string  | Código de moeda ISO |
| data.installments[]      | array   | Lista de parcelas do pagamento |
| data.installments[].number | integer | Número da parcela |
| data.installments[].amount | integer | Valor da parcela em centavos |
| data.installments[].due_date | string | Data de vencimento da parcela |
| data.installments[].status | string | Status da parcela |
| data.created_at          | string  | Timestamp ISO 8601 de criação |
| data.updated_at          | string  | Timestamp ISO 8601 da última atualização |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

Respostas de erro padrão:

```json
{
  "message": "Payment order not found."
}
```

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- A ordem de pagamento deve pertencer à plataforma autenticada
- Os dados de parcelas são carregados automaticamente ao visualizar uma ordem de pagamento específica
- Se a ordem de pagamento não existir ou não pertencer à plataforma, um erro 404 é retornado

## Relacionados

- [Índice de Ordens de Pagamento](./PaymentOrderIndex.md)
- [Domínio PaymentHub](../README.md)

## Changelog

- 2025-10-16: Documentação inicial
