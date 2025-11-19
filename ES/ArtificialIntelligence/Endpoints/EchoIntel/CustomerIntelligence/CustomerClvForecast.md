# Artificial Intelligence – CLV Forecast

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/clv-forecast
```

Predicts Customer Lifetime Value (CLV) using machine learning regression models. Forecasts the total revenue a customer will generate over their relationship with the business.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Obrigatório if not configured on the server. |
| X-Secret           | string | Condicional | 64-caracteres de segredo. Obrigatório if not configured on the server. |
| Accept-Language    | string | No         | Idioma de resposta (`en`, `es`, `pt`). Predeterminado: `en`. |
| Content-Tipo       | string | Sí         | `application/json`. |

## Parámetros

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo de la Requisição

| Parámetro | Tipo | Requerido | Descripción | Significado Empresarial | Padrão |
| --------- | ---- | -------- | ----------- | ---------------- | ------- |
| customer_data | array | Sí | array of customer registros with behavioral features. | Customer profiles for CLV prediction. | - |
| forecast_horizon | integer | No | Prediction window in meses (6-60). | How far ahead to predict value. | `12` |
| model_type | string | No | Model to use: `linear_regression`, `reom_forest`, `xgboost`, `auto`. | Algorithm selection for prediction. | `auto` |
| include_confidence | boolean | No | Include confidence intervals in predictions. | Add uncertainty bounds to estimates. | `true` |

### Customer Data object Fields

| Campo | Tipo | Requerido | Descripción | Significado Empresarial | Example |
| ----- | ---- | -------- | ----------- | ---------------- | ------- |
| customer_id | string | Sí | Unique customer identifier. | Customer account reference. | `"CUST-10234"` |
| tenure_dias | integer | Sí | Days as customer. | Relationship duration. | `365` |
| total_revenue | float | Sí | Historical total spending. | Cumulative customer value. | `1250.00` |
| purchase_frequency | integer | Sí | Total number of purchases. | Transaction count. | `8` |
| avg_order_value | float | Sí | Average transaction amount. | Typical basket size. | `156.25` |
| recency_dias | integer | Sí | Days since last purchase. | Purchase freshness. | `15` |

## Ejemplos

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
        "customer_id": "CUST-10234",
        "tenure_days": 365,
        "total_revenue": 1250.00,
        "purchase_frequency": 8,
        "avg_order_value": 156.25,
        "recency_days": 15
      }
    ],
    "forecast_horizon": 12,
    "model_type": "auto",
    "include_confidence": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/clv-forecast"
```

### Exemplo de Requisição (Python)

```python
import requests

url = "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/clv-forecast"
headers = {
    "Authorization": "Bearer <token>",
    "X-Customer-Api-Id": "<tenant-uuid>",
    "X-Secret": "<secret>",
    "Accept-Language": "en",
    "Content-Type": "application/json"
}
payload = {
    "customer_data": [{
        "customer_id": "CUST-10234",
        "tenure_days": 365,
        "total_revenue": 1250.00,
        "purchase_frequency": 8,
        "avg_order_value": 156.25,
        "recency_days": 15
    }],
    "forecast_horizon": 12,
    "model_type": "auto",
    "include_confidence": True
}

response = requests.post(url, headers=headers, json=payload)
result = response.json()
```

## Respuesta

### Éxito `200 OK`

```json
{
  "predictions": [
    {
      "customer_id": "CUST-10234",
      "predicted_clv": 1875.50,
      "historical_value": 1250.00,
      "predicted_future_value": 625.50,
      "confidence_interval": {
        "lower_bound": 1576.25,
        "upper_bound": 2174.75,
        "confidence_level": 0.95
      },
      "value_breakdown": {
        "short_term_3m": 187.50,
        "medium_term_6m": 312.50,
        "long_term_12m": 625.50
      }
    }
  ],
  "model_info": {
    "selected_model": "XGBoost",
    "model_performance": {
      "r2_score": 0.87,
      "mae": 125.43
    }
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "customer_data must contain at least one customer record with required fields."
}
```

## Estructura JSON

| Campo | Tipo | Descripción | Significado Empresarial |
| ----- | ---- | ----------- | ---------------- |
| `predictions` | array | array of CLV predictions, one per customer. | Forecasted customer values. |
| `predictions[].customer_id` | string | Customer identifier. | Links prediction to customer. |
| `predictions[].predicted_clv` | float | Total predicted customer lifetime value. | Complete value forecast. |
| `predictions[].historical_value` | float | Actual spending to date. | Past customer contribution. |
| `predictions[].predicted_future_value` | float | Expected future spending. | Incremental value forecast. |
| `predictions[].confidence_interval.lower_bound` | float | Lower 95% confidence bound. | Conservative estimate. |
| `predictions[].confidence_interval.upper_bound` | float | Upper 95% confidence bound. | Optimistic estimate. |
| `predictions[].value_breakdown` | object | CLV by time period. | Temporal value distribution. |
| `model_info.selected_model` | string | Chosen algorithm name. | Best-performing model. |
| `model_info.model_performance.r2_score` | float | R-squared goodness of fit (0-1). | Variance explained by model. |

