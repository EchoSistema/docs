# Artificial Intelligence – Sentiment Report

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-report
```

Generates comprehensive sentiment analysis reports by aggregating and analyzing customer feedback from multiple sources over time. Provides executive-level insights with trend analysis, anomaly detection, and actionable recommendations.

**Business Value:** Transforms scattered customer feedback into strategic insights, enabling data-driven decisions that improve customer satisfaction by 15-25%. Organizations typically see 30-40% reduction in time spent on manual sentiment analysis and faster identification of emerging issues.

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
| sentiment_data | array | Yes | - | array of sentiment registros from various sources with timestamps and scores. | Historical customer feedback sentiment to analyze for trends and patterns. |
| time_period | string | No | `30d` | Analysis time window: `7d`, `30d`, `90d`, `6m`, `1y`, or custom date range. | How far back to analyze sentiment trends. Longer periods reveal strategic patterns. |
| granularity | string | No | `daily` | Temporal aggregation: `hourly`, `daily`, `weekly`, `monthly`. | Level of detail for trend visualization and reporting. |
| group_by | array | No | `[]` | Dimensions to segment report: `source`, `product`, `region`, `segment`, `channel`. | How to break down sentiment for comparative analysis. |
| include_topics | boolean | No | `true` | Extract and rank common topics from feedback text using NLP. | Identifies what customers are talking about most. |
| include_trends | boolean | No | `true` | Calculate trend direction and statistical significance. | Shows whether sentiment is improving, declining, or stable. |
| include_anomalies | boolean | No | `true` | Detect unusual sentiment spikes or drops. | Flags unexpected changes requiring immediate attention. |
| benchmark_comparison | string | No | `null` | Compare against: `industry_avg`, `historical_avg`, or custom benchmark value. | Contextualizes your sentiment vs. competitors or past performance. |

### Sentiment Data object Fields

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| id | string | Yes | Unique identifier for the sentiment record. | `"sentiment_12345"` |
| timestamp | string | Yes | ISO 8601 timestamp when sentiment was recorded. | `"2025-11-01T10:30:00Z"` |
| score | float | Yes | Sentiment score from -1 (very negative) to +1 (very positive). | `0.75`, `-0.42` |
| sentiment | string | Yes | Classification: `positive`, `neutral`, or `negative`. | `"positive"` |
| source | string | No | Origin of the feedback. | `"product_review"`, `"support_ticket"`, `"social_media"` |
| text | string | No | Original feedback text (for topic extraction). | `"Great product but shipping was slow"` |
| metadata | object | No | Additional categorization (product_id, region, etc.). | `{"product_id": "PROD-123", "region": "US-West"}` |

## Examples

### Exemplo of Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "sentiment_data": [
      {
        "id": "s_001",
        "timestamp": "2025-11-01T10:00:00Z",
        "score": 0.85,
        "sentiment": "positive",
        "source": "product_review",
        "text": "Excellent product quality and fast delivery",
        "metadata": {"product_id": "PROD-123", "region": "US-East"}
      },
      {
        "id": "s_002",
        "timestamp": "2025-11-01T14:30:00Z",
        "score": -0.72,
        "sentiment": "negative",
        "source": "support_ticket",
        "text": "Product arrived damaged, support was unhelpful",
        "metadata": {"product_id": "PROD-456", "region": "US-West"}
      }
    ],
    "time_period": "30d",
    "granularity": "daily",
    "group_by": ["source", "product"],
    "include_topics": true,
    "include_trends": true,
    "include_anomalies": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-report"
```

### Exemplo of Requisição (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-report', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    sentiment_data: [
      {
        id: 's_001',
        timestamp: '2025-11-01T10:00:00Z',
        score: 0.85,
        sentiment: 'positive',
        source: 'product_review',
        text: 'Excellent product quality and fast delivery',
        metadata: {product_id: 'PROD-123', region: 'US-East'}
      },
      {
        id: 's_002',
        timestamp: '2025-11-01T14:30:00Z',
        score: -0.72,
        sentiment: 'negative',
        source: 'support_ticket',
        text: 'Product arrived damaged, support was unhelpful',
        metadata: {product_id: 'PROD-456', region: 'US-West'}
      }
    ],
    time_period: '30d',
    granularity: 'daily',
    group_by: ['source', 'product'],
    include_topics: true,
    include_trends: true,
    include_anomalies: true
  })
});

