# Artificial Intelligence – Total Cost Forecast

## Endpoint

```
POST /api/v1/ai/echointel/forecast/cost-totus
```

Total Cost Forecast considering multiple variables.

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
| 200 OK | Request successful. Returns total cost forecast results. |
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

The total cost forecasting system aggregates multiple cost components and variables for comprehensive predictions:

### Primary Algorithm

The system uses multi-variate forecasting to predict total costs across all business dimensions:

- **Vector Autoregression (VAR):** Models interdependencies between multiple cost time series simultaneously
- **Multi-Task Learning:** Jointly forecasts related cost streams (labor, materials, overhead) using shared representations
- **Bottom-Up Aggregation:** Forecasts individual cost components and aggregates to total cost
- **Constraint Optimization:** Ensures forecast consistency across cost categories and budget constraints
- **Scenario Modeling:** Generates forecasts under different business scenarios (optimistic, pessimistic, realistic)

### Processing Steps

1. **Component Decomposition:** Break down total costs into constituent categories (fixed, variable, direct, indirect)
2. **Multi-variate Modeling:** Train VAR or multi-task models capturing cross-category dependencies
3. **Individual Forecasts:** Generate forecasts for each cost component with uncertainty estimates
4. **Aggregation:** Sum component forecasts to produce total cost prediction
5. **Reconciliation:** Apply hierarchical reconciliation to ensure consistency across all levels

### Performance

- **Processing Time:** 800ms-3s for forecasting 10+ cost categories over 12 months
- **Data Requirements:** Minimum 12 months of data across all cost categories

## Typical Workflow

### Step 1: Prepare Data
Collect comprehensive cost data across all categories: labor, materials, overhead, marketing, etc. Ensure consistent timestamps and category definitions across historical data.

### Step 2: Configure Parameters
Define cost category structure, forecast horizon, inter-category relationships, and budget constraints. Specify scenario parameters if needed.

### Step 3: Make Request
Submit multi-category historical cost data. System models dependencies, generates component forecasts, and aggregates to total cost with breakdown by category.

### Step 4: Analyze Results
Review total cost forecast and component contributions. Examine which categories drive total cost growth. Validate aggregated forecasts against budget targets.

### Step 5: Take Action
- **Comprehensive Budgeting:** Allocate budgets across all cost categories based on forecasts
- **Cost Trade-offs:** Identify opportunities to shift spending between categories
- **Variance Analysis:** Compare actual vs forecasted costs to identify root causes
- **Strategic Decisions:** Use total cost projections for pricing, profitability, and investment planning

## Related

- [Forecast Cost](ForecastCost.md) - Single-metric cost forecasting
- [Forecast Cost Improved](ForecastCostImproved.md) - Enhanced cost forecasting algorithms
- [Forecast Units](ForecastUnits.md) - Unit sales forecasting for cost-revenue correlation
- [CLV Forecast](../CustomerIntelligence/CustomerClvForecast.md) - Customer lifetime value forecasting
- [Inventory Optimization](../Inventory/InventoryOptimization.md) - Optimize inventory carrying costs

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:249`
