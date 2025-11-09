# Inteligencia Artificial – Risco de Churn

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-risk
```

Analisa o risco de churn (cancelamento/abandono) de clientes utilizando modelos predictivos de machine learning.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Requerido si en el está configurado en el servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido si en el está configurado en el servidor. |
| Accept-Language    | string | No         | Idioma de la respuesta (`en`, `es`, `pt`). Predeterminado: `en`. |
| Content-Type       | string | Sí         | `application/json`. |

## Parámetros

### Parámetros del cuerpo

| Parámetro       | Tipo   | Requerido | Descripción |
| --------------- | ------ | ----------- | --------- |
| customer_data   | array  | Sí         | Datos de los clientes para análisis de risco. |
| model_type      | string | No         | Tipo de modelo (`logistic`, `random_forest`, `xgboost`). |
| threshold       | float  | No         | Limiar de probabilidade para classificação (0-1). Predeterminado: `0.5`. |

## Ejemplos

### Ejemplo de solicitud (curl)

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
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-risk"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-risk', {
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

## Respuesta

### Éxito `200 OK`

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

## Estructura JSON

| Campo                                        | Tipo    | Descripción |
| -------------------------------------------- | ------- | --------- |
| `predictions`                                | array   | Array de previsões de churn. |
| `predictions[].customer_id`                  | string  | ID del cliente. |
| `predictions[].churn_probability`            | float   | Probabilidad de churn (0-1). |
| `predictions[].churn_risk`                   | string  | Nivel de risco (`low`, `medium`, `high`). |
| `predictions[].risk_factors`                 | array   | Fatores que contribuem para o risco. |
| `predictions[].risk_factors[].factor`        | string  | Descripción del fator de risco. |
| `predictions[].risk_factors[].importance`    | float   | Importância del fator (0-1). |
| `predictions[].retention_recommendations`    | array   | Recomendações para retenção. |
| `model_metrics`                              | object  | Métricas de rendimiento del modelo. |
| `model_metrics.accuracy`                     | float   | Acurácia del modelo (0-1). |
| `model_metrics.precision`                    | float   | Precisão del modelo (0-1). |
| `model_metrics.recall`                       | float   | Recall del modelo (0-1). |
| `model_metrics.f1_score`                     | float   | F1-Score del modelo (0-1). |

## Notas

* Valores de `churn_risk`: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* O modelo é retreinado periodicamente con novos dados.
* Recomendações são geradas automaticamente con base en los fatores de risco identificados.

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:160`
