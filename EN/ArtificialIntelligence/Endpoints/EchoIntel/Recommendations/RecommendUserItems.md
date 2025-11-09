# Artificial Intelligence – User Item Recommendations

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/user-items
```

Generates personalized product/item recommendations for specific users based on collaborative filtering and content-based filtering.

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

### Request Body Parameters

| Parameter     | Type   | Required | Description |
| ------------- | ------ | ----------- | --------- |
| user_id       | string | Yes         | User ID. |
| n_recommendations | int | No       | Number of recommendations. Default: `10`. |
| exclude_purchased | boolean | No   | Exclude already purchased items. Default: `true`. |
| filters       | object | No         | Additional filters (category, price, etc.). |

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "U001",
    "n_recommendations": 5,
    "exclude_purchased": true,
    "filters": {
      "category": "electronics",
      "max_price": 1000
    }
  }' \
  "https://echosistema.online/api/v1/ai/echointel/recommendations/user-items"
```

## Response

### Success `200 OK`

```json
{
  "user_id": "U001",
  "recommendations": [
    {
      "item_id": "ITEM-789",
      "score": 0.94,
      "rank": 1,
      "reason": "Based on your recent purchases and similar users",
      "item_details": {
        "name": "Wireless Headphones Pro",
        "category": "electronics",
        "price": 299.99
      }
    },
    {
      "item_id": "ITEM-456",
      "score": 0.88,
      "rank": 2,
      "reason": "Frequently bought together with your previous items",
      "item_details": {
        "name": "Smartphone Case Premium",
        "category": "accessories",
        "price": 49.99
      }
    }
  ],
  "algorithm_used": "hybrid_collaborative_content",
  "confidence": 0.91
}
```

## JSON Structure

| Field                              | Type    | Description |
| ---------------------------------- | ------- | --------- |
| `user_id`                          | string  | User ID. |
| `recommendations`                  | array   | List of recommendations. |
| `recommendations[].item_id`        | string  | Recommended item ID. |
| `recommendations[].score`          | float   | Relevance score (0-1). |
| `recommendations[].rank`           | int     | Recommendation ranking. |
| `recommendations[].reason`         | string  | Recommendation reason. |
| `recommendations[].item_details`   | object  | Item details. |
| `algorithm_used`                   | string  | Algorithm used. |
| `confidence`                       | float   | Overall confidence (0-1). |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns user items recommendation results. |
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

* Recommendations are ordered by descending score.
* Available algorithms: `collaborative`, `content_based`, `hybrid`.
* Recommendations are updated in real-time as new interactions occur.

## How It Is Computed

The Recommend User Items endpoint uses hybrid filtering (collaborative + content-based) combining user behavior patterns with item attributes for personalized recommendations via matrix factorization and similarity algorithms.

### 1. Collaborative Filtering: Matrix factorization (ALS/SVD) decomposes user-item matrix into latent factors. User/item-based similarity using cosine similarity. Handles sparsity via dimensionality reduction.

### 2. Content-Based Filtering: User profile from liked items' features. TF-IDF for text, CNN embeddings for images. Matches items to user preferences.

### 3. Hybrid Strategy: Weighted combination (α×Collab + (1-α)×Content). Switches based on data availability. Deep learning: Neural Collaborative Filtering.

### 4. Ranking: Contextual factors (time, location). Diversity/novelty balance. Business rules (margin, inventory, exclusions).

### 5. Performance: 200-600ms, 72-85% CTR, 15-30% conversion uplift, daily retraining.

## Typical Workflow

### 1. Integration: Connect user interactions (purchases, views, ratings) and product catalog.
### 2. Training: Offline model training, precompute embeddings/profiles.
### 3. Request: Submit user_id, n_recommendations, filters.
### 4. Scoring: Real-time candidate generation, ranking, filtering.
### 5. Delivery: Top-N items with scores/explanations, track for feedback.
### 6. Optimization: Monitor CTR/conversion, A/B test, retrain weekly.

## Related

### Related Endpoints

- **[Recommend Similar Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendSimilarItems.md)** - Item-to-item recommendations
- **[Cross-Sell Matrix](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/CrossSellMatrix.md)** - Complementary products
- **[Upsell Suggestions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/UpsellSuggestions.md)** - Premium alternatives
- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Purchase likelihood

### Related Domain Concepts

- **Collaborative Filtering:** User/item-based, matrix factorization (ALS, SVD, NCF)
- **Content-Based Filtering:** Feature matching, TF-IDF, neural embeddings
- **Hybrid Systems:** Weighted, switching, feature-augmented approaches
- **Cold Start:** New user/item handling strategies

### Integration Points

- **E-commerce:** Homepage personalization, product pages, cart recommendations
- **Email Marketing:** Personalized product selections
- **Mobile Apps:** "For You" sections, push notifications
- **CRM:** Sales insights on customer preferences

### Use Cases

- Homepage personalization, email campaigns, mobile app recommendations, post-purchase suggestions, cart abandonment recovery

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:192`
