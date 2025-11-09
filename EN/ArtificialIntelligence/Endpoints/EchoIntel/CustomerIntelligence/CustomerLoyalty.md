# Artificial Intelligence – Customer Loyalty

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/loyalty
```

Customer Loyalty using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns customer loyalty results. |
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

The customer loyalty scoring system measures customer commitment and predicts future loyalty behavior:

### Primary Algorithm

The system combines multiple loyalty indicators into a composite score:

- **Repeat Purchase Rate:** Frequency of repeat purchases within time windows
- **Brand Engagement:** Interaction with marketing channels, reviews, referrals, social media
- **Product Attachment:** Purchase diversity, category loyalty, brand preference strength
- **Advocacy Indicators:** NPS scores, referrals made, review sentiment, social sharing
- **Resistance to Churn:** Response to competitor offers, price sensitivity, tenure

### Processing Steps

1. **Metric Calculation:** Compute individual loyalty metrics from customer behavior data
2. **Weighted Scoring:** Apply configurable weights to each metric based on business priorities
3. **Normalization:** Scale composite score to 0-100 range for interpretability
4. **Segmentation:** Categorize customers into loyalty tiers (advocates, loyalists, neutrals, at-risk)

### Performance

- **Processing Time:** 150-350ms for scoring 10,000 customers
- **Data Requirements:** Minimum 3 months of customer interaction and transaction history

## Related

- [NPS](NPS.md) - Net Promoter Score for loyalty measurement
- [Customer RFM](CustomerRFM.md) - RFM analysis for behavioral segmentation
- [Churn Label](ChurnLabel.md) - Identify customers at risk of churning
- [Sentiment Analysis](../Analytics/SentimentAnalysis.md) - Analyze customer sentiment from feedback
- [Customer Segmentation](CustomerSegmentation.md) - Segment customers by loyalty levels

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:140`
