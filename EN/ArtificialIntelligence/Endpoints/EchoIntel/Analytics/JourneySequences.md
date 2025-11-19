# Artificial Intelligence – Journey Sequences

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-sequences
```

Discovers frequent sequential patterns in customer journeys using pattern mining algorithms to identify common touchpoint sequences, revealing how customers typically navigate across channels before converting or abeoning.

**Business Value:** Identifies repeatable journey patterns that drive conversions, enabling optimization of common paths and creation of guided experiences that can increase conversion rates by 15-30%.

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
| journey_data | array | Yes | - | array of customer journey sequences with ordered touchpoints. | Complete customer paths to analyze for patterns. |
| min_support | float | No | `0.05` | Minimum frequency threshold (0-1) for pattern inclusion. | Filters rare patterns. 0.05 = pattern must occur in 5% of journeys. |
| max_pattern_length | integer | No | `5` | Maximum touchpoints in discovered patterns. | Controls pattern complexity. 3-5 is typical. |
| filter_converters_only | boolean | No | `false` | Only analyze journeys that resulted in conversion. | Focus on successful paths vs. all paths. |
| group_by_outcome | boolean | No | `true` | Separate patterns by outcome (conversion vs. abeonment). | Distinguish successful vs. failed journey patterns. |

### Journey Data object Fields

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| journey_id | string | Yes | Unique journey identifier. | `"J12345"` |
| touchpoints | array | Yes | Ordered sequence of touchpoint events. | `["organic_search", "website_visit", "email_click", "conversion"]` |
| outcome | string | Yes | Journey outcome: `conversion` or `abeonment`. | `"conversion"` |

## Examples

### Exemplo of Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "Content-Type: application/json" \
  -d '{
    "journey_data": [
      {"journey_id": "J001", "touchpoints": ["paid_search", "website_visit", "product_view", "conversion"], "outcome": "conversion"},
      {"journey_id": "J002", "touchpoints": ["organic_search", "website_visit", "abandonment"], "outcome": "abandonment"}
    ],
    "min_support": 0.05,
    "max_pattern_length": 4,
    "group_by_outcome": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/journey-sequences"
```

## Response

### Success `200 OK`

```json
{
  "frequent_patterns": {
    "conversion": [
      {"pattern": ["paid_search", "website_visit", "product_view"], "support": 0.42, "frequency": 845, "avg_conversion_time_hours": 2.3},
      {"pattern": ["email_click", "website_visit", "add_to_cart"], "support": 0.28, "frequency": 562, "avg_conversion_time_hours": 1.8}
    ],
    "abandonment": [
      {"pattern": ["organic_search", "website_visit"], "support": 0.35, "frequency": 1240, "avg_time_to_abandonment_hours": 0.5}
    ]
  },
  "pattern_divergence": [
    {"conversion_pattern": ["paid_search", "website_visit", "product_view"], "abandonment_pattern": ["paid_search", "website_visit"], "divergence_score": 0.67}
  ],
  "summary": {
    "total_journeys": 2008,
    "total_patterns_found": 47,
    "avg_pattern_length": 3.2
  }
}
```

## Campo Reference

| Field | Type | Description | Significado Empresarial |
|-------|------|-------------|------------------|
| `frequent_patterns` | object | Frequent journey sequences grouped by outcome. | Common paths customers take. |
| `frequent_patterns.conversion` | array | Patterns from converting journeys. | Successful journey sequences to replicate. |
| `frequent_patterns[].pattern` | array | Ordered touchpoint sequence. | Specific path customers follow. |
| `frequent_patterns[].support` | float | Percentage of journeys containing this pattern (0-1). | How common this pattern is. 0.42 = 42% of journeys. |
| `frequent_patterns[].frequency` | integer | Absolute count of journeys with this pattern. | Volume of customers following this path. |
| `pattern_divergence` | array | Points where conversion and abeonment paths diverge. | Critical decision points in the journey. |

