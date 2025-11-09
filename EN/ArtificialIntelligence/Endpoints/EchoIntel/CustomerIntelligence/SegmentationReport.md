# Artificial Intelligence – Segmentation Report

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segmentation-report
```

Segmentation Report using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns segmentation report results. |
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

The segmentation report generates comprehensive analytics and insights from customer segmentation results:

### Primary Algorithm

The system analyzes segmentation outputs to produce actionable business intelligence:

- **Segment Profiling:** Calculates descriptive statistics (size, demographics, behavior) for each segment
- **Comparative Analysis:** Identifies differentiating characteristics between segments
- **Trend Analysis:** Tracks segment evolution over time (growth, shrinkage, migration)
- **Value Assessment:** Computes CLV, revenue contribution, and profitability per segment
- **Actionability Scoring:** Ranks segments by business opportunity and risk level

### Processing Steps

1. **Data Aggregation:** Collect segmentation results and associated customer attributes
2. **Statistical Profiling:** Calculate mean, median, mode for key metrics per segment
3. **Visualization Preparation:** Generate distribution charts, heatmaps, and comparison tables
4. **Insight Generation:** Identify statistically significant differences and business opportunities
5. **Report Compilation:** Create comprehensive report with executive summary and detailed segment profiles

### Performance

- **Processing Time:** 500ms-3s for comprehensive report with 50,000 customers across 10 segments
- **Data Requirements:** Existing segmentation results plus customer transaction and profile data

## Related

- [Customer Segmentation](CustomerSegmentation.md) - Perform customer segmentation
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Detailed cluster profiling
- [Segment Hierarchy Chart](SegmentHierarchyChart.md) - Hierarchical segment visualization
- [Purchasing Segmentation](PurchasingSegmentation.md) - Purchase-based segmentation
- [Customer RFM](CustomerRFM.md) - RFM analysis and segmentation

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:110`
