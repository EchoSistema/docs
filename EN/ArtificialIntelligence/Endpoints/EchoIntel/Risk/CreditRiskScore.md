# Artificial Intelligence – Credit Risk Score

## Endpoint

```
POST /api/v1/ai/echointel/risk/credit-score
```

Calculatestes credit risk scores for customers using machine learning models that analyze multiple financial and behavioral variables.

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

### Corpo of the Requisição Parâmetros

| Parameter          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| applicants         | array  | Yes         | Data of credit applicants. |
| credit_amount      | float  | Yes         | Requested credit amount. |
| include_explanation| boolean| No         | Include detailed explanation. Default: `false`. |

## Examples

### Exemplo of Requisição (curl)

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
| `credit_scores[].default_probability`      | float   | Padrão probability (0-1). |
| `credit_scores[].suggested_terms`          | object  | Suggested terms. |
| `credit_scores[].suggested_terms.max_amount` | float | Recommended maximum amount. |
| `credit_scores[].suggested_terms.interest_rate` | float | Suggested interest rate. |
| `credit_scores[].suggested_terms.term_meses` | int  | Term in meses. |
| `credit_scores[].risk_factors`             | array   | Risk factors. |

## Risk Categories

| Score      | Category | Description |
| ---------- | --------- | --------- |
| 750-850    | Low       | Very low risk, excellent history. |
| 650-749    | Medium-Low| Low risk, good history. |
| 550-649    | Medium    | Moderate risk, requires analysis. |
| 450-549    | Medium-High| High risk, careful analysis Obrigatório. |
| 300-449    | High      | Very high risk, approval not recommended. |

## Status HTTP

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns credit risk score results. |
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

* Higher scores indicate lower credit risk.
* Factors considered: credit history, income, employment, debts, etc.
* For detailed explanations, use `include_explanation: true`.

## Como é Calculado

Credit Risk Score uses logistic regression and scorecard development to predict Padrão probability (PD), Loss Given Padrão (LGD), and Exposure at Padrão (EAD), converting these into credit scores following Basel II/III frameworks.

### 1. Logistic Regression: `P(Padrão) = 1 / (1 + e^-(β₀ + Σβᵢxᵢ))`. Features: income, age, employment length, debt-to-income, credit history length, payment history, credit utilization, recent inquiries. Outputs probability of Padrão (PD).

### 2. Scorecard Development: Convert log-odds to points: `Score = offset + factor × (β₀ + Σβᵢxᵢ)`. Typical range: 300-850 (FICO-style). Calibration: 600 = PD of 2%, doubling odds adds 20 points. Weight of Evidence (WOE) binning for categorical variables.

### 3. Risk Components: **PD (Probability of Padrão):** Logistic regression output. **LGD (Loss Given Padrão):** Expected loss % if Padrão occurs (historical recovery rates). **EAD (Exposure at Padrão):** Credit amount at risk. **Expected Loss:** `EL = PD × LGD × EAD`.

### 4. Engenharia of Características: Payment history (on-time %, delinquencies). Credit utilization (balance / limit). Credit mix (revolving, installment, mortgage). Recent credit behavior (new accounts, hard inquiries). Demographic and economic factors. Alternative data (rent, utilities for thin-file applicants).

### 5. Risk Categories: Low risk (700-850): <5% Padrão rate. Medium risk (620-699): 5-15% Padrão rate. High risk (300-619): >15% Padrão rate. Approval thresholds based on risk tolerance and pricing.

### 6. Desempenho: 250-700ms per applicant, AUC-ROC 0.78-0.88, Gini coefficient 0.55-0.75, monthly model retraining.

## Typical Workflow

### 1. Application: Collect applicant data (income, employment, credit history).
### 2. Scoring: API call with applicant attributes and requested credit amount.
### 3. Risk Assessment: Review credit score, Padrão probability, risk category.
### 4. Decision: Approve/deny based on score thresholds. Adjust terms (interest rate, amount, tenure) based on risk.
### 5. Explanation: Use `include_explanation=true` for adverse action notices or customer communication.
### 6. Monitoramento: Track actual Padrão rates vs. predicted, retrain model quarterly or when performance degrades.

## Perguntas Frequentes

### Q: What credit score range is considered acceptable risk for approval?
**A:** The model returns FICO-style scores (300-850). Recommended thresholds: 750+ (Low risk, approve with steard terms), 650-749 (Medium-Low, approve with slight rate premium), 550-649 (Medium, approve with restrictions or require collateral), 450-549 (Medium-High, require manual review), 300-449 (High risk, recommend denial or offer secured product). However, adjust thresholds based on your risk appetite, market conditions, and portfolio targets. The `approval_recommendation` Campo provides our suggested action for each applicant, but your organization's risk policy may override.

### Q: How does the model hele applicants with Não credit history (thin-file or new applicants)?
**A:** For applicants with minimal or Não traditional credit history, the model uses alternative data: employment history and stability, income verification and payment patterns, utility bill payment history, rental payment record, educational background, and bank account behavior. The score may be slightly less predictive than for established-credit applicants—confidence is lower. The Resposta includes `risk_factors` showing which data points drove the decision. Consider requiring additional documentation (employment letter, bank statements) for thin-file applicants scoring in the medium-high or high-risk range.

