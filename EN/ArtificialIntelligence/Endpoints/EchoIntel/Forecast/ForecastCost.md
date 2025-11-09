# Artificial Intelligence – Cost Forecast

## Endpoint

```
POST /api/v1/ai/echointel/forecast/cost
```

Cost Forecast using predictive models.

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
| 200 OK | Request successful. Returns cost forecast results. |
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

The cost forecasting system uses advanced time-series models to predict future costs based on historical patterns:

### Primary Algorithm

The system employs multiple forecasting algorithms and selects the best performer:

- **ARIMA (AutoRegressive Integrated Moving Average):** Captures trends, seasonality, and autocorrelation in cost data
- **Prophet:** Facebook's robust forecasting tool designed for business time series with strong seasonal effects
- **ETS (Exponential Smoothing):** State-space models for trend and seasonality components
- **Ensemble Methods:** Combines multiple model predictions using weighted averaging for improved accuracy

### Processing Steps

1. **Data Preprocessing:** Clean historical cost data, handle missing values, detect and remove outliers
2. **Decomposition:** Separate time series into trend, seasonal, and residual components
3. **Model Selection:** Automatically select best-performing model based on validation set accuracy (MAPE, RMSE)
4. **Forecasting:** Generate point forecasts and prediction intervals (80%, 95% confidence levels)
5. **Post-processing:** Apply business constraints (non-negative costs, max/min bounds)

### Performance

- **Processing Time:** 300ms-1.5s for forecasting 12 months with 24+ months of history
- **Data Requirements:** Minimum 12 months of historical cost data (24+ months recommended)

## Typical Workflow

### Step 1: Prepare Data
Collect historical cost data with timestamps (daily, weekly, or monthly). Ensure consistent time intervals and identify any known anomalies (promotional periods, one-time events).

### Step 2: Configure Parameters
Specify forecast horizon (e.g., 3, 6, 12 months), seasonality period (weekly, monthly, yearly), and any external regressors (holidays, events, economic indicators).

### Step 3: Make Request
Submit historical cost time series via API. System automatically selects optimal model, tunes hyperparameters, and generates forecasts with confidence intervals.

### Step 4: Analyze Results
Review forecast trends, confidence intervals, and model diagnostics. Compare forecasts against business expectations and domain knowledge. Check for unrealistic predictions.

### Step 5: Take Action
- **Budget Planning:** Use forecasts to set departmental budgets and cost targets
- **Resource Allocation:** Adjust staffing and procurement based on predicted cost trends
- **Cost Control:** Implement controls if forecasts exceed targets or show concerning trends
- **Monitoring:** Track actual costs against forecasts and retrain models quarterly

## Related

- [Forecast Cost Improved](ForecastCostImproved.md) - Enhanced cost forecasting with advanced algorithms
- [Forecast Cost Totus](ForecastCostTotus.md) - Total cost forecasting across multiple variables
- [Forecast Units](ForecastUnits.md) - Forecast unit sales to correlate with cost planning
- [Inventory Optimization](../Inventory/InventoryOptimization.md) - Optimize inventory based on cost forecasts
- [CLV Forecast](../CustomerIntelligence/CustomerClvForecast.md) - Forecast customer lifetime value

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:239`