const result = await response.json();
```

### Exemplo of Requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'en',
])->post('https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-report', [
    'sentiment_data' => [
        [
            'id' => 's_001',
            'timestamp' => '2025-11-01T10:00:00Z',
            'score' => 0.85,
            'sentiment' => 'positive',
            'source' => 'product_review',
            'text' => 'Excellent product quality and fast delivery',
            'metadata' => ['product_id' => 'PROD-123', 'region' => 'US-East'],
        ],
        [
            'id' => 's_002',
            'timestamp' => '2025-11-01T14:30:00Z',
            'score' => -0.72,
            'sentiment' => 'negative',
            'source' => 'support_ticket',
            'text' => 'Product arrived damaged, support was unhelpful',
            'metadata' => ['product_id' => 'PROD-456', 'region' => 'US-West'],
        ],
    ],
    'time_period' => '30d',
    'granularity' => 'daily',
    'group_by' => ['source', 'product'],
    'include_topics' => true,
    'include_trends' => true,
    'include_anomalies' => true,
]);

$result = $response->json();
```

## Response

### Success `200 OK`

```json
{
  "summary": {
    "total_records": 15420,
    "time_period": "30d",
    "avg_sentiment_score": 0.42,
    "sentiment_distribution": {
      "positive": 0.58,
      "neutral": 0.27,
      "negative": 0.15
    },
    "trend": {
      "direction": "improving",
      "change_rate": 0.08,
      "statistical_significance": 0.95,
      "p_value": 0.023
    }
  },
  "time_series": [
    {
      "period": "2025-10-01",
      "avg_score": 0.38,
      "positive_pct": 0.54,
      "neutral_pct": 0.29,
      "negative_pct": 0.17,
      "volume": 512
    },
    {
      "period": "2025-10-02",
      "avg_score": 0.41,
      "positive_pct": 0.56,
      "neutral_pct": 0.28,
      "negative_pct": 0.16,
      "volume": 498
    }
  ],
  "segments": [
    {
      "dimension": "source",
      "value": "product_review",
      "avg_score": 0.62,
      "positive_pct": 0.72,
      "neutral_pct": 0.18,
      "negative_pct": 0.10,
      "volume": 6420,
      "trend": "stable"
    },
    {
      "dimension": "source",
      "value": "support_ticket",
      "avg_score": -0.15,
      "positive_pct": 0.28,
      "neutral_pct": 0.32,
      "negative_pct": 0.40,
      "volume": 2150,
      "trend": "improving"
    }
  ],
  "topics": {
    "positive": [
      {"topic": "product_quality", "frequency": 3450, "avg_score": 0.78},
      {"topic": "fast_delivery", "frequency": 2890, "avg_score": 0.72},
      {"topic": "customer_service", "frequency": 1820, "avg_score": 0.65}
    ],
    "negative": [
      {"topic": "shipping_delays", "frequency": 850, "avg_score": -0.68},
      {"topic": "damaged_packaging", "frequency": 620, "avg_score": -0.75},
      {"topic": "poor_support", "frequency": 420, "avg_score": -0.82}
    ]
  },
  "anomalies": [
    {
      "date": "2025-10-15",
      "type": "sentiment_drop",
      "severity": "high",
      "avg_score": -0.32,
      "expected_score": 0.42,
      "deviation": -0.74,
      "z_score": -3.2,
      "likely_causes": ["shipping_delays", "product_defects"],
      "affected_volume": 145
    }
  ],
  "recommendations": [
    "Address shipping delays immediately - primary driver of negative sentiment",
    "Investigate product quality issues for PROD-456 (negative sentiment spike on 2025-10-15)",
    "Amplify positive feedback about fast delivery in marketing materials",
    "Improve support ticket resolution process - sentiment improved but still below neutral"
  ]
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The sentiment_data field is required and must contain at least 100 records for meaningful analysis."
}
```

