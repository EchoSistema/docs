# Artificial Intelligence – CLV Features

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/clv-features
```

Extracts engineered features from customer data for Customer Lifetime Value (CLV) prediction modeling. This Endpoint prepares behavioral, transactional, y demographic features that serve as inputs for CLV forecasting algorithms.

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
| customer_data | array | Sí | array of customer registros with transaction history. | Customer profiles with purchase behavior data. | - |
| feature_set | string | No | Feature group to generate: `basic`, `advanced`, `all`. | Determines complexity of feature engineering. | `all` |
| time_window | integer | No | Rolling window in dias for temporal features (30-365). | Analysis period for trend calculation. | `90` |
| include_metadata | boolean | No | Include feature engineering metadata y descriptions. | Adds explanations for each computed feature. | `false` |

### Customer Data object Fields

| Campo | Tipo | Requerido | Descripción | Significado Empresarial | Example |
| ----- | ---- | -------- | ----------- | ---------------- | ------- |
| customer_id | string | Sí | Unique customer identifier. | Customer account ID or email. | `"CUST-10234"` |
| transactions | array | Sí | array of transaction registros. | Purchase history with dates y amounts. | See below |
| join_date | string | No | Customer registration date (ISO 8601). | Account creation date for tenure calculation. | `"2023-01-15"` |
| demographics | object | No | Demographic attributes (age, location, segment). | Customer profile information. | `{"age": 34, "country": "US"}` |

### Transaction object Fields

| Campo | Tipo | Requerido | Descripción | Example |
| ----- | ---- | -------- | ----------- | ------- |
| transaction_id | string | Sí | Unique transaction identifier. | `"TXN-789012"` |
| date | string | Sí | Transaction date (ISO 8601). | `"2025-10-15"` |
| amount | float | Sí | Transaction amount in currency units. | `149.99` |
| product_category | string | No | Product category or SKU. | `"Electronics"` |
| channel | string | No | Purchase channel: `online`, `mobile`, `store`. | `"online"` |

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
        "join_date": "2023-01-15",
        "transactions": [
          {"transaction_id": "TXN-001", "date": "2025-10-15", "amount": 149.99, "product_category": "Electronics"},
          {"transaction_id": "TXN-002", "date": "2025-09-20", "amount": 79.50, "product_category": "Accessories"},
          {"transaction_id": "TXN-003", "date": "2025-08-10", "amount": 299.00, "product_category": "Electronics"}
        ],
        "demographics": {"age": 34, "country": "US"}
      }
    ],
    "feature_set": "all",
    "time_window": 90,
    "include_metadata": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/clv-features"
```

### Exemplo de Requisição (Python)

```python
import requests

url = "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/clv-features"
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
        "join_date": "2023-01-15",
        "transactions": [
            {"transaction_id": "TXN-001", "date": "2025-10-15", "amount": 149.99, "product_category": "Electronics"},
            {"transaction_id": "TXN-002", "date": "2025-09-20", "amount": 79.50, "product_category": "Accessories"},
            {"transaction_id": "TXN-003", "date": "2025-08-10", "amount": 299.00, "product_category": "Electronics"}
        ],
        "demographics": {"age": 34, "country": "US"}
    }],
    "feature_set": "all",
    "time_window": 90,
    "include_metadata": True
}

response = requests.post(url, headers=headers, json=payload)
result = response.json()
```

## Respuesta

### Éxito `200 OK`

