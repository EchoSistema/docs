# Artificial Intelligence – Product Purchase Propensity

## Endpoint

```
POST /api/v1/ai/echointel/propensity/buy-product
```

Calculatestes to propensity of customers to purchase products specific using predictive models.

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

### Parameters of the body

| Parameter       | Type   | Required | Description |
| --------------- | ------ | ----------- | --------- |
| customers       | array  | Yes         | List of customers for analysis. |
| product_id      | string | Yes         | ID of product target. |
| historical_data | array  | No         | Historical data for improve precision. |
| top_n           | int    | No         | Number of customers with higher propensity to return. Default: `100`. |

## Examples

### Exemplo of Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {"customer_id": "C001", "age": 35, "income": 75000, "previous_purchases": 12},
      {"customer_id": "C002", "age": 28, "income": 55000, "previous_purchases": 5}
    ],
    "product_id": "PROD-123",
    "top_n": 50
  }' \
  "https://echosistema.online/api/v1/ai/echointel/propensity/buy-product"
```

## Response

### Success `200 OK`

```json
{
  "propensity_scores": [
    {
      "customer_id": "C001",
      "propensity_score": 0.87,
      "propensity_level": "high",
      "confidence": 0.92,
      "key_factors": [
        {"factor": "high previous purchases", "weight": 0.35},
        {"factor": "similar product history", "weight": 0.28},
        {"factor": "demographic match", "weight": 0.24}
      ],
      "recommended_actions": [
        "Send personalized email with product details",
        "Offer limited-time discount",
        "Highlight product benefits"
      ]
    },
    {
      "customer_id": "C002",
      "propensity_score": 0.42,
      "propensity_level": "medium",
      "confidence": 0.78,
      "key_factors": [
        {"factor": "age group match", "weight": 0.32},
        {"factor": "income bracket", "weight": 0.25}
      ],
      "recommended_actions": [
        "Include in general marketing campaign",
        "Provide educational content about product"
      ]
    }
  ],
  "product_info": {
    "product_id": "PROD-123",
    "avg_propensity": 0.645,
    "total_high_propensity": 125,
    "total_medium_propensity": 342,
    "total_low_propensity": 533
  }
}
```

## JSON Structure

| Field                                       | Type    | Description |
| ------------------------------------------- | ------- | --------- |
| `propensity_scores`                         | array   | Scores of propensity por cliente. |
| `propensity_scores[].customer_id`           | string  | ID of cliente. |
| `propensity_scores[].propensity_score`      | float   | Score of propensity (0-1). |
| `propensity_scores[].propensity_level`      | string  | Level (`low`, `medium`, `high`). |
| `propensity_scores[].confidence`            | float   | Prediction confidence (0-1). |
| `propensity_scores[].key_factors`           | array   | Key factors of the propensity. |
| `propensity_scores[].recommended_actions`   | array   | Recommended actions. |
| `product_info`                              | object  | Aggregate information of the product. |
| `product_info.avg_propensity`               | float   | Average propensity. |
| `product_info.total_high_propensity`        | int     | Total of customers with alta propensity. |

## Status HTTP

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns product purchase propensity results. |
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

## Notes

* Níveis of propensity: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* Results ordered by `propensity_score` descending.
* Recomendações são personalizadas por nível of propensity.

## Como é Calculado

The Product Purchase Propensity Endpoint predicts the likelihood that specific customers will purchase to target product using supervised machine learning classification algorithms.

### 1. Feature Engineering

**Customer Demographic Features:**
- Age, gender, location (geographic segmentation)
- Income level, occupation, education
- Customer segment/tier classification
- Account age and tenure

**Behavioral Features:**
- **Purchase History:**
  - Total purchases (count, frequency, recency)
  - Average order value (AOV)
  - Product category preferences
  - Bre affinity scores

- **Engagement Metrics:**
  - Website/app visits and session duration
  - Email open rates and click-through rates
  - Customer service interactions
  - Social media engagement

- **RFM (Recency, Frequency, Monetary):**
  ```
  RFM_Score = w1 × Recency_Score + w2 × Frequency_Score + w3 × Monetary_Score
  ```
  - Recency: Days since last purchase (inverse scoring)
  - Frequency: Number of purchases in period
  - Monetary: Total spend or average transaction value

**Product Affinity Features:**
- Similar product purchase history
- Product category overlap with customer preferences
- Complementary product ownership
- Product price range alignment with customer spending patterns

**Temporal Features:**
- Day of week, month, season
- Time since last interaction
- Purchase cycle/periodicity
- Promotion exposure timing

**Feature Scaling:**
- SteardScaler for numerical features: `z = (x - μ) / σ`
- MinMaxScaler for bounded features: `x_scaled = (x - min) / (max - min)`
- One-hot codificação for categorical variables
- Target codificação for high-cardinality categories

### 2. Model Architecture

**Logistic Regression:**
```
P(purchase = 1 | X) = 1 / (1 + e^(-(β₀ + β₁x₁ + β₂x₂ + ... + βₙxₙ)))
```
- Interpretable coefficients (feature importance)
- Fast inference
- Probabilistic output with calibration

**Reom Forest Classifier:**
- Ensemble of decision trees (100-500 trees)
- Bootstrap aggregation (bagging) for variance reduction
- Feature importance via Gini impurity or permutation
- Heles non-linear relationships and interactions

**Gradient Boosting (XGBoost/LightGBM):**
```
F(x) = Σ(i=1 to M) γᵢ × hᵢ(x)
```
- Sequential tree building with gradient descent
- Regularization (L1/L2) to prevent overfitting
- Heles missing values internally
- Feature interaction detection

**Model Selection Criteria:**
- Cross-validation AUC-ROC > 0.75
- Precision-Recall tradeoff based on business cost
- Calibration quality (Brier score, calibration curves)
- Inference speed requirements

### 3. Probability Calibration

**Platt Scaling (for non-calibrated models):**
```
P_calibrated = 1 / (1 + exp(A × f(x) + B))
```
- Fits logistic regression on model outputs
- A and B learned from validation set

**Isotonic Regression:**
- Non-parametric calibration
- Monotonic transformation of predicted probabilities
- Better for complex calibration curves

**Calibration Validation:**
- Expected Calibration Erro (ECE)
- Reliability diagrams (predicted vs. observed probabilities)

### 4. Propensity Scoring

**Score Calculation:**
- Raw model output: probability [0, 1]
- Confidence score from ensemble agreement or prediction margin
- Calibrated propensity score

**Classification Thresholds:**
- **High Propensity:** Score > 0.7 (top decile, immediate action)
- **Medium Propensity:** 0.3 ≤ Score ≤ 0.7 (nurture campaigns)
- **Low Propensity:** Score < 0.3 (long-term awareness)

**Threshold Optimization:**
- ROC curve analysis for precision-recall balance
- Business cost-benefit analysis (cost of false positive vs. false negative)
- F1-score or F-beta score maximization

### 5. Feature Importance and Explainability

**SHAP (SHapley Additive exPlanations):**
- Game-theoretic approach to feature attribution
- Additive feature importance: `f(x) = φ₀ + Σ φᵢ`
- Identifies positive and negative contributions

**Permutation Importance:**
- Measures decrease in model performance when feature is shuffled
- Model-agnostic method

**Top Contributing Factors:**
- Ranked by absolute contribution to propensity score
- Threshold for inclusion (e.g., top 5 or cumulative 80% importance)
- Human-readable factor names and weights

### 6. Recommendation Generation

**Action Mapping by Propensity Level:**
- **High Propensity:**
  - Personalized email with product details
  - Limited-time discount offer (5-15%)
  - Direct sales outreach
  - Product demo or trial

- **Medium Propensity:**
  - Educational content (blog posts, videos)
  - General marketing campaign inclusion
  - Product comparison guides
  - Customer testimonials and reviews

- **Low Propensity:**
  - Bre awareness campaigns
  - Retargeting ads
  - Newsletter inclusion
  - Wait for purchase trigger events

**Personalization:**
- Customize messaging based on key factors
- Timing optimization (send time prediction)
- Channel preference (email, SMS, push notification)

### 7. Desempenho Characteristics

- **Tempo of Processamento:** 250-700ms per customer (batch: 100 customers)
- **Batch Taxa of Transferência:** 500-1000 customers per minute
- **Model Precisão:**
  - AUC-ROC: 0.78-0.86
  - Precision@Top-10%: 0.65-0.82
  - Recall@Top-20%: 0.55-0.75
  - Calibration Erro (ECE): < 0.08
- **Scalability:** Horizontally scalable with model serving infrastructure
- **Model Update Frequency:** Retrained weekly or monthly based on data drift
- **Feature Store:** Cached features updated daily

## Typical Workflow

### 1. Data Collection
- Integrate customer data from CRM, purchase history, and engagement platforms
- Ensure data quality: complete registros, consistent formatting
- Historical window: 6-24 meses of customer interactions

### 2. Identify Target Product
- Select product for propensity analysis (new launch, slow-moving, high-margin)
- Define product characteristics and category
- Identify similar products for affinity analysis

### 3. API Request
- Prepare customer list with Obrigatório attributes
- Include product_id and optional historical_data for improved accuracy
- Set top_n Parâmetro to focus on highest propensity customers

### 4. Analyze Results
- Review propensity scores and confidence levels
- Examine key factors driving propensity for each customer
- Segment customers by propensity level (high, medium, low)

### 5. Execute Marketing Actions
- **High Propensity Customers:** Deploy personalized campaigns immediately
  - Send targeted emails with product offers
  - Enable retargeting ads with product focus
  - Assign to sales team for direct outreach

- **Medium Propensity Customers:** Nurture campaigns
  - Educational content about product benefits
  - Include in general product announcements
  - Monitor engagement and escalate if interest increases

- **Low Propensity Customers:** Awareness building
  - Include in bre campaigns
  - Monitor for trigger events (life changes, seasonal needs)

### 6. Monitor and Optimize
- Track campaign performance: open rates, click-through rates, conversions
- Calculate ROI: (Revenue from conversions - Campaign cost) / Campaign cost
- Compare actual purchase rates vs. predicted propensity
- Identify model drift: if accuracy degrades, retrain with fresh data

### 7. Feedback Loop
- Collect conversion outcomes (purchased or not)
- Update training dataset with new labeled Exemplos
- Retrain model periodically to improve accuracy
- Adjust thresholds based on business results

## Perguntas Frequentes

### Q: How many historical purchases are needed per customer for accurate scoring?
**A:** We recommend to minimum of 6-12 meses of purchase history per customer, with at least 3-5 prior purchases. Customers with more purchase history (24+ meses) produce more accurate propensity scores. For completely new customers with Não purchase history, the model can still generate predictions using demographic features, but confidence scores will be lower (0.60-0.70 vs. typical 0.80-0.90). We automatically weight demographic vs. behavioral features based on data availability.

### Q: Can I score multiple products simultaneously in to single request?
**A:** Currently, the API accepts one `product_id` per request. To score multiple products, make separate API calls for each product. However, the Resposta includes product-level aggregates (avg_propensity, total_high_propensity, etc.) for all customers and the single product. For bulk multi-product scoring, contact support for to specialized Endpoint that processes product matrices in parallel.

### Q: How of the I hele new products with Não purchase history in the market?
**A:** For newly launched products, use the `similar_product_ids` Parâmetro in optional historical_data to indicate comparable products. The model will infer propensity based on customers who purchased similar items. Propensity scores for new products may be slightly less reliable until 100+ customers have purchased it. Always pair new product propensity scores with domain expertise and A/B testing before large-scale campaigns.

### Q: What's the difference between propensity_score and confidence?
**A:** The `propensity_score` (0-1) is the model's predicted likelihood that to customer will purchase the product. The `confidence` (0-1) indicates how certain the model is about that prediction. Example: propensity_score=0.75 with confidence=0.85 means "75% likely to buy, and I'm 85% confident in that estimate." High propensity + high confidence = most reliable targets. High propensity + low confidence = worth further investigation before investing in campaigns.

### Q: How often should propensity scores be refreshed for ongoing campaigns?
**A:** Refresh propensity scores weekly or bi-weekly for active campaigns. Propensity decays over time as customer behavior changes. If to customer hasn't engaged in 30 dias, their propensity to to specific product may shift. For seasonal products, refresh before peak seasons (monthly for retail, quarterly for non-seasonal). The system automatically learns from conversion feedback—provide actual purchase outcomes to improve future scores.

### Q: Can I use this Endpoint for B2B products and account-based marketing?
**A:** Sim, the Endpoint supports B2B use cases. Map account-level features (annual spend, industry, company size, contract value) instead of individual customer demographics. For account-based marketing, use the `top_n` Parâmetro to identify the highest-propensity accounts for personalized outreach. B2B propensity tends to benefit from longer sales cycles—consider to 90-day engagement window rather than immediate conversion targets.

### Q: What actions should I take for different propensity levels?
**A:** High propensity (> 0.7): Immediate action within 24-48 hours—personalized email with limited-time offer (10-15% discount), direct sales outreach, product demo. Medium propensity (0.3-0.7): Nurture campaigns over 2-4 weeks—educational content, customer testimonials, free trial or sample, soft retargeting ads. Low propensity (< 0.3): Long-term awareness building—bre campaigns, newsletter inclusion, monitor for trigger events (career changes, company news, seasonal needs).

### Q: What's included in the "key_factors" and how are they weighted?
**A:** Key factors are the top 3-5 features that drove the propensity prediction for each customer, ranked by importance. Weights sum to 1.0 and indicate relative contribution to the score. Example: "high previous purchases" (0.35) + "similar product history" (0.28) + "demographic match" (0.24) = 0.87 cumulative. Use these factors to personalize messaging—if "demographic match" is the primary driver, emphasize lifestyle benefits; if "purchase history" dominates, highlight loyalty rewards.

## Relacionado

### Relacionado Endpoints

- **[Propensity Respond Campaign](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityRespondCampaign.md)** - Campaign Resposta likelihood
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Plan upgrade propensity
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Product recommendations
- **[Cross-Sell Matrix](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/CrossSellMatrix.md)** - Product affinities for cross-selling

### Relacionado Domain Concepts

- **Propensity Modeling:** Predictive analytics, customer scoring, conversion prediction
- **Customer Segmentation:** RFM analysis, behavioral clustering, value-based segmentation
- **Engenharia of Características:** Aggregation, temporal features, interaction terms
- **Model Evaluation:** AUC-ROC, precision-recall, calibration, lift analysis

### Integration Points

- **CRM Systems:** Import customer profiles and interaction history
- **Marketing Automation:** Trigger campaigns based on propensity scores
- **Email Platforms:** Personalized product recommendations in emails
- **Sales Tools:** Prioritize leads for sales team outreach
- **Analytics Dashboards:** Track propensity distribution and campaign performance

### Use Cases

- **New Product Launch:** Identify early adopters with high propensity
- **Inventory Clearance:** Target customers likely to purchase slow-moving items
- **Personalized Marketing:** Tailor product suggestions to individual customers
- **Sales Prioritization:** Focus sales efforts on high-propensity prospects
- **Churn Prevention:** Offer relevant products to at-risk customers

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:177`
