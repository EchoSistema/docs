# Artificial Intelligence – Journey Sequences

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-sequences
```

Journey Sequences identifying patterns.

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
| 200 OK | Request successful. Returns journey sequence analysis results. |
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

The journey sequence analysis uses sequential pattern mining algorithms to discover frequent and meaningful customer behavior patterns:

### 1. Sequential Pattern Mining

The system employs advanced pattern discovery algorithms:

- **Apriori-Based Mining:** Uses sequential Apriori algorithm to find frequent subsequences meeting minimum support threshold
- **Prefix Span Algorithm:** Efficiently mines sequential patterns using pattern-growth methodology for large datasets
- **Constraint-Based Filtering:** Applies gap constraints, time windows, and minimum/maximum length filters

### 2. Pattern Discovery Process

- **Step 1:** Convert raw journey data into ordered sequences of events with timestamps
- **Step 2:** Mine frequent sequences using minimum support (default: 1% of all journeys)
- **Step 3:** Identify closed sequences (maximal patterns that subsume smaller patterns)
- **Step 4:** Rank patterns by support, confidence, and lift metrics

### 3. Sequence Analysis

- **Frequency Analysis:** Calculate absolute and relative support for each discovered pattern
- **Temporal Patterns:** Identify time-based patterns (hourly, daily, seasonal trends in sequences)
- **Divergence Detection:** Find sequences that differentiate converters from non-converters

### 4. Performance and Optimization

- **Processing Time:** 300-600ms for 100,000 journey sequences
- **Data Requirements:** Minimum 1,000 journeys for meaningful pattern discovery
- **Scalability:** Parallel mining for datasets exceeding 1 million sequences
- **Pattern Pruning:** Automatically removes redundant and statistically insignificant patterns

## Related

- [Customer Journey (Markov)](JourneyMarkov.md) - Markov chain analysis for journey paths
- [Channel Attribution](ChannelAttribution.md) - Marketing channel attribution modeling
- [Customer Segmentation](../CustomerIntelligence/CustomerSegmentation.md) - Group customers by behavioral patterns
- [Propensity to Respond to Campaign](../Propensity/PropensityRespondCampaign.md) - Predict campaign response likelihood

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:308`