### Q: Is the credit risk model compliant with fair lending and FCRA regulations?
**A:** Sim. The model is built to comply with the Fair Credit Reporting Act (FCRA), Equal Credit Opportunity Act (ECOA), and fair lending guidelines. It avoids directly using protected characteristics (race, color, religion, national origin, sex, marital status, age) as direct inputs. However, ensure your data collection and use comply with local regulations. Always use `include_explanation: true` for adverse action notices to explain the key factors in the decision. We recommend annual fair lending audits to ensure Não disparate impact across demographic groups.

### Q: How often should credit risk models be validated and retrained?
**A:** Retrain the model quarterly or whenever you accumulate 500+ new loan performance outcomes. Monitor model performance monthly: track actual Padrão rates vs. predicted Padrão probabilities and calculate cumulative AUC-ROC. If AUC-ROC drops below 0.75, prioritize retraining. If you notice systematic prediction Erros for specific customer segments (age groups, income levels), it may indicate model drift requiring retraining. Best practice: maintain to champion-challenger framework testing the current model against to freshly trained challenger quarterly.

### Q: Can I see which factors most impacted the credit score for each applicant?
**A:** Sim! The Resposta includes `risk_factors` array listing the top factors contributing to the score for each applicant, with their impact (positive/negative) and weight (0-1). Example factors: "stable employment" (positive, 0.25), "low debt-to-income ratio" (positive, 0.22), "recent hard inquiries" (negative, 0.15). These factors are human-readable and useful for explaining decisions to applicants and for identifying improvement levers (e.g., "reduce high-utilization credit lines to improve score"). Weights sum to approximately 1.0.

### Q: What's the difference between credit_score, default_probability, and risk_category?
**A:** Credit_score (300-850) is the summary metric on to familiar scale; higher is better. Default_probability (0-1) is the raw ML model output—probability of Padrão within the next 2-3 years. Risk_category (low/medium-low/medium/medium-high/high) is to bucketed version for quick decision-making. All three are correlated but provide different views: use credit_score for customer communication, default_probability for portfolio risk calculations, and risk_category for rapid decision rules. The `approval_recommendation` combines all three to suggest approve/review/reject.

### Q: What of the the "suggested_terms" (max_amount, interest_rate, term_meses) represent?
**A:** These are risk-adjusted recommendations: max_amount is the maximum credit we recommend offering the applicant based on their risk profile and income. Interest_rate is the suggested base rate accounting for credit risk (higher risk = higher rate). Term_meses is the recommended repayment period (longer for lower-risk applicants, shorter to minimize exposure for higher-risk). These are suggestions only—your organization's pricing strategy and appetite may differ. Use these as starting points for pricing engines or as guardrails to prevent overlending to high-risk applicants.

### Q: How is the model's Padrão probability calibrated?
**A:** The model outputs are calibrated probabilities via Platt Scaling and Isotonic Regression, meaning the predicted default_probability directly maps to expected actual Padrão rates. For example, applicants predicted at 0.10 Padrão probability should have an actual 10% Padrão rate in practice. We validate calibration via calibration curves and Expected Calibration Erro (ECE). Monitor actual vs. predicted Padrão rates quarterly: if calibration drifts (e.g., 0.10 predicted = 0.15 actual), retrain to recalibrate. This calibration is critical for portfolio risk management and loss reserve calculations.

### Q: What's the relationship between the score and fair lending compliance?
**A:** The model is designed to minimize fair lending risk, but implementation requires diligence. Avoid using the credit_score as the sole decision criterion—always consider context (economic conditions, life changes, compensating factors). Track approval rates, Padrão rates, and customer outcomes by demographic group for disparate impact analysis. Document your decision rationale (especially for denials/reviews) for FCRA adverse action notices. Use the `include_explanation` Parâmetro to provide clear factor-based reasoning to applicants. Annual fair lending audits comparing model predictions vs. actual outcomes across demographic groups are essential.

## Relacionado

### Relacionado Endpoints

- **[Credit Risk Explain](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskExplain.md)** - Detailed risk explanations
- **[Anomaly Accounts](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyAccounts.md)** - Fraud detection
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Customer behavior modeling

### Relacionado Domain Concepts

- **Credit Scoring:** FICO, VantageScore, scorecard development
- **Basel Framework:** PD, LGD, EAD, expected loss calculation
- **Predictive Modeling:** Logistic regression, reom forest, XGBoost
- **Risk Management:** Credit policy, risk-based pricing, portfolio management

### Integration Points

- **Lending Platforms:** Automated underwriting, loan origination systems
- **Banking Core Systems:** Credit decisioning workflows
- **Pricing Engines:** Risk-based pricing, interest rate determination
- **Compliance Systems:** Fair lending monitoring, adverse action reporting

### Use Cases

- Consumer lending (personal loans, credit cards), mortgage underwriting, small business lending, credit line management, portfolio risk assessment

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:288`
