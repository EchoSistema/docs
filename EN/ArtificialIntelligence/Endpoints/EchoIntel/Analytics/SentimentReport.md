# Artificial Intelligence – Sentiment Report

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-report
```

Sentiment Report consolidating multiple sources.

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
| 200 OK | Request successful. Returns sentiment report results. |
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

The sentiment report aggregates and analyzes sentiment data from multiple sources to provide comprehensive insights:

### 1. Data Aggregation

The system consolidates sentiment scores from various data sources:

- **Multi-Source Collection:** Aggregates sentiment from reviews, social media, customer feedback, support tickets, surveys
- **Temporal Grouping:** Groups sentiments by time periods (hourly, daily, weekly, monthly) for trend analysis
- **Source Weighting:** Applies configurable weights to different sources based on reliability and business importance

### 2. Statistical Analysis

- **Step 1:** Calculate descriptive statistics (mean, median, standard deviation) for sentiment scores
- **Step 2:** Compute sentiment distribution (positive %, neutral %, negative %) across time periods
- **Step 3:** Identify statistically significant trends using Mann-Kendall trend test
- **Step 4:** Detect sentiment anomalies using Z-score and IQR-based outlier detection

### 3. Insight Generation

- **Trend Detection:** Identifies improving, declining, or stable sentiment patterns over time
- **Topic Extraction:** Uses LDA (Latent Dirichlet Allocation) to discover main themes in positive/negative feedback
- **Comparative Analysis:** Compares sentiment across products, regions, customer segments, or time periods
- **Alert Triggers:** Flags sudden sentiment drops or spikes that exceed threshold changes

### 4. Performance and Optimization

- **Processing Time:** 500ms-2s for aggregating 100,000 sentiment records
- **Data Requirements:** Minimum 100 sentiment data points for reliable statistical analysis
- **Caching:** Temporal aggregations are cached for 1 hour to improve response times
- **Scalability:** Handles millions of sentiment records using time-based partitioning

## Related

- [Sentiment Analysis](SentimentAnalysis.md) - Real-time sentiment analysis of customer text
- [NPS](../CustomerIntelligence/NPS.md) - Net Promoter Score calculation and analysis
- [Customer Loyalty](../CustomerIntelligence/CustomerLoyalty.md) - Customer loyalty scoring and prediction
- [Segmentation Report](../CustomerIntelligence/SegmentationReport.md) - Comprehensive customer segmentation insights

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:328`
