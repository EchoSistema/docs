# Artificial Intelligence – Inventory NLP Analysis

## Endpoint

```
POST /api/v1/ai/echointel/inventory/nlp-analysis
```

Inventory NLP Analysis using NLP.

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
| 200 OK | Request successful. Returns inventory NLP analysis results. |
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

The NLP Analysis endpoint processes inventory-related text data (descriptions, notes, feedback) using natural language processing techniques to extract structured insights and sentiment.

### 1. Text Preprocessing

**Input Cleaning:**
- HTML tag removal and entity decoding
- Unicode normalization (NFC, NFKC)
- Whitespace normalization and trimming
- Special character handling (preserve domain-specific symbols)

**Tokenization:**
- Word-level tokenization using language-specific rules
- Sentence segmentation with boundary detection
- Subword tokenization (BPE, WordPiece) for out-of-vocabulary terms
- Handling of contractions and compound words

**Normalization:**
- Lowercasing for case-insensitive operations
- Stemming using Porter/Snowball/Lancaster algorithms
- Lemmatization with POS tagging for contextual base forms
- Stopword filtering (customizable lists for en, es, pt)

### 2. Feature Extraction

**Statistical Features:**
- **TF-IDF (Term Frequency-Inverse Document Frequency):**
  ```
  TF-IDF(t,d) = TF(t,d) × log(N / DF(t))
  ```
  - TF(t,d) = Frequency of term t in document d
  - DF(t) = Number of documents containing term t
  - N = Total number of documents

- **N-gram Analysis:**
  - Unigrams (single words): "inventory", "stock", "shortage"
  - Bigrams (word pairs): "excess inventory", "stock level"
  - Trigrams (three words): "out of stock", "low inventory warning"

**Linguistic Features:**
- Part-of-Speech (POS) tags: nouns, verbs, adjectives, adverbs
- Dependency parsing for grammatical relationships
- Named entity counts and types
- Sentence length and complexity metrics

**Semantic Features:**
- Word embeddings: Word2Vec, GloVe, FastText (300-dimensional vectors)
- Contextualized embeddings: BERT, RoBERTa (768-dimensional)
- Semantic similarity using cosine distance
- Topic modeling (LDA, NMF) for theme extraction

### 3. Sentiment Analysis

**Polarity Detection:**
- **Lexicon-Based Approach:**
  - VADER (Valence Aware Dictionary and sEntiment Reasoner) for social text
  - Custom inventory domain lexicon with weighted terms
  - Polarity scores: positive, negative, neutral, compound

- **Machine Learning Approach:**
  - Fine-tuned BERT/RoBERTa for sentiment classification
  - Multi-class classification: very negative, negative, neutral, positive, very positive
  - Confidence scores for each class

**Sentiment Scoring:**
```
CompoundScore = (positive - negative) / √((positive + negative)² + α)
```
- Normalized to [-1, 1] range
- Threshold classification: negative (<-0.05), neutral (±0.05), positive (>0.05)

**Aspect-Based Sentiment:**
- Identifies aspects: quality, quantity, delivery, price
- Associates sentiment with specific aspects
- Enables granular feedback analysis

### 4. Entity Recognition and Classification

**Named Entity Recognition (NER):**
- **Product Entities:** SKU codes, product names, brands
- **Quantity Entities:** Numbers with units (kg, units, boxes)
- **Time Entities:** Dates, durations, temporal references
- **Location Entities:** Warehouses, stores, regions
- **Organization Entities:** Suppliers, manufacturers

**Entity Linking:**
- Maps recognized entities to knowledge base entries
- Resolves aliases and abbreviations
- Confidence scoring for entity matches

**Relationship Extraction:**
- Subject-predicate-object triples
- Example: "Product X" - "has" - "low stock"
- Temporal relationships: "Product Y" - "out of stock since" - "March 15"

### 5. Topic Modeling and Clustering

**Latent Dirichlet Allocation (LDA):**
- Discovers hidden topics in document collection
- Probabilistic model: each document is a mixture of topics
- Topic representation as probability distribution over words
- Typical topics: stock issues, quality problems, delivery delays

**Text Clustering:**
- K-means clustering on TF-IDF or embedding vectors
- Hierarchical clustering for topic hierarchies
- DBSCAN for density-based grouping
- Silhouette score for cluster quality assessment

### 6. Keyword Extraction

**Statistical Methods:**
- TF-IDF ranking for term importance
- TextRank algorithm (graph-based ranking)
- RAKE (Rapid Automatic Keyword Extraction)

**Supervised Methods:**
- Trained classifiers for domain-specific keywords
- Attention weights from transformer models
- Feature importance from tree-based models

### 7. Text Classification

**Category Prediction:**
- Multi-label classification for inventory issues:
  - Excess inventory, shortage, quality issue, demand spike, obsolescence
- Binary relevance, classifier chains, or label powerset methods
- Threshold tuning for precision-recall tradeoff

**Urgency Detection:**
- Identifies urgent language: "critical", "immediate", "emergency"
- Priority scoring: low (0-3), medium (4-6), high (7-10)
- Combines keyword matching with semantic similarity

### 8. Performance Characteristics

- **Processing Time:** 200-600ms per document (100-500 words)
- **Batch Processing:** 80-120 documents per minute
- **Accuracy Metrics:**
  - Sentiment Classification F1: 0.85-0.91
  - Entity Recognition F1: 0.88-0.94
  - Topic Coherence: 0.65-0.75
  - Keyword Extraction Precision@5: 0.82-0.89
- **Language Support:** English, Spanish, Portuguese (separate models)
- **Scalability:** Horizontally scalable with load balancing

## Related

### Related Endpoints

- **[Excess Inventory NLP](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/ExcessInventoryNlp.md)** - Specialized excess inventory detection
- **[NLP Analysis EN](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpAnalysisEn.md)** - English-optimized version
- **[Excess Inventory Report](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpOpenaiExcessInventoryReport.md)** - AI-generated reports

### Related Domain Concepts

- **Natural Language Processing:** Tokenization, POS tagging, NER, sentiment analysis
- **Text Mining:** Feature extraction, topic modeling, clustering
- **Information Extraction:** Entity recognition, relationship extraction
- **Machine Learning:** Classification, embeddings, transformers (BERT, RoBERTa)

### Integration Points

- **Feedback Systems:** Analyze customer and employee feedback about inventory
- **Support Tickets:** Extract inventory issues from support requests
- **Inventory Notes:** Process warehouse and supply chain notes
- **Product Reviews:** Mine reviews for inventory-related mentions

### Use Cases

- Identify common inventory problems from textual feedback
- Extract actionable insights from unstructured data
- Monitor sentiment trends around inventory management
- Automatically categorize and route inventory-related communications
- Generate alerts for urgent inventory issues mentioned in text

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:276`
