# Artificial Intelligence – Channel Attribution

## Endpoint

```
POST /api/v1/ai/echointel/analytics/channel-attribution
```

Calculates marketing channel contribution to conversions using multiple attribution models (first-touch, last-touch, linear, time-decay, position-based, y data-driven Shapley value) to determine accurate channel ROI y optimize marketing spend allocation.

**Business Value:** Reveals true marketing channel effectiveness beyond last-click attribution, enabling budget reallocation that can improve marketing ROI by 20-40% through evidence-based channel investment decisions.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). |
| X-Secret           | string | Condicional | 64-caracteres de secreto. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Tipo       | string | Sí         | `application/json`. |

## Parámetros

> **Note:** Los parámetros aceptan tanto `snake_case` y `camelCase`.

### Cuerpo de la Solicitud Parámetros

| Parámetro | Tipo | Requerido | Por Defecto | Descripción | Significado Empresarial |
|-----------|------|----------|---------|-------------|------------------|
| journey_data | array | Sí | - | array of customer journey objects containing touchpoint sequences y conversion outcomes. | Complete customer interaction history across all marketing channels. |
| attribution_models | array | No | `["last_touch", "first_touch", "linear", "time_decay", "shapley"]` | Attribution models to apply. Available: `first_touch`, `last_touch`, `linear`, `time_decay`, `position_based`, `shapley`. | Different methodologies for crediting channels. Use multiple models to compare y validate insights. |
| time_decay_half_life | integer | No | `7` | Half-life in días for time-decay model (how quickly touchpoint credit decreases). | Controls how much recent touchpoints are favored. 7 días = styard, 1 day = aggressive recency bias. |
| position_based_weights | object | No | `{"first": 0.4, "last": 0.4, "middle": 0.2}` | Weight distribution for position-based attribution (first, last, middle touchpoints). | Customize credit allocation: 40% first touch, 40% last touch, 20% distributed to middle. |
| conversion_value_field | string | No | `"revenue"` | Campo name containing conversion value (for revenue-weighted attribution). | Which metric to use for attribution: revenue, profit, lifetime_value, etc. |
| include_comparison | boolean | No | `true` | Include side-by-side comparison of all attribution models. | Enables model comparison table to see how different models credit each channel. |
| group_by_period | string | No | `null` | Group attribution results by time period: `daily`, `weekly`, `monthly`. | Track channel attribution trends over time. |

### Journey Data object Fields

| Campo | Tipo | Requerido | Descripción | Example |
|-------|------|----------|-------------|---------|
| journey_id | string | Sí | Unique identifier for the customer journey. | `"J12345"` |
| touchpoints | array | Sí | Ordered array of marketing touchpoint interactions. | `[{"channel": "paid_search", "timestamp": "2025-11-01T10:00:00Z"}, ...]` |
| converted | boolean | Sí | Whether the journey resulted in conversion. | `true` |
| conversion_value | float | Condicional | Value of the conversion (Requerido if using value-weighted attribution). | `299.99` |
| conversion_timestamp | string | No | ISO 8601 timestamp of conversion event. | `"2025-11-05T14:30:00Z"` |

### Touchpoint object Fields

| Campo | Tipo | Requerido | Descripción | Example |
|-------|------|----------|-------------|---------|
| channel | string | Sí | Marketing channel name (e.g., paid_search, email, organic, social). | `"paid_search"` |
| timestamp | string | Sí | ISO 8601 timestamp of the touchpoint interaction. | `"2025-11-01T10:00:00Z"` |
| campaign | string | No | Campaign identifier for detailed attribution. | `"SUMMER2025"` |
| cost | float | No | Cost associated with this touchpoint (for ROI calculation). | `2.50` |

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "journey_data": [
      {
        "journey_id": "J001",
        "touchpoints": [
          {"channel": "paid_search", "timestamp": "2025-11-01T10:00:00Z", "cost": 3.50},
          {"channel": "organic_search", "timestamp": "2025-11-02T14:00:00Z", "cost": 0},
          {"channel": "email", "timestamp": "2025-11-04T09:00:00Z", "cost": 0.25},
          {"channel": "paid_social", "timestamp": "2025-11-05T11:00:00Z", "cost": 2.00}
        ],
        "converted": true,
        "conversion_value": 299.99,
        "conversion_timestamp": "2025-11-05T11:30:00Z"
      }
    ],
    "attribution_models": ["first_touch", "last_touch", "linear", "time_decay", "shapley"],
    "time_decay_half_life": 7,
    "include_comparison": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/channel-attribution"
