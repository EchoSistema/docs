# Artificial Intelligence – Units Forecast

## Endpoint

```
POST /api/v1/ai/echointel/forecast/units
```

Units Forecast using time series.

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
| 200 OK | Request successful. Returns units forecast results. |
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

The units forecasting system predicts future sales volumes using time-series models optimized for count data:

### Primary Algorithm

The system applies forecasting methods tailored for unit sales predictions:

- **ARIMA:** AutoRegressive Integrated Moving Average for capturing trends and seasonality in unit sales
- **Prophet:** Handles strong seasonal patterns (weekly, monthly, yearly) common in sales data
- **Croston's Method:** Specialized for intermittent demand (products with sporadic sales)
- **Holt-Winters:** Triple exponential smoothing for data with trend and seasonality
- **Integer Constraints:** Ensures forecasts produce whole-number unit predictions

### Processing Steps

1. **Data Validation:** Clean sales history, handle zeros and missing values, detect data quality issues
2. **Seasonality Detection:** Identify seasonal patterns (daily, weekly, monthly cycles) using autocorrelation
3. **Model Selection:** Auto-select optimal model based on data characteristics (regularity, trend, seasonality)
4. **Forecasting:** Generate unit forecasts with confidence intervals (constrained to non-negative integers)
5. **Business Rules:** Apply minimum order quantities, capacity constraints, and promotional adjustments

### Performance

- **Processing Time:** 200-800ms for 12-month forecast across multiple product SKUs
- **Data Requirements:** Minimum 12 months of daily/weekly sales data (24+ months preferred)

## Typical Workflow

### Step 1: Prepare Data
Collect historical unit sales data by SKU, time period, and location. Include promotional calendar, holidays, and known demand drivers.

### Step 2: Configure Parameters
Set forecast horizon (weeks/months), aggregation level (SKU vs product group), seasonality period, and any promotional events to account for.

### Step 3: Make Request
Submit sales history data. System detects patterns, selects optimal model per SKU, and generates unit forecasts with upper/lower bounds.

### Step 4: Analyze Results
Review forecast accuracy on hold-out periods. Examine seasonal patterns and trends. Validate forecasts against sales team input and market knowledge.

### Step 5: Take Action
- **Inventory Planning:** Set reorder points and safety stock based on forecasted demand
- **Production Scheduling:** Align manufacturing capacity with predicted unit requirements
- **Resource Allocation:** Staff warehouses and retail locations according to demand forecasts
- **Sales Targets:** Set realistic sales goals based on data-driven unit predictions

## Related

- [Forecast Units Asyncio](ForecastUnitsAsyncio.md) - Async high-volume unit forecasting
- [Forecast Cost](ForecastCost.md) - Cost forecasting to pair with unit predictions
- [Inventory Optimization](../Inventory/InventoryOptimization.md) - Optimize stock levels using unit forecasts
- [Inventory History Improved](../Inventory/InventoryHistoryImproved.md) - Enhanced inventory analysis
- [Recommend User Items](../Recommendations/RecommendUserItems.md) - Product recommendations

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:229`
