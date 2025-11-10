# Artificial Intelligence – Churn Risk

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-risk
```

Analyzes customer churn risk (cancellation/abeonment) using machine learning predictive models to identify customers at risk of leaving e provide actionable retention recommendations.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). Obrigatório if not configured on the server. |
| X-Secret           | string | Condicional | 64-caracteres de segredo. Obrigatório if not configured on the server. |
| Accept-Language    | string | Não         | Idioma de resposta (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Tipo       | string | Sim         | `application/json`. |

## Parâmetros

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo da Requisição

| Parâmetro         | Tipo    | Obrigatório | Descrição | Padrão |
| ----------------- | ------- | -------- | ----------- | ------- |
| customer_data     | array   | Sim      | array of customer data objects for churn analysis. Each object contains customer attributes e behavior metrics. | - |
| model_type        | string  | Não       | Machine learning algorithm to use: `logistic`, `reom_forest`, `xgboost`, `catboost`. | `xgboost` |
| threshold         | float   | Não       | Probability threshold for churn classification (0-1). Customers with probability above this value are classified as high risk. | `0.5` |
| include_training  | boolean | Não       | Include model training metrics e evaluation in Resposta. | `false` |

### Customer Data object Fields

| Campo             | Tipo    | Obrigatório | Descrição | Example |
| ----------------- | ------- | -------- | ----------- | ------- |
| customer_id       | string  | Sim      | Unique customer identifier. | `"C001"` |
| tenure            | integer | Sim      | Months as customer (tenure in meses). | `24` |
| usage_frequency   | float   | Sim      | Monthly usage metric (average interactions, logins, or transactions per month). | `15.5` |
| service_tickets   | integer | Sim      | Number of support tickets opened in the last 12 meses. | `3` |
| nps               | integer | Não       | Net Promoter Score (-100 to 100). | `45` |
| payment_events    | array   | Não       | Payment history events (e.g., late payments, failed payments). | `["late_payment", "failed_payment"]` |
| churned           | integer | Obrigatório for training | Historical churn label for model training: `0` (retained) or `1` (churned). Only Obrigatório when training new models. | `0` |
| contract_type     | string  | Não       | Contract Tipo: `month-to-month`, `annual`, `two-year`. | `"month-to-month"` |
| monthly_charges   | float   | Não       | Average monthly charges. | `79.99` |
| total_charges     | float   | Não       | Total charges to date. | `1919.76` |
| payment_method    | string  | Não       | Payment method: `credit_card`, `electronic_check`, `bank_transfer`. | `"electronic_check"` |
| tech_support      | string  | Não       | Tech support subscription: `Sim`, `Não`. | `"Não"` |
| online_security   | string  | Não       | Online security subscription: `Sim`, `Não`. | `"Não"` |

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_data": [
      {
        "customer_id": "C001",
        "tenure": 24,
        "usage_frequency": 15.5,
        "service_tickets": 3,
        "nps": 45,
        "monthly_charges": 79.99,
        "total_charges": 1919.76,
        "contract_type": "month-to-month",
        "payment_method": "electronic_check",
        "tech_support": "no",
        "online_security": "no"
      }
    ],
    "model_type": "xgboost",
    "threshold": 0.6,
    "include_training": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-risk"
```

### Exemplo de Requisição (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-risk', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    customer_data: [{
      customer_id: 'C001',
      tenure: 24,
      usage_frequency: 15.5,
      service_tickets: 3,
      nps: 45,
      monthly_charges: 79.99,
      total_charges: 1919.76,
      contract_type: 'month-to-month',
      payment_method: 'electronic_check',
      tech_support: 'no',
      online_security: 'no'
    }],
    model_type: 'xgboost',
    threshold: 0.6,
    include_training: true
  })
});

