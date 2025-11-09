# Artificial Intelligence – Segment Cluster Profiles

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-cluster-profiles
```

Segment Cluster Profiles using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns segment cluster profiles results. |
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

The cluster profiling system creates detailed descriptive profiles for each customer segment:

### Primary Algorithm

The system generates multi-dimensional profiles characterizing each cluster:

- **Centroid Analysis:** Calculates cluster centroids (mean feature values) to identify typical segment characteristics
- **Distribution Metrics:** Computes within-cluster variance, spread, and density for each feature
- **Demographic Profiling:** Aggregates age, gender, location, and other demographic attributes per segment
- **Behavioral Profiling:** Summarizes purchase patterns, product preferences, channel usage per cluster
- **Persona Generation:** Creates human-readable segment personas with names and descriptions

### Processing Steps

1. **Cluster Identification:** Extract cluster assignments from segmentation results
2. **Feature Aggregation:** Calculate statistics (mean, median, std, min, max) for all features per cluster
3. **Characteristic Extraction:** Identify top differentiating features for each segment
4. **Comparison:** Compare each cluster against overall population and other clusters
5. **Naming:** Generate interpretable cluster names based on distinguishing characteristics

### Performance

- **Processing Time:** 200-500ms for profiling 10 clusters with 50+ features
- **Data Requirements:** Segmentation results plus full feature matrix for all customers

## Related

- [Customer Segmentation](CustomerSegmentation.md) - Perform customer segmentation
- [Segmentation Report](SegmentationReport.md) - Comprehensive segmentation insights
- [Segment Hierarchy Chart](SegmentHierarchyChart.md) - Visualize segment relationships
- [Customer Features](CustomerFeatures.md) - Extract customer features for profiling
- [Customer RFM](CustomerRFM.md) - RFM-based segment profiling

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:125`
