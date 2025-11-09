# Artificial Intelligence – Campaign Response Propensity

## Endpoint

```
POST /api/v1/ai/echointel/propensity/respond-campaign
```

Campaign Response Propensity using predictive propensity models.

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

> **Note:** Parameters accept both `snake_case` and `camelCase`.


## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns campaign response propensity results. |
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

## How It Is Computed

The Campaign Response Propensity endpoint uses uplift modeling and treatment effect estimation to predict which customers are most likely to respond positively to a marketing campaign.

### 1. Uplift Modeling Framework

**Treatment vs. Control Groups:**
- **Treatment Group:** Customers who receive the campaign
- **Control Group:** Customers who do not receive the campaign (holdout)
- **Uplift:** Incremental response due to campaign exposure

**Uplift Calculation:**
```
Uplift = P(Response | Treatment) - P(Response | Control)
```

**Objective:**
- Identify customers where campaign significantly increases response likelihood
- Avoid spending on customers who would respond anyway (sure things)
- Avoid spending on customers who won't respond regardless (lost causes)

### 2. Feature Engineering

**Customer Profile Features:**
- Demographics: age, gender, location, income, occupation
- Account characteristics: tenure, plan type, lifetime value
- Engagement score: website visits, app usage, email interactions
- Loyalty indicators: repeat purchases, referrals, reviews

**Campaign-Specific Features:**
- **Historical Campaign Response:**
  - Previous campaign participation (yes/no)
  - Response rate to past campaigns (click, conversion)
  - Preferred campaign types (email, SMS, social)
  - Time since last campaign interaction

- **Campaign Attributes:**
  - Campaign type: promotional, educational, transactional
  - Channel: email, SMS, push, social media
  - Offer type: discount, free trial, content, announcement
  - Incentive level: percentage off, fixed amount, gift

**Behavioral Features:**
- Purchase frequency and recency
- Average order value and trend
- Product category preferences
- Seasonal purchase patterns
- Cart abandonment behavior

**Contextual Features:**
- Day of week, time of day
- Season, holidays, events
- Economic indicators (for B2B)
- Competitive activity awareness

### 3. Uplift Modeling Approaches

**Two-Model Approach (T-Learner):**
```
Uplift(x) = P_T(Response | x) - P_C(Response | x)
```
- Train separate models for treatment and control groups
- Predict response probability for each group
- Calculate uplift as difference

**Single-Model Approach (S-Learner):**
- Train one model with treatment indicator as a feature
- Predict response with treatment=1 and treatment=0
- Calculate uplift as difference in predictions

**Causal Forest / Uplift Random Forest:**
- Trees built to maximize treatment effect heterogeneity
- Splits chosen to maximize variance in treatment effect
- Ensemble of uplift trees for robust estimates

**Causal Inference Methods:**
- **Propensity Score Matching:** Match treated and control units
- **Inverse Propensity Weighting:** Weight samples by inverse of treatment probability
- **Doubly Robust Estimators:** Combine propensity scores with outcome models

### 4. Treatment Effect Estimation

**Conditional Average Treatment Effect (CATE):**
```
CATE(x) = E[Y₁ - Y₀ | X = x]
```
- Y₁ = Outcome if treated
- Y₀ = Outcome if not treated
- X = Customer features

**Individual Treatment Effect (ITE):**
- Personalized uplift estimate for each customer
- Uncertainty quantification (confidence intervals)

**Meta-Learners:**
- **X-Learner:** Improved T-Learner with imputation
- **R-Learner:** Residual-based approach for heterogeneous treatment effects
- **DR-Learner:** Doubly robust learner combining multiple approaches

### 5. Customer Segmentation by Uplift

**Four-Quadrant Segmentation:**
- **Persuadables (High Uplift, Low Baseline):** Target with campaign
  - Will respond only if contacted
  - Highest ROI segment

- **Sure Things (High Response in Both Groups):** Deprioritize
  - Will respond regardless of campaign
  - Save campaign budget

- **Lost Causes (Low Response in Both Groups):** Exclude
  - Won't respond even with campaign
  - Avoid wasting resources

- **Sleeping Dogs (Negative Uplift):** Avoid
  - Campaign actually decreases response
  - Possible fatigue or irritation

**Prioritization Score:**
```
Priority = Uplift × Expected_Value - Campaign_Cost
```

### 6. Response Prediction Models

**Logistic Regression with Interaction Terms:**
```
log(P / (1-P)) = β₀ + Σβᵢxᵢ + Σγⱼ(xⱼ × Treatment)
```
- Interaction terms capture treatment effect moderation

**Gradient Boosting Machines:**
- XGBoost or LightGBM for capturing complex interactions
- Feature importance for understanding drivers
- SHAP values for explainability

