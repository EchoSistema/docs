# Artificial Intelligence – Transaction Anomaly Detection

## Endpoint

```
POST /api/v1/ai/echointel/risk/anomaly-transactions
```

Transaction Anomaly Detection using anomaly detection algorithms.

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
| 200 OK | Request successful. Returns transaction anomaly detection results. |
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

Transaction Anomaly Detection uses statistical methods (Z-score, IQR) and clustering algorithms (DBSCAN) to identify unusual transactions based on amount, frequency, timing, and behavioral deviations.

### 1. Statistical Methods: Z-score `z = (x - μ) / σ`, flag if |z| > 3. Interquartile Range (IQR): `outlier if x < Q1 - 1.5×IQR or x > Q3 + 1.5×IQR`. Modified Z-score using MAD for robustness.

### 2. DBSCAN Clustering: Density-based clustering identifies low-density outliers. Parameters: epsilon (neighborhood radius), min_points (density threshold). Noise points flagged as anomalies.

### 3. Feature Engineering: Transaction amount (absolute, relative to history). Frequency (transactions per hour/day). Timing (unusual hours, weekends). Velocity (rapid succession of transactions). Geographic (location changes, foreign transactions). Merchant type (unusual categories).

### 4. Behavioral Profiling: Establish baseline behavior per user. Compare current transaction to profile. Deviations in amount, frequency, merchant type trigger alerts. Adaptive learning updates profiles.

### 5. Rule-Based Augmentation: Hard thresholds (e.g., amount > $10,000). Blacklist checks (known fraud merchants/locations). Velocity rules (>5 transactions in 10 minutes). Combined statistical + rule-based.

### 6. Performance: <300ms per transaction, 82-91% precision, 75-87% recall, real-time scoring.

## Typical Workflow

### 1. Baseline: Establish normal transaction patterns per user (30-90 days history).
### 2. Monitoring: Real-time transaction scoring against baseline.
### 3. Alert: Flag high-anomaly transactions for review.
### 4. Investigation: Manual or automated review of flagged transactions.
### 5. Feedback: Update models with confirmed fraud/legitimate cases.
### 6. Adaptation: Retrain models monthly or when fraud patterns evolve.

## Related

### Related Endpoints

- **[Anomaly Accounts](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyAccounts.md)** - Account-level anomalies
- **[Anomaly Graph](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyGraph.md)** - Network-based fraud
- **[Credit Risk Score](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskScore.md)** - Credit assessment

### Related Domain Concepts

- **Anomaly Detection:** Z-score, IQR, DBSCAN, Isolation Forest
- **Fraud Detection:** Transaction monitoring, behavioral analytics
- **Statistical Process Control:** Control charts, outlier detection
- **Real-Time Analytics:** Stream processing, online learning

### Integration Points

- **Payment Gateways:** Real-time transaction screening
- **Banking Systems:** Fraud prevention, AML compliance
- **E-commerce Platforms:** Chargeback prevention

### Use Cases

- Credit card fraud detection, account takeover prevention, money laundering detection, chargeback reduction, payment abuse prevention

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:313`