### Error `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid sentiment data structure",
  "details": {
    "sentiment_data.0.score": "Score must be between -1 and 1",
    "sentiment_data.1.timestamp": "Invalid ISO 8601 timestamp format"
  }
}
```

## Campo Reference

| Field | Type | Description | Significado Empresarial |
|-------|------|-------------|------------------|
| `summary` | object | Overall sentiment metrics for the entire analysis period. | Executive-level snapshot of customer sentiment health. |
| `summary.total_registros` | integer | Total number of sentiment registros analyzed. | Sample size for statistical confidence. |
| `summary.time_period` | string | Analysis window (e.g., "30d", "90d"). | How far back the data spans. |
| `summary.avg_sentiment_score` | float | Mean sentiment score across all registros (-1 to +1). | Overall customer satisfaction indicator: >0.5 = happy, <0 = issues. |
| `summary.sentiment_distribution` | object | Percentage breakdown of positive/neutral/negative sentiment. | Distribution of customer satisfaction levels. |
| `summary.sentiment_distribution.positive` | float | Percentage of positive sentiment (0-1). | Proportion of satisfied customers. |
| `summary.sentiment_distribution.neutral` | float | Percentage of neutral sentiment (0-1). | Proportion of indifferent customers. |
| `summary.sentiment_distribution.negative` | float | Percentage of negative sentiment (0-1). | Proportion of dissatisfied customers requiring attention. |
| `summary.trend` | object | Statistical trend analysis over the time period. | Whether sentiment is improving, declining, or stable. |
| `summary.trend.direction` | string | Trend direction: `improving`, `declining`, or `stable`. | Quick assessment of sentiment trajectory. |
| `summary.trend.change_rate` | float | Rate of change per period (-1 to +1). | How quickly sentiment is shifting. |
| `summary.trend.statistical_significance` | float | Confidence level of the trend (0-1). | Reliability: >0.90 = strong trend, <0.70 = noise. |
| `summary.trend.p_value` | float | Statistical significance p-value. | P-value <0.05 indicates statistically significant trend. |
| `time_series` | array | Sentiment metrics aggregated by time period. | Visualization data for trend charts and dashboards. |
| `time_series[].period` | string | ISO 8601 date or datetime for the period. | Time bucket for aggregated metrics. |
| `time_series[].avg_score` | float | Average sentiment score for this period. | Customer satisfaction level during this time window. |
| `time_series[].positive_pct` | float | Percentage of positive sentiment (0-1). | Satisfaction rate for this period. |
| `time_series[].neutral_pct` | float | Percentage of neutral sentiment (0-1). | Indifference rate for this period. |
| `time_series[].negative_pct` | float | Percentage of negative sentiment (0-1). | Dissatisfaction rate for this period. |
| `time_series[].volume` | integer | Number of sentiment registros in this period. | Sample size for this time bucket. |
| `segments` | array | Sentiment breakdown by specified dimensions (source, product, region, etc.). | Comparative analysis to identify best/worst performing segments. |
| `segments[].dimension` | string | Grouping dimension name. | What this segment represents (e.g., "source", "product"). |
| `segments[].value` | string | Specific value within the dimension. | Segment identifier (e.g., "product_review", "PROD-123"). |
| `segments[].avg_score` | float | Average sentiment score for this segment. | How this segment performs vs. others. |
| `segments[].positive_pct` | float | Percentage of positive sentiment in segment (0-1). | Satisfaction rate within this segment. |
| `segments[].neutral_pct` | float | Percentage of neutral sentiment in segment (0-1). | Indifference rate within this segment. |
| `segments[].negative_pct` | float | Percentage of negative sentiment in segment (0-1). | Dissatisfaction rate within this segment. |
| `segments[].volume` | integer | Number of registros in this segment. | Sample size for segment-level statistics. |
| `segments[].trend` | string | Trend direction for this segment: `improving`, `declining`, `stable`. | Whether this segment's sentiment is getting better or worse. |
| `topics` | object | Common themes extracted from feedback text, grouped by sentiment. | What customers are praising or criticizing most. |
| `topics.positive` | array | Topics frequently mentioned in positive feedback. | Strengths and differentiators driving satisfaction. |
| `topics.negative` | array | Topics frequently mentioned in negative feedback. | Pain points and issues causing dissatisfaction. |
| `topics.{sentiment}[].topic` | string | Topic name/label extracted from text. | Theme or subject matter (e.g., "product_quality", "shipping"). |
| `topics.{sentiment}[].frequency` | integer | Number of times this topic appears. | How often customers mention this topic. |
| `topics.{sentiment}[].avg_score` | float | Average sentiment score for this topic (-1 to +1). | Sentiment intensity for this specific topic. |
| `anomalies` | array | Unusual sentiment patterns detected (spikes, drops, outliers). | Unexpected changes requiring investigation or immediate action. |
| `anomalies[].date` | string | ISO 8601 date when anomaly occurred. | When the unusual sentiment pattern was detected. |
| `anomalies[].Tipo` | string | Anomaly category: `sentiment_spike`, `sentiment_drop`, `volume_spike`. | Nature of the unusual pattern. |
| `anomalies[].severity` | string | Severity level: `low`, `medium`, `high`. | Urgency for Resposta: high = immediate, medium = 48h, low = monitor. |
| `anomalies[].avg_score` | float | Actual sentiment score during anomaly. | Observed sentiment level during the event. |
| `anomalies[].expected_score` | float | Predicted sentiment score based on baseline. | What sentiment should have been without the anomaly. |
| `anomalies[].deviation` | float | Difference between actual and expected score. | Magnitude of the anomaly. |
| `anomalies[].z_score` | float | Statistical z-score of the deviation. | Steardized measure: |z| > 3 = extreme anomaly. |
| `anomalies[].likely_causes` | array | Probable topics or issues driving the anomaly. | Root causes identified through topic analysis. |
| `anomalies[].affected_volume` | integer | Number of feedback items affected. | Scale of the anomaly impact. |
| `recommendations` | array | Actionable insights and suggested interventions based on analysis. | Prioritized actions to improve customer sentiment. |

## Status HTTP

| Status Código | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns sentiment report with comprehensive insights. |
| 400 Bad Request | Invalid request Parâmetros. Check Parâmetro types and Obrigatório fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See Resposta for details. |
| 429 Too Many Requests | Limite of taxa excedido. Retry after cooldown period. |
| 500 Internal Server Erro | Server Erro. Contact support if persistent. |
| 503 Service Unavailable | Serviço of IA temporariamente indisponível. Retry with exponential backoff. |

## Erros

### Common Erro Responses

#### Insufficient Data
```json
{
  "error": "Validation failed",
  "message": "Insufficient sentiment data for analysis",
  "code": "INSUFFICIENT_DATA",
  "details": {
    "required_minimum": 100,
    "provided": 45,
    "recommendation": "Provide at least 100 sentiment records for meaningful statistical analysis"
  }
}
```

**Solution:** Collect more sentiment data or expe the time period to reach minimum threshold.

#### Invalid Time Period
```json
{
  "error": "Validation failed",
  "message": "Invalid time_period format",
  "code": "INVALID_TIME_PERIOD",
  "details": {
    "provided": "3months",
    "expected": "Valid formats: 7d, 30d, 90d, 6m, 1y, or ISO 8601 date range"
  }
}
```

**Solution:** Use supported time period formats: `7d`, `30d`, `90d`, `6m`, `1y`.

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

### 1. Data Aggregation

The system consolidates sentiment scores from various data sources:

- **Multi-Source Collection:** Aggregates sentiment from reviews, social media, customer feedback, support tickets, surveys
- **Temporal Grouping:** Groups sentiments by time periods (hourly, daily, weekly, monthly) for trend analysis
- **Source Weighting:** Applies configurable weights to different sources based on reliability and business importance

### 2. Statistical Analysis

**Descriptive Statistics:**
- Calculate mean, median, steard deviation for sentiment scores
- Compute sentiment distribution (positive %, neutral %, negative %) across time periods

**Trend Detection:**
- Uses Mann-Kendall trend test to identify statistically significant trends
- Calculates trend slope using Theil-Sen estimator (robust to outliers)
- Computes p-value to determine statistical significance (p < 0.05 = significant)

**Anomaly Detection:**
- Z-score method: identifies values > 3 steard deviations from mean
- IQR method: flags values outside Q1 - 1.5×IQR and Q3 + 1.5×IQR
- Time series decomposition: isolates unusual residuals after removing trend/seasonality

### 3. Topic Extraction

**NLP Pipeline:**
- Text preprocessing: tokenization, stopword removal, lemmatization
- Topic modeling: Latent Dirichlet Allocation (LDA) with 5-20 topics
- Sentiment mapping: associates extracted topics with sentiment scores
- Frequency ranking: orders topics by mention count and sentiment intensity

**Topic Labeling:**
- Automated label generation using top keywords per topic
- Common patterns: "product_quality", "shipping_speed", "customer_service", "pricing"

### 4. Segmentation Analysis

**Comparative Metrics:**
- Calculates avg sentiment score per segment (source, product, region, etc.)
- Computes segment-specific sentiment distribution
- Identifies best and worst performing segments
- Detects segment-level trends

### 5. Insight Generation

**Recommendation Engine:**
- Prioritizes issues by impact (negative sentiment volume × severity)
- Suggests actions based on topic analysis and segment performance
- Ranks opportunities by potential sentiment improvement

### 6. Desempenho Characteristics

- **Tempo of Processamento:** 500ms-2s for aggregating 100,000 sentiment registros
- **Requisitos of Dados:** Minimum 100 sentiment data points for reliable statistical analysis
- **Caching:** Temporal aggregations cached for 1 hour to improve Resposta times
- **Scalability:** Heles millions of sentiment registros using time-based partitioning
- **Precisão:** Trend detection 85-92% accurate for identifying significant shifts

## Typical Workflow

### 1. Data Collection

Gather sentiment registros from all customer touchpoints:

```sql
-- Example: Aggregate sentiment data from multiple sources
SELECT
  CONCAT('sent_', id) AS id,
  created_at AS timestamp,
  sentiment_score AS score,
  CASE
    WHEN sentiment_score >= 0.1 THEN 'positive'
    WHEN sentiment_score <= -0.1 THEN 'negative'
    ELSE 'neutral'
  END AS sentiment,
  source_type AS source,
  feedback_text AS text,
  JSON_OBJECT(
    'product_id', product_id,
    'region', region,
    'customer_segment', customer_segment
  ) AS metadata
