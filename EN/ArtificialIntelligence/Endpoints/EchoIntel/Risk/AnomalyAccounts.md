# Artificial Intelligence – Account Anomaly Detection

## Endpoint

```
POST /api/v1/ai/echointel/risk/anomaly-accounts
```

Account Anomaly Detection identifying suspicious behaviors.

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
| 200 OK | Request successful. Returns account anomaly detection results. |
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

Account Anomaly Detection uses unsupervised learning (Isolation Forest, Local Outlier Factor, One-Class SVM) to identify accounts with unusual behavior patterns indicating fraud, abuse, or security breaches.

### 1. Isolation Forest: Isolates anomalies by randomly partitioning feature space. Anomaly score based on path length to isolate point. Effective for high-dimensional data, detects global anomalies.

### 2. Local Outlier Factor (LOF): Measures local density deviation. `LOF = avg(density(neighbors)) / density(point)`. LOF >> 1 indicates anomaly. Detects local anomalies in varying-density regions.

### 3. One-Class SVM: Learns boundary of normal behavior. Kernel trick (RBF) for non-linear boundaries. Flags points outside learned boundary as anomalies.

### 4. Feature Engineering: Login patterns (frequency, time, location). Transaction behavior (volume, amount, velocity). Account changes (password resets, profile edits). Device fingerprinting. Behavioral biometrics.

### 5. Ensemble Approach: Combines Isolation Forest + LOF + One-Class SVM. Voting or weighted average. Consensus anomalies flagged as high-priority.

### 6. Performance: 200-600ms per account, 85-93% precision, 78-88% recall, false positive rate <5%.

## Related

### Related Endpoints

- **[Anomaly Transactions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyTransactions.md)** - Transaction-level anomalies
- **[Anomaly Graph](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyGraph.md)** - Graph-based fraud detection
- **[Credit Risk Score](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskScore.md)** - Credit risk assessment

### Related Domain Concepts

- **Anomaly Detection:** Isolation Forest, LOF, One-Class SVM, DBSCAN
- **Fraud Detection:** Behavioral analytics, pattern recognition
- **Cybersecurity:** Account takeover, unauthorized access detection
- **Unsupervised Learning:** Outlier detection, novelty detection

### Integration Points

- **Security Systems:** SIEM, fraud prevention platforms
- **Account Management:** Trigger account reviews, MFA requirements
- **Monitoring Dashboards:** Real-time anomaly alerts

### Use Cases

- Fraud detection, account takeover prevention, bot detection, insider threat identification, compliance monitoring

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:318`
