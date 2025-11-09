# Artificial Intelligence – Net Promoter Score

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/nps
```

Net Promoter Score using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns NPS analysis results. |
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

The NPS system calculates Net Promoter Score using standard methodology and provides predictive insights:

### Primary Algorithm

The system implements the standard NPS calculation and augments with ML predictions:

- **NPS Calculation:** NPS = % Promoters (9-10) - % Detractors (0-6), range: -100 to +100
- **Score Classification:** Promoters (9-10), Passives (7-8), Detractors (0-6)
- **Predictive NPS:** Machine learning models predict likely NPS score from customer behavior without survey
- **Trend Analysis:** Time-series analysis identifies NPS trends and seasonal patterns

### Processing Steps

1. **Data Collection:** Gather NPS survey responses or customer behavioral data for prediction
2. **Score Classification:** Categorize responses into Promoters, Passives, Detractors
3. **NPS Calculation:** Compute NPS = (Promoters - Detractors) / Total Responses × 100
4. **Segmentation:** Calculate NPS by customer segment, product, time period, or channel
5. **Prediction (optional):** Use ML model to predict NPS for unsurveyed customers

### Performance

- **Processing Time:** 50-150ms for analyzing 10,000 survey responses
- **Data Requirements:** Minimum 100 responses for reliable NPS calculation

## Related

- [Sentiment Analysis](../Analytics/SentimentAnalysis.md) - Analyze text sentiment from feedback
- [Sentiment Report](../Analytics/SentimentReport.md) - Aggregate sentiment insights
- [Customer Loyalty](CustomerLoyalty.md) - Multi-dimensional loyalty scoring
- [Churn Label](ChurnLabel.md) - Identify customers at risk of churning
- [Customer Segmentation](CustomerSegmentation.md) - Segment customers by NPS category

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:165`
