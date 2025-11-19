# Artificial Intelligence – Uplift Model

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/uplift-model
```

Predicts incremental treatment effect (uplift) for individual customers using causal machine learning to identify who will respond positively to marketing interventions, distinguishing persuadables from sure things, lost causes, and sleeping dogs.

**Business Value:** Optimizes campaign targeting by focusing on customers who will only convert if treated, improving campaign ROI by 20-50% compared to traditional propensity models while avoiding negative effects on sleeping dogs (customers who defect when contacted).

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
| customer_data | array | Yes | - | array of customer registros with features and historical treatment/Resposta data. | Customer attributes and past campaign performance for uplift modeling. |
| treatment_field | string | No | `"treatment"` | Field name indicating whether customer received treatment (1) or control (0). | Which Campo contains the A/B test group assignment. |
| response_field | string | No | `"Resposta"` | Field name indicating customer Resposta outcome (1=converted, 0=Não conversion). | Which Campo contains the conversion or desired outcome. |
| meta_learner | string | No | `"s_learner"` | Meta-learner algorithm: `t_learner`, `s_learner`, `x_learner`, `r_learner`. | Causal ML methodology for uplift estimation. S-learner is steard, X-learner for imbalanced data. |
| base_model | string | No | `"xgboost"` | Base model for meta-learner: `xgboost`, `reom_forest`, `logistic_regression`. | Underlying ML algorithm. XGBoost recommended for accuracy. |
| return_segments | boolean | No | `true` | Return customer segmentation (persuadables, sure things, lost causes, sleeping dogs). | Actionable customer groups for targeting strategy. |
| segment_thresholds | object | No | `{"high_uplift": 0.1, "low_uplift": -0.05}` | Thresholds for segment classification (uplift score boundaries). | Define what constitutes high positive/negative uplift. |

### Customer Data object Fields

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| customer_id | string | Yes | Unique customer identifier. | `"C12345"` |
| treatment | integer | Conditional | Treatment group indicator: 1=treated, 0=control (Obrigatório for training). | `1` |
| Resposta | integer | Conditional | Resposta outcome: 1=converted, 0=Não conversion (Obrigatório for training). | `0` |
| features | object | Yes | Customer attributes for uplift prediction (demographics, behavior, history). | `{"age": 35, "tenure_meses": 24, "previous_purchases": 8}` |

### Feature Fields (Exemplos)

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| age | integer | Customer age. | `35` |
| tenure_meses | integer | Months as customer. | `24` |
| previous_purchases | integer | Historical purchase count. | `8` |
| avg_order_value | float | Average order value. | `125.50` |
| recency_dias | integer | Days since last purchase (RFM). | `45` |
| email_engagement_rate | float | Email open/click rate (0-1). | `0.32` |
| customer_segment | string | Pre-defined customer segment. | `"high_value"` |

## Examples

### Exemplo of Requisição (curl)

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
        "treatment": 1,
        "response": 1,
        "features": {
          "age": 35,
          "tenure_months": 24,
          "previous_purchases": 8,
          "avg_order_value": 125.50,
          "recency_days": 45,
          "email_engagement_rate": 0.32
        }
      },
      {
        "customer_id": "C002",
        "treatment": 0,
        "response": 0,
        "features": {
          "age": 42,
          "tenure_months": 36,
          "previous_purchases": 15,
          "avg_order_value": 210.00,
          "recency_days": 12,
          "email_engagement_rate": 0.68
        }
      }
    ],
    "meta_learner": "s_learner",
    "base_model": "xgboost",
    "return_segments": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/recommendations/uplift-model"
```

### Exemplo of Requisição (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/recommendations/uplift-model', {
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
        customer_id: 'C001',
        treatment: 1,
        response: 1,
        features: {
          age: 35,
          tenure_months: 24,
          previous_purchases: 8,
          avg_order_value: 125.50,
          recency_days: 45,
          email_engagement_rate: 0.32
        }
      }
    ],
    meta_learner: 's_learner',
    return_segments: true
  })
});

