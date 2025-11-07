# Inteligência Artificial – Segmentação de Compras

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/purchasing-segmentation
```

Realiza segmentação de clientes com base em padrões de compra, identificando grupos com comportamentos similares.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). Requerido se não configurado no servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido se não configurado no servidor. |
| Accept-Language    | string | Não         | Idioma (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| customer_data      | array  | Sim         | Dados dos clientes para segmentação. |
| segmentation_type  | string | Não         | Tipo de segmentação (`behavioral`, `value`, `frequency`). |
| time_period        | object | Não         | Período temporal para análise. |

> **Nota:** Os parâmetros aceitam tanto `snake_case` quanto `camelCase` (ex.: `customer_data` ou `customerData`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_data": [
      {"customer_id": "C001", "total_purchases": 1500, "avg_order_value": 150},
      {"customer_id": "C002", "total_purchases": 3000, "avg_order_value": 300}
    ],
    "segmentation_type": "behavioral"
  }' \
  "https://your-domain.com/api/v1/ai/echointel/customer-intelligence/purchasing-segmentation"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "segments": [
    {
      "segment_id": "high_value",
      "customer_count": 150,
      "avg_purchase_value": 2500,
      "customers": ["C002"]
    },
    {
      "segment_id": "regular",
      "customer_count": 300,
      "avg_purchase_value": 800,
      "customers": ["C001"]
    }
  ]
}
```

## Notas

* Segmentação baseada em algoritmos de machine learning.
* Suporta múltiplos critérios de segmentação.
* Parâmetros aceitam `snake_case` ou `camelCase`.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:105`
