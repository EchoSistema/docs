# Artificial Intelligence – Segment Hierarchy Chart

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-hierarchy-chart
```

Segment Hierarchy Chart using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns segment hierarchy chart results. |
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

The hierarchy chart system visualizes nested segment relationships using hierarchical clustering:

### Primary Algorithm

The system creates dendrogram structures showing segment relationships:

- **Hierarchical Clustering:** Applies agglomerative (bottom-up) clustering to build segment hierarchy
- **Linkage Methods:** Supports Ward's, complete, average, or single linkage for merging clusters
- **Distance Metrics:** Uses Euclidean, Manhattan, or cosine distance to measure cluster similarity
- **Dendrogram Generation:** Constructs tree structure showing how segments merge at different similarity levels
- **Cut-off Selection:** Determines optimal hierarchy levels for actionable segmentation

### Processing Steps

1. **Distance Matrix:** Compute pairwise distances between all customer points or existing segments
2. **Agglomerative Merging:** Iteratively merge closest clusters using selected linkage method
3. **Dendrogram Construction:** Build hierarchical tree structure recording merge sequence and distances
4. **Level Selection:** Identify meaningful cut-off points (using dendrogram gaps or business criteria)
5. **Visualization:** Generate chart with segment hierarchy, sizes, and nesting relationships

### Performance

- **Processing Time:** 300ms-2s for hierarchical clustering of 50,000 customers
- **Data Requirements:** Customer feature matrix or existing flat segmentation results

## Related

- [Customer Segmentation](CustomerSegmentation.md) - Flat customer segmentation
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Profile segments at each hierarchy level
- [Segment Subsegment Explore](SegmentSubsegmentExplore.md) - Drill down into segment hierarchies
- [Segmentation Report](SegmentationReport.md) - Multi-level segmentation analysis
- [Customer RFM](CustomerRFM.md) - Hierarchical RFM segmentation

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:115`
