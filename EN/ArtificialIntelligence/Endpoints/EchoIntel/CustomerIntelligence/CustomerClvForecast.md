# Artificial Intelligence – CLV Forecast

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/clv-forecast
```

CLV Forecast using artificial intelligence and machine learning.

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


## Response

Consult the official documentation for complete response format.

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns CLV forecast results. |
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

The CLV forecasting system uses probabilistic and regression models to predict future customer lifetime value:

### Primary Algorithm

The system employs multiple modeling approaches for accurate CLV prediction:

- **Probabilistic Models:** BG/NBD (Beta-Geometric/Negative Binomial Distribution) for purchase frequency and Gamma-Gamma for monetary value
- **Machine Learning:** Gradient boosting (XGBoost, LightGBM) trained on customer features to predict future spend
- **Cohort Analysis:** Segments customers by acquisition cohort and applies cohort-specific models
- **Ensemble Prediction:** Combines multiple model outputs using weighted averaging for robust forecasts

### Processing Steps

1. **Feature Preparation:** Extract CLV-predictive features (RFM, tenure, engagement metrics)
2. **Model Selection:** Choose appropriate model based on data characteristics and business context
3. **Prediction Generation:** Apply trained model to generate CLV forecasts with confidence intervals
4. **Value Segmentation:** Categorize customers by predicted CLV (high/medium/low value)

### Performance

- **Processing Time:** 200-600ms for 10,000 customer CLV predictions
- **Data Requirements:** Minimum 6 months of transaction history per customer

## Typical Workflow

### Step 1: Prepare Data
Collect historical customer transaction data including purchase dates, amounts, customer IDs, and any available demographic or behavioral features. Ensure data quality by handling missing values and outliers.

### Step 2: Configure Parameters
Specify the prediction horizon (e.g., 12 months), model type (probabilistic vs ML), and any cohort segmentation criteria. Set confidence level for prediction intervals (default: 95%).

### Step 3: Make Request
Send POST request with customer transaction data and configuration parameters. The API will automatically select optimal models and generate CLV forecasts for each customer.

### Step 4: Analyze Results
Review predicted CLV values, confidence intervals, and customer value segments. Identify high-value customers for retention efforts and low-value customers for cost optimization.

### Step 5: Take Action
- **High CLV Customers:** Assign dedicated account managers, offer premium services, prioritize support
- **Medium CLV Customers:** Implement upsell/cross-sell campaigns, loyalty programs
- **Low CLV Customers:** Automate service delivery, optimize acquisition costs, consider churn risk

## Related

- [CLV Features](CustomerClvFeatures.md) - Extract features for CLV modeling
- [Customer RFM](CustomerRFM.md) - RFM analysis for value-based segmentation
- [Customer Segmentation](CustomerSegmentation.md) - Machine learning customer segmentation
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Product purchase propensity scoring
- [Recommend User Items](../Recommendations/RecommendUserItems.md) - Personalized product recommendations

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:155`
