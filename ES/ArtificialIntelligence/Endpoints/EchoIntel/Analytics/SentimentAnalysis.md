# Artificial Intelligence – Real-Time Sentiment Analysis

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-realtime
```

Performs real-time sentiment analysis of texts (reviews, comments, feedbacks) using natural language processing (NLP).

**Business Value:** Automatically classifies customer feedback sentiment with 88-92% accuracy, enabling rapid Respuesta to negative experiences y identification of satisfaction drivers. Organizations typically see 20-30% faster issue resolution y 15% improvement in customer satisfaction scores.

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
| texts | array | Sí | - | array of text objects for sentiment analysis. Each object contains text content, ID, y optional metadata. | Customer feedback, reviews, comments, or survey responses to analyze for sentiment. |
| language | string | No | `auto` | Text language Código (`en`, `es`, `pt`, or `auto` for automatic detection). | Improves accuracy when specified, but auto-detection works for 100+ languages. |
| include_entities | boolean | No | `false` | Extract named entities (products, services, brys) mentioned in text. | Identifies what specific items customers are discussing in feedback. |
| include_aspects | boolean | No | `false` | Perform aspect-based sentiment analysis to identify sentiment per feature/topic. | Shows sentiment for specific product features (e.g., "shipping", "quality", "support"). |
| sentiment_threshold | float | No | `0.1` | Neutral zone threshold for classification (0-0.5). Higher values expy neutral category. | Adjusts sensitivity: lower threshold = more definitive classifications. |

### Text object Fields

| Campo | Tipo | Requerido | Descripción | Example |
|-------|------|----------|-------------|---------|
| id | string | Sí | Unique identifier for the text (for tracking results). | `"review_001"`, `"comment_12345"` |
| text | string | Sí | Text content to analyze (max 10,000 characters per text). | `"Great product! Fast shipping y excellent customer service."` |
| source | string | No | Source of the text (for categorization y reporting). | `"product_review"`, `"customer_feedback"`, `"social_media"` |
| metadata | object | No | Additional metadata about the text (custom fields). | `{"product_id": "PROD-123", "customer_id": "C001"}` |

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "texts": [
      {
        "id": "review_001",
        "text": "Produto excelente! Entrega rápida and atendimento impecável.",
        "source": "product_review"
      },
      {
        "id": "comment_045",
        "text": "Péssima experiência. O product chegou danificado and o suporte não resolveu.",
        "source": "customer_feedback"
      }
    ],
    "language": "pt",
    "include_entities": true,
    "include_aspects": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-realtime"
```

### Ejemplo de Solicitud (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-realtime', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    texts: [
      {
        id: 'review_001',
        text: 'Excellent product! Fast delivery and impeccable service.',
        source: 'product_review'
      },
      {
        id: 'comment_045',
        text: 'Terrible experience. The product arrived damaged and support did not help.',
        source: 'customer_feedback'
      }
    ],
    language: 'en',
    include_entities: true,
    include_aspects: true
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
])->post('https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-realtime', [
    'texts' => [
        [
            'id' => 'review_001',
            'text' => 'Excellent product! Fast delivery and impeccable service.',
            'source' => 'product_review',
        ],
        [
            'id' => 'comment_045',
            'text' => 'Terrible experience. The product arrived damaged and support did not help.',
            'source' => 'customer_feedback',
        ],
    ],
    'language' => 'en',
    'include_entities' => true,
    'include_aspects' => true,
]);

