# Artificial Intelligence – RFM Analysis (Recency, Frequency, Monetary)

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/rfm
```

Performs RFM analysis (Recency, Frequency and Monetary Value) to segment customers based on their purchasing behavior.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-character secret. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). Default: `en`. |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Body Parameters

| Parameter     | Type   | Required | Description |
| ------------- | ------ | ----------- | --------- |
| transactions  | array  | Yes         | Customer transaction history. |
| reference_date| string | No         | Reference date for calculation (ISO 8601). Default: today. |
| quintiles     | boolean| No         | Whether to use quintiles (5 groups) instead of quartiles (4). Default: `false`. |

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
    "transactions": [
      {"customer_id": "C001", "date": "2025-01-05", "amount": 150.00},
      {"customer_id": "C001", "date": "2024-12-20", "amount": 89.90},
      {"customer_id": "C002", "date": "2024-10-15", "amount": 500.00}
    ],
    "reference_date": "2025-01-07",
    "quintiles": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/rfm"
```

## Response

### Success `200 OK`

```json
{
  "rfm_analysis": [
    {
      "customer_id": "C001",
      "recency_score": 5,
      "frequency_score": 4,
      "monetary_score": 3,
      "rfm_score": "543",
      "segment": "Champions",
      "recency_days": 2,
      "frequency_count": 12,
      "monetary_total": 1450.00,
      "recommendations": [
        "Reward with loyalty benefits",
        "Upsell premium products",
        "Request referrals"
      ]
    },
    {
      "customer_id": "C002",
      "recency_score": 1,
      "frequency_score": 1,
      "monetary_score": 4,
      "rfm_score": "114",
      "segment": "At Risk",
      "recency_days": 84,
      "frequency_count": 2,
      "monetary_total": 3200.00,
      "recommendations": [
        "Send reactivation campaign",
        "Offer special discount",
        "Survey to understand issues"
      ]
    }
  ],
  "segments_summary": {
    "Champions": 15,
    "Loyal Customers": 28,
    "Potential Loyalists": 22,
    "At Risk": 18,
    "Lost": 12
  }
}
```

## JSON Structure

| Field                                  | Type    | Description |
| -------------------------------------- | ------- | --------- |
| `rfm_analysis`                         | array   | Array of RFM analyses per customer. |
| `rfm_analysis[].customer_id`           | string  | Customer ID. |
| `rfm_analysis[].recency_score`         | int     | Recency score (1-5). |
| `rfm_analysis[].frequency_score`       | int     | Frequency score (1-5). |
| `rfm_analysis[].monetary_score`        | int     | Monetary score (1-5). |
| `rfm_analysis[].rfm_score`             | string  | Combined RFM score. |
| `rfm_analysis[].segment`               | string  | Customer segment. |
| `rfm_analysis[].recency_days`          | int     | Days since last purchase. |
| `rfm_analysis[].frequency_count`       | int     | Number of purchases. |
| `rfm_analysis[].monetary_total`        | float   | Total amount spent. |
| `rfm_analysis[].recommendations`       | array   | Action recommendations. |
| `segments_summary`                     | object  | Summary of customers per segment. |

## RFM Segments

| Segment               | Scores | Description |
| --------------------- | ------ | --------- |
| Champions             | 555, 554, 544, 545, 454, 455, 445 | Best customers. |
| Loyal Customers       | 543, 444, 435, 355, 354, 345, 344, 335 | Loyal customers. |
| Potential Loyalists   | 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 | Promising customers. |
| At Risk               | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 | At risk of churn. |
| Lost                  | 111, 112, 121, 131, 141,211, 212, 114, 214, 141, 221, 213, 113, 222, 132 | Lost customers. |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns RFM analysis results. |
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

* Scores range from 1 (worst) to 5 (best).
* Segments are automatically assigned based on RFM scores.
* Recommendations are personalized for each segment.

## How It Is Computed

The RFM analysis uses quintile-based scoring to segment customers by purchasing behavior patterns:

### Primary Algorithm

The system calculates three key customer metrics and assigns scores:

- **Recency (R):** Days since last purchase - lower is better, scored 1-5 (5 = most recent)
- **Frequency (F):** Number of purchases in time period - higher is better, scored 1-5
- **Monetary (M):** Total spend amount - higher is better, scored 1-5
- **Scoring Method:** Quintile-based (divides customers into 5 equal groups per metric) or quartile-based (4 groups)

### Processing Steps

1. **Transaction Aggregation:** Aggregate customer transactions to calculate R, F, M raw values
2. **Quintile Assignment:** Divide customers into 5 groups per metric using percentile thresholds (20th, 40th, 60th, 80th)
3. **Score Assignment:** Assign scores 1-5 based on quintile position (5 = best quintile)
4. **RFM Combination:** Concatenate scores to create RFM code (e.g., "543" = R:5, F:4, M:3)
5. **Segment Mapping:** Map RFM codes to business segments (Champions, Loyal, At Risk, etc.)

### Performance

- **Processing Time:** 100-300ms for 10,000 customers
- **Data Requirements:** Minimum 3 months of transaction history for stable scores

## Typical Workflow

### Step 1: Prepare Data
Gather customer transaction data with customer_id, transaction_date, and transaction_amount. Ensure data covers sufficient time period (6-12 months recommended) for meaningful analysis.

### Step 2: Configure Parameters
Set reference_date (typically today or end of analysis period) and choose quintiles vs quartiles scoring. Define time window for analysis if different from full history.

### Step 3: Make Request
Send transaction data via API. System automatically calculates RFM scores, assigns segments, and generates actionable recommendations for each customer segment.

### Step 4: Analyze Results
Review segment distribution (Champions, Loyal, At Risk, Lost). Identify key opportunities: Champions for upsell, At Risk for retention, Lost for reactivation campaigns.

### Step 5: Take Action
- **Champions (555, 554):** VIP programs, early access, premium support, referral rewards
- **Loyal (543, 444):** Loyalty programs, personalized offers, thank-you campaigns
- **At Risk (255, 254):** Win-back campaigns, surveys, special discounts
- **Lost (111, 121):** Reactivation campaigns, feedback surveys, incentive offers

## Related

- [Customer Segmentation](CustomerSegmentation.md) - ML-based customer segmentation
- [CLV Forecast](CustomerClvForecast.md) - Predict customer lifetime value
- [Customer Loyalty](CustomerLoyalty.md) - Loyalty scoring and prediction
- [Purchasing Segmentation](PurchasingSegmentation.md) - Behavioral purchase segmentation
- [Churn Label](ChurnLabel.md) - Identify at-risk customers

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:145`