FROM (
  SELECT id, created_at, sentiment_score, 'product_review' AS source_type,
         review_text AS feedback_text, product_id, region, customer_segment
  FROM product_reviews
  WHERE created_at >= DATE_SUB(NOW(), INTERVAL 30 DAY)

  UNION ALL

  SELECT id, created_at, sentiment_score, 'support_ticket' AS source_type,
         ticket_description AS feedback_text, product_id, region, customer_segment
  FROM support_tickets
  WHERE created_at >= DATE_SUB(NOW(), INTERVAL 30 DAY)

  UNION ALL

  SELECT id, created_at, sentiment_score, 'social_media' AS source_type,
         post_content AS feedback_text, NULL AS product_id, region, customer_segment
  FROM social_mentions
  WHERE created_at >= DATE_SUB(NOW(), INTERVAL 30 DAY)
) AS combined_feedback;
```

### 2. Define Analysis Parâmetros

Configure report settings based on business needs:
- **Time Period:** `30d` for monthly reviews, `90d` for quarterly, `1y` for annual
- **Granularity:** `daily` for tactical monitoring, `weekly` for operational reports, `monthly` for executive summaries
- **Grouping:** Segment by `product` to identify problem areas, `source` to compare channels, `region` for geographic insights

### 3. API Request Submission

Submit consolidated sentiment data to generate the report.

### 4. Analyze Report Results

**Executive Summary Review:**
- Check overall avg_sentiment_score: >0.5 = healthy, 0-0.5 = neutral, <0 = issues
- Review sentiment_distribution: aim for >60% positive, <20% negative
- Assess trend direction: "improving" = good, "declining" = investigate, "stable" = maintain

**Time Series Analysis:**
- Plot time_series data to visualize sentiment trends
- Identify seasonal patterns, weekly cycles, or event-driven spikes
- Compare current period vs. historical baseline

**Segment Comparison:**
- Identify best-performing segments (highest avg_score, positive_pct)
- Flag worst-performing segments (lowest avg_score, high negative_pct)
- Investigate segments with "declining" trends

**Topic Insights:**
- Review positive topics: amplify these strengths in marketing
- Analyze negative topics: prioritize for product/service improvements
- Compare topic frequency to gauge relative importance

**Anomaly Investigation:**
- High-severity anomalies: investigate within 24 hours
- Medium-severity anomalies: review within 48-72 hours
- Low-severity anomalies: monitor for recurrence

### 5. Take Action

**For Declining Sentiment:**
- Identify root causes via negative topics
- Create action plans for top 3-5 negative drivers
- Assign ownership to product, operations, or support teams
- Set measurable improvement targets

**For Improving Sentiment:**
- Document what's working (process changes, product improvements)
- Replicate successful strategies across other areas
- Celebrate wins with teams responsible

**For Anomalies:**
- Investigate likely_causes immediately
- Correlate with business events (product launches, marketing campaigns, operational changes)
- Implement fixes or mitigation strategies
- Monitor sentiment recovery

### 6. Monitor and Iterate

**Weekly Monitoramento:**
- Re-run reports weekly to track sentiment trajectory
- Alert on new anomalies or continued declines
- Measure impact of interventions on sentiment scores

**Monthly Reviews:**
- Compare month-over-month sentiment trends
- Assess effectiveness of improvement initiatives
- Adjust strategies based on segment performance

**Quarterly Strategic Planning:**
- Use sentiment insights to inform product roadmap
- Align customer experience initiatives with pain points
- Set quarterly sentiment improvement goals

### 7. Integration with Business Systems

**BI Dashboards:**
- Feed time_series data into Tableau, Power BI, or Looker for visualization
- Create executive scorecards with summary metrics
- Build drill-down reports by segment

**Alert Systems:**
- Trigger Slack/email alerts for high-severity anomalies
- Set up automated workflows for sentiment drops
- Route negative sentiment spikes to appropriate teams

**CRM Enrichment:**
- Tag customer registros with sentiment scores
- Segment customers for targeted campaigns
- Personalize outreach based on sentiment history

## Perguntas Frequentes

### Q: What's the minimum amount of sentiment data Obrigatório for to meaningful report?

**A:** We recommend at least 100 sentiment registros for basic statistical analysis. For robust trend detection and anomaly identification, 500+ registros are ideal. Segment-level analysis requires 50+ registros per segment. If you have fewer than 100 registros, the report will still generate but with limited statistical confidence and Não trend/anomaly detection. Increase your time_period to gather more data points.

### Q: How does the system determine if to sentiment trend is "improving" vs. "stable"?

**A:** The Mann-Kendall trend test calculates statistical significance (p-value). If p < 0.05 and the slope is positive, the trend is "improving". If p < 0.05 and slope is negative, it's "declining". If p >= 0.05, the trend is "stable" (Não statistically significant change). This prevents false alarms from reom fluctuations. Check the `statistical_significance` Campo (>0.90 = high confidence trend).

### Q: Can I compare sentiment across different time periods or products?

**A:** Sim! Use the `group_by` Parâmetro with dimensions like `product`, `region`, `source`, or `segment`. The Resposta includes a `segments` array showing comparative metrics for each group. You can also run multiple reports with different time_periods and compare results client-side. For formal A/B comparisons, ensure segment sample sizes are >50 registros for statistical validity.

### Q: How are topics extracted and what if they don't make sense?

**A:** Topics are extracted using Latent Dirichlet Allocation (LDA) from the `text` Campo in sentiment_data. The system automatically labels topics based on top keywords (e.g., "shipping_delays", "product_quality"). For best results, include the original feedback text. If topics seem generic or unclear, it may indicate: (1) Insufficient text data (need 200+ text samples), (2) Very diverse feedback (Não common themes), or (3) Low-quality text (too short, boilerplate responses). Pre-process text to remove boilerplate before submission.

### Q: What's the difference between z_score and statistical_significance in anomalies?

**A:** `z_score` measures how many steard deviations an anomaly is from the mean (e.g., z_score=-3.2 means 3.2 SD below average). |z| > 3 indicates an extreme anomaly. `statistical_significance` in trends (not anomalies) is to confidence level (0-1) from the Mann-Kendall test, showing how confident we are that to trend is real vs. reom noise. For anomalies, focus on `severity` (high/medium/low) for prioritization e `z_score` for magnitude.

### Q: How often should I generate sentiment reports?

**A:** It depends on your feedback volume and use case. Tactical monitoring (high volume): Daily or weekly reports with granularity="daily" to catch issues quickly. Operational reviews (moderate volume): Weekly or monthly reports with granularity="weekly". Strategic planning (any volume): Monthly or quarterly reports with granularity="monthly" for long-term trends. High-severity anomalies should trigger immediate investigation regardless of scheduled reports.

### Q: Can the report detect seasonal sentiment patterns?

**A:** Sim, if you provide sufficient historical data (>90 dias). The time_series data will reveal recurring patterns (e.g., holiday sentiment spikes, quarterly dips). Use time_period="1y" with granularity="monthly" to visualize annual seasonality. The trend analysis accounts for seasonality when determining if sentiment is improving/declining. For formal seasonal decomposition, export time_series data and use statistical tools like R or Python's statsmodels.

### Q: What should I of the when there's to high-severity anomaly?

**A:** Follow this protocol: (1) Investigate within 24 hours - check `likely_causes` for root issues. (2) Correlate with business events - did something change that day (product launch, outage, marketing campaign)? (3) Review affected_volume to gauge impact. (4) Read raw feedback for the anomaly date to underste customer issues. (5) Implement immediate fix or mitigation. (6) Monitor sentiment recovery with daily reports. (7) Document findings and update processes to prevent recurrence.

### Q: How of the recommendations get generated?

**A:** The recommendation engine uses to rule-based system that analyzes: (1) Negative topics with high frequency and low sentiment scores, (2) Segments with declining trends or poor performance, (3) Detected anomalies requiring investigation, (4) Positive topics to amplify. Recommendations are ranked by potential impact (negative sentiment volume × severity). They're intended as starting points - combine with domain expertise for final action plans.

## Manuais Comerciais

| Use Case | Action | Expected Impact |
|----------|--------|-----------------|
| **Product Quality Crisis Detection** | If anomaly detection flags to high-severity sentiment_drop for to specific product (via metadata.product_id), immediately halt shipments, investigate production batch, notify QA team, and launch customer outreach program for affected buyers. | Prevent 60-80% of potential negative reviews, reduce churn by 35-50% through proactive Resposta, contain reputation damage within 24-48 hours vs. 7-10 dias reactive approach. |
| **Customer Support Improvement** | If segments analysis shows "support_ticket" source has negative_pct > 40% and declining trend, prioritize support process improvements: reduce resolution time, improve agent training, implement proactive follow-ups for negative sentiment tickets. | Increase support CSAT by 20-30%, reduce ticket volume by 15% through proactive issue resolution, improve first-contact resolution rate by 25%, shift support sentiment from negative to neutral/positive within 60-90 dias. |
| **Feature Prioritization** | Analyze negative topics to identify top 3-5 product features with lowest sentiment scores and highest frequency. Prioritize development resources to address these pain points in next sprint/release. | Increase feature satisfaction by 30-40%, reduce negative reviews mentioning those features by 50-70%, improve overall product sentiment score by 0.10-0.15 points within 90 dias post-fix. |
| **Marketing Message Optimization** | Use positive topics (high avg_score, high frequency) to inform marketing messaging. Highlight strengths customers already appreciate. Use topic sentiment to A/B test messaging variations. | Increase ad click-through rates by 15-25% through customer-validated messaging, improve conversion rates by 10-20% by emphasizing proven value propositions, reduce cost-per-acquisition by focusing on resonant themes. |
| **Regional Desempenho Management** | If group_by="region" shows certain geographies with significantly lower sentiment, investigate regional operations: shipping partners, local customer service, product availability, cultural factors. | Identify operational inefficiencies in 1-2 weeks vs. 6-8 weeks of manual analysis, equalize regional sentiment scores within 3-6 meses, reduce regional churn disparities by 40-60%. |
| **Competitor Benchmarking** | Include competitor sentiment data (from review scraping or social listening) with benchmark_comparison="competitor_avg". Identify where your sentiment outperforms or underperforms competitors. | Prioritize investments in areas where competitors excel (close gaps), amplify marketing in areas where you excel (differentiation), improve competitive win rate by 15-25% through data-driven positioning. |
| **Executive Reporting Automation** | Replace manual sentiment analysis (40+ hours/month) with automated weekly/monthly reports. Generate executive summaries with summary metrics, trend direction, and top recommendations. | Reduce reporting time from 40 hours to <2 hours per month, increase reporting frequency from quarterly to monthly/weekly, enable data-driven decisions 30-60 dias faster, improve strategic alignment through consistent metrics. |

## Notes

### Implementation Melhores Práticas

**Qualidade of Dados:**
- Ensure timestamps are accurate and in ISO 8601 format (UTC timezone recommended)
- Validate sentiment scores are in -1 to +1 range
- Include text Campo for topic extraction (min 200 samples for meaningful topics)
- Use consistent source naming conventions across all sentiment data

**Statistical Validity:**
- Minimum 100 registros for basic analysis
- Minimum 500 registros for robust trend detection
- Minimum 50 registros per segment for segment-level analysis
- Longer time periods (90d+) improve trend reliability

**Interpretation Guidelines:**
- Avg sentiment score: >0.5 = excellent, 0.3-0.5 = good, 0-0.3 = neutral, <0 = needs improvement
- Sentiment distribution: Aim for >60% positive, <20% negative
- Statistical significance: p < 0.05 = confident trend, p > 0.05 = noise
- Z-score for anomalies: |z| > 3 = extreme, |z| > 2 = moderate, |z| < 2 = minor

**Frequency Recommendations:**
- Daily reports: High-volume businesses (>500 sentiment registros/day) for tactical monitoring
- Weekly reports: Moderate-volume businesses (100-500/day) for operational reviews
- Monthly reports: Low-volume businesses (<100/day) or executive strategic planning
- Quarterly reports: Annual planning, competitive benchmarking, long-term trend analysis

### Desempenho and Scalability

- **Maximum Payload:** 10MB per request (~100,000 sentiment registros at 100 bytes each)
- **Timeout:** 300 seconds (5 minutes) for very large datasets
- **Rate Limiting:** 50 requisições por minuto por tenant
- **Taxa of Transferência:** Can process 100,000 registros in 1-2 seconds
- **Caching:** Temporal aggregations cached for 1 hour (use cache busting for real-time updates)

### Topic Extraction Optimization

**Melhores Práticas:**
- Include original feedback text in `text` Campo (minimum 10 characters per record)
- Aim for 200+ text samples for meaningful topic extraction
- Pre-process text to remove boilerplate, signatures, disclaimers
- Use consistent language (don't mix EN/ES/PT in same request)
- Topic quality improves with more diverse vocabulary

**Topic Labeling:**
- Topics automatically labeled with top keywords
- Common patterns: "product_quality", "shipping_speed", "customer_service", "value_for_money"
- If labels seem unclear, may indicate diverse/unfocused feedback

### Anomaly Detection Tuning

**Sensitivity:**
- Padrão threshold: |z| > 2.5 (flags top 1% of outliers)
- High sensitivity: |z| > 2.0 (more anomalies, higher false positive rate)
- Low sensitivity: |z| > 3.0 (fewer anomalies, may miss issues)

**Severity Calculation:**
- High: |z| > 3 AND affected_volume > 50
- Medium: |z| > 2.5 OR affected_volume > 30
- Low: |z| > 2 OR affected_volume > 10

### Security and Privacy

- All sentiment data encrypted in transit (TLS 1.3) and em repouso
- Sentiment registros not stored after report generation (ephemeral processing)
- Customer IDs and metadata hashed for privacy
- GDPR compliant - data retention: 0 dias (immediate deletion after Resposta)
- Aggregate metrics only - individual feedback text not exposed in reports

### Integration Patterns

**BI Dashboard Integration:**
- Export time_series data to Tableau, Power BI, Looker for visualization
- Use summary metrics for executive scorecards
- Drill down via segments for detailed analysis

**Alerting Integration:**
- Monitor anomalies array for high-severity events
- Trigger Slack/email/PagerDuty notifications for immediate Resposta
- Set up automated workflows for declining trends

**Data Pipeline:**
- Schedule daily/weekly API calls via cron jobs or Airflow
- Store historical report results in data warehouse for long-term trend analysis
- Compare sequential reports to track improvement initiatives

## Relacionado

- [Sentiment Analysis](SentimentAnalysis.md) - Real-time sentiment analysis of customer text
- [NPS](../CustomerIntelligence/NPS.md) - Net Promoter Score calculation and analysis
- [Customer Loyalty](../CustomerIntelligence/CustomerLoyalty.md) - Customer loyalty scoring and prediction
- [Segmentation Report](../CustomerIntelligence/SegmentationReport.md) - Comprehensive customer segmentation insights
- [Journey Markov](JourneyMarkov.md) - Customer journey path analysis

## References

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:328`
