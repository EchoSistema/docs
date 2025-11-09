# Artificial Intelligence – Real-Time Sentiment Analysis

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-realtime
```

Performs real-time sentiment analysis of texts (reviews, comments, feedbacks) using natural language processing (NLP).

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

### Body Parameters

| Parameter       | Type   | Required | Description |
| --------------- | ------ | ----------- | --------- |
| texts           | array  | Yes         | Texts for sentiment analysis. |
| language        | string | No         | Text language (`auto` for automatic detection). |
| include_entities| boolean| No         | Extract mentioned entities. Default: `false`. |
| include_aspects | boolean| No         | Sentiment analysis by aspect. Default: `false`. |

## Examples

### Request Example (curl)

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

## Response

### Success `200 OK`

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

## JSON Structure

| Field                                    | Type    | Description |
| ---------------------------------------- | ------- | --------- |
| `sentiment_analysis`                     | array   | Analysis per text. |
| `sentiment_analysis[].id`                | string  | Text ID. |
| `sentiment_analysis[].text`              | string  | Analyzed text. |
| `sentiment_analysis[].sentiment`         | string  | Sentiment (`positive`, `neutral`, `negative`). |
| `sentiment_analysis[].confidence`        | float   | Classification confidence (0-1). |
| `sentiment_analysis[].score`             | float   | Sentiment score (-1 to +1). |
| `sentiment_analysis[].aspects`           | array   | Analysis by aspect (if requested). |
| `sentiment_analysis[].entities`          | array   | Extracted entities (if requested). |
| `sentiment_analysis[].keywords`          | array   | Identified keywords. |
| `summary`                                | object  | General summary. |
| `summary.total_analyzed`                 | int     | Total texts analyzed. |
| `summary.positive`                       | int     | Number of positive sentiments. |
| `summary.neutral`                        | int     | Number of neutral sentiments. |
| `summary.negative`                       | int     | Number of negative sentiments. |
| `summary.avg_score`                      | float   | Average score. |

## Sentiment Classification

| Score       | Sentiment | Description |
| ----------- | ---------- | --------- |
| 0.5 to 1.0   | Positive   | Clearly positive sentiment. |
| 0.1 to 0.49  | Positive   | Slightly positive sentiment. |
| -0.1 to 0.1  | Neutral    | Neutral or mixed sentiment. |
| -0.49 to -0.1| Negative   | Slightly negative sentiment. |
| -1.0 to -0.5 | Negative   | Clearly negative sentiment. |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns sentiment analysis results. |
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

The sentiment analysis system uses state-of-the-art NLP models to classify text sentiment and extract insights:

### 1. NLP Model Architecture

The system employs transformer-based models fine-tuned for sentiment classification:

- **BERT-based Models:** Uses multilingual BERT (mBERT) or language-specific BERT variants for context-aware sentiment detection
- **Aspect-Based Sentiment:** Applies targeted sentiment extraction for specific aspects/features mentioned in text
- **Entity Recognition:** Uses Named Entity Recognition (NER) to identify products, services, and brands

### 2. Sentiment Scoring Process

- **Step 1:** Text preprocessing (tokenization, normalization, lowercasing)
- **Step 2:** Contextual embedding generation using BERT encoder (768-dimensional vectors)
- **Step 3:** Classification head predicts sentiment probabilities [positive, neutral, negative]
- **Step 4:** Convert probabilities to continuous score: score = P(positive) - P(negative) ∈ [-1, +1]

### 3. Confidence and Calibration

- **Confidence Score:** Maximum class probability after temperature scaling calibration
- **Threshold-Based Classification:** Neutral zone (-0.1 to +0.1) reduces false positive/negative classifications
- **Ensemble Aggregation:** Combines predictions from multiple models for improved accuracy (optional)

### 4. Performance and Optimization

- **Processing Time:** 50-100ms per text (batch processing: 20 texts/second)
- **Accuracy:** 88-92% on benchmark datasets (SST-2, IMDB, custom domain data)
- **Supported Languages:** 100+ languages via mBERT, optimized for EN, ES, PT
- **GPU Acceleration:** Reduces latency by 5-10x for batch requests (>50 texts)

## Notes

* Supports multiple languages (auto-detection available).
* Aspect analysis identifies sentiments about different characteristics.
* Useful for analyzing reviews, customer feedback, social media, etc.

## Related

- [Sentiment Report](SentimentReport.md) - Aggregate sentiment data across multiple sources
- [NPS](../CustomerIntelligence/NPS.md) - Net Promoter Score analysis and predictions
- [Customer Loyalty](../CustomerIntelligence/CustomerLoyalty.md) - Measure and predict customer loyalty
- [NLP Analysis](../Inventory/NlpAnalysis.md) - Natural language processing for inventory insights

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:333`