## Typical Workflow

### 1. Collect Journey Data
Extract customer journeys with ordered touchpoints and outcomes.

### 2. Configure Pattern Mining
Set min_support (0.05-0.10 typical) and max_pattern_length (3-5).

### 3. Analyze Frequent Patterns
- **Conversion patterns:** Optimize and guide customers along these paths
- **Abeonment patterns:** Identify drop-off points and fix friction

### 4. Compare Pattern Divergence
Find where successful and failed journeys split - critical optimization points.

### 5. Implement Guided Experiences
Use patterns to create recommended next-action suggestions for customers.

## Perguntas Frequentes

### Q: What's the difference between this and Markov analysis?
A: Journey Sequences finds specific frequent patterns (e.g., "80% of converters follow: email → website → cart"). Markov analysis calculates transition probabilities between any two touchpoints. Use sequences for pattern discovery, Markov for probability modeling.

### Q: How of the I choose min_support threshold?
A: Start with 0.05 (5%). Lower = more patterns but includes rare ones. Higher = fewer patterns but more statistically significant. Adjust based on data volume: 10K+ journeys use 0.05, <1K journeys use 0.10-0.15.

### Q: What if I find too many patterns?
A: Increase min_support or reduce max_pattern_length. Focus on patterns with high support (>0.20) and patterns that diverge between converters and abeoners.

## Notes

### Desempenho
- **Tempo of Processamento:** 300-600ms for 100,000 journeys
- **Requisitos of Dados:** Minimum 1,000 journeys for meaningful patterns
- **Scalability:** Parallel mining for >1M sequences

### Use Cases
- **Journey optimization:** Identify and replicate high-converting sequences
- **Content recommendations:** Suggest next best action based on common patterns
- **Friction detection:** Find abeonment patterns to fix

## Como é Calculado

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns journey sequence analysis results. |
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

## Como é Calculado

The journey sequence analysis uses sequential pattern mining algorithms to discover frequent and meaningful customer behavior patterns:

### 1. Sequential Pattern Mining

The system employs advanced pattern discovery algorithms:

- **Apriori-Based Mining:** Uses sequential Apriori algorithm to find frequent subsequences meeting minimum support threshold
- **Prefix Span Algorithm:** Efficiently mines sequential patterns using pattern-growth methodology for large datasets
- **Constraint-Based Filtering:** Applies gap constraints, time windows, and minimum/maximum length filters

### 2. Pattern Discovery Process

- **Step 1:** Convert raw journey data into ordered sequences of events with timestamps
- **Step 2:** Mine frequent sequences using minimum support (Default: 1% of all journeys)
- **Step 3:** Identify closed sequences (maximal patterns that subsume smaller patterns)
- **Step 4:** Rank patterns by support, confidence, and lift metrics

### 3. Sequence Analysis

- **Frequency Analysis:** Calculate absolute and relative support for each discovered pattern
- **Temporal Patterns:** Identify time-based patterns (hourly, daily, seasonal trends in sequences)
- **Divergence Detection:** Find sequences that differentiate converters from non-converters

### 4. Desempenho and Optimization

- **Tempo of Processamento:** 300-600ms for 100,000 journey sequences
- **Requisitos of Dados:** Minimum 1,000 journeys for meaningful pattern discovery
- **Scalability:** Parallel mining for datasets exceeding 1 million sequences
- **Pattern Pruning:** Automatically removes redundant and statistically insignificant patterns

## Relacionado

- [Customer Journey (Markov)](JourneyMarkov.md) - Markov chain analysis for journey paths
- [Channel Attribution](ChannelAttribution.md) - Marketing channel attribution modeling
- [Customer Segmentation](../CustomerIntelligence/CustomerSegmentation.md) - Group customers by behavioral patterns
- [Propensity to Respond to Campaign](../Propensity/PropensityRespondCampaign.md) - Predict campaign Resposta likelihood

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:308`
