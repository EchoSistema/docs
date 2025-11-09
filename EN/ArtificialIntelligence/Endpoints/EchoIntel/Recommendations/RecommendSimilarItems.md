# Artificial Intelligence – Similar Items Recommendation

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/similar-items
```

Similar Items Recommendation using recommendation systems based on collaborative and content-based filtering.

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
| 200 OK | Request successful. Returns similar items recommendation results. |
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

The Similar Items Recommendation uses collaborative filtering (user-item interactions), content-based filtering (product attributes), and hybrid approaches to find items similar to a given product using cosine similarity and matrix factorization.

### 1. Collaborative Filtering Approach

**Item-Item Similarity from User Interactions:**
- User-item interaction matrix: rows=users, columns=items, values=ratings/purchases
- Compute item similarity based on co-occurrence patterns
```
Similarity(i, j) = cosine(vector_i, vector_j) = (i · j) / (||i|| × ||j||)
```
- Items with similar user interaction patterns are deemed similar

**Matrix Factorization (SVD, ALS):**
- Decompose user-item matrix into latent factors
- Item similarity in latent space captures deeper patterns
- Handles sparse data better than raw similarity

### 2. Content-Based Filtering

**Product Feature Extraction:**
- Categorical attributes: category, brand, color, size
- Numerical attributes: price, rating, review count
- Text features: product title, description (TF-IDF embeddings)
- Image features: CNN embeddings from product images

**Similarity Calculation:**
```
Content_Similarity = w1×Category_Match + w2×Price_Similarity + w3×Text_Similarity + w4×Image_Similarity
```
- Weights adjusted based on feature importance
- Normalized to [0, 1] range

### 3. Hybrid Recommendation

**Weighted Combination:**
```
Final_Similarity = α×Collaborative_Score + (1-α)×Content_Score
```
- α tuned via cross-validation (typically 0.6-0.7)
- Mitigates cold-start problem (new items with no interactions)

### 4. Ranking and Filtering

**Diversity and Relevance:**
- Penalize items too similar (avoid redundancy)
- Balance similarity with diversity (different categories/brands)
- Boost items with high ratings or recent popularity

**Business Rules:**
- Exclude out-of-stock items
- Filter by price range (±30% of anchor item)
- Same category or complementary categories

### 5. Performance Characteristics

- **Processing Time:** 150-400ms per item
- **Precomputation:** Similarity matrix updated daily (offline)
- **Online Lookup:** Fast retrieval from precomputed matrix
- **Accuracy:** 75-88% user acceptance rate for top-3 recommendations

## Related

### Related Endpoints

- **[Cross-Sell Matrix](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/CrossSellMatrix.md)** - Complementary products
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations
- **[Upsell Suggestions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/UpsellSuggestions.md)** - Higher-tier alternatives

### Related Domain Concepts

- **Collaborative Filtering:** Matrix factorization, SVD, ALS
- **Content-Based Filtering:** Feature similarity, TF-IDF, embeddings
- **Similarity Metrics:** Cosine similarity, Euclidean distance, Jaccard index
- **Cold Start Problem:** Handling new items with no interaction data

### Integration Points

- **E-commerce Platforms:** "Customers also viewed", "Similar items"
- **Product Detail Pages:** Recommendation carousels
- **Search Results:** Alternative product suggestions

### Use Cases

- Product discovery and exploration
- Out-of-stock substitutes
- Size/color variant suggestions
- Alternative brands at similar price points

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:197`
