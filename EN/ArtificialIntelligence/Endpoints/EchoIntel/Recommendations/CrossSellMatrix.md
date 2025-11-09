# Artificial Intelligence – Cross-Sell Matrix

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/cross-sell-matrix
```

Cross-Sell Matrix using recommendation systems based on collaborative and content-based filtering.

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
| 200 OK | Request successful. Returns cross-sell matrix results. |
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

The Cross-Sell Matrix uses association rule mining (Apriori algorithm) and market basket analysis to discover product affinities and generate cross-sell recommendations based on co-purchase patterns.

### 1. Transaction Data Preparation

**Data Collection:**
- Historical transaction/order data with product combinations
- Basket structure: Transaction ID + list of products purchased together
- Time window: typically 6-12 months of purchase history
- Minimum transactions: 1,000+ baskets for statistical significance

**Data Cleaning:**
- Remove returns and canceled orders
- Filter out one-time bulk purchases (outliers)
- Normalize product identifiers and group variants
- Handle product hierarchies (SKU, category, brand levels)

### 2. Apriori Algorithm for Association Rules

**Frequent Itemset Mining:**
```
Support(X) = Count(Transactions containing X) / Total Transactions
```
- Generate candidate itemsets: {A}, {B}, {A,B}, {A,B,C}, etc.
- Prune itemsets below minimum support threshold (e.g., 0.01 = 1%)
- Use Apriori property: if itemset is infrequent, all supersets are infrequent

**Association Rule Generation:**
```
Rule: {A} → {B}
Support = P(A ∩ B)
Confidence = P(B | A) = P(A ∩ B) / P(A)
Lift = P(A ∩ B) / (P(A) × P(B))
```

**Key Metrics:**
- **Support:** How often products appear together (popularity)
- **Confidence:** Purchase probability of B given A was purchased (reliability)
- **Lift:** How much more likely B is purchased with A vs. independently (strength)
  - Lift > 1: Positive association (cross-sell opportunity)
  - Lift = 1: Independent (no relationship)
  - Lift < 1: Negative association (substitutes or avoid pairing)

### 3. Cross-Sell Matrix Construction

**Matrix Structure:**
```
        Product_1  Product_2  Product_3  ...
Product_1    -      0.85       0.42
Product_2   0.85      -        0.78
Product_3   0.42     0.78       -
```
- Rows: Anchor products (currently owned/viewed)
- Columns: Candidate cross-sell products
- Values: Affinity scores (combining lift, confidence, support)

**Affinity Score Calculation:**
```
Affinity = w1 × Lift + w2 × Confidence + w3 × Support
```
- Weighted combination normalized to [0, 1] range
- Typical weights: w1=0.5, w2=0.3, w3=0.2

### 4. Filtering and Ranking

**Rule Filtering Criteria:**
- Minimum support: 0.01 (appears in 1% of transactions)
- Minimum confidence: 0.15 (15% conversion rate)
- Minimum lift: 1.2 (20% stronger than random)

**Ranking Strategy:**
- Sort by affinity score descending
- Apply business rules (margin, inventory availability, strategic products)
- Personalization layer (customer segment, purchase history)
- Recency boost for trending products

### 5. Performance Characteristics

- **Processing Time:** 1-3 seconds for matrix computation (offline), <200ms for lookup
- **Matrix Size:** Typically 1,000-10,000 products, sparse matrix representation
- **Update Frequency:** Daily or weekly recomputation
- **Accuracy:** Lift >1.5 for top recommendations, 20-40% conversion improvement
- **Scalability:** Handles 100,000+ products with distributed computing

## Typical Workflow

### 1. Data Integration
- Connect to e-commerce platform or order management system
- Extract transaction history with product combinations
- Ensure data quality: valid product IDs, complete transactions

### 2. Matrix Generation (Offline)
- Run Apriori algorithm on transaction data (batch process)
- Generate association rules and compute metrics
- Build cross-sell matrix with affinity scores
- Store matrix in fast lookup database (Redis, in-memory cache)

### 3. API Request
- Provide anchor product(s) currently in customer's cart or viewed
- Optionally include customer segment or purchase history
- Specify number of recommendations needed (top_n)

### 4. Recommendation Retrieval
- Lookup anchor product(s) in cross-sell matrix
- Retrieve top-N products by affinity score
- Apply filters: exclude already purchased, out-of-stock, incompatible
- Personalize based on customer preferences if available

### 5. Display and Track
- Present recommendations: "Frequently bought together", "Customers also bought"
- Track impressions, clicks, and conversions
- Measure lift in average order value (AOV) and units per transaction

### 6. Continuous Improvement
- Update matrix weekly with latest transaction data
- A/B test different affinity score weightings
- Monitor for seasonal changes and trending combinations
- Retrain when new products launch or catalog changes significantly

## Related

### Related Endpoints

- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations
- **[Recommend Similar Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendSimilarItems.md)** - Similar product recommendations
- **[Upsell Suggestions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/UpsellSuggestions.md)** - Upsell opportunities
- **[Propensity Buy Product](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)** - Purchase propensity

### Related Domain Concepts

- **Association Rule Mining:** Apriori, FP-Growth, frequent itemsets
- **Market Basket Analysis:** Support, confidence, lift, conviction
- **Recommendation Systems:** Collaborative filtering, content-based filtering
- **E-commerce Analytics:** Cross-sell, bundle optimization, AOV improvement

### Integration Points

- **E-commerce Platforms:** Shopify, WooCommerce, Magento
- **Point of Sale Systems:** In-store transaction data
- **Product Catalogs:** SKU information, pricing, inventory
- **Marketing Automation:** Triggered cross-sell campaigns
- **Shopping Cart Systems:** Real-time recommendation widgets

### Use Cases

- **"Frequently Bought Together" Widgets:** Amazon-style product bundles
- **Cart Optimization:** Suggest complementary items during checkout
- **Email Marketing:** Cross-sell campaigns to existing customers
- **Product Bundling:** Create pre-packaged bundles based on affinities
- **Inventory Planning:** Stock complementary products together

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:202`