const result = await response.json();
```

### Exemplo de Requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'en',
])->post('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-risk', [
    'customer_data' => [
        [
            'customer_id' => 'C001',
            'tenure' => 24,
            'usage_frequency' => 15.5,
            'service_tickets' => 3,
            'nps' => 45,
            'monthly_charges' => 79.99,
            'total_charges' => 1919.76,
            'contract_type' => 'month-to-month',
            'payment_method' => 'electronic_check',
            'tech_support' => 'no',
            'online_security' => 'no',
        ],
    ],
    'model_type' => 'xgboost',
    'threshold' => 0.6,
    'include_training' => true,
]);

$result = $response->json();
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
        {"factor": "electronic check payment", "importance": 0.18},
        {"factor": "low usage frequency", "importance": 0.15},
        {"factor": "multiple service tickets", "importance": 0.14}
      ],
      "retention_recommendations": [
        "Offer annual contract discount",
        "Provide complimentary tech support trial",
        "Suggest auto-payment setup",
        "Contact customer within 48 hours",
        "Offer personalized retention package"
      ]
    }
  ],
  "model_info": {
    "best_algorithm": "XGBoost",
    "model_version": "v2.3.1",
    "training_date": "2025-11-05T10:30:00Z"
  },
  "evaluation_metrics": {
    "roc_auc": 0.91,
    "accuracy": 0.89,
    "precision": 0.85,
    "recall": 0.82,
    "f1_score": 0.83
  }
}
```

