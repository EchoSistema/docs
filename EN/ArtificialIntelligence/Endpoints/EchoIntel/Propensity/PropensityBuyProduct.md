# Artificial Intelligence – Product Purchase Propensity

## Endpoint

```
POST /api/v1/ai/echointel/propensity/buy-product
```

Calculates to propensity of customers to purchase products specific using predictive models.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-character secret. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Parameters of the body

| Parameter       | Type   | Required | Description |
| --------------- | ------ | ----------- | --------- |
| customers       | array  | Yes         | List of customers for analysis. |
| product_id      | string | Yes         | ID of product target. |
| historical_data | array  | No         | Historical data for improve precision. |
| top_n           | int    | No         | Number of customers with higher propensity to return. Default: `100`. |

## Examples

### Request Example (curl)

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

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns product purchase propensity results. |
| 400 Bad Request | Invalid request parameters. Check parameter types and required fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See response for details. |
| 429 Too Many Requests | Rate limit exceeded. Retry after cooldown period. |
| 500 Internal Server Error | Server error. Contact support if persistent. |
| 503 Service Unavailable | AI service temporarily unavailable. Retry with exponential backoff. |

## Errors

### Common Error Responses

#### Missing Required Parameters
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

**Solution:** Ensure all required parameters are provided in the request body.

#### Invalid Authentication
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid and not expired. Check `X-Customer-Api-Id` and `X-Secret` headers.

## Notes

* Níveis of propensity: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* Results ordered by `propensity_score` descending.
* Recomendações são personalizadas por nível of propensity.

## How It Is Computed

The Product Purchase Propensity endpoint predicts the likelihood that specific customers will purchase a target product using supervised machine learning classification algorithms.

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
  - Brand affinity scores

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
- StandardScaler for numerical features: `z = (x - μ) / σ`
- MinMaxScaler for bounded features: `x_scaled = (x - min) / (max - min)`
- One-hot encoding for categorical variables
- Target encoding for high-cardinality categories

### 2. Model Architecture

**Logistic Regression:**
```
P(purchase = 1 | X) = 1 / (1 + e^(-(β₀ + β₁x₁ + β₂x₂ + ... + βₙxₙ)))
```
- Interpretable coefficients (feature importance)
- Fast inference
- Probabilistic output with calibration

**Random Forest Classifier:**
- Ensemble of decision trees (100-500 trees)
- Bootstrap aggregation (bagging) for variance reduction
- Feature importance via Gini impurity or permutation
- Handles non-linear relationships and interactions

**Gradient Boosting (XGBoost/LightGBM):**
```
F(x) = Σ(i=1 to M) γᵢ × hᵢ(x)
```
- Sequential tree building with gradient descent
- Regularization (L1/L2) to prevent overfitting
- Handles missing values internally
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
- Expected Calibration Error (ECE)
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
  - Brand awareness campaigns
  - Retargeting ads
  - Newsletter inclusion
  - Wait for purchase trigger events

**Personalization:**
- Customize messaging based on key factors
- Timing optimization (send time prediction)
- Channel preference (email, SMS, push notification)

### 7. Performance Characteristics

- **Processing Time:** 250-700ms per customer (batch: 100 customers)
- **Batch Throughput:** 500-1000 customers per minute
- **Model Accuracy:**
  - AUC-ROC: 0.78-0.86
  - Precision@Top-10%: 0.65-0.82
  - Recall@Top-20%: 0.55-0.75
  - Calibration Error (ECE): < 0.08
- **Scalability:** Horizontally scalable with model serving infrastructure
- **Model Update Frequency:** Retrained weekly or monthly based on data drift
- **Feature Store:** Cached features updated daily

## Typical Workflow

### 1. Data Collection
- Integrate customer data from CRM, purchase history, and engagement platforms
- Ensure data quality: complete records, consistent formatting
- Historical window: 6-24 months of customer interactions

### 2. Identify Target Product
- Select product for propensity analysis (new launch, slow-moving, high-margin)
- Define product characteristics and category
- Identify similar products for affinity analysis

### 3. API Request
- Prepare customer list with required attributes
- Include product_id and optional historical_data for improved accuracy
- Set top_n parameter to focus on highest propensity customers

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
  - Include in brand campaigns
  - Monitor for trigger events (life changes, seasonal needs)

### 6. Monitor and Optimize
- Track campaign performance: open rates, click-through rates, conversions
- Calculate ROI: (Revenue from conversions - Campaign cost) / Campaign cost
- Compare actual purchase rates vs. predicted propensity
- Identify model drift: if accuracy degrades, retrain with fresh data

### 7. Feedback Loop
- Collect conversion outcomes (purchased or not)
- Update training dataset with new labeled examples
- Retrain model periodically to improve accuracy
- Adjust thresholds based on business results

## Related

### Related Endpoints

- **[Propensity Respond Campaign](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityRespondCampaign.md)** - Campaign response likelihood
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Plan upgrade propensity
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Product recommendations
- **[Cross-Sell Matrix](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/CrossSellMatrix.md)** - Product affinities for cross-selling

### Related Domain Concepts

- **Propensity Modeling:** Predictive analytics, customer scoring, conversion prediction
- **Customer Segmentation:** RFM analysis, behavioral clustering, value-based segmentation
- **Feature Engineering:** Aggregation, temporal features, interaction terms
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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:177`
