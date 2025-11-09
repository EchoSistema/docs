# Artificial Intelligence – Segment Subsegment Exploration

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-subsegment-explore
```

Segment Subsegment Exploration using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns segment subsegment exploration results. |
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

The subsegment exploration system enables drill-down analysis within customer segments:

### Primary Algorithm

The system performs iterative clustering to discover subsegments within parent segments:

- **Recursive Clustering:** Applies clustering algorithms within each parent segment to find subsegments
- **Feature Refinement:** Uses segment-specific features to identify meaningful subdivisions
- **Subsegment Validation:** Tests subsegment quality using silhouette scores and business interpretability
- **Multi-Level Hierarchy:** Supports multiple levels of nesting (segments → subsegments → micro-segments)
- **Interactive Exploration:** Allows dynamic exploration of any segment branch

### Processing Steps

1. **Segment Selection:** Choose parent segment to explore for subsegments
2. **Subset Extraction:** Filter customers belonging to selected parent segment
3. **Reclustering:** Apply clustering algorithm to parent segment customers only
4. **Subsegment Profiling:** Profile each discovered subsegment with distinguishing characteristics
5. **Comparison:** Compare subsegments to each other and to parent segment average

### Performance

- **Processing Time:** 200-800ms per segment exploration (depends on segment size)
- **Data Requirements:** Existing parent segmentation plus customer feature data

## Related

- [Segment Hierarchy Chart](SegmentHierarchyChart.md) - Visualize full segment hierarchy
- [Customer Segmentation](CustomerSegmentation.md) - Initial customer segmentation
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Profile subsegments
- [Segmentation Report](SegmentationReport.md) - Multi-level segmentation insights
- [Purchasing Segmentation](PurchasingSegmentation.md) - Purchase-based subsegments

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:120`