## Status HTTP

| Status Código | Descripción |
|-------------|-------------|
| 200 OK | Request successful. Returns CLV forecast results. |
| 400 Bad Request | Invalid request Parâmetros. Check Parâmetro types y Obrigatório fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Resposta for details. |
| 429 Too Many Requests | Limite de taxa excedido. Retry after cooldown period. |
| 500 Internal Server Erro | Server Erro. Contact support if persistent. |
| 503 Service Unavailable | Serviço de IA temporariamente indisponível. Retry with exponential backoff. |

## Erros

### Common Erro Responses

#### Missing Obrigatório Parâmetros
```json
{
  "error": "Validation failed",
  "message": "Required parameter 'data' is missing",
  "code": "MISSING_PARAMETER",
  "details": {
    "parameter": "data",
    "location": "body"
  }
}
```

**Solution:** Ensure all Obrigatório Parâmetros are provided in the Corpo de la Requisição.

#### Invalid Autenticação
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid y not expired. Check `X-Customer-Api-Id` e `X-Secret` Cabeçalhos.

## Como é Calculado

The CLV forecasting system uses probabilistic y regression models to predict future customer lifetime value:

### Algoritmo Principal

The system employs multiple modeling approaches for accurate CLV prediction:

- **Probabilistic Models:** BG/NBD (Beta-Geometric/Negative Binomial Distribution) for purchase frequency y Gamma-Gamma for monetary value
- **Machine Learning:** Gradient boosting (XGBoost, LightGBM) trained on customer features to predict future spend
- **Cohort Analysis:** Segments customers by acquisition cohort y applies cohort-specific models
- **Ensemble Prediction:** Combines multiple model outputs using weighted averaging for robust forecasts

### Passos de Processamento

1. **Feature Preparation:** Extract CLV-predictive features (RFM, tenure, engagement metrics)
2. **Seleção del Modelo:** Choose appropriate model based on data characteristics y business context
3. **Prediction Generation:** Apply trained model to generate CLV forecasts with confidence intervals
4. **Value Segmentation:** Categorize customers by predicted CLV (high/medium/low value)

### Desempenho

- **Tempo de Processamento:** 200-600ms for 10,000 customer CLV predictions
- **Requisitos de Dados:** Minimum 6 meses of transaction history per customer

## Typical Workflow

### Step 1: Prepare Data
Collect historical customer transaction data including purchase dates, amounts, customer IDs, y any available demographic or behavioral features. Ensure data quality by heling missing values y outliers.

### Step 2: Configure Parâmetros
Specify the prediction horizon (e.g., 12 meses), model Tipo (probabilistic vs ML), y any cohort segmentation criteria. Set confidence level for prediction intervals (Predeterminado: 95%).

### Step 3: Make Request
Send POST request with customer transaction data y configuration Parâmetros. The API will automatically select optimal models y generate CLV forecasts for each customer.

### Step 4: Analyze Results
Review predicted CLV values, confidence intervals, y customer value segments. Identify high-value customers for retention efforts y low-value customers for cost optimization.

### Step 5: Take Action
- **High CLV Customers:** Assign dedicated account managers, offer premium services, prioritize support
- **Medium CLV Customers:** Implement upsell/cross-sell campaigns, loyalty programs
- **Low CLV Customers:** Automate service delivery, optimize acquisition costs, consider churn risk

## Perguntas Frequentes

### Q: How accurate are CLV predictions?
**A:** Model accuracy varies by data quality y business Tipo. Typical R² scores range from 0.82-0.92, meaning the model explains 82-92% of CLV variance. Mean absolute Erro (MAE) is typically 8-12% of average CLV. Predictions are most accurate for customers with 6+ meses of history y stable purchase patterns.

### Q: What's the difference between predicted_clv y historical_value?
**A:** `historical_value` is the actual revenue generated by the customer to date. `predicted_clv` is the total forecasted lifetime value including both historical y future revenue. `predicted_future_value` is the difference between these two, representing expected future revenue over the forecast horizon.