```

### Ejemplo de Solicitud (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/analytics/channel-attribution', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    journey_data: [
      {
        journey_id: 'J001',
        touchpoints: [
          {channel: 'paid_search', timestamp: '2025-11-01T10:00:00Z', cost: 3.50},
          {channel: 'organic_search', timestamp: '2025-11-02T14:00:00Z', cost: 0},
          {channel: 'email', timestamp: '2025-11-04T09:00:00Z', cost: 0.25},
          {channel: 'paid_social', timestamp: '2025-11-05T11:00:00Z', cost: 2.00}
        ],
        converted: true,
        conversion_value: 299.99
      }
    ],
    attribution_models: ['first_touch', 'last_touch', 'linear', 'time_decay', 'shapley'],
    include_comparison: true
  })
});

const result = await response.json();
```

### Ejemplo de Solicitud (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'en',
])->post('https://echosistema.online/api/v1/ai/echointel/analytics/channel-attribution', [
    'journey_data' => [
        [
            'journey_id' => 'J001',
            'touchpoints' => [
                ['channel' => 'paid_search', 'timestamp' => '2025-11-01T10:00:00Z', 'cost' => 3.50],
                ['channel' => 'organic_search', 'timestamp' => '2025-11-02T14:00:00Z', 'cost' => 0],
                ['channel' => 'email', 'timestamp' => '2025-11-04T09:00:00Z', 'cost' => 0.25],
                ['channel' => 'paid_social', 'timestamp' => '2025-11-05T11:00:00Z', 'cost' => 2.00],
            ],
            'converted' => true,
            'conversion_value' => 299.99,
        ],
    ],
    'attribution_models' => ['first_touch', 'last_touch', 'linear', 'time_decay', 'shapley'],
    'include_comparison' => true,
]);

