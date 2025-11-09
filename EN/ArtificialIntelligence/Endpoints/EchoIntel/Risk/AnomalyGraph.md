# Artificial Intelligence – Graph Anomaly Detection

## Endpoint

```
POST /api/v1/ai/echointel/risk/anomaly-graph
```

Graph Anomaly Detection analyzing relationship networks.

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
| 200 OK | Request successful. Returns graph anomaly detection results. |
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

Anomaly Graph Detection uses graph neural networks (GNNs) and community detection algorithms to identify suspicious patterns in networks of accounts, transactions, or relationships indicating organized fraud or collusion.

### 1. Graph Construction: Nodes (accounts, entities). Edges (transactions, relationships). Node features (account attributes). Edge features (transaction amounts, timestamps). Directed/weighted graphs.

### 2. Graph Neural Networks (GNN): Message passing neural networks propagate information across graph. Graph Attention Networks (GAT) learn importance of neighbors. GraphSAGE for inductive learning on large graphs. Node embeddings capture structural and attribute information.

### 3. Community Detection: Louvain algorithm for modularity optimization. Label propagation for fast clustering. Girvan-Newman for hierarchical communities. Detect tightly connected suspicious clusters.

### 4. Anomaly Scoring: Graph-level anomalies (unusual subgraphs). Node-level anomalies (accounts with abnormal connections). Edge-level anomalies (unusual transactions). Combined structural + attribute anomalies.

### 5. Patterns Detected: Fraud rings (circular money flows). Mule networks (layered money laundering). Bot farms (coordinated fake accounts). Collusion networks (review fraud, bid rigging).

### 6. Performance: 1-5s for graph analysis, 88-94% precision for fraud rings, handles graphs with 100K+ nodes.

## Related

### Related Endpoints

- **[Anomaly Accounts](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyAccounts.md)** - Individual account anomalies
- **[Anomaly Transactions](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/AnomalyTransactions.md)** - Transaction anomalies
- **[Credit Risk Score](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Risk/CreditRiskScore.md)** - Credit assessment

### Related Domain Concepts

- **Graph Analytics:** Network analysis, centrality measures, community detection
- **Graph Neural Networks:** GCN, GAT, GraphSAGE, message passing
- **Fraud Detection:** Money laundering, collusion, organized crime
- **Network Science:** Social network analysis, graph clustering

### Integration Points

- **Fraud Prevention:** Real-time graph-based alerts
- **Compliance Systems:** AML, Know Your Customer (KYC)
- **Investigation Tools:** Visualize suspicious networks

### Use Cases

- Money laundering detection, fraud ring identification, fake account networks, insider trading detection, review/rating manipulation

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:323`
