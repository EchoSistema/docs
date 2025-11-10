# Artificial Intelligence – Segment Subsegment Exploration

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-subsegment-explore
```

Performs interactive drill-down analysis within customer segments by discovering subsegments using recursive clustering, enabling marketers to explore nested customer groups e uncover hidden micro-segments with distinct characteristics.

**Business Value:** Enables progressive segmentation refinement, allowing teams to start with broad segments e drill into specific subsegments for targeted campaigns, improving campaign precision by 30-50% compared to flat segmentation.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). |
| X-Secret           | string | Condicional | 64-caracteres de segredo. |
| Accept-Language    | string | Não         | Language (`en`, `es`, `pt`). |
| Content-Tipo       | string | Sim         | `application/json`. |

## Parâmetros

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo da Requisição Parâmetros

| Parâmetro | Tipo | Obrigatório | Padrão | Descrição | Significado Empresarial |
|-----------|------|----------|---------|-------------|------------------|
| parent_segment_id | string | Sim | - | ID of parent segment to explore for subsegments. | Which existing segment to drill into. |
| customer_data | array | Sim | - | Customers belonging to parent segment with features. | Customer attributes for subsegmentation. |
| num_subsegments | integer | Não | `3` | Number of subsegments to create within parent segment (2-10). | Granularity of subdivision. 3-5 typical. |
| clustering_algorithm | string | Não | `"kmeans"` | Algorithm: `kmeans`, `dbscan`, `hierarchical`. | How to discover subsegments. KMeans is steard. |
| min_subsegment_size | integer | Não | `50` | Minimum customers per subsegment. | Prevents tiny, non-actionable groups. |
| include_profiles | boolean | Não | `true` | Return detailed subsegment profiles with differentiating characteristics. | Enables subsegment interpretation e naming. |

### Customer Data object Fields

| Campo | Tipo | Obrigatório | Descrição | Example |
|-------|------|----------|-------------|---------|
| customer_id | string | Sim | Unique customer identifier. | `"C12345"` |
| parent_segment | string | Sim | Parent segment this customer belongs to. | `"High_Value"` |
| features | object | Sim | Customer attributes for subsegmentation. | `{"recency": 15, "frequency": 8, "monetary": 850}` |

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "Content-Type: application/json" \
  -d '{
    "parent_segment_id": "High_Value",
    "customer_data": [
      {"customer_id": "C001", "parent_segment": "High_Value", "features": {"recency": 10, "frequency": 12, "monetary": 1200}},
      {"customer_id": "C002", "parent_segment": "High_Value", "features": {"recency": 15, "frequency": 8, "monetary": 850}}
    ],
    "num_subsegments": 3,
    "include_profiles": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segment-subsegment-explore"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "parent_segment": {
    "segment_id": "High_Value",
    "size": 2340,
    "avg_features": {"recency": 18, "frequency": 9.2, "monetary": 920}
  },
  "subsegments": [
    {
      "subsegment_id": "High_Value_Champions",
      "size": 890,
      "percentage": 38.0,
      "profile": {"avg_recency": 8, "avg_frequency": 14.5, "avg_monetary": 1450},
      "differentiators": ["Very high frequency", "Very high monetary", "Recent purchases"],
      "description": "Most engaged and valuable customers within High Value segment"
    },
    {
      "subsegment_id": "High_Value_Loyalists",
      "size": 780,
      "percentage": 33.3,
      "profile": {"avg_recency": 25, "avg_frequency": 7.8, "avg_monetary": 780},
      "differentiators": ["Moderate frequency", "Lower monetary", "Older purchases"],
      "description": "Steady customers with room for growth"
    },
    {
      "subsegment_id": "High_Value_BigSpenders",
      "size": 670,
      "percentage": 28.6,
      "profile": {"avg_recency": 20, "avg_frequency": 4.2, "avg_monetary": 980},
      "differentiators": ["Low frequency", "High average order value"],
      "description": "Infrequent but high-value purchasers"
    }
  ],
  "quality_metrics": {
    "silhouette_score": 0.72,
    "within_segment_variance": 0.18
  }
}
```

## Campo Reference

| Campo | Tipo | Descrição | Significado Empresarial |
|-------|------|-------------|------------------|
| `parent_segment` | object | Information about the parent segment being explored. | Context for subsegmentation. |
| `subsegments` | array | Discovered subsegments within parent segment. | Nested customer groups for targeted actions. |
| `subsegments[].subsegment_id` | string | Unique subsegment identifier. | Reference for targeting e reporting. |
| `subsegments[].size` | integer | Number of customers in subsegment. | Subsegment reach e targeting volume. |
| `subsegments[].percentage` | float | Percentage of parent segment in this subsegment. | Subsegment distribution within parent. |
| `subsegments[].profile` | object | Average feature values for subsegment. | Subsegment characteristics. |
| `subsegments[].differentiators` | array | Key features that distinguish this subsegment from others. | What makes this subsegment unique. |
| `quality_metrics.silhouette_score` | float | Clustering quality (0-1). Higher = better subsegment separation. | Confidence in subsegment distinctions. 0.70+ = good quality. |

