# Artificial Intelligence – Credit Risk Score

## Endpoint

```
POST /api/v1/ai/echointel/risk/credit-score
```

Calculates credit risk scores for customers using machine learning models that analyze multiple financial and behavioral variables.

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

### Request Body Parameters

| Parameter          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| applicants         | array  | Yes         | Data of credit applicants. |
| credit_amount      | float  | Yes         | Requested credit amount. |
| include_explanation| boolean| No         | Include detailed explanation. Default: `false`. |

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "applicants": [
      {
        "applicant_id": "A001",
        "age": 35,
        "annual_income": 75000,
        "employment_length": 8,
        "debt_to_income": 0.28,
        "credit_history_length": 12,
        "previous_defaults": 0,
        "open_credit_lines": 3
      }
    ],
    "credit_amount": 25000,
    "include_explanation": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/risk/credit-score"
```

## Response

### Success `200 OK`

```json
{
  "credit_scores": [
    {
      "applicant_id": "A001",
      "credit_score": 742,
      "risk_category": "low",
      "approval_recommendation": "approve",
      "default_probability": 0.08,
      "suggested_terms": {
        "max_amount": 30000,
        "interest_rate": 0.065,
        "term_months": 60
      },
      "risk_factors": [
        {
          "factor": "stable employment",
          "impact": "positive",
          "weight": 0.25
        },
        {
          "factor": "low debt-to-income ratio",
          "impact": "positive",
          "weight": 0.22
        },
        {
          "factor": "no previous defaults",
          "impact": "positive",
          "weight": 0.20
        }
      ]
    }
  ],
  "model_version": "v2.5.3",
  "evaluation_date": "2025-01-07T15:30:00Z"
}
```

## JSON Structure

| Field                                      | Type    | Description |
| ------------------------------------------ | ------- | --------- |
| `credit_scores`                            | array   | Scores by applicant. |
| `credit_scores[].applicant_id`             | string  | Applicant ID. |
| `credit_scores[].credit_score`             | int     | Credit score (300-850). |
| `credit_scores[].risk_category`            | string  | Risk category (`low`, `medium`, `high`). |
| `credit_scores[].approval_recommendation`  | string  | Recommendation (`approve`, `review`, `reject`). |
| `credit_scores[].default_probability`      | float   | Default probability (0-1). |
| `credit_scores[].suggested_terms`          | object  | Suggested terms. |
| `credit_scores[].suggested_terms.max_amount` | float | Recommended maximum amount. |
| `credit_scores[].suggested_terms.interest_rate` | float | Suggested interest rate. |
| `credit_scores[].suggested_terms.term_months` | int  | Term in months. |
| `credit_scores[].risk_factors`             | array   | Risk factors. |

## Risk Categories

| Score      | Category | Description |
| ---------- | --------- | --------- |
| 750-850    | Low       | Very low risk, excellent history. |
| 650-749    | Medium-Low| Low risk, good history. |
| 550-649    | Medium    | Moderate risk, requires analysis. |
| 450-549    | Medium-High| High risk, careful analysis required. |
| 300-449    | High      | Very high risk, approval not recommended. |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns credit risk score results. |
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

* Higher scores indicate lower credit risk.
* Factors considered: credit history, income, employment, debts, etc.
* For detailed explanations, use `include_explanation: true`.

## How It Is Computed

Credit Risk Score uses logistic regression and scorecard development to predict default probability (PD), Loss Given Default (LGD), and Exposure at Default (EAD), converting these into credit scores following Basel II/III frameworks.

### 1. Logistic Regression: `P(default) = 1 / (1 + e^-(β₀ + Σβᵢxᵢ))`. Features: income, age, employment length, debt-to-income, credit history length, payment history, credit utilization, recent inquiries. Outputs probability of default (PD).

### 2. Scorecard Development: Convert log-odds to points: `Score = offset + factor × (β₀ + Σβᵢxᵢ)`. Typical range: 300-850 (FICO-style). Calibration: 600 = PD of 2%, doubling odds adds 20 points. Weight of Evidence (WOE) binning for categorical variables.

### 3. Risk Components: **PD (Probability of Default):** Logistic regression output. **LGD (Loss Given Default):** Expected loss % if default occurs (historical recovery rates). **EAD (Exposure at Default):** Credit amount at risk. **Expected Loss:** `EL = PD × LGD × EAD`.

### 4. Feature Engineering: Payment history (on-time %, delinquencies). Credit utilization (balance / limit). Credit mix (revolving, installment, mortgage). Recent credit behavior (new accounts, hard inquiries). Demographic and economic factors. Alternative data (rent, utilities for thin-file applicants).

### 5. Risk Categories: Low risk (700-850): <5% default rate. Medium risk (620-699): 5-15% default rate. High risk (300-619): >15% default rate. Approval thresholds based on risk tolerance and pricing.

### 6. Performance: 250-700ms per applicant, AUC-ROC 0.78-0.88, Gini coefficient 0.55-0.75, monthly model retraining.

## Typical Workflow

### 1. Application: Collect applicant data (income, employment, credit history).
### 2. Scoring: API call with applicant attributes and requested credit amount.
### 3. Risk Assessment: Review credit score, default probability, risk category.
### 4. Decision: Approve/deny based on score thresholds. Adjust terms (interest rate, amount, tenure) based on risk.
### 5. Explanation: Use `include_explanation=true` for adverse action notices or customer communication.
### 6. Monitoring: Track actual default rates vs. predicted, retrain model quarterly or when performance degrades.

## Related

### Related Endpoints

- **[Credit Risk Explain](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskExplain.md)** - Detailed risk explanations
- **[Anomaly Accounts](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyAccounts.md)** - Fraud detection
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Customer behavior modeling

### Related Domain Concepts

- **Credit Scoring:** FICO, VantageScore, scorecard development
- **Basel Framework:** PD, LGD, EAD, expected loss calculation
- **Predictive Modeling:** Logistic regression, random forest, XGBoost
- **Risk Management:** Credit policy, risk-based pricing, portfolio management

### Integration Points

- **Lending Platforms:** Automated underwriting, loan origination systems
- **Banking Core Systems:** Credit decisioning workflows
- **Pricing Engines:** Risk-based pricing, interest rate determination
- **Compliance Systems:** Fair lending monitoring, adverse action reporting

### Use Cases

- Consumer lending (personal loans, credit cards), mortgage underwriting, small business lending, credit line management, portfolio risk assessment

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:288`
