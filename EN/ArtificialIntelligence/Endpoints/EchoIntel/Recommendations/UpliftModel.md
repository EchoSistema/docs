# Artificial Intelligence – Uplift Model

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/uplift-model
```

Uplift Model using recommendation systems based on collaborative and content-based filtering.

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
| 200 OK | Request successful. Returns uplift model results. |
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

Uplift modeling estimates treatment effect heterogeneity using causal inference (CATE estimation, meta-learners: T/S/X/R-Learner) to identify individuals most responsive to interventions, measuring `Uplift = P(Response|Treatment) - P(Response|Control)`.

### 1. Causal Framework: Two-model approach (treatment/control groups) or causal forests for heterogeneous effects. Propensity score matching and inverse weighting for bias correction.

### 2. CATE Estimation: Conditional Average Treatment Effect per individual. Meta-learners (T/S/X/R-Learner) for personalized uplift. Doubly robust estimators combining models.

### 3. Segmentation: Persuadables (high uplift, target), Sure Things (deprioritize), Lost Causes (exclude), Sleeping Dogs (negative uplift, avoid).

### 4. Evaluation: Qini coefficient, uplift curves, AUUC. Campaign ROI: 20-50% improvement vs. random targeting.

### 5. Performance: 300-900ms, Uplift AUC 0.62-0.78, retraining after campaigns with A/B results.

## Related

### Related Endpoints

- **[Propensity Respond Campaign](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityRespondCampaign.md)** - Campaign response uplift
- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Purchase propensity
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations

### Related Domain Concepts

- **Uplift Modeling:** Causal inference, treatment effects, heterogeneous effects
- **Causal ML:** CATE, meta-learners, propensity scores, doubly robust
- **A/B Testing:** Randomization, incrementality, statistical significance
- **Intervention Optimization:** Campaign targeting, resource allocation

### Integration Points

- **Marketing Automation:** Target persuadables, avoid sleeping dogs
- **CRM Systems:** Prioritize high-uplift customers
- **Experimentation Platforms:** A/B testing and validation

### Use Cases

- Marketing campaign targeting, product recommendations, promotional offers, retention interventions, personalized pricing

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:217`
