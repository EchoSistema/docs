# Artificial Intelligence – Plan Upgrade Propensity

## Endpoint

```
POST /api/v1/ai/echointel/propensity/upgrade-plan
```

Plan Upgrade Propensity using predictive propensity models.

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
| 200 OK | Request successful. Returns plan upgrade propensity results. |
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

The Plan Upgrade Propensity endpoint predicts customer likelihood to upgrade their subscription plan using decision trees, random forests, and propensity scoring algorithms based on usage patterns and engagement signals.

### 1. Feature Engineering

**Subscription Profile Features:**
- Current plan tier (basic, standard, premium)
- Subscription duration (tenure in months)
- Plan price point and payment history
- Billing cycle (monthly, annual)
- Auto-renewal status
- Historical plan changes (downgrades, upgrades)

**Usage Behavior Features:**
- **Feature Utilization:**
  - Usage rate vs. plan limits (storage, API calls, users)
  - Feature adoption rate (% of available features used)
  - Premium feature trial attempts
  - Limit approaching events (nearing quota)

- **Engagement Metrics:**
  - Login frequency (daily, weekly, monthly)
  - Session duration and depth
  - Active days per month
  - Time-of-day and day-of-week patterns
  - Mobile vs. desktop usage

**Value Realization Indicators:**
- Time-to-value metrics (days to first key action)
- ROI indicators (if measurable)
- Goal completion rates
- Integration adoption (connected tools/services)
- Collaboration metrics (multi-user activity)

**Pain Point Signals:**
- **Constraint Indicators:**
  - Hitting plan limits (storage full, user slots filled)
  - Feature access attempts for higher tiers
  - Support tickets about plan limitations
  - Workarounds for missing features

- **Friction Points:**
  - Failed operations due to limits
  - Downgrade considerations (browsing lower plans)
  - Cancellation page visits
  - Competitor comparison activities

**Growth Trajectory Features:**
- User base growth (team size expansion)
- Data volume growth trend
- API usage growth rate
- Revenue/business size growth (for B2B)

**Temporal Features:**
- Days until renewal/end of current period
- Season ality effects (Q4 budgets, January planning)
- Time since last upgrade evaluation
- Days since account creation

### 2. Decision Tree and Random Forest Models

**Decision Tree Structure:**
```
Root: Usage vs. Plan Limits
├─ Branch 1: High Usage (>80% of limits)
│  ├─ Leaf: High Propensity (0.75-0.90)
│  └─ ...
├─ Branch 2: Medium Usage (40-80%)
│  ├─ Sub-branch: High Engagement
│  │  └─ Leaf: Medium-High Propensity (0.55-0.75)
│  └─ ...
└─ Branch 3: Low Usage (<40%)
   └─ Leaf: Low Propensity (0.10-0.30)
```

**Random Forest Ensemble:**
- 200-500 decision trees with bootstrapped samples
- Max depth: 8-15 levels to prevent overfitting
- Min samples per leaf: 20-50 for stability
- Feature subset per split: √(num_features)

**Splitting Criteria:**
- Gini impurity for classification: `Gini = 1 - Σ(pᵢ²)`
- Information gain for maximum separability
- Weighted split based on class balance

**Tree Pruning:**
- Cost-complexity pruning (α parameter)
- Validation set pruning to reduce variance
- Minimum impurity decrease threshold

### 3. Propensity Score Calculation

**Base Propensity from Random Forest:**
```
P(Upgrade) = (Σ Tree_i Predicts Upgrade) / Total Trees
```

**Adjusted Propensity with Contextual Factors:**
```
Adjusted_P = Base_P × Context_Multiplier
```
Where Context_Multiplier considers:
- Renewal proximity: Higher weight near renewal date
- Recent support interactions: Boost if positive, reduce if negative
- Competitor activity: Increase during competitive threats
- Economic conditions: Adjust for budget cycles

**Confidence Score:**
- Variance across ensemble predictions
- Prediction margin (distance from decision boundary)
- Feature importance weight concentration

### 4. Segmentation and Scoring

**Propensity Tiers:**
- **Very High (0.80-1.00):** Immediate upgrade candidates
  - Hitting limits, high engagement, budget available
- **High (0.65-0.80):** Strong upgrade signals
  - Growing usage, positive sentiment, near limits
- **Medium (0.40-0.65):** Nurture candidates
  - Moderate usage, some feature interest
- **Low (0.20-0.40):** Long-term prospects
  - Early in journey, limited feature adoption
