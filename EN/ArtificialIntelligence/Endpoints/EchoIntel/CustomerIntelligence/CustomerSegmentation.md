# Artificial Intelligence – Customer Segmentation

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segmentation
```

Performs customer segmentation analysis using machine learning techniques to identify groups of customers with similar characteristics.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). Required if not configured on the server. |
| X-Secret           | string | Conditional | 64-character secret. Required if not configured on the server. |
| Accept-Language    | string | No         | Response language (`en`, `es`, `pt`). Default: `en`. |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Request Body Parameters

Parameters vary according to the segmentation algorithm requirements. Consult the EchoIntel API documentation for specific details.

| Parameter    | Type   | Required | Description |
| ------------ | ------ | ----------- | --------- |
| data         | array  | Yes         | Array of data of the customers for segmentation. |
| algorithm    | string | No         | Algorithm to be used (ex.: `kmeans`, `hierarchical`). |
| n_clusters   | int    | No         | Number of clusters desired. |
| features     | array  | No         | Specific features for analysis. |

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "data": [
      {"customer_id": "123", "total_purchases": 1500, "frequency": 12},
      {"customer_id": "456", "total_purchases": 3000, "frequency": 24}
    ],
    "algorithm": "kmeans",
    "n_clusters": 3
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation"
```

### Request Example (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'pt',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    data: [
      {customer_id: '123', total_purchases: 1500, frequency: 12},
      {customer_id: '456', total_purchases: 3000, frequency: 24}
    ],
    algorithm: 'kmeans',
    n_clusters: 3
  })
});

const result = await response.json();
console.log(result);
```

### Request Example (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'pt',
])->post('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', [
    'data' => [
        ['customer_id' => '123', 'total_purchases' => 1500, 'frequency' => 12],
        ['customer_id' => '456', 'total_purchases' => 3000, 'frequency' => 24],
    ],
    'algorithm' => 'kmeans',
    'n_clusters' => 3,
]);

$result = $response->json();
```

## Response

### Success `200 OK`

```json
{
  "segments": [
    {
      "segment_id": 0,
      "size": 150,
      "characteristics": {
        "avg_purchases": 1200,
        "avg_frequency": 10
      },
      "customers": ["123", "789"]
    },
    {
      "segment_id": 1,
      "size": 200,
      "characteristics": {
        "avg_purchases": 3500,
        "avg_frequency": 25
      },
      "customers": ["456"]
    }
  ],
  "metrics": {
    "silhouette_score": 0.72,
    "davies_bouldin_score": 0.45
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required."
}
```

### Error `401 Unauthorized`

```json
{
  "error": "Unauthorized",
  "message": "Invalid authentication credentials"
}
```

### Error `500 Internal Server Error`

```json
{
  "error": "Failed to communicate with AI service",
  "message": "Internal server error"
}
```

## JSON Structure

| Field                              | Type    | Description |
| ---------------------------------- | ------- | --------- |
| `segments`                         | array   | Array of segments identified. |
| `segments[].segment_id`            | int     | Identifier of segment. |
| `segments[].size`                  | int     | Number of customers in the segment. |
| `segments[].characteristics`       | object  | Average characteristics of the segment. |
| `segments[].customers`             | array   | IDs of the customers in the segment. |
| `metrics`                          | object  | Quality metrics of the segmentation. |
| `metrics.silhouette_score`         | float   | Silhouette score (0-1, higher is better). |
| `metrics.davies_bouldin_score`     | float   | Score Davies-Bouldin (lower is better). |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns customer segmentation results. |
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

* The secret must be rotated every 90 days according to security policy.
* Processing time may vary depending on data volume (timeout: 300 seconds).
* The headers `X-Customer-Api-Id` and `X-Secret` can be configured on the server via `.env`.
* The response may vary depending on the EchoIntel API. Consult the official documentation for specific details.

## How It Is Computed

The customer segmentation system uses unsupervised machine learning to discover natural customer groups:

### Primary Algorithm

The system employs multiple clustering algorithms to identify customer segments:

- **K-Means Clustering:** Partitions customers into k clusters by minimizing within-cluster variance
- **Hierarchical Clustering:** Builds dendrogram of nested clusters using agglomerative or divisive methods
- **DBSCAN:** Density-based clustering that identifies clusters of arbitrary shape and detects outliers
- **Gaussian Mixture Models:** Probabilistic clustering using EM algorithm for soft cluster assignments

### Processing Steps

1. **Feature Selection:** Choose discriminative features (RFM, demographics, behavior, engagement)
2. **Preprocessing:** Standardize features using z-score normalization, handle missing values
3. **Optimal K Selection:** Determine number of clusters using elbow method, silhouette analysis, or BIC
4. **Clustering:** Apply selected algorithm to assign each customer to segment
5. **Profiling:** Calculate segment characteristics (size, means, medians) and business interpretations

### Performance

- **Processing Time:** 300ms-2s for 100,000 customers (depends on features and algorithm)
- **Data Requirements:** Minimum 500 customers for stable segmentation

## Typical Workflow

### Step 1: Prepare Data
Collect customer data including transactional (purchases, amounts), behavioral (website visits, engagement), and demographic features. Clean data and handle missing values.

### Step 2: Configure Parameters
Specify clustering algorithm (kmeans, hierarchical, dbscan), number of clusters (or use auto-detection), and features to include. Set validation method (silhouette, davies-bouldin).

### Step 3: Make Request
Submit customer data with configuration. API performs clustering, validates results using quality metrics, and returns segment assignments with interpretable characteristics.

### Step 4: Analyze Results
Review segment profiles (size, avg_purchases, avg_frequency, characteristics). Check quality metrics (silhouette_score >0.5 is good, davies_bouldin_score <1.0 is good). Interpret business meaning of each segment.

### Step 5: Take Action
- **High-Value Segment:** Premium services, account management, exclusive offers
- **Growth Segment:** Nurture campaigns, upsell opportunities, education content
- **Price-Sensitive Segment:** Discount campaigns, value propositions, bundle offers
- **At-Risk Segment:** Retention programs, feedback collection, reactivation efforts

## Related

- [Customer RFM](CustomerRFM.md) - RFM-based customer segmentation
- [Purchasing Segmentation](PurchasingSegmentation.md) - Behavior-based purchase segmentation
- [Segmentation Report](SegmentationReport.md) - Comprehensive segmentation analysis
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Detailed cluster profiling
- [CLV Forecast](CustomerClvForecast.md) - Predict value by segment

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:130`
