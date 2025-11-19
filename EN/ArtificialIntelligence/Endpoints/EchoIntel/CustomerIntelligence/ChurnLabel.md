# Artificial Intelligence – Churn Labeling

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-label
```

Automatically generates churn labels for customer datasets by applying rule-based classification algorithms that identify churned customers based on behavioral patterns, subscription status, and engagement metrics.

**Business Value:** Automates creation of training data for churn prediction models, reducing manual labeling time from weeks to minutes. Organizations achieve 85-92% labeling accuracy, enabling effective churn prediction model development that reduces customer attrition by 20-35%.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-caracteres of segredo. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Tipo       | string | Yes         | `application/json`. |

## Parameters

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo of the Requisição Parâmetros

| Parameter | Type | Required | Padrão | Description | Significado Empresarial |
|-----------|------|----------|---------|-------------|------------------|
| customer_data | array | Yes | - | array of customer activity registros with timestamps and behavioral metrics. | Historical customer interaction data to analyze for churn patterns. |
| churn_definition | object | No | Auto | Customizable rules defining what constitutes churn for your business model. | Business-specific criteria: inactivity period, cancellation events, engagement thresholds. |
| lookback_period | integer | No | 180 | Days to look back for customer activity history. | How far back to analyze behavior (e.g., 180 dias = 6 meses). |
| inactivity_threshold | integer | No | 90 | Days of inactivity after which customer is labeled as churned. | Time without engagement that indicates abeonment (e.g., 90 dias for SaaS). |
| include_reactivations | boolean | No | false | Track customers who churned then returned (useful for win-back analysis). | Identifies customers who left but came back, for retention pattern analysis. |
| min_tenure | integer | No | 30 | Minimum dias as customer before churn labeling applies (filters early dropouts). | Excludes very new customers who never properly onboarded. |

### Customer Data object Fields

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| customer_id | string | Yes | Unique customer identifier. | `"C12345"` |
| signup_date | string | Yes | ISO 8601 date when customer signed up. | `"2024-01-15"` |
| last_activity | string | Yes | ISO 8601 date of most recent customer activity. | `"2024-11-01"` |
| subscription_status | string | No | Current subscription state: `active`, `cancelled`, `suspended`, `expired`. | `"active"` |
| total_transactions | integer | No | Total number of transactions/interactions. | `45` |
| lifetime_value | float | No | Total revenue from customer. | `1250.00` |
| cancellation_date | string | No | ISO 8601 date when customer explicitly canceled (if applicable). | `"2024-10-15"` |

## Examples

### Exemplo of Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_data": [
      {
        "customer_id": "C12345",
        "signup_date": "2024-01-15",
        "last_activity": "2024-11-01",
        "subscription_status": "active",
        "total_transactions": 45,
        "lifetime_value": 1250.00
      },
      {
        "customer_id": "C67890",
        "signup_date": "2024-02-20",
        "last_activity": "2024-06-30",
        "subscription_status": "cancelled",
        "total_transactions": 12,
        "lifetime_value": 350.00,
        "cancellation_date": "2024-07-01"
      }
    ],
    "lookback_period": 180,
    "inactivity_threshold": 90,
    "include_reactivations": true,
    "min_tenure": 30
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-label"
```

### Exemplo of Requisição (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-label', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    customer_data: [
      {
        customer_id: 'C12345',
        signup_date: '2024-01-15',
        last_activity: '2024-11-01',
        subscription_status: 'active',
        total_transactions: 45,
        lifetime_value: 1250.00
      },
      {
        customer_id: 'C67890',
        signup_date: '2024-02-20',
        last_activity: '2024-06-30',
        subscription_status: 'cancelled',
        total_transactions: 12,
        lifetime_value: 350.00,
        cancellation_date: '2024-07-01'
      }
    ],
    lookback_period: 180,
    inactivity_threshold: 90,
    include_reactivations: true,
    min_tenure: 30
  })
});