**Neural Networks:**
- Deep learning for complex patterns
- Separate heads for treatment and control predictions
- Representation learning for latent features

### 7. Model Evaluation Metrics

**Uplift-Specific Metrics:**
- **Qini Coefficient:** Area between uplift curve and random
- **Uplift Curve:** Cumulative response difference by targeting quantile
- **AUUC (Area Under Uplift Curve):** Overall uplift model quality

**Business Metrics:**
- **Incremental Response Rate:** Additional responses from campaign
- **Campaign ROI:** (Revenue - Cost) / Cost
- **Cost per Incremental Conversion:** Campaign cost / extra conversions

**Statistical Tests:**
- A/B test validation of predicted uplift
- Randomization inference for causal validity
- Sensitivity analysis for confounding

### 8. Performance Characteristics

- **Processing Time:** 300-900ms per customer (batch: 200-500 customers)
- **Throughput:** 400-800 customers per minute
- **Model Accuracy:**
  - Uplift AUC: 0.62-0.78 (>0.5 indicates positive uplift detection)
  - Top Decile Uplift: 15-35% incremental response
  - Qini Coefficient: 0.25-0.45
  - Campaign ROI Improvement: 20-50% vs. random targeting
- **Model Retraining:** After each major campaign with A/B test results
- **Feature Importance Stability:** Tracked for model drift detection

## Typical Workflow

### 1. Campaign Planning
- Define campaign objective: awareness, conversion, retention, upsell
- Specify target audience constraints (e.g., active customers, specific regions)
- Determine campaign budget and capacity limits

### 2. Historical Data Preparation
- Collect past campaign data with treatment assignment and responses
- Ensure proper A/B test structure (randomized treatment/control)
- Minimum dataset: 5,000+ customers with 10-20% response rate

### 3. API Request
- Submit customer list with demographic and behavioral features
- Include campaign characteristics (type, channel, offer)
- Optionally specify holdout percentage for validation

### 4. Uplift Analysis
- Review uplift scores for each customer
- Segment customers into four uplift quadrants
- Identify persuadables (high uplift, low baseline response)

### 5. Campaign Targeting
- **Persuadables:** Primary target, maximize campaign resources here
- **Sure Things:** Include selectively or deprioritize to save budget
- **Lost Causes:** Exclude from campaign
- **Sleeping Dogs:** Exclude (negative uplift risk)

### 6. Campaign Execution
- Deploy campaign to persuadables and selected sure things
- Maintain small random control group (5-10%) for validation
- Track responses in real-time

### 7. Post-Campaign Analysis
- Measure actual uplift: Treatment response - Control response
- Calculate campaign ROI and cost per incremental conversion
- Compare predicted vs. actual uplift (model calibration)
- Update model with new campaign results for future improvement

### 8. Continuous Optimization
- Retrain uplift models quarterly or after major campaigns
- Experiment with different campaign variations (A/B/n testing)
- Refine customer segmentation based on observed behaviors
- Adjust budget allocation to highest-uplift segments

## Related

### Related Endpoints

- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Product purchase likelihood
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Upgrade propensity modeling
- **[Uplift Model](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/UpliftModel.md)** - General uplift modeling endpoint

### Related Domain Concepts

- **Uplift Modeling:** Causal inference, treatment effect estimation, heterogeneous effects
- **Causal Machine Learning:** Potential outcomes, CATE, meta-learners
- **A/B Testing:** Randomized controlled trials, statistical significance, power analysis
- **Marketing Analytics:** Campaign ROI, incrementality, attribution

### Integration Points

- **Marketing Automation Platforms:** Import customer data, trigger campaigns
- **Email Service Providers:** Send emails to targeted segments
- **CRM Systems:** Track campaign responses and customer interactions
- **Analytics Dashboards:** Monitor uplift curves, ROI, and segment performance

### Use Cases

- **Promotional Campaigns:** Identify customers most influenced by discounts
- **Product Launches:** Target early adopters who need campaign push
- **Win-Back Campaigns:** Re-engage lapsed customers with high uplift potential
- **Loyalty Programs:** Recruit members who will increase engagement
- **Subscription Renewals:** Target customers who need reminder/incentive

### Best Practices

- **Maintain Control Groups:** Always hold out 5-10% for uplift validation
- **Randomize Treatment Assignment:** Ensures causal validity
- **Monitor Sleeping Dogs:** Exclude negative uplift segments
- **Test Campaign Variations:** A/B test different offers, messages, channels
- **Update Models Regularly:** Retrain after each campaign with results
- **Consider Fatigue:** Track contact frequency and adjust targeting

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:182`
