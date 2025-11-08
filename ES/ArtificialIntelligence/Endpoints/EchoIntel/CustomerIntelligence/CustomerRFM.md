# Inteligência Artificial – Análise RFM (Recency, Frequency, Monetary)

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/rfm
```

Realiza análise RFM (Recência, Frequência e Valor Monetário) para segmentar clientes com base em seu comportamento de compra.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). |
| X-Secret           | string | Condicional | Secret de 64 caracteres. |
| Accept-Language    | string | Não         | Idioma (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| transactions  | array  | Sim         | Histórico de transações dos clientes. |
| reference_date| string | Não         | Data de referência para cálculo (ISO 8601). Padrão: hoje. |
| quintiles     | boolean| Não         | Se deve usar quintis (5 grupos) ao invés de quartis (4). Padrão: `false`. |

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
    "transactions": [
      {"customer_id": "C001", "date": "2025-01-05", "amount": 150.00},
      {"customer_id": "C001", "date": "2024-12-20", "amount": 89.90},
      {"customer_id": "C002", "date": "2024-10-15", "amount": 500.00}
    ],
    "reference_date": "2025-01-07",
    "quintiles": true
  }' \
  "https://your-domain.com/api/v1/ai/echointel/customer-intelligence/rfm"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "rfm_analysis": [
    {
      "customer_id": "C001",
      "recency_score": 5,
      "frequency_score": 4,
      "monetary_score": 3,
      "rfm_score": "543",
      "segment": "Champions",
      "recency_days": 2,
      "frequency_count": 12,
      "monetary_total": 1450.00,
      "recommendations": [
        "Reward with loyalty benefits",
        "Upsell premium products",
        "Request referrals"
      ]
    },
    {
      "customer_id": "C002",
      "recency_score": 1,
      "frequency_score": 1,
      "monetary_score": 4,
      "rfm_score": "114",
      "segment": "At Risk",
      "recency_days": 84,
      "frequency_count": 2,
      "monetary_total": 3200.00,
      "recommendations": [
        "Send reactivation campaign",
        "Offer special discount",
        "Survey to understand issues"
      ]
    }
  ],
  "segments_summary": {
    "Champions": 15,
    "Loyal Customers": 28,
    "Potential Loyalists": 22,
    "At Risk": 18,
    "Lost": 12
  }
}
```

## Estrutura JSON

| Campo                                  | Tipo    | Descrição |
| -------------------------------------- | ------- | --------- |
| `rfm_analysis`                         | array   | Array de análises RFM por cliente. |
| `rfm_analysis[].customer_id`           | string  | ID do cliente. |
| `rfm_analysis[].recency_score`         | int     | Score de recência (1-5). |
| `rfm_analysis[].frequency_score`       | int     | Score de frequência (1-5). |
| `rfm_analysis[].monetary_score`        | int     | Score monetário (1-5). |
| `rfm_analysis[].rfm_score`             | string  | Score RFM combinado. |
| `rfm_analysis[].segment`               | string  | Segmento do cliente. |
| `rfm_analysis[].recency_days`          | int     | Dias desde última compra. |
| `rfm_analysis[].frequency_count`       | int     | Número de compras. |
| `rfm_analysis[].monetary_total`        | float   | Valor total gasto. |
| `rfm_analysis[].recommendations`       | array   | Recomendações de ação. |
| `segments_summary`                     | object  | Resumo de clientes por segmento. |

## Segmentos RFM

| Segmento              | Scores | Descrição |
| --------------------- | ------ | --------- |
| Champions             | 555, 554, 544, 545, 454, 455, 445 | Melhores clientes. |
| Loyal Customers       | 543, 444, 435, 355, 354, 345, 344, 335 | Clientes fiéis. |
| Potential Loyalists   | 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 | Clientes promissores. |
| At Risk               | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 | Em risco de churn. |
| Lost                  | 111, 112, 121, 131, 141,211, 212, 114, 214, 141, 221, 213, 113, 222, 132 | Clientes perdidos. |

## Notas

* Scores variam de 1 (pior) a 5 (melhor).
* Segmentos são atribuídos automaticamente com base nos scores RFM.
* Recomendações são personalizadas para cada segmento.

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:145`