const result = await response.json();
```

### Exemplo of Requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'en',
])->post('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/churn-label', [
    'customer_data' => [
        [
            'customer_id' => 'C12345',
            'signup_date' => '2024-01-15',
            'last_activity' => '2024-11-01',
            'subscription_status' => 'active',
            'total_transactions' => 45,
            'lifetime_value' => 1250.00,
        ],
        [
            'customer_id' => 'C67890',
            'signup_date' => '2024-02-20',
            'last_activity' => '2024-06-30',
            'subscription_status' => 'cancelled',
            'total_transactions' => 12,
            'lifetime_value' => 350.00,
            'cancellation_date' => '2024-07-01',
        ],
    ],
    'lookback_period' => 180,
    'inactivity_threshold' => 90,
    'include_reactivations' => true,
    'min_tenure' => 30,
]);

$result = $response->json();
```

## Response

### Success `200 OK`

```json
{
  "labeled_customers": [
    {
      "customer_id": "C12345",
      "churn_label": 0,
      "churn_status": "active",
      "days_since_last_activity": 8,
      "tenure_days": 290,
      "label_confidence": "high",
      "label_reason": "Active customer with recent engagement within 30 days"
    },
    {
      "customer_id": "C67890",
      "churn_label": 1,
      "churn_status": "churned",
      "churn_date": "2024-07-01",
      "days_since_last_activity": 132,
      "tenure_days": 131,
      "label_confidence": "high",
      "label_reason": "Explicit cancellation event and 132 days of inactivity"
    }
  ],
  "summary": {
    "total_customers": 2,
    "churned_count": 1,
    "active_count": 1,
    "churn_rate": 0.50,
    "avg_tenure_churned": 131,
    "avg_tenure_active": 290
  },
  "labeling_rules_applied": {
    "inactivity_threshold": 90,
    "min_tenure": 30,
    "explicit_cancellations": true,
    "reactivation_tracking": true
  },
  "data_quality": {
    "missing_last_activity": 0,
    "missing_signup_date": 0,
    "tenure_too_short": 0,
    "valid_records": 2
  }
}
```

## Campo Reference

