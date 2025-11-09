# Artificial Intelligence – Churn Labeling

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-label
```

Churn Labeling using artificial intelligence and machine learning.

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
| 200 OK | Request successful. Returns churn labeling results. |
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

The churn labeling system uses rule-based classification to create training labels from historical customer data:

### Primary Algorithm

The system applies business rules to identify churned customers based on behavioral patterns:

- **Inactivity Detection:** Labels customers as churned if no activity for defined period (e.g., 90 days)
- **Engagement Metrics:** Evaluates declining engagement trends (reduced logins, purchases, interactions)
- **Subscription Status:** Identifies explicit churn events (cancellations, non-renewals)
- **Predictive Indicators:** Flags early churn signals based on behavior changes

### Processing Steps

1. **Data Collection:** Aggregate customer transaction, engagement, and subscription data
2. **Rule Application:** Apply configurable business rules to identify churn events and dates
3. **Label Assignment:** Create binary labels (churned=1, active=0) with churn timestamps
4. **Validation:** Verify label consistency and handle edge cases (reactivations, seasonality)

### Performance

- **Processing Time:** 100-300ms for 10,000 customers
- **Data Requirements:** Minimum 6 months of customer activity history

## Related

- [Customer Segmentation](CustomerSegmentation.md) - Segment customers to identify at-risk groups
- [Customer RFM](CustomerRFM.md) - RFM analysis for customer value segmentation
- [Customer Loyalty](CustomerLoyalty.md) - Loyalty scoring to predict retention
- [Propensity to Upgrade Plan](../Propensity/PropensityUpgradePlan.md) - Predict likelihood of plan upgrades
- [NPS](NPS.md) - Net Promoter Score analysis for satisfaction measurement

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:170`