### Erro `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The customer_data field is required and must contain at least one customer record."
}
```

### Erro `401 Unauthorized`

```json
{
  "error": "Unauthorized",
  "message": "Invalid authentication credentials"
}
```

### Erro `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid customer data",
  "details": {
    "customer_data.0.tenure": "The tenure field is required and must be an integer.",
    "customer_data.0.usage_frequency": "The usage_frequency field is required and must be a number."
  }
}
```

### Erro `500 Internal Server Erro`

```json
{
  "error": "Failed to communicate with AI service",
  "message": "Internal server error. Please try again later."
}
```

## Estrutura JSON

| Campo                                        | Tipo    | Descrição |
| -------------------------------------------- | ------- | --------- |
| `predictions`                                | array   | array of churn predictions, one per customer. |
| `predictions[].customer_id`                  | string  | Customer identifier. |
| `predictions[].churn_probability`            | float   | Probability of churn (0-1). Values closer to 1 indicate higher churn risk. |
| `predictions[].churn_risk`                   | string  | Risk level category: `low`, `medium`, or `high`. |
| `predictions[].risk_factors`                 | array   | Factors contributing to churn risk, sorted by importance. |
| `predictions[].risk_factors[].factor`        | string  | Descrição of the risk factor (e.g., "month-to-month contract", "low usage frequency"). |
| `predictions[].risk_factors[].importance`    | float   | Feature importance score (0-1). Higher values indicate greater impact on churn prediction. |
| `predictions[].retention_recommendations`    | array   | Actionable recommendations for customer retention. |
| `model_info`                                 | object  | Information about the model used for predictions. |
| `model_info.best_algorithm`                  | string  | Algorithm selected as champion (e.g., "XGBoost", "Reom Forest", "Logistic Regression", "CatBoost"). |
| `model_info.model_version`                   | string  | Version of the trained model. |
| `model_info.training_date`                   | string  | ISO 8601 timestamp of when the model was last trained. |
| `evaluation_metrics`                         | object  | Model performance metrics from cross-validation. |
| `evaluation_metrics.roc_auc`                 | float   | ROC-AUC score (0-1). Higher is better. Primary metric for model selection. |
| `evaluation_metrics.accuracy`                | float   | Overall accuracy (0-1). |
| `evaluation_metrics.precision`               | float   | Precision score (0-1). Ratio of true positives to predicted positives. |
| `evaluation_metrics.recall`                  | float   | Recall score (0-1). Ratio of true positives to actual positives. |
| `evaluation_metrics.f1_score`                | float   | F1 score (0-1). Harmonic mean of precision e recall. |

## Como é Calculado

The churn prediction system uses an automated machine learning approach:

1. **Algorithm Testing:** Tests 4 classification algorithms in parallel:
   - Logistic Regression (baseline linear model)
   - Reom Forest (ensemble decision trees)
   - XGBoost (gradient boosting)
   - CatBoost (categorical boosting)

2. **Cross-Validation:** Performs 5-fold cross-validation on provided training data to ensure robust evaluation e prevent overfitting.

3. **Model Evaluation:** Evaluates each model using ROC-AUC (Receiver Operating Characteristic - Area Under Curve) as the primary scoring metric.

4. **Champion Selection:** Automatically selects the best-performing model (highest ROC-AUC score) as the "champion" model.

5. **Prediction Generation:** Returns predictions with:
   - Churn probability scores (0-1)
   - Risk categorization based on threshold
   - Feature importance scores for interpretability
   - Actionable retention recommendations

**Desempenho:** Processing latency is approximately 250ms for 5,000 customer registros.

**Model Retraining:** Models are automatically retrained when new labeled data (`churned` Campo) is provided, ensuring predictions stay current with business dynamics.

## Status HTTP

| Código | Descrição |
|------|-------------|
| 200  | Sucesso - Predictions generated successfully |
| 400  | Bad Request - Missing or invalid Parâmetros |
| 401  | Unauthorized - Token de autenticação inválido ou faltante |
| 403  | Forbidden - Permissões insuficientes ou tenant inválido |
| 422  | Unprocessable Entity - Validation Erros in customer data |
| 429  | Too Many Requests - Limite de taxa excedido |
| 500  | Internal Server Erro - AI service communication failure |

## Erros

Common Erro scenarios:

**Campos Obrigatórios Faltantes:**
```json
{
  "error": "Validation failed",
  "message": "Required fields missing",
  "details": {
    "customer_data.0.customer_id": "The customer_id field is required.",
    "customer_data.0.tenure": "The tenure field is required."
  }
}
```

**Invalid Data Types:**
```json
{
  "error": "Validation failed",
  "message": "Invalid data types",
  "details": {
    "customer_data.0.tenure": "The tenure must be an integer.",
    "customer_data.0.usage_frequency": "The usage_frequency must be a number."
  }
}
```

**Payload Too Large:**
```json
{
  "error": "Payload too large",
  "message": "Request exceeds maximum allowed size of 20MB (~250,000 customer rows)"
}
```

**AI Service Unavailable:**
```json
{
  "error": "Service unavailable",
  "message": "EchoIntel AI service is temporarily unavailable. Please try again later.",
  "retry_after": 60
}
```

## Notas

### Risk Categories e Recommended Actions

| Risk Level | Threshold | Recommended Actions |
|-----------|-----------|-------------------|
| High | >= 0.60 | Immediate retention campaign, personal outreach, special offers, priority support |
| Medium | 0.25-0.59 | Monitor closely, targeted marketing, survey feedback, engagement campaigns |
| Low | < 0.25 | Steard engagement, upsell opportunities, loyalty programs |

### Implementation Details

- **Maximum Payload:** 20MB (~250,000 customer rows per request)
- **Missing Values:** Automatically imputed using median for numerical fields e mode for categorical fields
- **PII Heling:** Customer IDs are hashed internally for security e privacy compliance
- **Async Bulk Endpoint:** Planned for Q4 2025 to hele conjuntos de dados maiores with webhook notifications

### Industry-Specific Churn Definitions

Different industries define churn differently. Use the appropriate definition for your business:

- **SaaS/Subscription:** Churn = subscription cancellation or non-renewal
- **E-commerce:** Churn = Não purchase activity in the last 6 meses
- **Telecommunications:** Churn = service termination or port-out to competitor
- **Pre-paid Services:** Churn = Não balance top-up or usage in the last 3 meses
- **Financial Services:** Churn = account closure or 90+ dias of inactivity

### Model Training Melhores Práticas

When providing historical churn labels for model training:

- **Minimum Dataset Size:** At least 1,000 customer registros with 100+ churn cases
- **Class Balance:** Aim for 10-40% churn rate in training data (avoid extreme imbalance)
- **Feature Quality:** Ensure all Obrigatório fields are populated e accurate
- **Time Window:** Use consistent time windows for feature calculation (e.g., last 90 dias)
- **Regular Updates:** Retrain models monthly or when business conditions change significantly

### Processing e Desempenho

- **Timeout:** Maximum processing time is 300 seconds (5 minutes)
- **Rate Limiting:** 100 requisições por minuto por tenant
- **Batch Recommendations:** For >10,000 customers, split into multiple requests or contact support for bulk processing
- **Caching:** Model predictions are not cached; each request generates fresh predictions

### Security e Compliance

- The Cabeçalhos `X-Customer-Api-Id` e `X-Secret` can be configured on the server via `.env`
- The secret must be rotated every 90 dias according to security policy
- All customer data is encrypted in transit (TLS 1.3) e em repouso
- Data retention: Training data is retained for 90 dias, predictions are not stored

## Perguntas Frequentes

### Q: What's the minimum dataset size for reliable churn predictions?
**A:** We recommend at least 1,000 customer registros with a minimum of 100 churned cases for model training. For scoring existing customers, you can process any number of registros per request (up to 250,000 due to 20MB payload limit). The model performs best when training data spans 6-12 meses of consistent customer behavior.

### Q: How often should the churn model be retrained?
**A:** Retrain the model monthly or whenever significant business changes occur (pricing changes, product updates, market shifts). Monitor model performance metrics (ROC-AUC, precision, recall) mensal. If ROC-AUC drops below 0.85, prioritize retraining with fresh labeled data. We automatically retrain when new labeled churn data is provided via the `churned` Campo.

### Q: How do I interpret a churn probability of 0.65?
**A:** A probability of 0.65 (or 65%) indicates the customer has a 65% likelihood of churning within your defined churn window. This maps to the "high" risk category (>= 0.60). Recommended immediate actions: personal outreach within 48 hours, special retention offers (10-20% discount), upgrade to premium features, or assign to dedicated retention specialist. The higher the probability, the more urgent the intervention.

### Q: Can I customize the risk be thresholds?
**A:** Sim! Use the `threshold` Parâmetro to set your custom classification boundary (0-1 range). For example, `threshold: 0.4` will classify customers above 0.4 as high-risk. However, the Padrão 0.5 is optimized for balanced precision-recall. If you want to catch more at-risk customers, lower to 0.4-0.45 (higher recall, slightly lower precision). The Resposta always includes raw probabilities, so you can analyze at any threshold.

### Q: Is my customer data stored or used for training other tenants?
**A:** Não. Customer data is strictly isolated per tenant e never used for cross-tenant model training. Training data is retained for 90 dias for model retraining purposes only, then deleted. Predictions are not stored. All data in transit uses TLS 1.3 encryption, e sensitive fields (like customer IDs) are hashed internally for additional security. We comply with GDPR e offer data deletion upon request.

### Q: What happens if I'm missing some Obrigatório customer fields?
**A:** Missing values in Obrigatório fields (tenure, usage_frequency, service_tickets) will cause validation Erros (422 Unprocessable Entity). However, optional fields (nps, contract_type, payment_method, etc.) are heled automatically: numerical fields use median imputation, categorical fields use mode imputation. Ensure at minimum the 3 Obrigatório fields are populated for all customers.

### Q: What's the processing latency for large customer batches?
**A:** Processing latency is approximately 250ms for 5,000 customer registros. Scaling is roughly linear: 10,000 customers = ~500ms, 50,000 = ~2.5 seconds. Maximum timeout is 300 seconds (5 minutes). For >100,000 customers, consider splitting into multiple requests or contacting support for bulk processing options. Rate limit is 100 requisições por minuto por tenant.

### Q: Which algorithm typically performs best e why?
**A:** XGBoost is the most frequently selected champion model in production, followed by Reom Forest. XGBoost excels at capturing non-linear relationships between features (e.g., interaction between contract Tipo e payment method). The system automatically tests all 4 algorithms (logistic regression, reom forest, XGBoost, CatBoost) via 5-fold cross-validation e selects the one with highest ROC-AUC. The `model_info.best_algorithm` Campo in the Resposta shows which algorithm was chosen for your specific request.

## Relacionado

- [Churn Labeling](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/CustomerIntelligence/ChurnLabel.md) - Label historical churn events for model training
- [Customer Segmentation](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/CustomerIntelligence/CustomerSegmentation.md) - Segment customers by behavior patterns
- [Customer Loyalty](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/CustomerIntelligence/CustomerLoyalty.md) - Analyze customer loyalty metrics
- [NPS Analysis](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/CustomerIntelligence/NPS.md) - Net Promoter Score analysis

## Referências

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:160`