```json
{
  "features": [
    {
      "customer_id": "CUST-10234",
      "transactional_features": {
        "total_revenue": 528.49,
        "average_order_value": 176.16,
        "purchase_frequency": 3,
        "recency_days": 25,
        "monetary_value": 528.49,
        "time_between_purchases_avg": 20.0
      },
      "temporal_features": {
        "customer_tenure_days": 663,
        "purchase_velocity_30d": 1,
        "purchase_velocity_60d": 2,
        "purchase_velocity_90d": 3,
        "trend_coefficient": 0.15
      },
      "behavioral_features": {
        "product_diversity": 2,
        "category_concentration": 0.67,
        "primary_channel": "online",
        "discount_sensitivity": 0.0,
        "return_rate": 0.0
      },
      "rfm_scores": {
        "recency_score": 5,
        "frequency_score": 3,
        "monetary_score": 4,
        "rfm_segment": "Champions"
      },
      "clv_indicators": {
        "predicted_lifetime_months": 36,
        "estimated_future_value": 2115.96,
        "churn_risk_score": 0.15
      }
    }
  ],
  "metadata": {
    "feature_engineering_version": "v3.2.1",
    "total_features_generated": 24,
    "feature_groups": ["transactional", "temporal", "behavioral", "rfm"],
    "computation_time_ms": 187
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "customer_data must contain at least one customer record with transaction history."
}
```