$result = $response->json();
```

## Respuesta

### Éxito `200 OK`

```json
{
  "sentiment_analysis": [
    {
      "id": "review_001",
      "text": "Produto excelente! Entrega rápida and atendimento impecável.",
      "sentiment": "positive",
      "confidence": 0.96,
      "score": 0.88,
      "aspects": [
        {"aspect": "product", "sentiment": "positive", "score": 0.92},
        {"aspect": "entrega", "sentiment": "positive", "score": 0.85},
        {"aspect": "atendimento", "sentiment": "positive", "score": 0.90}
      ],
      "entities": [
        {"entity": "product", "type": "PRODUCT"},
        {"entity": "entrega", "type": "SERVICE"},
        {"entity": "atendimento", "type": "SERVICE"}
      ],
      "keywords": ["excelente", "rápida", "impecável"]
    },
    {
      "id": "comment_045",
      "text": "Péssima experiência. O product chegou danificado and o suporte não resolveu.",
      "sentiment": "negative",
      "confidence": 0.94,
      "score": -0.82,
      "aspects": [
        {"aspect": "experiência", "sentiment": "negative", "score": -0.95},
        {"aspect": "product", "sentiment": "negative", "score": -0.78},
        {"aspect": "suporte", "sentiment": "negative", "score": -0.75}
      ],
      "entities": [
        {"entity": "product", "type": "PRODUCT"},
        {"entity": "suporte", "type": "SERVICE"}
      ],
      "keywords": ["péssima", "danificado", "não resolveu"]
    }
  ],
  "summary": {
    "total_analyzed": 2,
    "positive": 1,
    "neutral": 0,
    "negative": 1,
    "avg_score": 0.03
  }
}
```

## Campo Reference

| Campo | Tipo | Descripción | Significado Empresarial |
|-------|------|-------------|------------------|
| `sentiment_analysis` | array | Analysis results for each submitted text. | Individual sentiment assessment for every customer feedback item. |
| `sentiment_analysis[].id` | string | Unique identifier matching the input text ID. | Tracks which analysis corresponds to which feedback item. |
| `sentiment_analysis[].text` | string | Original text that was analyzed. | The actual customer feedback content. |
| `sentiment_analysis[].sentiment` | string | Overall sentiment classification: `positive`, `neutral`, or `negative`. | Quick categorization: happy customers (positive), indifferent (neutral), or dissatisfied (negative). |
| `sentiment_analysis[].confidence` | float | Model's confidence in the classification (0-1). Higher is more certain. | Reliability indicator: >0.85 = high confidence, <0.70 = review manually. |
| `sentiment_analysis[].score` | float | Continuous sentiment score from -1 (very negative) to +1 (very positive). | Granular sentiment intensity: +0.8 = very happy, -0.8 = very upset, 0.0 = neutral. |
| `sentiment_analysis[].aspects` | array | Aspect-based sentiment breakdown (if `include_aspects: true`). | Shows sentiment for specific topics like "product quality", "delivery", "support". |
| `sentiment_analysis[].aspects[].aspect` | string | Aspect/feature name extracted from text. | Specific topic or feature mentioned (e.g., "shipping", "price", "customer service"). |
| `sentiment_analysis[].aspects[].sentiment` | string | Sentiment for this specific aspect. | Whether this particular feature received positive/neutral/negative feedback. |
| `sentiment_analysis[].aspects[].score` | float | Sentiment score for this aspect (-1 to +1). | Intensity of sentiment toward this specific feature. |
| `sentiment_analysis[].entities` | array | Named entities mentioned in text (if `include_entities: true`). | Products, services, brys, or people mentioned in feedback. |
| `sentiment_analysis[].entities[].entity` | string | Entity text extracted. | Name of the product, service, or bry mentioned. |
| `sentiment_analysis[].entities[].Tipo` | string | Entity category: `PRODUCT`, `SERVICE`, `BRAND`, `PERSON`, `LOCATION`. | Classification of what Tipo of thing was mentioned. |
| `sentiment_analysis[].keywords` | array | Key sentiment-bearing words extracted from text. | Words that drove the sentiment classification (e.g., "excellent", "terrible"). |
| `summary` | object | Aggregate statistics across all analyzed texts. | Overall sentiment distribution y averages for the batch. |
| `summary.total_analyzed` | integer | Total number of texts processed. | How many feedback items were analyzed in this request. |
| `summary.positive` | integer | Count of texts classified as positive. | Number of satisfied customers in this batch. |
| `summary.neutral` | integer | Count of texts classified as neutral. | Number of indifferent or mixed feedback items. |
| `summary.negative` | integer | Count of texts classified as negative. | Number of dissatisfied customers requiring attention. |
| `summary.avg_score` | float | Average sentiment score across all texts (-1 to +1). | Overall sentiment health: positive = happy customers, negative = issues to address. |

## Sentiment Classification

| Score       | Sentiment | Descripción |
| ----------- | ---------- | --------- |
| 0.5 to 1.0   | Positive   | Clearly positive sentiment. |
| 0.1 to 0.49  | Positive   | Slightly positive sentiment. |
| -0.1 to 0.1  | Neutral    | Neutral or mixed sentiment. |
| -0.49 to -0.1| Negative   | Slightly negative sentiment. |
| -1.0 to -0.5 | Negative   | Clearly negative sentiment. |

## Estado HTTP

| Status Código | Descripción |
|-------------|-------------|
| 200 OK | Request successful. Returns sentiment analysis results. |
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

## Cómo se Calcula

The sentiment analysis system uses state-of-the-art NLP models to classify text sentiment y extract insights:

### 1. NLP Model Architecture

The system employs transformer-based models fine-tuned for sentiment classification:

- **BERT-based Models:** Uses multilingual BERT (mBERT) or language-specific BERT variants for context-aware sentiment detection
- **Aspect-Based Sentiment:** Applies targeted sentiment extraction for specific aspects/features mentioned in text
- **Entity Recognition:** Uses Named Entity Recognition (NER) to identify products, services, y brys

### 2. Sentiment Scoring Process

- **Step 1:** Text preprocessing (tokenization, normalization, lowercasing)
- **Step 2:** Contextual embedding generation using BERT encoder (768-dimensional vectors)
- **Step 3:** Classification head predicts sentiment probabilities [positive, neutral, negative]
- **Step 4:** Convert probabilities to continuous score: score = P(positive) - P(negative) ∈ [-1, +1]

### 3. Confidence y Calibration

- **Confidence Score:** Maximum class probability after temperature scaling calibration
- **Threshold-Based Classification:** Neutral zone (-0.1 to +0.1) reduces false positive/negative classifications
- **Ensemble Aggregation:** Combines predictions from multiple models for improved accuracy (optional)

### 4. Rendimiento y Optimization

- **Tiempo de Procesamiento:** 50-100ms per text (batch processing: 20 texts/second)
- **Precisión:** 88-92% on benchmark datasets (SST-2, IMDB, custom domain data)
- **Supported Languages:** 100+ languages via mBERT, optimized for EN, ES, PT
- **GPU Acceleration:** Reduces latency by 5-10x for batch requests (>50 texts)

## Typical Workflow

### 1. Data Collection y Preparation

Collect customer feedback from multiple sources:

```sql
-- Example: Extract customer feedback from database
SELECT
  CONCAT('review_', id) AS id,
  review_text AS text,
  'product_review' AS source,
  JSON_OBJECT(
    'product_id', product_id,
    'customer_id', customer_id,
    'rating', rating
  ) AS metadata