- **Very Low (0.00-0.20):** Not ready
  - Low engagement, recent downgrade, churn risk

**Upgrade Readiness Score:**
```
Readiness = w1 × Propensity + w2 × Engagement + w3 × Financial_Capacity + w4 × Timing
```

### 5. Feature Importance and Drivers

**SHAP Value Analysis:**
- Identifies which features contribute most to upgrade propensity
- Positive drivers: usage near limits, feature trial attempts, growth
- Negative drivers: recent downgrade, low engagement, support issues

**Top Upgrade Drivers (Typical):**
1. Usage approaching plan limits (weight: 0.28)
2. Premium feature access attempts (weight: 0.22)
3. Team size growth (weight: 0.18)
4. High session frequency (weight: 0.15)
5. Approaching renewal date (weight: 0.12)

### 6. Recommendation Engine

**Personalized Upgrade Path:**
- Recommend specific next-tier plan based on usage patterns
- Highlight features that customer is most likely to value
- Calculate estimated value increase from upgrade

**Incentive Strategy:**
- **High Propensity:** Minimal incentive needed, focus on value
- **Medium Propensity:** Small discount (5-10%) or trial extension
- **Low Propensity:** Extended trial of premium features to demonstrate value

**Timing Optimization:**
- Best time to approach: High engagement days, near renewal
- Avoid: After support issues, during known busy periods
- Channel preference: In-app message, email, sales call

### 7. Model Evaluation and Validation

**Performance Metrics:**
- **AUC-ROC:** 0.75-0.88 (discrimination ability)
- **Precision@Top-15%:** 0.60-0.78 (accuracy in high propensity segment)
- **Recall@Top-30%:** 0.65-0.82 (coverage of actual upgraders)
- **Lift:** 3.5-6.0x vs. random targeting

**Business Metrics:**
- **Upgrade Conversion Rate:** 15-35% improvement vs. untargeted approach
- **Time to Upgrade:** 25-40% reduction in sales cycle
- **Customer Lifetime Value (CLV) Impact:** 20-45% increase

**Calibration:**
- Predicted propensity aligns with actual upgrade rates
- Reliability diagrams show good calibration across bins
- Brier score < 0.15

### 8. Performance Characteristics

- **Processing Time:** 200-600ms per customer
- **Batch Processing:** 600-1200 customers per minute
- **Model Retraining:** Monthly or when significant product changes occur
- **Feature Freshness:** Daily feature updates from usage database
- **Scalability:** Handles 100,000+ active subscriptions
- **Latency (95th percentile):** < 800ms for real-time scoring

## Related

### Related Endpoints

- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Product purchase propensity
- **[Propensity Respond Campaign](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityRespondCampaign.md)** - Campaign response likelihood
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations
- **[Upsell Suggestions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/UpsellSuggestions.md)** - Upsell opportunities

### Related Domain Concepts

- **Subscription Analytics:** Churn prediction, expansion revenue, cohort analysis
- **Product-Led Growth:** Usage-based selling, value realization, feature adoption
- **Customer Success:** Health scoring, engagement tracking, lifecycle management
- **Decision Trees:** Classification, ensemble methods, feature importance

### Integration Points

- **Subscription Management Systems:** Billing platforms (Stripe, Chargebee)
- **Product Analytics:** Usage tracking (Mixpanel, Amplitude, Segment)
- **CRM Systems:** Salesforce, HubSpot for sales workflow integration
- **Customer Success Platforms:** Gainsight, ChurnZero for proactive outreach
- **Marketing Automation:** Personalized upgrade campaigns

### Use Cases

- **Proactive Upgrade Offers:** Target customers approaching plan limits
- **Renewal Optimization:** Upgrade at renewal for maximum retention
- **Sales Prioritization:** Focus sales team on highest propensity accounts
- **Product Development:** Identify features driving upgrades
- **Pricing Optimization:** Understand willingness to pay for higher tiers
- **Churn Prevention:** Upgrade customers before they hit frustration points

### Best Practices

- **Monitor Feature Usage:** Track which features drive upgrades
- **Timing Matters:** Approach near renewal or during high engagement
- **Personalize Messaging:** Highlight relevant features for each customer
- **Test Incentives:** A/B test discount levels and messaging
- **Track Conversion Funnel:** Monitor upgrade page visits and completion
- **Retrain Regularly:** Update models as product and pricing evolve

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:187`
