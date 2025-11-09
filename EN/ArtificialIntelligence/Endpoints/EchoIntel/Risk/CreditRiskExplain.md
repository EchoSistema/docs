# Artificial Intelligence – Credit Risk Explanation

## Endpoint

```
POST /api/v1/ai/echointel/risk/credit-explain
```

Credit Risk Explanation with explainable model (XAI).

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
| 200 OK | Request successful. Returns credit risk explanation results. |
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

Credit Risk Explanation provides interpretable insights into credit risk scores using explainability techniques (SHAP values, LIME) to show which factors drive risk assessment decisions, ensuring transparency and regulatory compliance.

### 1. SHAP (SHapley Additive exPlanations): Game-theoretic approach assigns contribution to each feature. Additive: `f(x) = φ₀ + Σφᵢ` where φᵢ is SHAP value for feature i. Positive φ increases risk, negative decreases. TreeSHAP for tree models, KernelSHAP for any model.

### 2. LIME (Local Interpretable Model-agnostic Explanations): Fits interpretable linear model locally around prediction. Perturbs input and observes output changes. Weighted by proximity to original point. Provides local feature importance.

### 3. Feature Attribution: Ranks features by absolute contribution. Positive factors (increase approval/decrease risk): stable income, long credit history, low debt-to-income, no defaults. Negative factors (decrease approval/increase risk): high debt, short employment, recent defaults, many credit inquiries.

### 4. Counterfactual Explanations: "If debt-to-income ratio decreased from 0.45 to 0.35, score would improve by 50 points." Actionable insights for applicants. Minimal changes required for approval.

### 5. Global Interpretability: Feature importance across all predictions. Partial dependence plots show relationship between feature and score. Interaction effects between features.

### 6. Performance: 300-800ms for explanation generation, supports regulatory requirements (GDPR, FCRA), human-understandable output.

## Related

### Related Endpoints

- **[Credit Risk Score](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskScore.md)** - Credit score calculation
- **[Anomaly Accounts](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyAccounts.md)** - Account behavior analysis
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Customer behavior prediction

### Related Domain Concepts

- **Explainable AI (XAI):** SHAP, LIME, feature importance, interpretability
- **Credit Risk:** Default probability, scorecard development, adverse action notices
- **Regulatory Compliance:** Fair lending, GDPR right to explanation, model transparency
- **Feature Attribution:** Contribution analysis, sensitivity analysis

### Integration Points

- **Lending Platforms:** Provide explanations with credit decisions
- **Regulatory Reporting:** Demonstrate model fairness and transparency
- **Customer Service:** Help applicants understand rejection reasons

### Use Cases

- Adverse action notices, model validation and auditing, customer communication, regulatory compliance, model debugging and improvement

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:293`