$result = $response->json();
```

## Respuesta

### Éxito `200 OK`

```json
{
  "attribution_results": {
    "first_touch": {
      "paid_search": {"conversions": 487, "attributed_value": 145890.50, "percentage": 100.0},
      "organic_search": {"conversions": 0, "attributed_value": 0, "percentage": 0},
      "email": {"conversions": 0, "attributed_value": 0, "percentage": 0},
      "paid_social": {"conversions": 0, "attributed_value": 0, "percentage": 0}
    },
    "last_touch": {
      "paid_search": {"conversions": 89, "attributed_value": 26685.30, "percentage": 18.3},
      "organic_search": {"conversions": 142, "attributed_value": 42544.20, "percentage": 29.2},
      "email": {"conversions": 98, "attributed_value": 29356.80, "percentage": 20.1},
      "paid_social": {"conversions": 158, "attributed_value": 47304.20, "percentage": 32.4}
    },
    "linear": {
      "paid_search": {"conversions": 121.75, "attributed_value": 36472.63, "percentage": 25.0},
      "organic_search": {"conversions": 121.75, "attributed_value": 36472.63, "percentage": 25.0},
      "email": {"conversions": 121.75, "attributed_value": 36472.63, "percentage": 25.0},
      "paid_social": {"conversions": 121.75, "attributed_value": 36472.63, "percentage": 25.0}
    },
    "time_decay": {
      "paid_search": {"conversions": 45.2, "attributed_value": 13542.18, "percentage": 9.3},
      "organic_search": {"conversions": 68.9, "attributed_value": 20640.51, "percentage": 14.1},
      "email": {"conversions": 112.4, "attributed_value": 33675.96, "percentage": 23.1},
      "paid_social": {"conversions": 260.5, "attributed_value": 78031.85, "percentage": 53.5}
    },
    "shapley": {
      "paid_search": {"conversions": 156.3, "attributed_value": 46823.47, "percentage": 32.1},
      "organic_search": {"conversions": 98.7, "attributed_value": 29572.13, "percentage": 20.3},
      "email": {"conversions": 87.2, "attributed_value": 26123.28, "percentage": 17.9},
      "paid_social": {"conversions": 144.8, "attributed_value": 43371.62, "percentage": 29.7}
    }
  },
  "model_comparison": {
    "channels": ["paid_search", "organic_search", "email", "paid_social"],
    "models": ["first_touch", "last_touch", "linear", "time_decay", "shapley"],
    "comparison_matrix": [
      [100.0, 18.3, 25.0, 9.3, 32.1],
      [0.0, 29.2, 25.0, 14.1, 20.3],
      [0.0, 20.1, 25.0, 23.1, 17.9],
      [0.0, 32.4, 25.0, 53.5, 29.7]
    ]
  },
  "roi_analysis": {
    "paid_search": {
      "total_cost": 12450.00,
      "attributed_revenue_shapley": 46823.47,
      "roi": 2.76,
      "roas": 376
    },
    "email": {
      "total_cost": 890.50,
      "attributed_revenue_shapley": 26123.28,
      "roi": 28.33,
      "roas": 2933
    },
    "paid_social": {
      "total_cost": 8920.00,
      "attributed_revenue_shapley": 43371.62,
      "roi": 3.86,
      "roas": 486
    }
  },
  "summary": {
    "total_conversions": 487,
    "total_conversion_value": 145890.50,
    "total_journeys_analyzed": 2840,
    "avg_touchpoints_per_journey": 3.7,
    "conversion_rate": 0.171,
    "recommended_model": "shapley",
    "model_agreement_score": 0.68
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The journey_data field is required and must contain at least one journey with touchpoints."
}
```

### Error `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid journey structure",
  "details": {
    "journey_data.0.touchpoints": "Each journey must have at least one touchpoint.",
    "journey_data.0.converted": "The converted field is required and must be a boolean."
  }
}
```

## Campo Reference

| Campo | Tipo | Descripción | Significado Empresarial |
|-------|------|-------------|------------------|
| `attribution_results` | object | Attribution results grouped by model, showing how each model credits channels. | Comprehensive view of channel contribution across different methodologies. |
| `attribution_results.{model}.{channel}` | object | Channel attribution metrics for a specific model. | How much credit a channel receives under a particular attribution methodology. |
| `attribution_results.{model}.{channel}.conversions` | float | Number of conversions attributed to this channel. May be fractional for distributed models. | Conversion volume credit. Higher = more important for driving conversions. |
| `attribution_results.{model}.{channel}.attributed_value` | float | Total conversion value attributed to this channel. | Revenue or value credit. Higher = more important for driving revenue. |
| `attribution_results.{model}.{channel}.percentage` | float | Percentage of total attributed value for this channel (0-100). | Channel's relative importance. Use to allocate marketing budget proportionally. |
| `model_comparison` | object | Side-by-side comparison of how different models attribute value to channels. | Reveals attribution model sensitivity. High variance = model choice matters significantly. |
| `model_comparison.comparison_matrix` | array | Matrix where rows are channels y columns are models, values are percentage attribution. | Quick visual comparison of model differences. Identify channels with consistent vs. variable attribution. |
| `model_comparison.models` | array | List of attribution models included in the comparison. | Models analyzed in this request. |
| `model_comparison.channels` | array | List of channels analyzed. | All unique channels found in journey data. |
| `roi_analysis` | object | Return on investment analysis per channel (only for channels with cost data). | Financial performance of each marketing channel. |
| `roi_analysis.{channel}.total_cost` | float | Total marketing spend on this channel across all journeys. | Investment in channel. |
| `roi_analysis.{channel}.attributed_revenue_shapley` | float | Revenue attributed to channel using Shapley value (recommended model). | Return from channel investment. |
| `roi_analysis.{channel}.roi` | float | Return on Investment = (Revenue - Cost) / Cost. | Profitability multiplier. ROI of 2.76 = 276% return ($2.76 earned per $1 spent). |
| `roi_analysis.{channel}.roas` | integer | Return on Ad Spend = Revenue / Cost × 100. | Revenue efficiency. ROAS of 376 = $3.76 revenue per $1 ad spend. |
| `summary` | object | Aggregate statistics across all journeys y models. | Overall campaign performance y data quality indicators. |
| `summary.total_conversions` | integer | Total number of conversions in the dataset. | Campaign Éxito volume. |
| `summary.total_conversion_value` | float | Total value of all conversions. | Total revenue or value generated. |
| `summary.total_journeys_analyzed` | integer | Number of customer journeys analyzed (converted y non-converted). | Sample size for statistical confidence. |
| `summary.avg_touchpoints_per_journey` | float | Average number of touchpoints per journey. | Journey complexity. Higher = more multi-touch attribution needed. |
| `summary.conversion_rate` | float | Percentage of journeys that converted (0-1). | Overall campaign effectiveness. |
| `summary.recommended_model` | string | Recommended attribution model based on data characteristics. | Best model for this dataset. Shapley is recommended for multi-touch journeys. |
| `summary.model_agreement_score` | float | Correlation between different models (0-1). 1 = all models agree, 0 = No agreement. | Attribution stability. Low scores indicate high model sensitivity. |

## Estado HTTP

| Status Código | Descripción |
|-------------|-------------|
| 200 OK | Request successful. Returns channel attribution results. |
| 400 Bad Request | Invalid request Parámetros. Check Parámetro types y Requerido fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Respuesta for details. |
| 429 Too Many Requests | Límite de tasa excedido. Retry after cooldown period. |
| 500 Internal Server Error | Server Error. Contact support if persistent. |
| 503 Service Unavailable | Servicio de IA temporalmente No disponible. Retry with exponential backoff. |

## Errores

### Common Error Responses

#### Missing Requerido Parámetros
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

**Solution:** Ensure all Requerido Parámetros are provided in the Cuerpo de la Solicitud.

#### Invalid Autenticación
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid y not expired. Check `X-Customer-Api-Id` y `X-Secret` Encabezados.

## Typical Workflow

### 1. Data Collection
Extract customer journey y touchpoint data from marketing platforms:

```sql
-- Example: Extract multi-touch journey data
SELECT
  journey_id,
  JSON_ARRAYAGG(
    JSON_OBJECT(
      'channel', channel_name,
      'timestamp', interaction_timestamp,
      'campaign', campaign_id,
      'cost', channel_cost
    ) ORDER BY interaction_timestamp
  ) AS touchpoints,
  MAX(converted) AS converted,
  SUM(conversion_value) AS conversion_value
