# Artificial Intelligence – Channel Attribution

## Endpoint

```
POST /api/v1/ai/echointel/analytics/channel-attribution
```

Channel Attribution using attribution models.

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
| 200 OK | Request successful. Returns channel attribution results. |
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

The channel attribution system uses multiple attribution models to assign credit for conversions across marketing touchpoints:

### 1. Attribution Models

The system supports five standard attribution models:

- **First-Touch Attribution:** Assigns 100% credit to the first touchpoint in the customer journey
- **Last-Touch Attribution:** Assigns 100% credit to the final touchpoint before conversion
- **Linear Attribution:** Distributes credit equally across all touchpoints in the journey
- **Time-Decay Attribution:** Assigns exponentially increasing credit to touchpoints closer to conversion
- **Shapley Value Attribution:** Uses game theory to calculate fair credit distribution based on marginal contribution

### 2. Computation Process

- **Step 1:** Aggregate customer journey paths from all conversion and non-conversion events
- **Step 2:** For each model, calculate attribution weights based on touchpoint positions and timestamps
- **Step 3:** Apply weights to conversion values to determine channel contribution
- **Step 4:** Normalize results to ensure total attribution equals 100% for each conversion path

### 3. Performance and Optimization

- **Processing Time:** 150-300ms for 10,000 journey paths
- **Data Requirements:** Minimum 100 complete customer journeys for reliable attribution
- **Scalability:** Handles up to 1 million touchpoints per request using distributed computation

### 4. Model Selection

- **Shapley Value** is recommended for the most statistically rigorous attribution but has higher computational cost (O(n!))
- **Time-Decay** provides a good balance between accuracy and performance for most use cases
- **First/Last-Touch** are useful for baseline comparisons and simple attribution scenarios

## Related

- [Customer Journey (Markov)](JourneyMarkov.md) - Analyze customer journey paths using Markov chain probabilities
- [Journey Sequences](JourneySequences.md) - Discover frequent patterns in customer journey sequences
- [Sentiment Analysis](SentimentAnalysis.md) - Analyze sentiment of customer feedback and reviews
- [Customer Segmentation](../CustomerIntelligence/CustomerSegmentation.md) - Segment customers based on behavior and characteristics

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:298`
