# Artificial Intelligence – CLV Features

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/clv-features
```

CLV Features using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns CLV features results. |
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

The CLV feature engineering system extracts predictive features from customer data for lifetime value modeling:

### Primary Algorithm

The system computes statistical and behavioral features that correlate with customer lifetime value:

- **Transactional Features:** Total purchases, average order value, purchase frequency, recency
- **Temporal Features:** Customer tenure, days since first purchase, purchase velocity trends
- **Behavioral Features:** Product diversity, category preferences, discount sensitivity
- **Engagement Features:** Website visits, email opens, support interactions, loyalty program activity

### Processing Steps

1. **Data Aggregation:** Collect customer transactional, behavioral, and demographic data
2. **Feature Engineering:** Calculate derived features (e.g., 30/60/90-day purchase trends)
3. **Normalization:** Standardize features using z-score or min-max scaling
4. **Feature Selection:** Rank features by correlation with actual CLV using mutual information

### Performance

- **Processing Time:** 150-400ms for 5,000 customers with full feature extraction
- **Data Requirements:** Minimum 3 months of customer transaction history

## Related

- [CLV Forecast](CustomerClvForecast.md) - Predict future customer lifetime value
- [Customer Features](CustomerFeatures.md) - Extract general customer features
- [Customer RFM](CustomerRFM.md) - RFM analysis for customer segmentation
- [Customer Segmentation](CustomerSegmentation.md) - Machine learning-based customer segmentation
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Predict product purchase likelihood

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:150`
