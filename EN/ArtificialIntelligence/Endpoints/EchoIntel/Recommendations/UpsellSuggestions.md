# Artificial Intelligence – Upsell Suggestions

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/upsell-suggestions
```

Upsell Suggestions using recommendation systems based on collaborative and content-based filtering.

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
| 200 OK | Request successful. Returns upsell suggestions results. |
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

Upsell Suggestions identifies higher-value alternatives using product affinity scoring, upgrade path analysis, and basket analysis to recommend premium versions, add-ons, or bundles that increase customer lifetime value.

### 1. Product Hierarchy: Mapping products to tiers (basic→standard→premium). Identifying upgrade paths with price differentials. Feature comparison matrices.

### 2. Affinity Scoring: Historical upgrade patterns (customers who bought X upgraded to Y). Basket analysis for complementary premium add-ons. `Upsell_Score = Affinity × Value_Delta × Propensity`.

### 3. Customer Segmentation: Willingness-to-pay estimation. High-value customers get premium suggestions. Price-sensitive customers get modest upsells.

### 4. Contextualization: Cart value thresholds (suggest upsell at specific cart values). Product lifecycle stage (new buyers vs. renewals). Seasonality and promotions.

### 5. Performance: 200-500ms, 18-35% upsell conversion rate, 25-45% AOV increase.

## Related

### Related Endpoints

- **[Cross-Sell Matrix](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/CrossSellMatrix.md)** - Complementary products
- **[Recommend User Items](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)** - Personalized recommendations
- **[Propensity Upgrade Plan](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Propensity/PropensityUpgradePlan.md)** - Upgrade propensity
- **[Dynamic Pricing](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Recommendations/DynamicPricingRecommend.md)** - Price optimization

### Related Domain Concepts

- **Upselling:** Premium upgrades, value ladders, tier progression
- **Product Affinity:** Upgrade patterns, basket analysis
- **Customer Value Optimization:** CLV, AOV, revenue per customer
- **Pricing Psychology:** Price anchoring, value perception

### Integration Points

- **E-commerce:** Product pages, cart, checkout upsells
- **Sales Tools:** Sales rep recommendations, quote optimization
- **Subscription Management:** Plan upgrade suggestions
- **Point of Sale:** In-store upgrade offers

### Use Cases

- Product page premium suggestions, cart value optimization, subscription upgrades, warranty/insurance add-ons, service tier increases

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:207`
