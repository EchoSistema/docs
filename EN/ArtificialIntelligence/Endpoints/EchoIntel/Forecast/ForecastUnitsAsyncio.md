# Artificial Intelligence – Units Forecast (Async)

## Endpoint

```
POST /api/v1/ai/echointel/forecast/units-asyncio
```

Units Forecast (Async) asynchronous version optimized for large volumes.

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
| 200 OK | Request successful. Returns asynchronous units forecast results. |
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

The async units forecasting system uses parallel processing for high-volume SKU forecasting at scale:

### Primary Algorithm

The system employs asynchronous processing for efficient large-scale forecasting:

- **Parallel Model Training:** Distributes forecasting across multiple SKUs using async IO and worker pools
- **Batched Predictions:** Groups similar SKUs for batch processing to optimize computational efficiency
- **Model Sharing:** Applies hierarchical forecasting where similar products share model parameters
- **Incremental Updates:** Supports incremental retraining for only changed SKUs rather than full recomputation
- **Result Streaming:** Returns forecasts progressively as they complete rather than waiting for all SKUs

### Processing Steps

1. **SKU Partitioning:** Group SKUs by similarity (category, sales pattern) for efficient parallel processing
2. **Async Execution:** Launch parallel forecasting tasks for each SKU group using async workers
3. **Model Caching:** Cache trained models for SKUs with stable patterns to reduce computation
4. **Progressive Results:** Stream completed forecasts to client as they finish
5. **Aggregation:** Consolidate individual SKU forecasts into category and total forecasts

### Performance

- **Processing Time:** 2-10s for 1,000+ SKUs (vs 30+ minutes sequential)
- **Scalability:** Handles 10,000+ SKUs efficiently using distributed processing
- **Data Requirements:** Same as standard units forecast (12+ months per SKU)

## Typical Workflow

### Step 1: Prepare Data
Compile historical sales data for large SKU catalog (hundreds to thousands of products). Organize by SKU ID with consistent time periods.

### Step 2: Configure Parameters
Set forecast parameters, enable async mode, configure parallelism level based on available resources, and specify result delivery method (webhook, polling, streaming).

### Step 3: Make Request
Submit large-scale forecasting request. API initiates async processing, returns job ID for tracking, and begins streaming results as SKU forecasts complete.

### Step 4: Analyze Results
Monitor forecast job progress via status endpoint. Retrieve completed forecasts incrementally. Aggregate results by product category for business review.

### Step 5: Take Action
- **Enterprise Planning:** Forecast entire product catalog for company-wide demand planning
- **Supply Chain:** Optimize global inventory allocation across warehouses and regions
- **Procurement:** Generate purchase orders for thousands of SKUs based on forecasts
- **Portfolio Management:** Identify slow-moving SKUs for clearance or discontinuation

## Related

- [Forecast Units](ForecastUnits.md) - Standard synchronous unit forecasting
- [Forecast Cost](ForecastCost.md) - Cost forecasting for budget planning
- [Forecast Cost Improved](ForecastCostImproved.md) - Advanced cost predictions
- [Inventory Optimization](../Inventory/InventoryOptimization.md) - Multi-SKU inventory optimization
- [Excess Inventory NLP](../Inventory/ExcessInventoryNlp.md) - Identify overstocked items

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:234`
