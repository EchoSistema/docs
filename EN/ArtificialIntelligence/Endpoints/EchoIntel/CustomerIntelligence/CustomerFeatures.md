# Artificial Intelligence – Customer Features

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/features
```

Customer Features using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns customer features results. |
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

The customer feature extraction system transforms raw customer data into structured features for machine learning models:

### Primary Algorithm

The system computes comprehensive customer features across multiple dimensions:

- **Transactional Features:** Purchase frequency, average order value, total spend, recency, product variety
- **Behavioral Features:** Browse-to-buy ratio, cart abandonment rate, wishlist activity, review participation
- **Engagement Features:** Email open/click rates, app usage frequency, customer service interactions
- **Temporal Features:** Customer lifetime, days since last activity, purchase seasonality patterns
- **Demographic Features:** Age group, location, device preferences, channel preferences

### Processing Steps

1. **Data Collection:** Aggregate customer data from transactional, behavioral, and engagement sources
2. **Feature Calculation:** Compute statistical aggregates (mean, median, std, percentiles) and ratios
3. **Temporal Windowing:** Calculate features over multiple time windows (7/30/90/365 days)
4. **Encoding:** Convert categorical features to numerical representations (one-hot, target encoding)

### Performance

- **Processing Time:** 100-250ms for extracting 50+ features per customer
- **Data Requirements:** Minimum 30 days of customer activity data

## Related

- [CLV Features](CustomerClvFeatures.md) - Specialized features for CLV modeling
- [Customer Segmentation](CustomerSegmentation.md) - Use features for customer clustering
- [Customer RFM](CustomerRFM.md) - RFM scoring based on transactional features
- [Churn Label](ChurnLabel.md) - Churn prediction using customer features
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Purchase propensity modeling

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:135`
