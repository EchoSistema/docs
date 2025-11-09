# Artificial Intelligence – Improved Cost Forecast

## Endpoint

```
POST /api/v1/ai/echointel/forecast/cost-improved
```

Improved Cost Forecast enhanced version with more accurate algorithms.

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
| 200 OK | Request successful. Returns improved cost forecast results. |
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

The improved cost forecasting system uses enhanced algorithms with advanced feature engineering for superior accuracy:

### Primary Algorithm

The system combines state-of-the-art forecasting techniques with machine learning enhancements:

- **Advanced Prophet:** Enhanced Prophet with custom seasonality models and external regressors (economic indicators, events)
- **SARIMA:** Seasonal ARIMA with automatic order selection using AIC/BIC criteria
- **LightGBM:** Gradient boosting with engineered features (lags, rolling statistics, calendar features)
- **Neural Networks:** LSTM/GRU models for capturing complex non-linear temporal patterns
- **Hybrid Ensemble:** Stacks multiple models using meta-learning for optimal prediction accuracy

### Processing Steps

1. **Feature Engineering:** Create lag features, rolling means, seasonal indicators, holiday effects, and domain-specific variables
2. **Outlier Detection:** Use statistical methods (IQR, Z-score) and domain knowledge to identify anomalies
3. **Model Ensemble:** Train multiple models in parallel and combine using validation performance weights
4. **Uncertainty Quantification:** Generate probabilistic forecasts with calibrated prediction intervals
5. **Validation:** Perform walk-forward validation and calculate accuracy metrics (MAPE <10% target)

### Performance

- **Processing Time:** 500ms-2.5s for 12-month forecast with comprehensive feature engineering
- **Data Requirements:** Minimum 18 months of historical data (36+ months recommended for best accuracy)
- **Accuracy Improvement:** 15-30% better MAPE compared to standard forecast methods

## Typical Workflow

### Step 1: Prepare Data
Gather historical cost data with rich context: timestamps, cost categories, influencing factors (promotions, seasonality, market conditions). Clean and validate data quality.

### Step 2: Configure Parameters
Set forecast horizon, specify external regressors (if available), configure confidence levels, and enable advanced features like anomaly detection and seasonality decomposition.

### Step 3: Make Request
Submit comprehensive historical dataset. System performs feature engineering, trains ensemble models, and generates highly accurate forecasts with uncertainty estimates.

### Step 4: Analyze Results
Examine forecast accuracy metrics, confidence intervals, and model diagnostics. Review which features most influence predictions. Validate against business domain knowledge.

### Step 5: Take Action
- **Strategic Planning:** Use improved forecasts for long-term budgeting and investment decisions
- **Risk Management:** Leverage prediction intervals to quantify forecast uncertainty and plan scenarios
- **Process Optimization:** Identify cost drivers from feature importance analysis
- **Continuous Improvement:** Monitor forecast accuracy and retrain models with new data

## Related

- [Forecast Cost](ForecastCost.md) - Standard cost forecasting baseline
- [Forecast Cost Totus](ForecastCostTotus.md) - Multi-variable total cost forecasting
- [Forecast Units](ForecastUnits.md) - Unit sales forecasting for cost correlation
- [Forecast Units Asyncio](ForecastUnitsAsyncio.md) - High-volume async unit forecasting
- [Inventory Optimization](../Inventory/InventoryOptimization.md) - Inventory planning based on forecasts

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:244`
