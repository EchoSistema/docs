# Artificial Intelligence – Purchasing Segmentation

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/purchasing-segmentation
```

Performs customer segmentation based on purchasing patterns, identifying groups with similar behaviors.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). Required if not configured on the server. |
| X-Secret           | string | Conditional | 64-character secret. Required if not configured on the server. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). Default: `en`. |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Body Parameters

| Parameter          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| customer_data      | array  | Yes         | Customer data for segmentation. |
| segmentation_type  | string | No         | Segmentation type (`behavioral`, `value`, `frequency`). |
| time_period        | object | No         | Time period for analysis. |

> **Note:** Parameters accept both `snake_case` and `camelCase` (e.g., `customer_data` or `customerData`).

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_data": [
      {"customer_id": "C001", "total_purchases": 1500, "avg_order_value": 150},
      {"customer_id": "C002", "total_purchases": 3000, "avg_order_value": 300}
    ],
    "segmentation_type": "behavioral"
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/purchasing-segmentation"
```

## Response

### Success `200 OK`

```json
{
  "segments": [
    {
      "segment_id": "high_value",
      "customer_count": 150,
      "avg_purchase_value": 2500,
      "customers": ["C002"]
    },
    {
      "segment_id": "regular",
      "customer_count": 300,
      "avg_purchase_value": 800,
      "customers": ["C001"]
    }
  ]
}
```

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns purchasing segmentation results. |
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

* Segmentation based on machine learning algorithms.
* Supports multiple segmentation criteria.
* Parameters accept `snake_case` or `camelCase`.

## How It Is Computed

The purchasing segmentation system groups customers based on buying behavior patterns using specialized clustering:

### Primary Algorithm

The system focuses on purchase-specific features for behavioral segmentation:

- **Behavioral Clustering:** Groups customers by purchase patterns (frequency, consistency, timing)
- **Value-Based Segmentation:** Segments by spending levels, average order value, price sensitivity
- **Product Affinity:** Clusters by product categories, brands, and cross-category purchases
- **Temporal Patterns:** Identifies seasonal buyers, event-driven purchasers, regular shoppers

### Processing Steps

1. **Feature Engineering:** Extract purchase-specific features (basket size, category diversity, repurchase rate)
2. **Segmentation Type Selection:** Choose behavioral, value, or frequency-based segmentation approach
3. **Clustering:** Apply K-means or hierarchical clustering optimized for purchase data
4. **Segment Profiling:** Calculate average purchase metrics per segment
5. **Labeling:** Assign interpretable names (high-value, bargain hunters, loyal, experimental)

### Performance

- **Processing Time:** 200-500ms for 50,000 customers
- **Data Requirements:** Minimum 3 months of purchase history

## Related

- [Customer Segmentation](CustomerSegmentation.md) - General customer segmentation
- [Customer RFM](CustomerRFM.md) - RFM-based segmentation approach
- [CLV Forecast](CustomerClvForecast.md) - Predict value by purchase segment
- [Cross-Sell Matrix](../Recommendations/CrossSellMatrix.md) - Product cross-sell opportunities
- [Recommend User Items](../Recommendations/RecommendUserItems.md) - Personalized recommendations

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:105`
