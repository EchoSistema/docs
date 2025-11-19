# Artificial Intelligence – Segment Hierarchy Chart

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-hierarchy-chart
```

Generates hierarchical customer segment visualizations using dendrogram charts that reveal nested relationships between segments, showing how broad customer groups split into increasingly granular subsegments at different similarity levels.

**Business Value:** Enables multi-level segmentation strategy, allowing marketers to target at the optimal granularity level (broad segments for bre campaigns, micro-segments for personalization), improving campaign relevance by 25-40%.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-caracteres of segredo. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Tipo       | string | Yes         | `application/json`. |

## Parameters

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo of the Requisição Parâmetros

| Parameter | Type | Required | Padrão | Description | Significado Empresarial |
|-----------|------|----------|---------|-------------|------------------|
| customer_data | array | Yes | - | array of customer registros with features for hierarchical clustering. | Customer attributes used to build segment hierarchy. |
| linkage_method | string | No | `"ward"` | Clustering linkage: `ward`, `complete`, `average`, `single`. | How segments merge. Ward minimizes within-cluster variance (recommended). |
| distance_metric | string | No | `"euclidean"` | Distance calculation: `euclidean`, `manhattan`, `cosine`. | How customer similarity is measured. |
| max_levels | integer | No | `5` | Maximum hierarchy depth (number of nested segment levels). | How granular the segmentation goes. 3-5 levels typical. |
| min_segment_size | integer | No | `100` | Minimum customers per segment at any level. | Prevents overly small, non-actionable segments. |
| include_dendrogram_data | boolean | No | `true` | Return dendrogram structure for visualization. | Enables chart rendering in BI tools. |

### Customer Data object Fields

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| customer_id | string | Yes | Unique customer identifier. | `"C12345"` |
| features | object | Yes | Customer attributes for clustering (RFM, demographics, behavior). | `{"recency": 30, "frequency": 5, "monetary": 500}` |

## Examples

### Exemplo of Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_data": [
      {"customer_id": "C001", "features": {"recency": 10, "frequency": 12, "monetary": 1200}},
      {"customer_id": "C002", "features": {"recency": 90, "frequency": 2, "monetary": 50}}
    ],
    "linkage_method": "ward",
    "max_levels": 4,
    "include_dendrogram_data": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segment-hierarchy-chart"
```

## Response

### Success `200 OK`

```json
{
  "hierarchy": {
    "level_1": [
      {"segment_id": "L1_S1", "name": "High Value", "size": 2340, "parent": null}
    ],
    "level_2": [
      {"segment_id": "L2_S1", "name": "Champions", "size": 1250, "parent": "L1_S1"},
      {"segment_id": "L2_S2", "name": "Loyalists", "size": 1090, "parent": "L1_S1"}
    ],
    "level_3": [
      {"segment_id": "L3_S1", "name": "VIP Champions", "size": 450, "parent": "L2_S1"},
      {"segment_id": "L3_S2", "name": "Regular Champions", "size": 800, "parent": "L2_S1"}
    ]
  },
  "dendrogram": {
    "nodes": [...],
    "links": [...],
    "merge_distances": [0.5, 1.2, 2.8, 5.1]
  },
  "segment_profiles": {
    "L1_S1": {"avg_recency": 15, "avg_frequency": 10.5, "avg_monetary": 980}
  }
}
```

## Campo Reference

| Field | Type | Description | Significado Empresarial |
|-------|------|-------------|------------------|
| `hierarchy` | object | Multi-level segment structure organized by hierarchy depth. | Complete segment taxonomy from broad to granular. |
| `hierarchy.level_{n}` | array | Segments at hierarchy level n (1=broadest, higher=more granular). | Level to target for campaigns. Level 1-2 for broad, level 3-5 for personalization. |
| `hierarchy.level_{n}[].segment_id` | string | Unique segment identifier. | Reference for targeting and reporting. |
| `hierarchy.level_{n}[].parent` | string | Parent segment ID (null for level 1). | Shows which broad segment this belongs to. |
| `dendrogram` | object | Dendrogram visualization data showing merge sequence. | Use to visualize hierarchy as tree chart. |
| `segment_profiles` | object | Average feature values per segment. | Segment characteristics for interpretation. |

