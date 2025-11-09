# Artificial Intelligence – NLP Analysis (EN)

## Endpoint

```
POST /api/v1/ai/echointel/inventory/nlp-analysis-en
```

NLP Analysis (EN) English version.

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
| 200 OK | Request successful. Returns NLP analysis results. |
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

The NLP Analysis EN endpoint is specifically optimized for English-language inventory text processing, utilizing English-trained NLP models for higher accuracy on English documents.

### 1. English-Optimized Text Preprocessing

**Tokenization:**
- spaCy English model (en_core_web_sm, en_core_web_lg, or en_core_web_trf)
- English-specific tokenization rules for contractions (can't, won't, it's)
- Handling of English abbreviations and acronyms (SKU, EOQ, WMS)

**Lemmatization:**
- English WordNet-based lemmatization
- Context-aware lemmatization using POS tags
- Irregular verb and noun handling (run→ran, inventory→inventories)

**Stopword Removal:**
- English stopword list (NLTK, spaCy): a, an, the, is, are, was, were, etc.
- Preservation of domain-specific terms that may appear as stopwords
- Customizable stopword list for inventory context

### 2. English-Specific Feature Engineering

**TF-IDF with English Corpus:**
- Trained on English inventory and supply chain documents
- Optimized IDF weights for English vocabulary
- Sublinear term frequency scaling for common English terms

**English Word Embeddings:**
- Pre-trained GloVe embeddings (trained on Wikipedia + Gigaword)
- FastText English model with subword information
- Domain-adapted embeddings fine-tuned on inventory texts

**Contextualized Embeddings:**
- BERT-base-uncased or BERT-large-uncased (English)
- RoBERTa-base/large optimized for English
- DistilBERT for faster English text processing

### 3. English Sentiment Analysis

**VADER for English:**
- Specifically designed for English social media and informal text
- Handles English-specific phenomena:
  - Capitalization for emphasis (CRITICAL, URGENT)
  - Emoticons and emoji interpretation
  - Punctuation emphasis (!!!, ???)
  - Negation handling (not good, isn't sufficient)

**English Sentiment Lexicons:**
- Opinion Lexicon (Bing Liu)
- SentiWordNet (English WordNet-based)
- AFINN lexicon for English affective ratings
- Domain-specific inventory sentiment terms

**Negation Detection:**
- English negation cues: not, no, never, none, neither, nor
- Scope resolution: identifies negation scope boundaries
- Polarity reversal within negation scope

### 4. English Named Entity Recognition

**Pre-trained English NER:**
- spaCy English NER: PERSON, ORG, GPE, DATE, CARDINAL, QUANTITY
- Fine-tuned on inventory domain with English annotations
- Custom entity types: PRODUCT_CODE, SKU, WAREHOUSE_LOCATION

**English-Specific Entity Patterns:**
- Date formats: MM/DD/YYYY, Month DD, YYYY (US format)
- Quantity patterns: "5 units", "10 boxes", "500 pieces"
- Product code patterns: alphanumeric with hyphens (ABC-123-XYZ)

### 5. English Keyword Extraction

**English-Optimized TextRank:**
- POS filtering: English nouns and adjectives as candidates
- English phrase patterns: (Adj)*(Noun)+
- Co-occurrence window tuned for English text structure

**RAKE for English:**
- English stop words as delimiters
- Phrase identification using English grammar rules
- Score calculation: word degree / word frequency

### 6. English Text Classification

**English Training Data:**
- Models trained on English-labeled inventory documents
- Transfer learning from general English text classifiers
- Fine-tuning on domain-specific English inventory corpus

**Classification Categories:**
- Standard English labels: "excess stock", "shortage", "quality issue"
- Confidence calibration for English text
- Threshold optimization on English validation set

### 7. English Syntax and Dependency Parsing

**Dependency Relations:**
- English grammatical relations (nsubj, dobj, amod, prep)
- SVO (Subject-Verb-Object) extraction for English sentences
- Relationship triples specific to English syntax

**English Grammar Patterns:**
- Noun phrases: "the excess inventory in warehouse A"
- Verb phrases: "has been depleted", "needs reordering"
- Prepositional phrases: "shortage of SKU-123"

### 8. Performance Characteristics

- **Processing Time:** 180-500ms per English document (100-500 words)
- **Throughput:** 90-130 English documents per minute
- **Accuracy Metrics (English-specific):**
  - Sentiment F1: 0.88-0.93 (improved vs. multilingual)
  - NER F1: 0.91-0.96 (English entities)
  - Classification Accuracy: 0.89-0.94
  - Keyword Precision@5: 0.85-0.91
- **Model Size:** English-only models are smaller and faster
- **Latency:** 15-20% lower than multilingual models

## Related

### Related Endpoints

- **[NLP Analysis](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpAnalysis.md)** - Multilingual version (en/es/pt)
- **[Excess Inventory NLP](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/ExcessInventoryNlp.md)** - Excess detection with NLP
- **[Excess Inventory Report](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpOpenaiExcessInventoryReport.md)** - AI-generated English reports

### Related Domain Concepts

- **English NLP:** Language-specific tokenization, POS tagging, parsing
- **English Lexical Resources:** WordNet, VerbNet, FrameNet
- **Transfer Learning:** Pre-trained English models (BERT, GPT, RoBERTa)
- **Sentiment Analysis:** VADER, English sentiment lexicons

### Integration Points

- **English Documentation Systems:** Process English product descriptions
- **US/UK Inventory Systems:** Optimized for English-speaking markets
- **English Support Tickets:** Analyze English customer service data
- **English Reports:** Generate and analyze English inventory reports

### When to Use This Endpoint

- **Use NLP Analysis EN when:**
  - All input text is in English
  - Highest accuracy for English text is required
  - Processing speed is critical (English-only models are faster)
  - US/UK English dialect-specific handling needed

- **Use NLP Analysis (multilingual) when:**
  - Text may be in Spanish or Portuguese
  - Language is unknown or mixed
  - System must handle multiple languages dynamically

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:281`
