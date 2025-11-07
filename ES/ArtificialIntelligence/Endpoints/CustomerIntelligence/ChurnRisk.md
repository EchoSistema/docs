# Inteligência Artificial – Risco de Churn

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-risk
```

Analisa o risco de churn (cancelamento/abandono) de clientes utilizando modelos preditivos de machine learning.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). Requerido se não configurado no servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido se não configurado no servidor. |
| Accept-Language    | string | Não         | Idioma da resposta (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| customer_data   | array  | Sim         | Dados dos clientes para análise de risco. |
| model_type      | string | Não         | Tipo de modelo (`logistic`, `random_forest`, `xgboost`). |
| threshold       | float  | Não         | Limiar de probabilidade para classificação (0-1). Padrão: `0.5`. |

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
      {
        "customer_id": "C001",
        "tenure_months": 24,
        "monthly_charges": 79.99,
        "total_charges": 1919.76,
        "contract_type": "month-to-month",
        "payment_method": "electronic_check",
        "tech_support": "no",
        "online_security": "no"
      }
    ],
    "model_type": "xgboost",
    "threshold": 0.7
  }' \
  "https://your-domain.com/api/v1/ai/echointel/customer-intelligence/churn-risk"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://your-domain.com/api/v1/ai/echointel/customer-intelligence/churn-risk', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'pt',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    customer_data: [{
      customer_id: 'C001',
      tenure_months: 24,
      monthly_charges: 79.99,
      total_charges: 1919.76,
      contract_type: 'month-to-month',
      payment_method: 'electronic_check'
    }],
    model_type: 'xgboost',
    threshold: 0.7
  })
});

const result = await response.json();
```

## Resposta

### Sucesso `200 OK`

```json
{
  "predictions": [
    {
      "customer_id": "C001",
      "churn_probability": 0.85,
      "churn_risk": "high",
      "risk_factors": [
        {"factor": "month-to-month contract", "importance": 0.32},
        {"factor": "no tech support", "importance": 0.21},
        {"factor": "electronic check payment", "importance": 0.18}
      ],
      "retention_recommendations": [
        "Offer annual contract discount",
        "Provide complimentary tech support trial",
        "Suggest auto-payment setup"
      ]
    }
  ],
  "model_metrics": {
    "accuracy": 0.89,
    "precision": 0.85,
    "recall": 0.82,
    "f1_score": 0.83
  }
}
```

## Estrutura JSON

| Campo                                        | Tipo    | Descrição |
| -------------------------------------------- | ------- | --------- |
| `predictions`                                | array   | Array de previsões de churn. |
| `predictions[].customer_id`                  | string  | ID do cliente. |
| `predictions[].churn_probability`            | float   | Probabilidade de churn (0-1). |
| `predictions[].churn_risk`                   | string  | Nível de risco (`low`, `medium`, `high`). |
| `predictions[].risk_factors`                 | array   | Fatores que contribuem para o risco. |
| `predictions[].risk_factors[].factor`        | string  | Descrição do fator de risco. |
| `predictions[].risk_factors[].importance`    | float   | Importância do fator (0-1). |
| `predictions[].retention_recommendations`    | array   | Recomendações para retenção. |
| `model_metrics`                              | object  | Métricas de performance do modelo. |
| `model_metrics.accuracy`                     | float   | Acurácia do modelo (0-1). |
| `model_metrics.precision`                    | float   | Precisão do modelo (0-1). |
| `model_metrics.recall`                       | float   | Recall do modelo (0-1). |
| `model_metrics.f1_score`                     | float   | F1-Score do modelo (0-1). |

## Notas

* Valores de `churn_risk`: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* O modelo é retreinado periodicamente com novos dados.
* Recomendações são geradas automaticamente com base nos fatores de risco identificados.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:160`