## Status HTTP

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns segment hierarchy chart results. |
| 400 Bad Request | Invalid request Parâmetros. Check Parâmetro types and Obrigatório fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Resposta for details. |
| 429 Too Many Requests | Limite of taxa excedido. Retry after cooldown period. |
| 500 Internal Server Erro | Server Erro. Contact support if persistent. |
| 503 Service Unavailable | Serviço of IA temporariamente indisponível. Retry with exponential backoff. |

## Erros

### Common Erro Responses

#### Missing Obrigatório Parâmetros
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

**Solution:** Ensure all Obrigatório Parâmetros are provided in the Corpo of the Requisição.

#### Invalid Autenticação
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid and not expired. Check `X-Customer-Api-Id` e `X-Secret` Cabeçalhos.

## Typical Workflow

### 1. Prepare Customer Features
Extract RFM or behavioral features for all customers.

### 2. Generate Hierarchy
Submit to API with appropriate max_levels (3-5 typical) and min_segment_size (50-200).

### 3. Analyze Hierarchy Levels
- **Level 1-2:** Broad segments for strategic planning and bre campaigns
- **Level 3-4:** Tactical segments for channel-specific campaigns
- **Level 5+:** Micro-segments for 1:1 personalization

### 4. Select Optimal Granularity
Choose hierarchy level based on campaign Tipo and resources.

### 5. Visualize Dendrogram
Use dendrogram data to create tree charts in BI tools for stakeholder presentations.

## Perguntas Frequentes

### Q: How many hierarchy levels should I use?
A: Start with 3-4 levels. Level 1 gives 2-4 broad segments, level 3 gives 8-20 tactical segments. More levels = more granularity but harder to action. Most businesses use level 2-3 for campaigns.

### Q: What's the difference between this and regular segmentation?
A: Regular segmentation creates flat, independent segments. Hierarchical segmentation shows nested relationships (e.g., "High Value" splits into "Champions" e "Loyalists"). Use hierarchy when you need multi-level targeting strategies.

### Q: Which linkage method should I use?
A: Ward linkage (Padrão) works best for most cases as it creates balanced, compact segments. Use complete linkage if you want tight, well-separated segments. Avoid single linkage (creates chain-like segments).

## Notes

### Desempenho
- **Tempo of Processamento:** 300ms-2s for 50,000 customers
- **Maximum Customers:** 200,000 per request
- **Rate Limiting:** 20 requests per minute

### Use Cases
- **Multi-tier loyalty programs:** Define tier hierarchy (Bronze → Silver → Gold → Platinum)
- **Account-based marketing:** Nest accounts within industries within regions
- **Product recommendations:** Group products hierarchically by category → subcategory → specific items

## Como é Calculado

The hierarchy chart system visualizes nested segment relationships using hierarchical clustering:

### Algoritmo Principal

The system creates dendrogram structures showing segment relationships:

- **Hierarchical Clustering:** Applies agglomerative (bottom-up) clustering to build segment hierarchy
- **Linkage Methods:** Supports Ward's, complete, average, or single linkage for merging clusters
- **Distance Metrics:** Uses Euclidean, Manhattan, or cosine distance to measure cluster similarity
- **Dendrogram Generation:** Constructs tree structure showing how segments merge at different similarity levels
- **Cut-off Selection:** Determines optimal hierarchy levels for actionable segmentation

### Passos of Processamento

1. **Distance Matrix:** Compute pairwise distances between all customer points or existing segments
2. **Agglomerative Merging:** Iteratively merge closest clusters using selected linkage method
3. **Dendrogram Construction:** Build hierarchical tree structure recording merge sequence and distances
4. **Level Selection:** Identify meaningful cut-off points (using dendrogram gaps or business criteria)
5. **Visualization:** Generate chart with segment hierarchy, sizes, and nesting relationships

### Desempenho

- **Tempo of Processamento:** 300ms-2s for hierarchical clustering of 50,000 customers
- **Requisitos of Dados:** Customer feature matrix or existing flat segmentation results

## Relacionado

- [Customer Segmentation](CustomerSegmentation.md) - Flat customer segmentation
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Profile segments at each hierarchy level
- [Segment Subsegment Explore](SegmentSubsegmentExplore.md) - Drill down into segment hierarchies
- [Segmentation Report](SegmentationReport.md) - Multi-level segmentation analysis
- [Customer RFM](CustomerRFM.md) - Hierarchical RFM segmentation

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:115`
