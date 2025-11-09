# Artificial Intelligence – Customer Journey (Markov)

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-markov
```

Customer Journey (Markov) with Markov chains.

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
| 200 OK | Request successful. Returns customer journey Markov analysis results. |
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

The journey analysis uses Markov chain modeling to understand customer path probabilities and conversion likelihood:

### 1. Markov Chain Construction

The system builds a probabilistic state machine from customer journey data:

- **State Identification:** Each unique touchpoint (channel, page, action) becomes a state in the Markov chain
- **Transition Matrix:** Calculates probability P(state_j | state_i) for all state pairs based on observed journey sequences
- **Absorbing States:** Defines terminal states (conversion, abandonment) that customers cannot leave once entered

### 2. Transition Probability Calculation

- **Step 1:** Count all observed transitions from state i to state j across all customer journeys
- **Step 2:** Normalize by total transitions from state i to get P(j|i) = count(i→j) / sum(count(i→*))
- **Step 3:** Handle sparse data using Laplace smoothing (add-one smoothing) for rare transitions

### 3. Journey Analytics

- **Conversion Probability:** Calculate likelihood of reaching conversion from any state using absorbing chain analysis
- **Expected Path Length:** Compute mean number of steps to conversion or abandonment
- **Critical Paths:** Identify high-probability sequences that lead to conversion using Viterbi algorithm

### 4. Performance and Optimization

- **Processing Time:** 200-400ms for 50,000 journey sequences
- **Data Requirements:** Minimum 500 complete journeys for stable transition probabilities
- **Memory Efficiency:** Uses sparse matrix representation for large state spaces (10,000+ states)

## Related

- [Channel Attribution](ChannelAttribution.md) - Attribution modeling for marketing channel effectiveness
- [Journey Sequences](JourneySequences.md) - Sequential pattern mining for customer journeys
- [Propensity to Buy Product](../Propensity/PropensityBuyProduct.md) - Predict likelihood of product purchase
- [Recommend User Items](../Recommendations/RecommendUserItems.md) - Personalized item recommendations

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:303`