FROM marketing_touchpoints
WHERE interaction_date >= DATE_SUB(CURDATE(), INTERVAL 90 DAY)
GROUP BY journey_id
HAVING COUNT(*) >= 1;
```

### 2. Choose Attribution Models
Select models based on business needs:
- **Exploratory Analysis:** Run all models to compare results
- **Simple Attribution:** Use first_touch or last_touch for baseline
- **Advanced Attribution:** Use shapley for statistically rigorous results
- **Recency Bias:** Use time_decay when recent touchpoints matter most

### 3. Configure Model Parámetros
Adjust Parámetros to match business logic:
- Set `time_decay_half_life` based on sales cycle (1-30 días)
- Customize `position_based_weights` if first/last touches are more valuable
- Enable `include_comparison` to validate model choice

### 4. Analyze Attribution Results
Compare channel performance across models:
- Identify channels with stable attribution (low variance across models)
- Flag channels with high variance (model-sensitive attribution)
- Use Shapley values as the recommended baseline for budget decisions

### 5. Calculate Channel ROI
Use cost data to compute return on investment:
- Compare attributed revenue vs. channel cost
- Prioritize high-ROI channels for budget increases
- Audit or reduce spend on negative-ROI channels

### 6. Optimize Marketing Mix
Reallocate budget based on attribution insights:
- Increase spend on underinvested high-attribution channels
- Test new channels similar to top performers
- Reduce spend on over-attributed low-ROI channels
- Balance short-term (last-touch) y long-term (first-touch) value

### 7. Monitor y Iterate
Track attribution over time:
- Re-run monthly to detect shifts in channel effectiveness
- Compare attribution before y after marketing changes
- Use `group_by_period` to analyze seasonal trends

## Preguntas Frecuentes

### Q: Which attribution model should I use?
A: Start with Shapley value for multi-touch journeys as it provides the most statistically rigorous attribution based on game theory. Use first-touch to understy awareness drivers, last-touch for immediate conversion drivers, y time-decay for recency-weighted attribution. Running multiple models simultaneously helps validate insights.

### Q: What does it mean if different models show very different results for a channel?
A: High variance across models indicates that the channel's role varies significantly depending on its position in the journey. For example, a channel showing 40% attribution in first-touch but only 10% in last-touch is primarily an awareness driver, not a converter. Use this to align channel strategy with its actual role.

### Q: How do I interpret Shapley values?
A: Shapley value calculates each channel's marginal contribution by considering all possible combinations of channels. It answers: "How much does adding this channel to the journey increase conversion probability?" Higher Shapley values mean the channel is genuinely contributing to conversions, not just correlating with them.

### Q: What is a good ROAS (Return on Ad Spend)?
A: Industry benchmarks vary widely. E-commerce typically aims for 400-600% ROAS (4-6x return), B2B SaaS for 300-500%, y bry awareness for 200-300%. Use your own historical data as a baseline y optimize from there. ROAS below 100% means you're spending more than you're earning.

### Q: Should I only look at last-touch attribution?
A: No. Last-touch significantly undervalues awareness y nurturing channels. A customer might discover your bry through paid social, research via organic search, y convert via email. Last-touch gives 100% credit to email, ignoring the critical role of paid social y organic. Multi-touch attribution reveals the full customer journey.

### Q: How many journeys do I need for reliable attribution?
A: Minimum 500 converted journeys for basic attribution, 2,000+ for stable Shapley value calculations. More complex journeys (5+ touchpoints) require more data for statistical significance. Low sample sizes may show unstable attribution week-to-week.

### Q: What if most of my journeys are single-touch?
A: If >70% of journeys have only one touchpoint, multi-touch attribution provides limited additional value. Stick with simpler models (first-touch, last-touch) or focus on optimizing conversion rates rather than attribution complexity.

### Q: How often should I recalculate attribution?
A: Recalculate monthly for stable businesses, weekly for fast-changing campaigns. After major marketing changes (new channels, budget shifts, creative updates), wait 2-4 weeks for data to stabilize before recalculating attribution to avoid over-reacting to noise.

## Guías Comerciales

| Use Case | Action | Expected Impact |
|----------|--------|-----------------|
| **Over-Attribution to Last-Touch** | If last-touch model shows paid search at 60% but Shapley shows 30%, your awareness channels (social, display) are undervalued. Increase investment in early-journey channels. | Reduce customer acquisition cost by 15-25% by balancing full-funnel strategy. |
| **Email Undervalued** | If email shows <10% last-touch attribution but 25%+ in linear or Shapley, it's a critical nurturing channel. Increase email cadence y personalization. | Improve conversion rates by 20-30% through better email nurturing. |
| **Paid Social Low ROI** | If paid social ROAS <200% y Shapley attribution <15%, reduce spend or shift to retargeting only. Test creative y audience targeting before cutting entirely. | Reduce wasted ad spend by 30-40%, reallocate to higher-ROI channels. |
| **Organic Search High Attribution** | If organic search shows 30%+ Shapley attribution with zero direct cost, invest in SEO to amplify this high-ROI channel. Create more content targeting high-intent keywords. | Increase organic traffic by 40-60%, reducing dependency on paid channels. |
| **First-Touch Dominance** | If first-touch y last-touch agree on top channels, your funnel is efficient. Optimize conversion rates on these proven channels rather than expying channel mix. | Increase conversion rate by 15-25% through channel-specific optimization. |
| **Attribution Volatility** | If model_agreement_score <0.50, your journeys are highly variable. Segment customers by journey Tipo y create channel strategies for each segment. | Improve marketing efficiency by 20-30% through segment-specific tactics. |

## Cómo se Calcula

The channel attribution system uses multiple attribution models to assign credit for conversions across marketing touchpoints:

### 1. Attribution Models

The system supports five styard attribution models:

- **First-Touch Attribution:** Assigns 100% credit to the first touchpoint in the customer journey
- **Last-Touch Attribution:** Assigns 100% credit to the final touchpoint before conversion
- **Linear Attribution:** Distributes credit equally across all touchpoints in the journey
- **Time-Decay Attribution:** Assigns exponentially increasing credit to touchpoints closer to conversion
- **Shapley Value Attribution:** Uses game theory to calculate fair credit distribution based on marginal contribution

### 2. Computation Process

- **Step 1:** Aggregate customer journey paths from all conversion y non-conversion events
- **Step 2:** For each model, calculate attribution weights based on touchpoint positions y timestamps
- **Step 3:** Apply weights to conversion values to determine channel contribution
- **Step 4:** Normalize results to ensure total attribution equals 100% for each conversion path

### 3. Rendimiento y Optimization

- **Tiempo de Procesamiento:** 150-300ms for 10,000 journey paths
- **Requisitos de Datos:** Minimum 100 complete customer journeys for reliable attribution
- **Scalability:** Hyles up to 1 million touchpoints per request using distributed computation

### 4. Model Selection

- **Shapley Value** is recommended for the most statistically rigorous attribution but has higher computational cost (O(n!))
- **Time-Decay** provides a good balance between accuracy y performance for most use cases
- **First/Last-Touch** are useful for baseline comparisons y simple attribution scenarios

## Notas

### Attribution Model Selection Guide

**First-Touch Attribution:**
- **Use when:** Understying awareness y discovery channels
- **Best for:** Top-of-funnel optimization, bry awareness campaigns
- **Limitation:** Ignores nurturing y closing channels

**Last-Touch Attribution:**
- **Use when:** Understying immediate conversion drivers
- **Best for:** Rendimiento marketing, direct Respuesta campaigns
- **Limitation:** Ignores awareness y nurturing channels

**Linear Attribution:**
- **Use when:** All touchpoints are equally valuable
- **Best for:** Balanced multi-channel strategies
- **Limitation:** Oversimplifies complex journey dynamics

**Time-Decay Attribution:**
- **Use when:** Recent touchpoints matter more (short sales cycles)
- **Best for:** E-commerce, consumer goods with quick purchase decisions
- **Limitation:** May undervalue early awareness efforts

**Position-Based Attribution:**
- **Use when:** First y last touches are most valuable (U-shaped funnel)
- **Best for:** Traditional marketing funnels with clear awareness y conversion stages
- **Limitation:** Arbitrary weight assignment

**Shapley Value Attribution:**
- **Use when:** Need statistically rigorous, unbiased attribution
- **Best for:** Complex multi-touch journeys, budget optimization decisions
- **Limitation:** Higher computational cost, requires sufficient data

### Data Quality Requirements

**Minimum Requisitos de Datos:**
- At least 500 converted journeys for basic attribution
- At least 100 journeys per channel for reliable channel-level metrics
- Minimum 2,000 journeys for stable Shapley value calculations

**Touchpoint Calidad de Datos:**
- Ensure accurate timestamps (timezone consistency)
- Use styardized channel names across all touchpoints
- Include cost data for all paid channels for ROI analysis
- Remove bot traffic y invalid interactions

**Journey Completeness:**
- Include all touchpoints, not just paid channels
- Track organic, direct, y referral traffic
- Capture both online y offline touchpoints when possible
- Ensure conversion timestamp is after last touchpoint

### Rendimiento y Scalability

- **Maximum Payload:** 50MB per request (~300,000 journeys)
- **Tiempo de Procesamiento:** 150-300ms for 10,000 journeys, 1-3 seconds for Shapley with 100,000 journeys
- **Timeout:** 300 seconds (5 minutes) for very large datasets
- **Rate Limiting:** 50 solicitudes por minuto por tenant
- **Batch Processing:** For >500,000 journeys, split into multiple requests

### Common Pitfalls

**Attribution Window:**
- Por Defecto: 30-day attribution window (touchpoints within 30 días of conversion)
- Longer sales cycles (B2B, high-value products) may need 60-90 day windows
- Shorter cycles (impulse purchases) may only need 7-14 day windows

**Channel Naming Consistency:**
- Use consistent channel names: "paid_search" not "ppc", "adwords", "google_ads"
- Normalize campaign names for aggregation
- Separate bry vs. non-bry search for better insights

**Non-Converting Journeys:**
- Include non-converting journeys (converted=false) for more accurate baseline
- Omitting non-converters inflates attribution percentages
- Helps identify channels with high traffic but low conversion

### Security y Compliance

- All journey data is encrypted in transit (TLS 1.3) y en reposo
- Journey IDs are hashed internally for privacy
- Cost data is aggregated y anonymized in reports
- Data retention: 90 días for model training, then purged

## Relacionado

- [Customer Journey (Markov)](JourneyMarkov.md) - Analyze customer journey paths using Markov chain probabilities
- [Journey Sequences](JourneySequences.md) - Discover frequent patterns in customer journey sequences
- [Sentiment Analysis](SentimentAnalysis.md) - Analyze sentiment of customer feedback y reviews
- [Customer Segmentation](../CustomerIntelligence/CustomerSegmentation.md) - Segment customers based on behavior y characteristics

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:298`
