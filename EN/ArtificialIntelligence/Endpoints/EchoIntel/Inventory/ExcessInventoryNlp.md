# Artificial Intelligence – Excess Inventory Analysis (NLP)

## Endpoint

```
POST /api/v1/ai/echointel/inventory/excess-nlp
```

Excess Inventory Analysis (NLP) with natural language processing.

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
| 200 OK | Request successful. Returns excess inventory NLP analysis results. |
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

The Excess Inventory NLP Analysis applies natural language processing techniques to identify and classify excess inventory items from unstructured text data (product descriptions, notes, reports).

### 1. Text Preprocessing Pipeline

**Tokenization:**
- Text segmentation into words, phrases, and sentences using spaCy or NLTK
- Handling of special characters, punctuation, and whitespace
- Sentence boundary detection for context preservation

**Normalization:**
- Lowercasing for case-insensitive matching
- Stemming (Porter/Snowball stemmer) to reduce words to root forms
- Lemmatization for grammatically correct base forms
- Stopword removal (language-specific: en, es, pt)

**Feature Extraction:**
- Term Frequency-Inverse Document Frequency (TF-IDF) vectorization
- N-gram extraction (unigrams, bigrams, trigrams) for phrase detection
- Part-of-speech tagging for identifying key entities

### 2. Named Entity Recognition (NER)

**Entity Extraction:**
- Product identification: SKU codes, product names, categories
- Quantity detection: numerical values with units (pieces, boxes, pallets)
- Time expressions: dates, durations, temporal references
- Location entities: warehouse locations, storage zones

**Classification Model:**
- Pre-trained NER models (BERT-based or spaCy) fine-tuned on inventory domain
- Multi-label classification for inventory status: "excess", "slow-moving", "obsolete", "seasonal"
- Confidence scoring for each classification (0-1 range)

### 3. Sentiment and Context Analysis

**Sentiment Scoring:**
- Analyzes descriptive text for urgency indicators ("urgent", "critical", "attention needed")
- Identifies negative sentiment patterns associated with excess inventory
- Contextual embeddings using transformer models (BERT, RoBERTa)

**Relationship Extraction:**
- Links entities to extract meaningful relationships (e.g., "Product X has been in storage for Y months")
- Temporal relation detection for aging analysis
- Causal relationship identification (why items are excess)

### 4. Classification and Scoring

**Excess Inventory Score Calculation:**
```
ExcessScore = w1 * KeywordMatch + w2 * TemporalAge + w3 * SentimentScore + w4 * EntityDensity
```
Where:
- **w1-w4** = Weighted coefficients (sum to 1.0)
- **KeywordMatch** = Presence of excess-related terms (normalized 0-1)
- **TemporalAge** = Time-based aging factor from extracted dates
- **SentimentScore** = Negative sentiment intensity (0-1)
- **EntityDensity** = Concentration of relevant entities in text

**Thresholds:**
- High Excess: Score > 0.7
- Medium Excess: 0.4 < Score ≤ 0.7
- Low Excess: Score ≤ 0.4

### 5. Output Generation

**Report Synthesis:**
- Aggregates classified entities into structured inventory records
- Generates human-readable summaries using text generation
- Creates actionable recommendations based on classification results
- Highlights key phrases and context from source text

### 6. Performance Characteristics

- **Processing Time:** 300-800ms for typical document (500-2000 words)
- **Throughput:** 100-150 documents per minute (batch processing)
- **Accuracy Metrics:**
  - Precision: 0.87-0.92 (entity extraction)
  - Recall: 0.84-0.89 (entity extraction)
  - F1 Score: 0.86-0.90 (overall classification)
- **Language Support:** English, Spanish, Portuguese with dedicated models
- **Scalability:** Horizontal scaling via message queue (SQS, RabbitMQ)

## Related

### Related Endpoints

- **[Inventory History Improved](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryHistoryImproved.md)** - Time series analysis of inventory movements
- **[Inventory Optimization](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryOptimization.md)** - Mathematical optimization for inventory levels
- **[NLP Analysis](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpAnalysis.md)** - General NLP analysis for inventory text
- **[Excess Inventory Report](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpOpenaiExcessInventoryReport.md)** - AI-generated comprehensive reports

### Related Domain Concepts

- **Natural Language Processing (NLP):** Text analysis, entity extraction, sentiment analysis
- **Inventory Management:** Stock keeping, turnover rates, obsolescence tracking
- **Text Classification:** Multi-label classification, confidence scoring
- **Feature Engineering:** TF-IDF, embeddings, linguistic features

### Integration Points

- **Warehouse Management Systems (WMS):** Import inventory descriptions and notes
- **ERP Systems:** Export structured excess inventory data
- **Business Intelligence:** Feed classified data into dashboards and reports
- **Notification Services:** Trigger alerts for high-risk excess inventory

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:271`