## Typical Workflow

### 1. Start with Broad Segmentation
Run initial customer segmentation to create 3-5 parent segments.

### 2. Select Segment to Explore
Choose high-value or strategically important parent segment for drill-down.

### 3. Configure Subsegmentation
Set num_subsegments (3-5 typical) based on campaign capacity.

### 4. Analyze Subsegment Profiles
Review differentiators e descriptions to underste what makes each subsegment unique.

### 5. Name e Document Subsegments
Assign business-friendly names based on characteristics (e.g., "Champions", "Loyalists").

### 6. Create Targeted Campaigns
Develop segment-specific messaging e offers for each subsegment.

### 7. Iterate as Needed
Further drill into subsegments to create micro-segments for 1:1 personalization.

## Perguntas Frequentes

### Q: When should I use subsegment exploration vs. hierarchical segmentation?
A: Use subsegment exploration for interactive, ad-hoc drill-down (explore one segment at a time). Use hierarchical segmentation for planned multi-level structures (all levels computed upfront). Exploration is better for iterative discovery, hierarchy for systematic planning.

### Q: How many subsegments should I create?
A: Start with 3-4 subsegments. Too few (2) = insufficient granularity, too many (>6) = diluted targeting e resource constraints. Balance subsegment size with campaign execution capacity.

### Q: What if subsegments are too similar?
A: This indicates low variance within the parent segment. Either: (1) reduce num_subsegments, (2) add more discriminating features to customer_data, or (3) accept that this parent segment is homogeneous e doesn't need subdivision.

### Q: Can I drill into subsegments repeatedly?
A: Sim. Take a subsegment e submit it as the parent_segment for another level of drill-down. Typical depth: 2-3 levels before segments become too small to be actionable.

## Notas

### Desempenho
- **Tempo de Processamento:** 200-800ms per segment exploration
- **Maximum Customers:** 100,000 per parent segment
- **Rate Limiting:** 30 requests per minute

### Use Cases
- **High-value segment refinement:** Drill into top customers to create VIP programs
- **At-risk customer analysis:** Explore churn-risk segment to find distinct sub-groups requiring different retention strategies
- **New customer onboarding:** Subdivide new customers by engagement level for tailored nurture campaigns

### Quality Metrics
- **Silhouette score >0.70:** Excellent subsegment separation
- **Silhouette score 0.50-0.70:** Good subsegmentation
- **Silhouette score <0.50:** Weak subsegmentation, consider fewer subsegments or different features

## Como é Calculado

## Status HTTP

| Status Código | Descrição |
|-------------|-------------|
| 200 OK | Request successful. Returns segment subsegment exploration results. |
| 400 Bad Request | Invalid request Parâmetros. Check Parâmetro types e Obrigatório fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Resposta for details. |
| 429 Too Many Requests | Limite de taxa excedido. Retry after cooldown period. |
| 500 Internal Server Erro | Server Erro. Contact support if persistent. |
| 503 Service Unavailable | Serviço de IA temporariamente indisponível. Retry with exponential backoff. |

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

**Solution:** Ensure all Obrigatório Parâmetros are provided in the Corpo da Requisição.

#### Invalid Autenticação
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid e not expired. Check `X-Customer-Api-Id` e `X-Secret` Cabeçalhos.

## Como é Calculado

The subsegment exploration system enables drill-down analysis within customer segments:

### Algoritmo Principal

The system performs iterative clustering to discover subsegments within parent segments:

- **Recursive Clustering:** Applies clustering algorithms within each parent segment to find subsegments
- **Feature Refinement:** Uses segment-specific features to identify meaningful subdivisions
- **Subsegment Validation:** Tests subsegment quality using silhouette scores e business interpretability
- **Multi-Level Hierarchy:** Supports multiple levels of nesting (segments → subsegments → micro-segments)
- **Interactive Exploration:** Allows dynamic exploration of any segment branch

### Passos de Processamento

1. **Segment Selection:** Choose parent segment to explore for subsegments
2. **Subset Extraction:** Filter customers belonging to selected parent segment
3. **Reclustering:** Apply clustering algorithm to parent segment customers only
4. **Subsegment Profiling:** Profile each discovered subsegment with distinguishing characteristics
5. **Comparison:** Compare subsegments to each other e to parent segment average

### Desempenho

- **Tempo de Processamento:** 200-800ms per segment exploration (depends on segment size)
- **Requisitos de Dados:** Existing parent segmentation plus customer feature data

## Relacionado

- [Segment Hierarchy Chart](SegmentHierarchyChart.md) - Visualize full segment hierarchy
- [Customer Segmentation](CustomerSegmentation.md) - Initial customer segmentation
- [Segment Cluster Profiles](SegmentClusterProfiles.md) - Profile subsegments
- [Segmentation Report](SegmentationReport.md) - Multi-level segmentation insights
- [Purchasing Segmentation](PurchasingSegmentation.md) - Purchase-based subsegments

## Referências

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:120`