### Error `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid customer data structure",
  "details": {
    "customer_data.0.transactions": "Each customer must have at least 2 transactions for feature extraction."
  }
}
```

## Estructura JSON

| Campo | Tipo | Descripción | Significado Empresarial |
| ----- | ---- | ----------- | ---------------- |
| `features` | array | array of feature sets, one per customer. | Processed customer behavioral profiles. |
| `features[].customer_id` | string | Customer identifier. | Links features to specific customer. |
| `features[].transactional_features` | object | Purchase-based metrics. | Direct transaction behavior indicators. |
| `features[].transactional_features.total_revenue` | float | Sum of all transaction amounts. | Total customer spending to date. |
| `features[].transactional_features.average_order_value` | float | Mean transaction amount. | Average basket size per purchase. |
| `features[].transactional_features.purchase_frequency` | integer | Count of transactions. | How often customer purchases. |
| `features[].transactional_features.recency_dias` | integer | Days since last purchase. | Purchase recency indicator. |
| `features[].transactional_features.monetary_value` | float | Total spending (same as total_revenue). | Monetary contribution metric. |
| `features[].transactional_features.time_between_purchases_avg` | float | Average dias between consecutive purchases. | Purchase cadence indicator. |
| `features[].temporal_features` | object | Time-based behavior patterns. | Trends y velocity metrics. |
| `features[].temporal_features.customer_tenure_dias` | integer | Days since customer registration. | Customer relationship age. |
| `features[].temporal_features.purchase_velocity_30d` | integer | Purchases in last 30 dias. | Short-term activity level. |
| `features[].temporal_features.purchase_velocity_60d` | integer | Purchases in last 60 dias. | Medium-term activity level. |
| `features[].temporal_features.purchase_velocity_90d` | integer | Purchases in last 90 dias. | Long-term activity level. |
| `features[].temporal_features.trend_coefficient` | float | Linear trend slope of purchase amounts. | Spending growth or decline rate. |
| `features[].behavioral_features` | object | Customer behavior patterns. | Qualitative behavior indicators. |
| `features[].behavioral_features.product_diversity` | integer | Count of unique product categories purchased. | Breadth of product interest. |
| `features[].behavioral_features.category_concentration` | float | Concentration ratio in top category (0-1). | Purchase focus vs. diversity. |
| `features[].behavioral_features.primary_channel` | string | Most frequently used purchase channel. | Preferred shopping method. |
| `features[].behavioral_features.discount_sensitivity` | float | Ratio of discounted purchases (0-1). | Price sensitivity indicator. |
| `features[].behavioral_features.return_rate` | float | Product return ratio (0-1). | Return behavior metric. |
| `features[].rfm_scores` | object | RFM segmentation scores. | Classic customer value segmentation. |
| `features[].rfm_scores.recency_score` | integer | Recency score (1-5, 5 is best). | How recently customer purchased. |
| `features[].rfm_scores.frequency_score` | integer | Frequency score (1-5, 5 is best). | Purchase frequency rating. |
| `features[].rfm_scores.monetary_score` | integer | Monetary score (1-5, 5 is best). | Spending amount rating. |
| `features[].rfm_scores.rfm_segment` | string | RFM segment label. | Customer value classification. |
| `features[].clv_indicators` | object | CLV predictive indicators. | Forward-looking value metrics. |
| `features[].clv_indicators.predicted_lifetime_meses` | integer | Expected customer lifetime duration. | Predicted retention period. |
| `features[].clv_indicators.estimated_future_value` | float | Forecasted future revenue from customer. | Expected future contribution. |
| `features[].clv_indicators.churn_risk_score` | float | Probability of churn (0-1). | Risk of customer leaving. |
| `metadata` | object | Feature engineering metadata. | Processing information y versioning. |
| `metadata.feature_engineering_version` | string | Version of feature engineering pipeline. | Algorithm version for reproducibility. |
| `metadata.total_features_generated` | integer | Count of features computed. | Number of derived attributes. |
| `metadata.feature_groups` | array | Categories of features generated. | Feature taxonomy. |
| `metadata.computation_time_ms` | integer | Duração del processamento em milissegundos. | Métrica de desempenho. |

## Como é Calculado

The CLV feature engineering system extracts predictive features from customer data for lifetime value modeling:

### Algoritmo Principal

The system computes statistical y behavioral features that correlate with customer lifetime value:

- **Transactional Features:** Total purchases, average order value, purchase frequency, recency
- **Temporal Features:** Customer tenure, dias since first purchase, purchase velocity trends
- **Behavioral Features:** Product diversity, category preferences, discount sensitivity
- **Engagement Features:** Website visits, email opens, support interactions, loyalty program activity

### Passos de Processamento

1. **Data Aggregation:** Collect customer transactional, behavioral, y demographic data
2. **Engenharia de Características:** Calculate derived features (e.g., 30/60/90-day purchase trends)
3. **Normalization:** Steardize features using z-score or min-max scaling
4. **Feature Selection:** Rank features by correlation with actual CLV using mutual information

### Feature Categories

**Transactional Features (7 features):**
- Total revenue, average order value, purchase count
- Recency, frequency, monetary value (RFM components)
- Inter-purchase time statistics

**Temporal Features (5 features):**
- Customer tenure (dias since registration)
- Purchase velocity across multiple windows (30/60/90 dias)
- Trend analysis (spending growth/decline rate)

**Behavioral Features (6 features):**
- Product category diversity
- Channel preference (online, mobile, store)
- Discount usage patterns
- Return behavior
- Shopping cart characteristics

**RFM Segmentation (4 features):**
- Recency score (1-5 scale)
- Frequency score (1-5 scale)
- Monetary score (1-5 scale)
- Segment classification (Champions, Loyal, At Risk, etc.)

**CLV Indicators (3 features):**
- Predicted customer lifetime duration
- Estimated future value
- Churn risk probability

### Desempenho

- **Tempo de Processamento:** 150-400ms for 5,000 customers with full feature extraction
- **Requisitos de Dados:** Minimum 3 meses of customer transaction history
- **Tamanho del Lote:** Process up to 50,000 customers per request
- **Memory Footprint:** ~2MB per 1,000 customers

## Status HTTP

| Código | Descripción |
|------|-------------|
| 200  | Sucesso - Features generated successfully |
| 400  | Bad Request - Missing or invalid Parâmetros |
| 401  | Unauthorized - Token de autenticação inválido o faltante |
| 403  | Forbidden - Permissões insuficientes o tenant inválido |
| 422  | Unprocessable Entity - Validation Erros in customer data |
| 429  | Too Many Requests - Limite de taxa excedido |
| 500  | Internal Server Erro - Feature extraction failure |

## Erros

Common Erro scenarios:

**Insufficient Transaction History:**
```json
{
  "error": "Insufficient data",
  "message": "Each customer must have at least 2 transactions for meaningful feature extraction.",
  "customer_id": "CUST-10234"
}
```

**Invalid Date Format:**
```json
{
  "error": "Validation failed",
  "message": "Invalid date format",
  "details": {
    "customer_data.0.transactions.1.date": "Date must be in ISO 8601 format (YYYY-MM-DD)."
  }
}
```

**Campos Obrigatórios Faltantes:**
```json
{
  "error": "Validation failed",
  "message": "Required fields missing",
  "details": {
    "customer_data.0.customer_id": "The customer_id field is required.",
    "customer_data.0.transactions": "The transactions field is required and must be an array."
  }
}
```

## Notas

### Feature Engineering Melhores Práticas

- **Minimum Data:** At least 2 transactions per customer (3+ recommended for robust features)
- **Historical Window:** 90-day window captures seasonal patterns; 365 dias for annual trends
- **Qualidade de Dados:** Ensure transaction dates are accurate y amounts are positive
- **Demographic Data:** Optional but improves feature quality when available

### Feature Set Options

| Feature Set | Features Included | Use Case | Processing Time |
|------------|-------------------|----------|-----------------|
| `basic` | Transactional + RFM (11 features) | Quick CLV estimates, dashboards | ~80ms/5K customers |
| `advanced` | + Temporal + Behavioral (22 features) | ML model training, segmentation | ~180ms/5K customers |
| `all` | + CLV Indicators (24 features) | Comprehensive analysis, research | ~250ms/5K customers |

### Integration Patterns

**CLV Prediction Pipeline:**
1. Extract features using this Endpoint
2. Feed features to [CLV Forecast](CustomerClvForecast.md) Endpoint
3. Use predictions for customer segmentation y targeting

**Churn Prevention:**
1. Generate features for active customer base
2. Filter customers with `churn_risk_score > 0.6`
3. Create retention campaigns for high-risk segments

**Marketing Personalization:**
1. Use `behavioral_features` to identify product preferences
2. Target cross-sell campaigns based on `product_diversity`
3. Optimize discount strategies using `discount_sensitivity`

## Perguntas Frequentes

### Q: What's the minimum transaction history Obrigatório for reliable features?
**A:** We recommend at least 2 transactions per customer, but 3+ transactions yield more reliable temporal features (velocity, trend). For robust CLV prediction, aim for 6+ meses of transaction history. Customers with only 1 transaction will have limited features (Não frequency, velocity, or trend metrics).

### Q: How are missing or null demographic fields heled?
**A:** Demographic fields (age, location) are optional y del not affect core feature extraction. If provided, they're passed through in the Resposta for downstream model usage. Missing demographics don't impact transactional, temporal, or behavioral feature computation.

### Q: Can I use these features directly for CLV prediction?
**A:** Sim! These features are designed as inputs for CLV forecasting models. Use the `all` feature set for comprehensive modeling. The features are pre-normalized y ready for machine learning algorithms. For automated CLV prediction, use the [CLV Forecast](CustomerClvForecast.md) Endpoint, which internally uses these features.

### Q: What does the trend_coefficient represent y how is it calculated?
**A:** The trend coefficient measures the linear growth or decline in customer spending over time. It's calculated using ordinary least squares regression on transaction amounts against transaction dates. Positive values (e.g., 0.15) indicate increasing spending; negative values indicate declining spending. A value of 0 suggests flat spending. This metric helps identify customers with growing engagement vs. those losing interest.

### Q: How often should I refresh customer features?
**A:** For real-time applications (e.g., personalization engines), refresh features weekly or when significant customer actions occur (new purchase, large order). For monthly reporting y batch campaigns, refresh mensal. Features are stateless—each API call computes fresh features from provided transaction data without caching.

### Q: Can I process customers with different currencies?
**A:** Sim, but ensure all amounts for a given customer are in the same currency. The system doesn't perform currency conversion—it treats all amounts as numeric values. For multi-currency customers, pre-convert transactions to a single base currency (e.g., USD) before submission. Currency metadata isn't stored in features.

## Manuais Comerciais

### Playbook 1: High-Value Customer Identification
**Objetivo:** Identify top 10% customers by predicted lifetime value for VIP program enrollment.

**Implementação:**
1. Extract features for entire customer base using `feature_set: "all"`
2. Rank customers by `clv_indicators.estimated_future_value`
3. Filter top 10% (e.g., estimated_future_value > $5,000)
4. Cross-check with `rfm_segment` (Champions, Loyal Customers)
5. Enroll in VIP program with exclusive benefits

**Resultados Esperados:**
- 30-40% increase in retention for VIP segment
- 2-3x higher purchase frequency vs. general population
- 15-20% revenue growth from targeted upsell campaigns

### Playbook 2: Churn Prevention Campaign
**Objetivo:** Reduce churn by 25% through early intervention on at-risk customers.

**Implementação:**
1. Generate features for all active customers
2. Filter customers with `churn_risk_score > 0.6` e `recency_dias > 45`
3. Segment by `average_order_value` (high-value vs. low-value)
4. Deploy personalized retention offers:
   - High-value: 20% discount + free shipping
   - Low-value: 10% discount + loyalty points bonus
5. Monitor conversion within 14 dias

**Resultados Esperados:**
- 25-35% of at-risk customers reactivate
- 15-20% reduction in overall churn rate
- ROI of 3-5x on retention spend

### Playbook 3: Product Recommendation Optimization
**Objetivo:** Increase cross-sell conversion by 40% using behavioral features.

**Implementação:**
1. Extract features focusing on `behavioral_features.product_diversity` e `category_concentration`
2. Identify customers with low diversity (<3 categories) y high frequency (5+ purchases)
3. Recommend complementary products from underrepresented categories
4. Personalize messaging based on `primary_channel` (email for online, SMS for mobile)
5. A/B test generic vs. feature-based recommendations

**Resultados Esperados:**
- 40-50% increase in cross-category purchases
- 10-15% higher average order value
- 25-30% improvement in email/SMS engagement rates

### Playbook 4: Win-Back Campaign for Dormant Customers
**Objetivo:** Reactivate 15% of dormant customers (Não purchase in 90+ dias).

**Implementação:**
1. Filter customers with `recency_dias > 90` e `purchase_frequency >= 3`
2. Segment by historical `average_order_value` e `monetary_value`
3. Create tiered win-back offers:
   - High spenders (AOV > $150): $50 voucher
   - Medium spenders (AOV $50-$150): $20 voucher
   - Low spenders (AOV < $50): 15% discount
4. Send multi-touch campaign (email → SMS → push notification)
5. Track reactivation within 30 dias

**Resultados Esperados:**
- 15-20% of dormant customers make a purchase
- 60-70% of reactivated customers make 2+ purchases in next 90 dias
- 8-12% increase in quarterly revenue

### Playbook 5: Discount Strategy Optimization
**Objetivo:** Reduce unnecessary discounting by 30% while maintaining conversion rates.

**Implementação:**
1. Generate features with focus on `discount_sensitivity` metric
2. Segment customers into 3 groups:
   - Low sensitivity (<0.2): Rarely uses discounts
   - Medium sensitivity (0.2-0.6): Occasional discount users
   - High sensitivity (>0.6): Primarily discount-driven
3. Adjust discount strategy:
   - Low: Não discounts, focus on value messaging
   - Medium: Occasional 10-15% offers on birthdias/holidias
   - High: Maintain current discount levels
4. Monitor conversion rates y AOV by segment

**Resultados Esperados:**
- 30-40% reduction in discount spend on low-sensitivity customers
- Maintained or improved conversion rates
- 5-10% increase in gross margins
- More efficient marketing budget allocation

## Relacionado

- [CLV Forecast](CustomerClvForecast.md) - Predict future customer lifetime value
- [Customer Features](CustomerFeatures.md) - Extract general customer features
- [Customer RFM](CustomerRFM.md) - RFM analysis for customer segmentation
- [Customer Segmentation](CustomerSegmentation.md) - Machine learning-based customer segmentation
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Predict product purchase likelihood

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:150`