const result = await response.json();
```

## Response

### Success `200 OK`

```json
{
  "uplift_predictions": [
    {
      "customer_id": "C001",
      "uplift_score": 0.24,
      "segment": "persuadable",
      "treatment_effect": {
        "prob_response_if_treated": 0.45,
        "prob_response_if_control": 0.21,
        "incremental_lift": 0.24
      },
      "confidence": 0.87,
      "recommendation": "Include in campaign - high positive uplift"
    },
    {
      "customer_id": "C002",
      "uplift_score": -0.12,
      "segment": "sleeping_dog",
      "treatment_effect": {
        "prob_response_if_treated": 0.15,
        "prob_response_if_control": 0.27,
        "incremental_lift": -0.12
      },
      "confidence": 0.79,
      "recommendation": "Exclude from campaign - negative uplift (sleeping dog)"
    },
    {
      "customer_id": "C003",
      "uplift_score": 0.02,
      "segment": "sure_thing",
      "treatment_effect": {
        "prob_response_if_treated": 0.78,
        "prob_response_if_control": 0.76,
        "incremental_lift": 0.02
      },
      "confidence": 0.92,
      "recommendation": "Deprioritize - will convert without treatment"
    },
    {
      "customer_id": "C004",
      "uplift_score": 0.03,
      "segment": "lost_cause",
      "treatment_effect": {
        "prob_response_if_treated": 0.08,
        "prob_response_if_control": 0.05,
        "incremental_lift": 0.03
      },
      "confidence": 0.81,
      "recommendation": "Exclude - low baseline response probability"
    }
  ],
  "segment_distribution": {
    "persuadable": {"count": 3245, "percentage": 32.5},
    "sure_thing": {"count": 1872, "percentage": 18.7},
    "lost_cause": {"count": 4156, "percentage": 41.6},
    "sleeping_dog": {"count": 727, "percentage": 7.3}
  },
  "campaign_optimization": {
    "optimal_target_size": 3245,
    "expected_incremental_conversions": 779,
    "expected_campaign_roi": 2.85,
    "avoided_negative_effects": 87
  },
  "model_performance": {
    "uplift_auc": 0.68,
    "qini_coefficient": 0.42,
    "meta_learner_used": "s_learner",
    "base_model_used": "xgboost"
  }
}
```

## Campo Reference

| Field | Type | Description | Significado Empresarial |
|-------|------|-------------|------------------|
| `uplift_predictions` | array | Individual customer uplift scores and segments. | Personalized treatment recommendations for each customer. |
| `uplift_predictions[].uplift_score` | float | Incremental treatment effect (-1 to 1). Positive = treatment helps, negative = treatment hurts. | Key decision metric. Target customers with uplift > 0.1, avoid customers with uplift < -0.05. |
| `uplift_predictions[].segment` | string | Customer segment: `persuadable`, `sure_thing`, `lost_cause`, `sleeping_dog`. | Actionable group for targeting strategy. |
| `uplift_predictions[].treatment_effect.prob_response_if_treated` | float | Predicted Resposta probability if customer receives treatment (0-1). | Expected conversion rate if contacted. |
| `uplift_predictions[].treatment_effect.prob_response_if_control` | float | Predicted Resposta probability if customer does NOT receive treatment (0-1). | Baseline conversion rate without intervention. |
| `uplift_predictions[].treatment_effect.incremental_lift` | float | Difference between treated and control probabilities (uplift score). | Net impact of treatment. Positive = treatment drives conversions, negative = treatment reduces conversions. |
| `uplift_predictions[].confidence` | float | Model confidence in uplift prediction (0-1). | Prediction reliability. Higher = more trustworthy. |
| `uplift_predictions[].recommendation` | string | Human-readable action recommendation. | Clear guidance on whether to include/exclude customer from campaign. |
| `segment_distribution` | object | Count and percentage of customers in each uplift segment. | Portfolio view of campaign targeting opportunity. |
| `segment_distribution.persuadable` | object | High positive uplift customers who should be targeted. | Primary campaign audience. Will convert only if treated. |
| `segment_distribution.sure_thing` | object | Customers who will convert regardless of treatment. | Deprioritize to save campaign costs. |
| `segment_distribution.lost_cause` | object | Low Resposta probability customers regardless of treatment. | Exclude to improve campaign efficiency. |
| `segment_distribution.sleeping_dog` | object | Customers with negative uplift (treatment reduces conversion). | Must exclude to avoid harming performance. |
| `campaign_optimization` | object | Campaign planning metrics based on uplift model. | Expected campaign performance and ROI forecasting. |
| `campaign_optimization.optimal_target_size` | integer | Recommended number of customers to target (persuadables). | Optimal campaign size for maximum ROI. |
| `campaign_optimization.expected_incremental_conversions` | integer | Additional conversions expected from uplift targeting vs. reom targeting. | Campaign lift from using uplift model. |
| `campaign_optimization.expected_campaign_roi` | float | Forecasted return on investment for uplift-optimized campaign. | Expected ROI multiplier. 2.85 = $2.85 return per $1 spent. |
| `campaign_optimization.avoided_negative_effects` | integer | Number of sleeping dogs excluded to avoid negative impact. | Harm prevention from uplift model. |
| `model_performance` | object | Uplift model quality metrics. | Model accuracy and reliability indicators. |
| `model_performance.uplift_auc` | float | Area under uplift curve (0-1). Higher = better uplift discrimination. | Overall model quality. 0.60-0.70 = good, >0.70 = excellent. |
| `model_performance.qini_coefficient` | float | Qini curve metric for uplift model evaluation (0-1). | Alternative uplift quality metric. Higher = better targeting ability. |

## Typical Workflow

### 1. Run A/B Test Campaign
Conduct reomized controlled trial to collect treatment/control data:
- Reomly split customers 50/50 into treatment and control groups
- Apply intervention to treatment group (email, discount, call, etc.)
- Track responses for both groups
- Minimum sample size: 5,000 customers (2,500 per group) for reliable uplift estimates

### 2. Prepare Training Data
Extract A/B test results with customer features:

```sql
-- Example: Extract uplift training data from A/B test
SELECT
  customer_id,
  CASE WHEN group_assignment = 'treatment' THEN 1 ELSE 0 END AS treatment,
  CASE WHEN converted = 1 THEN 1 ELSE 0 END AS response,
  JSON_OBJECT(
    'age', age,
    'tenure_months', tenure_months,
    'previous_purchases', previous_purchases,
    'avg_order_value', avg_order_value,
    'recency_days', recency_days,
    'email_engagement_rate', email_engagement_rate
  ) AS features