### Q: How del I choose the right forecast_horizon?
**A:** Align forecast horizon with your business planning cycle. For monthly campaigns, use 6-12 meses. For annual budgeting, use 12-24 meses. For strategic planning, use 24-60 meses. Note that accuracy decreases with longer horizons—predictions beyond 24 meses should be treated as directional estimates.

### Q: Which model_type should I use?
**A:** Use `auto` for production (recommended)—it tests all algorithms y selects the best performer. For specific needs: `linear_regression` for interpretability, `reom_forest` for robustness, `xgboost` for maximum accuracy. The `selected_model` Campo in the Resposta shows which was chosen.

### Q: Why are confidence intervals important?
**A:** Confidence intervals show prediction uncertainty. A wide interval (e.g., $800-$2,200) indicates high uncertainty—use conservative lower bound for planning. A narrow interval (e.g., $1,400-$1,600) indicates high confidence—safe to use point estimate. Intervals help with risk assessment y scenario planning.

### Q: Can I retrain the model with my own historical CLV data?
**A:** The model is pre-trained on cross-industry data. For custom model training on your specific customer base, contact EchoIntel support. Custom models typically improve accuracy by 10-15% but require minimum 1,000 customers with known lifetime values spanning 12+ meses.

## Manuais Comerciais

### Playbook 1: VIP Customer Prioritization
**Objetivo:** Allocate 80% of retention budget to top 20% of customers by predicted CLV.

**Implementação:**
1. Run CLV forecast for entire customer base
2. Rank by `predicted_clv` descending
3. Identify top 20% (e.g., predicted_clv > $3,500)
4. Assign dedicated account managers to top 5%
5. Provide VIP perks: free shipping, early access, priority support

**Resultados Esperados:**
- 40-50% increase in retention for VIP segment
- 3-5x ROI on VIP program investment
- 25-30% increase in average order value from VIP customers

### Playbook 2: Customer Acquisition Cost (CAC) Optimization
**Objetivo:** Set maximum CAC based on predicted CLV to improve acquisition ROI.

**Implementação:**
1. Analyze CLV distribution by acquisition channel
2. Set CAC ceiling at 30% of average CLV per channel
3. Example: If email acquisition yields avg CLV of $2,000, max CAC = $600
4. Pause channels where CAC exceeds threshold
5. Double down on channels with CLV/CAC ratio > 3:1

**Resultados Esperados:**
- 20-30% improvement in customer acquisition ROI
- 15-25% reduction in unprofitable acquisition spend
- Better channel mix optimization

### Playbook 3: Upsell Campaign Targeting
**Objetivo:** Increase revenue by 20% through CLV-driven upsell targeting.

**Implementação:**
1. Identify customers with `predicted_future_value > $500` e `purchase_frequency >= 3`
2. Analyze `value_breakdown.short_term_3m` to time campaigns
3. Target upsell 7-10 dias before predicted next purchase
4. Personalize offer value: 10-15% of `avg_order_value`
5. Measure incremental lift vs. control group

**Resultados Esperados:**
- 25-35% upsell conversion rate
- 15-20% increase in average order value
- 5-8% overall revenue lift

### Playbook 4: Annual Budget Forecasting
**Objetivo:** Create accurate revenue forecasts using aggregate CLV predictions.

**Implementação:**
1. Run CLV forecast on active customer base (12-month horizon)
2. Sum `predicted_future_value` across all customers
3. Apply 15% conservative discount for uncertainty
4. Segment forecast by customer cohort (new, returning, VIP)
5. Compare quarterly actuals to forecast y adjust model

**Resultados Esperados:**
- Revenue forecast accuracy within 5-10%
- Improved budget allocation decisions
- Better investor/board communication with data-backed projections

### Playbook 5: Churn Prevention ROI Optimization
**Objetivo:** Maximize retention ROI by targeting high-CLV at-risk customers.

**Implementação:**
1. Combine CLV forecast with churn risk scores
2. Filter customers with `predicted_clv > $1,500` y high churn risk
3. Calculate maximum acceptable retention cost: `0.3 * predicted_future_value`
4. Deploy tiered retention offers within budget constraints
5. Monitor win-back rate y actual vs. predicted CLV

**Resultados Esperados:**
- 30-40% reduction in high-value churn
- 2-4x ROI on retention campaigns
- $500-$1,500 saved per retained high-CLV customer

## Relacionado

- [CLV Features](CustomerClvFeatures.md) - Extract features for CLV modeling
- [Customer RFM](CustomerRFM.md) - RFM analysis for value-based segmentation
- [Customer Segmentation](CustomerSegmentation.md) - Machine learning customer segmentation
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Product purchase propensity scoring
- [Recommend User Items](../Recommendations/RecommendUserItems.md) - Personalized product recommendations

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:155`