FROM product_reviews
WHERE created_at >= DATE_SUB(NOW(), INTERVAL 7 DAY)
  AND review_text IS NOT NULL
  AND LENGTH(review_text) > 10
UNION ALL
SELECT
  CONCAT('support_', ticket_id) AS id,
  customer_message AS text,
  'customer_support' AS source,
  JSON_OBJECT(
    'ticket_id', ticket_id,
    'priority', priority
  ) AS metadata
FROM support_tickets
WHERE status = 'closed'
  AND created_at >= DATE_SUB(NOW(), INTERVAL 7 DAY);
```

### 2. API Request Configuration

Choose appropriate analysis Parámetros:
- **Basic Sentiment:** Set `include_aspects: false` y `include_entities: false` for fast classification
- **Detailed Analysis:** Enable `include_aspects: true` to understy feature-specific sentiment
- **Bry Monitoreo:** Enable `include_entities: true` to track mentions of products/brys

### 3. Batch Processing

Process feedback in batches of 50-100 texts per request for optimal performance:
- Maximum 500 texts per request
- Processing time: ~50-100ms per text
- Use parallel requests for >1,000 texts

### 4. Analyze Results

**Prioritize by Urgency:**
- **High Priority:** Negative sentiment with confidence >0.85 - respond within 24 hours
- **Medium Priority:** Negative sentiment with confidence 0.70-0.85 - review within 48 hours
- **Low Priority:** Neutral sentiment or low confidence - periodic review

**Identify Patterns:**
- Group by sentiment category y analyze common aspects
- Track sentiment trends over time (daily/weekly)
- Compare sentiment across product lines or customer segments

### 5. Take Action

**For Negative Sentiment:**
- Create support tickets for high-confidence negative feedback
- Route to appropriate teams (product, support, operations)
- Follow up with personalized responses
- Track issue resolution y measure sentiment improvement

**For Positive Sentiment:**
- Identify bry advocates for testimonials y case studies
- Understy what drives satisfaction (via aspect analysis)
- Amplify positive feedback in marketing materials

**For Neutral Sentiment:**
- Investigate mixed signals in aspect-level sentiment
- Identify opportunities to convert neutral to positive
- A/B test messaging or product improvements

### 6. Monitor y Optimize

Track key sentiment metrics over time:
- Overall sentiment distribution (positive/neutral/negative %)
- Average sentiment score trend
- Sentiment by product, service, or customer segment
- Respuesta time for negative feedback
- Sentiment shift after interventions

### 7. Integration with Business Systems

**CRM Integration:**
- Update customer registros with sentiment scores
- Trigger workflows for negative sentiment (support tickets, alerts)
- Segment customers by sentiment for targeted campaigns

**Analytics Dashboards:**
- Visualize sentiment trends y distribution
- Create alerts for sentiment score drops
- Track resolution time for negative feedback

**Product Development:**
- Analyze aspect-based sentiment for feature prioritization
- Identify most-praised y most-criticized features
- Guide product roadmap based on customer sentiment

## Preguntas Frecuentes

### Q: How accurate is the sentiment analysis for different languages?

**A:** Accuracy is highest for English, Spanish, y Portuguese (88-92% on benchmark datasets). For other languages supported by multilingual BERT, accuracy ranges from 82-88%. Using the `language` Parámetro with explicit language codes improves accuracy by 2-5% compared to auto-detection. If your content is consistently in one language, always specify it. For domain-specific terminology (e.g., medical, legal), accuracy may be slightly lower (85-88%) but can be improved with custom training.

### Q: What should I do when confidence scores are below 0.70?

**A:** Low confidence (<0.70) typically indicates ambiguous text with mixed signals, sarcasm, or unusual phrasing. Recommended actions: (1) Review these texts manually to establish ground truth, (2) Use aspect-based sentiment to identify conflicting signals, (3) Consider the text as neutral until manually validated, (4) Collect more context (customer history, product rating) to disambiguate. For batch processing, flag low-confidence items for human review rather than automatically routing them to action workflows.

### Q: Can I analyze very short texts like tweets or SMS messages?

**A:** Sí, but with caveats. Minimum recommended length is 10 characters. Very short texts (5-20 words) may have lower confidence scores (0.65-0.75) due to limited context. Emoji y emoticons are interpreted as sentiment signals. For tweet-like content, accuracy is typically 80-85%. To improve short-text accuracy: (1) Enable `include_aspects: false` to focus on overall sentiment, (2) Batch similar short texts together to identify patterns, (3) Combine with metadata (ratings, emoji reactions) for validation.

### Q: How do I hyle sarcasm or irony in customer feedback?

**A:** Sarcasm detection is challenging for all sentiment models. The system uses contextual BERT embeddings which capture some sarcasm (e.g., "Oh great, another delay" is correctly classified as negative), but accuracy for heavily sarcastic text is lower (70-75%). Best practices: (1) Look for low confidence scores as sarcasm indicators, (2) Cross-reference with structured data (e.g., low star ratings with "positive" words), (3) Use aspect-based sentiment to identify contradictions, (4) For critical use cases, manually review suspected sarcasm (use keywords like "oh great", "wonderful", "perfect" combined with negative context).

### Q: What's the difference between sentiment score y confidence?

**A:** The `sentiment` Campo is the classification (positive/neutral/negative), while `score` is the continuous intensity (-1 to +1). The `confidence` measures model certainty (0-1). Example: score=-0.75 with confidence=0.92 means "strongly negative y we're very sure." Score=-0.15 with confidence=0.65 means "slightly negative but uncertain" (might be neutral). Use confidence to filter: >0.85 for automated actions, 0.70-0.85 for review, <0.70 for manual validation.

### Q: How often should I retrain or update sentiment models for my domain?

**A:** The pre-trained models work well for general sentiment analysis without retraining. However, for domain-specific terminology or evolving language (e.g., new product features, industry jargon), we recommend quarterly reviews. Contact support if your accuracy drops below 80% or if you notice systematic Errores (e.g., industry terms misclassified). For high-volume use cases (>100k texts/month), consider custom model fine-tuning with your labeled data to achieve 92-95% accuracy.

### Q: Can I use this for real-time customer support chatbots?

**A:** Sí! Processing latency is 50-100ms per text, suitable for real-time applications. Integration pattern: (1) Analyze each customer message as it arrives, (2) Use sentiment score to adjust chatbot tone (empathetic for negative, upbeat for positive), (3) Escalate to human agents when sentiment drops below -0.5 with confidence >0.80, (4) Track sentiment progression throughout conversation to measure resolution Éxito. For high-volume chatbots, use batch processing with 10-20 messages per request to reduce API calls.

### Q: What's the maximum text length per request?

**A:** Maximum 10,000 characters per text. Texts longer than 512 tokens (~400 words) are truncated y analyzed in chunks, with sentiment aggregated. For very long documents (>2,000 words), consider splitting into logical sections (paragraphs, topics) y analyzing separately with `include_aspects: true` to maintain granularity. Batch limit is 500 texts per request (maximum 5MB payload). For larger volumes, make multiple parallel requests.

## Guías Comerciales

| Use Case | Action | Expected Impact |
|----------|--------|-----------------|
| **Negative Review Respuesta** | If sentiment is negative (score <-0.5) with confidence >0.85, automatically create a high-priority support ticket, notify account manager, y send personalized Respuesta within 24 hours. Use aspect analysis to route to correct team (shipping, product quality, support). | Reduce customer churn by 25-35% through rapid Respuesta, improve satisfaction scores by 15-20%, increase positive review conversion from 10% to 25% after issue resolution. |
| **Product Feature Prioritization** | Analyze aspect-based sentiment across product reviews mensual. Prioritize development of features with high negative sentiment volume (>100 mentions, score <-0.4). Amplify marketing for features with high positive sentiment (score >0.7). | Increase product adoption by 20-30% through addressing pain points, improve feature satisfaction scores by 30-40%, reduce support tickets Relacionado to poor features by 25%. |
| **Social Media Monitoring** | Monitor bry mentions with hourly sentiment analysis. Create alerts when negative sentiment spikes (>20% increase in negative mentions hour-over-hour). Enable `include_entities: true` to identify which products/campaigns are mentioned. | Detect PR crises 6-12 hours earlier, enable rapid Respuesta to viral negative content, reduce bry damage by 40-50% through timely intervention. |
| **Customer Segmentation** | Segment customers by average sentiment score from feedback history. Target promoters (avg score >0.6) for referral campaigns, detractors (avg score <-0.3) for win-back offers, passives (score -0.3 to 0.6) for engagement campaigns. | Increase referral program participation by 35-50% through targeting happy customers, reduce churn by 15-20% via proactive outreach to detractors, improve NPS by 10-15 points. |
| **Support Ticket Prioritization** | Analyze incoming support ticket text for sentiment. Auto-escalate tickets with score <-0.7 y confidence >0.85 to senior agents. Route positive sentiment tickets to junior agents for skill development. Track sentiment before/after resolution. | Reduce escalation time from 48 hours to <2 hours, improve CSAT scores by 20-25% through better routing, decrease ticket resolution time by 15% via appropriate skill matching. |
| **Survey Analysis Automation** | Replace manual survey review with automated sentiment analysis. Process open-ended responses with aspect-based analysis to identify key themes. Generate executive summaries showing sentiment distribution by topic. | Reduce survey analysis time from 40 hours to <1 hour, increase survey frequency from quarterly to monthly, improve actionability by identifying specific themes rather than overall scores. |
| **Competitive Intelligence** | Analyze competitor product reviews y social mentions using sentiment y entity extraction. Compare sentiment scores for similar features across brys. Identify competitive weaknesses (their negative aspects) y strengths (your positive differentiators). | Improve competitive positioning by targeting messaging at competitor weaknesses, increase win rate by 15-25% through data-driven differentiation, guide product strategy with competitive gap analysis. |

## Notas

### Implementation Mejores Prácticas

**Text Preprocessing:**
- Clean HTML tags y special characters before submission
- Preserve punctuation (!, ?) as they carry sentiment signals
- Remove excessive whitespace but keep sentence structure
- Translate non-supported languages to English/Spanish/Portuguese for better accuracy

**Batch Optimization:**
- Optimal batch size: 50-100 texts per request (balances latency y throughput)
- Maximum batch size: 500 texts per request
- For >1,000 texts, use parallel requests (respect rate limits: 100 requests/minute)
- Processing time: ~50-100ms per text (batch of 100 = ~5-10 seconds total)

**Confidence Score Guidelines:**
- **>0.90:** Very high confidence - safe for automated actions
- **0.85-0.90:** High confidence - suitable for most automated workflows
- **0.70-0.85:** Moderate confidence - consider manual review for critical actions
- **<0.70:** Low confidence - requires human validation

**Aspect-Based Analysis:**
- Enable `include_aspects: true` when you need feature-specific sentiment
- Adds ~30-50ms latency per text
- Aspects automatically extracted (No predefined list needed)
- Common aspects: "product", "quality", "shipping", "support", "price", "value"

**Entity Extraction:**
- Enable `include_entities: true` to identify products, brys, people mentioned
- Useful for bry monitoring y competitive intelligence
- Entity types: PRODUCT, SERVICE, BRAND, PERSON, LOCATION, ORGANIZATION
- Adds ~20-30ms latency per text

### Rendimiento y Scalability

- **Maximum Payload:** 5MB per request (~500 texts at 10KB each)
- **Timeout:** 300 seconds (5 minutes) for very large batches
- **Rate Limiting:** 100 solicitudes por minuto por tenant
- **Rendimiento:** Up to 10,000 texts per minute (with parallel requests)
- **Latency:** 50-100ms per text (single), 5-10 seconds for batch of 100

### Security y Privacy

- All text data encrypted in transit (TLS 1.3) y en reposo
- Text content is not stored after analysis completes
- Customer IDs y metadata hashed for privacy
- GDPR compliant - data retention: 0 días (immediate deletion after Respuesta)
- PII detection: Avoid sending personal identifiable information in text

### Language Support

**Optimized Languages (88-92% accuracy):**
- English (en)
- Spanish (es)
- Portuguese (pt)

**Supported Languages via mBERT (82-88% accuracy):**
- 100+ languages including French, German, Italian, Dutch, Chinese, Japanese, Korean, Arabic, Russian
- Auto-detection available but explicit language Código recommended for best accuracy

**Language-Specific Considerations:**
- Idioms y cultural expressions may have lower accuracy
- Domain-specific terminology (technical, medical, legal) reduces accuracy by 3-5%
- Código-switching (mixed languages) should be split into separate texts

### Model Updates y Versioning

- Models updated quarterly with latest NLP research
- Backward compatible - API contract remains stable
- Model version included in Respuesta metadata (future feature)
- Accuracy improvements: +1-2% per year through continuous learning

## Relacionado

- [Sentiment Report](SentimentReport.md) - Aggregate sentiment data across multiple sources
- [NPS](../CustomerIntelligence/NPS.md) - Net Promoter Score analysis y predictions
- [Customer Loyalty](../CustomerIntelligence/CustomerLoyalty.md) - Measure y predict customer loyalty
- [NLP Analysis](../Inventory/NlpAnalysis.md) - Natural language processing for inventory insights

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:333`