| Field | Type | Description | Significado Empresarial |
|-------|------|-------------|------------------|
| `labeled_customers` | array | array of customers with assigned churn labels. | Each customer record with churn classification for model training. |
| `labeled_customers[].customer_id` | string | Unique customer identifier. | Matches input customer ID for tracking. |
| `labeled_customers[].churn_label` | integer | Binary churn label: 0 = active, 1 = churned. | Training label for supervised machine learning models. |
| `labeled_customers[].churn_status` | string | Human-readable status: `active`, `churned`, `at_risk`. | Interpretable churn state for business users. |
| `labeled_customers[].churn_date` | string | ISO 8601 date when churn occurred (if churned). | When customer abeoned service (for survival analysis). |
| `labeled_customers[].dias_since_last_activity` | integer | Days between last activity and analysis date. | Recency metric: higher values indicate disengagement. |
| `labeled_customers[].tenure_dias` | integer | Total dias as customer (signup to last activity or churn). | Customer lifetime: longer tenure often means higher value. |
| `labeled_customers[].label_confidence` | string | Confidence in label assignment: `high`, `medium`, `low`. | Data quality indicator: high = clear churn signal, low = ambiguous. |
| `labeled_customers[].label_reason` | string | Explanation for why churn label was assigned. | Transparency for validation and rule tuning. |
| `summary` | object | Aggregate statistics across all labeled customers. | Overall churn rate and dataset health metrics. |
| `summary.total_customers` | integer | Total number of customers processed. | Dataset size for statistical significance assessment. |
| `summary.churned_count` | integer | Number of customers labeled as churned. | How many churn cases identified (positive class). |
| `summary.active_count` | integer | Number of customers labeled as active. | How many retained customers (negative class). |
| `summary.churn_rate` | float | Proportion of churned customers (0-1). | Overall attrition rate: 0.20 = 20% churn rate. |
| `summary.avg_tenure_churned` | float | Average tenure dias for churned customers. | How long churned customers stayed before leaving. |
| `summary.avg_tenure_active` | float | Average tenure dias for active customers. | How long current active customers have been with you. |
| `labeling_rules_applied` | object | Configuration rules used for labeling. | Documentation of labeling logic for reproducibility. |
| `data_quality` | object | Data quality metrics and validation results. | Identifies missing data or invalid registros. |
| `data_quality.missing_last_activity` | integer | Count of registros without last_activity date. | Records excluded due to missing critical data. |
| `data_quality.tenure_too_short` | integer | Customers below min_tenure threshold (excluded). | New customers filtered out (haven't had time to churn). |
| `data_quality.valid_registros` | integer | Number of successfully labeled registros. | Usable training data volume. |

## Status HTTP

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns churn labeling results. |
| 400 Bad Request | Invalid request Parâmetros. Check Parâmetro types and Obrigatório fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Resposta for details. |
| 429 Too Many Requests | Limite of taxa excedido. Retry after cooldown period. |
| 500 Internal Server Erro | Server Erro. Contact support if persistent. |
| 503 Service Unavailable | Serviço of IA temporariamente indisponível. Retry with exponential backoff. |

## Erros

### Common Erro Responses

#### Missing Obrigatório Parâmetros
```json
{
  "error": "Validation failed",
  "message": "Required parameter 'customer_data' is missing",
  "code": "MISSING_PARAMETER",
  "details": {
    "parameter": "customer_data",
    "location": "body"
  }
}
```

**Solution:** Ensure all Obrigatório Parâmetros are provided in the Corpo of the Requisição.

#### Invalid Autenticação
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid and not expired. Check `X-Customer-Api-Id` e `X-Secret` Cabeçalhos.

#### Insufficient Data
```json
{
  "error": "Validation failed",
  "message": "Insufficient customer data for labeling",
  "code": "INSUFFICIENT_DATA",
  "details": {
    "minimum_required": 10,
    "provided": 5,
    "recommendation": "Provide at least 10 customer records for meaningful churn labeling"
  }
}
```

**Solution:** Submit more customer registros to reach minimum threshold.

## Como é Calculado

The churn labeling system uses rule-based classification to create training labels from historical customer data:

### Algoritmo Principal

The system applies business rules to identify churned customers based on behavioral patterns:

- **Inactivity Detection:** Labels customers as churned if Não activity for defined period (e.g., 90 dias)
- **Engagement Metrics:** Evaluates declining engagement trends (reduced logins, purchases, interactions)
- **Subscription Status:** Identifies explicit churn events (cancellations, non-renewals)
- **Predictive Indicators:** Flags early churn signals based on behavior changes

### Passos of Processamento

1. **Data Collection:** Aggregate customer transaction, engagement, and subscription data
2. **Rule Application:** Apply configurable business rules to identify churn events and dates
3. **Label Assignment:** Create binary labels (churned=1, active=0) with churn timestamps
4. **Validation:** Verify label consistency and hele edge cases (reactivations, seasonality)

### Labeling Logic

**Rule 1: Explicit Cancellation**
- If `subscription_status = 'cancelled'` or `cancellation_date` exists → churn_label = 1
- Churn date = cancellation_date

**Rule 2: Inactivity-Based Churn**
- If dias_since_last_activity > inactivity_threshold → churn_label = 1
- Churn date = last_activity + inactivity_threshold dias

**Rule 3: Active Customer**
- If dias_since_last_activity <= 30 AND subscription_status = 'active' → churn_label = 0

**Rule 4: Minimum Tenure Filter**
- If tenure_dias < min_tenure → exclude from labeling (too early to assess churn)

**Rule 5: Reactivation Tracking**
- If include_reactivations = true AND customer had prior churn but returned → flag as reactivation

### Confidence Scoring

- **High Confidence:** Explicit cancellation OR >120 dias inactivity
- **Medium Confidence:** 90-120 dias inactivity with declining engagement
- **Low Confidence:** Edge cases (60-90 dias inactivity, mixed signals)

### Desempenho

- **Tempo of Processamento:** 100-300ms for 10,000 customers
- **Requisitos of Dados:** Minimum 6 meses of customer activity history
- **Labeling Precisão:** 85-92% agreement with manual expert labeling

## Typical Workflow

### 1. Extract Customer Data

Gather customer activity registros from your database:

```sql
-- Example: Extract customer activity for churn labeling
SELECT
  customer_id,
  signup_date,
  MAX(activity_date) AS last_activity,
  current_subscription_status AS subscription_status,
  COUNT(transaction_id) AS total_transactions,
  SUM(transaction_amount) AS lifetime_value,
  cancellation_date
FROM customers c
LEFT JOIN transactions t ON c.customer_id = t.customer_id
WHERE signup_date <= DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY)  -- Min 30 days tenure
GROUP BY customer_id
```

### 2. Configure Labeling Rules

Define churn criteria based on your business model:

- **SaaS/Subscriptions:** inactivity_threshold = 90 dias
- **E-commerce:** inactivity_threshold = 180 dias (seasonal buyers)
- **Mobile Apps:** inactivity_threshold = 60 dias (faster churn cycles)
- **Financial Services:** inactivity_threshold = 365 dias (long-tail retention)

### 3. Submit Labeling Request

Send customer data to the API with appropriate thresholds.

### 4. Validate Labeled Data

Review labeling results for quality:

- Check `label_confidence` distribution (target: >80% high confidence)
- Verify `churn_rate` matches business expectations (typical: 5-30% annually)
- Inspect low-confidence cases for manual review
- Validate `label_reason` explanations make business sense

### 5. Use Labels for Model Training

Export labeled dataset for churn prediction model development:

```python
# Example: Prepare training data for churn prediction model
import pandas as pd

# Load labeled data from API response
labeled_df = pd.DataFrame(api_response['labeled_customers'])

# Filter high-confidence labels for training
training_data = labeled_df[labeled_df['label_confidence'] == 'high']

# Export for model training
training_data[['customer_id', 'churn_label']].to_csv('churn_training_labels.csv', index=False)
```

### 6. Iterate and Refine

Monitor labeling quality and adjust rules:

- Compare predicted churn vs. actual churn for accuracy
- Adjust `inactivity_threshold` based on false positives/negatives
- Inbodyrate domain expertise into custom `churn_definition` rules
- Re-label periodically as business conditions change

## Perguntas Frequentes

### Q: What inactivity threshold should I use for my business?

**A:** It depends on your industry and customer behavior patterns. SaaS/subscriptions typically use 60-90 dias. E-commerce can use 120-180 dias due to seasonal purchasing. Mobile apps often use 30-60 dias due to faster engagement cycles. B2B services may use 180-365 dias. Start with industry benchmarks, then validate: if 90 dias yields to churn_rate of 10-20%, it's likely appropriate. If churn_rate is 50%+, your threshold may be too short. Analyze your customer activity distribution to find natural engagement gaps.

### Q: How of the I hele customers who cancel but continue using the service?

**A:** Use the explicit cancellation date as the primary signal. If `cancellation_date` exists, label as churned regardless of `last_activity`. However, track these cases separately via `include_reactivations: true` to identify "churned but engaged" patterns. These customers may be using free tiers or have grace periods. For accurate modeling, create to custom `churn_definition` rule that considers both cancellation events and actual usage cessation.

### Q: What's the minimum dataset size for reliable labeling?

**A:** Minimum 100 customer registros for basic labeling, but 1,000+ is recommended for statistical reliability. The API accepts 10-1,000,000 registros per request. More important than raw count is data quality: ensure 6+ meses of activity history per customer, accurate signup dates, and reliable last_activity timestamps. If you have <100 registros, the churn_rate metric may be unstable. For model training, aim for at least 100 churn cases (positive labels) for meaningful pattern detection.

### Q: How of the I hele seasonal businesses with irregular customer activity?

**A:** Adjust `inactivity_threshold` to account for your longest natural gap between purchases. For seasonal retail (e.g., holiday-focused), use 365+ dias inactivity threshold. Alternatively, create custom `churn_definition` rules that inbodyrate seasonal patterns: e.g., "churned if Não purchase in last 2 seasonal cycles." Use `lookback_period` to capture full seasonal cycles (e.g., 730 dias for 2 years). Consider labeling separately for peak vs. off-peak customer cohorts.

### Q: What does label_confidence mean and how should I use it?

**A:** `label_confidence` indicates certainty of the churn label assignment. High confidence = unambiguous churn signal (explicit cancellation, 120+ dias inactivity). Medium = likely churned but some ambiguity (90-120 dias inactivity). Low = edge cases requiring manual review. For model training, use only high-confidence labels to reduce noise. Medium-confidence cases can be reviewed manually or excluded. Track confidence distribution: if <70% are high-confidence, your labeling rules may need tuning.

### Q: Can I use this for real-time churn detection?

**A:** Não, this Endpoint is for batch labeling of historical data to create training datasets. For real-time churn risk scoring, use the [Churn Risk](ChurnRisk.md) Endpoint which predicts churn probability for active customers. The workflow is: (1) Use Churn Labeling to create training data from historical customers, (2) Train to churn prediction model with those labels, (3) Use Churn Risk Endpoint to score current customers in real-time. This Endpoint processes 10,000 customers in ~300ms, suitable for daily batch labeling.

### Q: How of the I hele customers who churn and then return (reactivations)?

**A:** Enable `include_reactivations: true` to track customers who churned then came back. The Resposta flags these customers with special notation in `label_reason`. For model training, you have two options: (1) Exclude reactivations to train a "first churn" prediction model, or (2) Include them with time-based features to train a "win-back Sucesso" model. Reactivations complicate simple binary labeling, so consider creating separate datasets for different prediction goals (churn prevention vs. win-back).

### Q: What if I don't have explicit cancellation dates for all churned customers?

**A:** The system heles this gracefully by using inactivity-based churn detection as to fallback. If `cancellation_date` is missing, the algorithm calculates `dias_since_last_activity` and compares to `inactivity_threshold`. This hybrid approach (explicit cancellations + inactivity rules) captures both voluntary churn (cancellations) and silent churn (gradual disengagement). For best results, populate cancellation_date when available, but missing values won't block labeling.

## Manuais Comerciais

| Use Case | Action | Expected Impact |
|----------|--------|-----------------|
| **Churn Prediction Model Training** | Use labeled dataset (churn_label=1 for churned, 0 for active) to train supervised ML models. Filter for high-confidence labels only (label_confidence='high'). Split data 70/30 train/test. Train XGBoost or Reom Forest classifiers on customer features. | Achieve 80-90% churn prediction accuracy, enabling proactive retention interventions 30-60 dias before churn. Reduce churn by 20-35% through targeted retention campaigns for high-risk customers. |
| **Churn Rate Benchmarking** | Track summary.churn_rate monthly to measure retention performance. Compare against industry benchmarks: SaaS 5-7%/month, e-commerce 20-30%/year, telecom 2-3%/month. Set churn rate reduction targets (e.g., reduce from 6% to 4% monthly). | Establish KPI baselines for retention teams, align executive goals with data-driven targets, justify retention program ROI by tracking churn_rate trend over 6-12 meses. |
| **Cohort Churn Analysis** | Label customers by signup cohort (month/quarter). Calculate churn_rate by cohort to identify which acquisition periods had highest attrition. Correlate with product changes, pricing shifts, marketing campaigns during those cohorts. | Identify root causes of cohort-specific churn (e.g., Q3 2024 cohort has 40% churn vs. 20% average due to pricing increase). Improve onboarding for high-risk cohorts, adjust acquisition strategies. |
| **Retention Strategy Validation** | Label customers before and after retention initiatives (loyalty programs, support improvements, pricing changes). Compare churn_rate pre vs. post-initiative. Use avg_tenure_churned to see if interventions extended customer lifetimes. | Measure retention initiative ROI: if churn_rate drops from 10% to 7% post-intervention, calculate retained LTV. Justify continued investment in successful programs, pivot away from ineffective tactics. |
| **High-Value Churn Prevention** | Cross-reference churn labels with lifetime_value. Identify high-LTV churned customers (e.g., >$5,000 LTV). Prioritize win-back campaigns for these customers. Analyze common features of high-value churners to prevent future losses. | Recover 15-25% of high-value churned customers through targeted win-back offers, prevent future high-value churn by addressing root causes (pricing, features, support gaps). |
| **Churn Reason Analysis** | Analyze label_reason Campo to underste churn drivers (inactivity vs. explicit cancellation). Segment by reason: voluntary churn (cancellations) vs. involuntary churn (payment failures) vs. silent churn (disengagement). Address each category differently. | Reduce voluntary churn by 20-30% through product improvements, reduce involuntary churn by 50-70% via better payment recovery processes, reduce silent churn through re-engagement campaigns. |
| **Customer Lifecycle Optimization** | Track avg_tenure_churned to underste typical customer lifespan. If customers churn at 6 meses on average, intensify engagement during meses 4-5. Build intervention timelines based on churn timing patterns. | Extend avg_tenure_churned from 6 to 9 meses (+50% lifetime) through proactive engagement at critical lifecycle stages. Increase customer LTV by 30-50% via longer retention periods. |

## Notes

### Implementation Melhores Práticas

**Data Quality Requirements:**
- Minimum 6 meses of customer activity history per customer
- Accurate `signup_date` e `last_activity` timestamps (ISO 8601 format)
- At least 100 customer registros for statistical reliability (1,000+ recommended)
- Populate `cancellation_date` when available for explicit churn signals

**Threshold Configuration by Industry:**
- SaaS/Subscriptions: inactivity_threshold = 60-90 dias
- E-commerce: inactivity_threshold = 120-180 dias
- Mobile Apps: inactivity_threshold = 30-60 dias
- Financial Services: inactivity_threshold = 180-365 dias
- B2B Services: inactivity_threshold = 180-365 dias

**Labeling Frequency:**
- Re-label monthly to keep training data current
- Update labels quarterly for seasonal businesses
- Re-label whenever business model changes (pricing, product features)
- Track labeling consistency over time (churn_rate should be relatively stable)

### Desempenho and Scalability

- **Maximum Payload:** 5MB per request (~50,000 customer registros)
- **Timeout:** 60 seconds for very large datasets
- **Rate Limiting:** 100 requisições por minuto por tenant
- **Taxa of Transferência:** 10,000 customers processed in 100-300ms
- **Batch Processing:** For >50,000 customers, split into multiple requests

### Confidence Level Guidelines

**High Confidence (use for model training):**
- Explicit cancellation with cancellation_date
- Inactivity >120 dias
- Subscription_status = 'cancelled' or 'expired'

**Medium Confidence (manual review recommended):**
- Inactivity 90-120 dias
- Mixed signals (some activity but declining engagement)
- Subscription_status = 'suspended'

**Low Confidence (exclude from training):**
- Inactivity 60-90 dias (ambiguous)
- Edge cases near min_tenure threshold
- Data quality issues (missing critical fields)

### Security and Compliance

- All customer data encrypted in transit (TLS 1.3) and em repouso
- Customer IDs hashed internally for privacy
- Training data retained for 90 dias, then deleted
- GDPR compliant - data deletion upon request
- Não cross-tenant data sharing or model training

### Integration Patterns

**Data Warehouse Integration:**
- Export labeled_customers to data warehouse for long-term analysis
- Join labels with customer feature tables for model training
- Track label changes over time (label drift analysis)

**ML Pipeline Integration:**
- Automate monthly labeling via cron job or Airflow DAG
- Feed labels directly to ML training pipeline
- Version control labeled datasets for reproducibility

**BI Dashboard Integration:**
- Visualize churn_rate trends in Tableau, Power BI, or Looker
- Create alerts for churn_rate spikes
- Segment churn analysis by customer attributes

## Relacionado

- [Churn Risk](ChurnRisk.md) - Predict churn probability for active customers
- [Customer Segmentation](CustomerSegmentation.md) - Segment customers to identify at-risk groups
- [Customer RFM](CustomerRFM.md) - RFM analysis for customer value segmentation
- [Customer Loyalty](CustomerLoyalty.md) - Loyalty scoring to predict retention
- [Propensity to Upgrade Plan](../Propensity/PropensityUpgradePlan.md) - Predict likelihood of plan upgrades
- [NPS](NPS.md) - Net Promoter Score analysis for satisfaction measurement

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:170`