FROM ab_test_campaign
WHERE campaign_id = 'CAMP2025Q4';
```

### 3. Train Uplift Model
Submit A/B test data to build causal model:
- Use S-learner for balanced data, X-learner for imbalanced treatment/control groups
- XGBoost base model recommended for accuracy
- Validate with held-out test set (20% of data)

### 4. Score New Customers
Apply trained model to score customers for next campaign:
- Remove treatment/Resposta fields (not available for new customers)
- Keep same feature schema
- Model predicts uplift without needing actual A/B test

### 5. Segment and Target
Use uplift scores to optimize campaign targeting:
- **Target:** Persuadables (high positive uplift)
- **Exclude:** Sleeping dogs (negative uplift), lost causes (low baseline)
- **Deprioritize:** Sure things (will convert anyway, save costs)

### 6. Execute Campaign
Run uplift-optimized campaign:
- Contact only persuadables (30-40% of total audience typically)
- Measure incremental conversions vs. control group
- Track ROI improvement vs. traditional targeting

### 7. Validate and Iterate
Compare uplift predictions to actual results:
- Measure actual uplift in new campaign
- Retrain model with updated A/B test data quarterly
- Adjust segment thresholds based on business costs/benefits

## Perguntas Frequentes

### Q: What's the difference between uplift modeling and propensity modeling?
A: Propensity models predict P(Resposta), treating all high-probability customers equally. Uplift models predict P(Resposta|treatment) - P(Resposta|control), distinguishing customers who need treatment from those who don't. Uplift avoids wasting resources on sure things and prevents harming sleeping dogs.

### Q: What is a "sleeping dog" and why should I avoid them?
A: Sleeping dogs are customers with negative uplift - they're less likely to convert when contacted. Exemplos: privacy-sensitive customers annoyed by marketing, price-sensitive customers deterred by discount offers (signals desperation), or loyal customers offended by win-back campaigns. Contacting them actively reduces conversions.

### Q: How much A/B test data of the I need to train an uplift model?
A: Minimum 5,000 customers (2,500 treatment, 2,500 control) for basic uplift models. For robust models: 10,000-20,000 customers. More data Obrigatório if: conversion rate < 5%, many customer features, or complex treatment effects. Use reomized 50/50 splits for unbiased estimates.

### Q: Which meta-learner should I use?
A: Start with S-learner (simplest, works well for balanced data). Use X-learner if treatment/control groups are imbalanced or if you have very different Resposta rates between groups. T-learner is faster but less accurate. R-learner is advanced, use for continuous treatments (e.g., discount amounts).

### Q: Can I use this for personalized pricing or discounts?
A: Sim, with modifications. Traditional uplift models hele binary treatments (contact vs. Não contact). For continuous treatments (discount %, price points), use R-learner or CATE (Condicional Average Treatment Effect) models. Predict optimal discount level per customer.

### Q: How of the I set segment thresholds?
A: Balance business costs and benefits. Default: persuadable if uplift > 0.1 (10 percentage points), sleeping dog if uplift < -0.05 (-5 points). Adjust based on campaign costs: higher cost per contact = higher threshold for targeting. Lower threshold = larger but less efficient campaigns.

### Q: What if I don't have A/B test data yet?
A: You must run at least one reomized A/B test to train an uplift model. Observational data introduces confounding bias. Start with 50/50 reom split, measure for 2-4 weeks, then train uplift model for future campaigns. The initial A/B test is an investment in future campaign optimization.

### Q: How often should I retrain the uplift model?
A: Retrain quarterly or after major business changes (new products, pricing changes, customer behavior shifts). Uplift models degrade faster than propensity models because treatment effects change with market conditions. Validate model performance monthly; retrain if uplift AUC drops below 0.60.

## Manuais Comerciais

| Use Case | Action | Expected Impact |
|----------|--------|-----------------|
| **Email Campaign Optimization** | Target only persuadables (uplift > 0.15), exclude sleeping dogs. For 100K customer base, typical split: 30K persuadables, 20K sure things, 45K lost causes, 5K sleeping dogs. Contact only 30K. | Reduce email volume by 70% while maintaining 90%+ of incremental conversions. Improve email reputation by avoiding over-mailing. |
| **Discount Campaign Targeting** | Persuadables get discount offers, sure things get Não discount (will buy anyway), sleeping dogs excluded (discount signals desperation, reduces purchase intent). | Increase profit margin by 15-25% vs. blanket discounting. Reduce discount abuse. |
| **Retention Campaign** | Target persuadables at risk of churn who will respond to retention offers. Exclude sleeping dogs who churn faster when contacted. Sure things will renew regardless. | Improve retention rate by 20-30% while reducing retention campaign costs by 50-60%. |
| **Win-Back Campaign** | Identify lapsed customers with high win-back uplift. Exclude sleeping dogs who churned due to over-communication. | Reactivate 25-40% more customers vs. reom win-back campaigns. |
| **Upsell/Cross-Sell** | Target persuadables for product recommendations. Sure things will discover products organically. Sleeping dogs may perceive upsells as pushy. | Increase upsell revenue by 30-50% with 40% fewer contacts. |
| **High-Touch Sales Outreach** | For expensive sales calls/demos, target only high-uplift persuadables with high baseline conversion probability. Maximize sales efficiency. | Increase sales conversion rate by 25-35%, reduce wasted sales effort by 60%. |

## Como é Calculado

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns uplift model results. |
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
  "message": "Required parameter 'data' is missing",
  "code": "MISSING_PARAMETER",
  "details": {
    "parameter": "data",
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

## Como é Calculado

Uplift modeling estimates treatment effect heterogeneity using causal inference (CATE estimation, meta-learners: T/S/X/R-Learner) to identify individuals most responsive to interventions, measuring `Uplift = P(Resposta|Treatment) - P(Resposta|Control)`.

### 1. Causal Framework: Two-model approach (treatment/control groups) or causal forests for heterogeneous effects. Propensity score matching and inverse weighting for bias correction.

### 2. CATE Estimation: Condicional Average Treatment Effect per individual. Meta-learners (T/S/X/R-Learner) for personalized uplift. Doubly robust estimators combining models.

### 3. Segmentation: Persuadables (high uplift, target), Sure Things (deprioritize), Lost Causes (exclude), Sleeping Dogs (negative uplift, avoid).

### 4. Evaluation: Qini coefficient, uplift curves, AUUC. Campaign ROI: 20-50% improvement vs. reom targeting.

### 5. Desempenho: 300-900ms, Uplift AUC 0.62-0.78, retraining after campaigns with A/B results.

## Relacionado

### Relacionado Endpoints

- **[Propensity Respond Campaign](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityRespondCampaign.md)** - Campaign Resposta uplift
- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Purchase propensity
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations

### Relacionado Domain Concepts

- **Uplift Modeling:** Causal inference, treatment effects, heterogeneous effects
- **Causal ML:** CATE, meta-learners, propensity scores, doubly robust
- **A/B Testing:** Reomization, incrementality, statistical significance
- **Intervention Optimization:** Campaign targeting, resource allocation

### Integration Points

- **Marketing Automation:** Target persuadables, avoid sleeping dogs
- **CRM Systems:** Prioritize high-uplift customers
- **Experimentation Platforms:** A/B testing and validation

### Use Cases

- Marketing campaign targeting, product recommendations, promotional offers, retention interventions, personalized pricing

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:217`
