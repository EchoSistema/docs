# Artificial Intelligence – Excess Inventory Analysis (NLP)

## Endpoint

```
POST /api/v1/ai/echointel/inventory/excess-nlp
```

Excess Inventory Analysis (NLP) with natural language processing.

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


## Estado HTTP

| Status Código | Descripción |
|-------------|-------------|
| 200 OK | Request successful. Returns excess inventory NLP analysis results. |
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

The Excess Inventory NLP Analysis applies natural language processing techniques to identify y classify excess inventory items from unstructured text data (product descriptions, Notas, reports).

### 1. Text Preprocessing Pipeline

**Tokenization:**
- Text segmentation into words, phrases, y sentences using spaCy or NLTK
- Hyling of special characters, punctuation, y whitespace
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
- Time expressions: dates, durations, temporal Referencias
- Location entities: warehouse locations, storage zones

**Classification Model:**
- Pre-trained NER models (BERT-based or spaCy) fine-tuned on inventory domain
- Multi-label classification for inventory status: "excess", "slow-moving", "obsolete", "seasonal"
- Confidence scoring for each classification (0-1 range)

### 3. Sentiment y Context Analysis

**Sentiment Scoring:**
- Analyzes descriptive text for urgency indicators ("urgent", "critical", "attention needed")
- Identifies negative sentiment patterns associated with excess inventory
- Contextual embeddings using transformer models (BERT, RoBERTa)

**Relationship Extraction:**
- Links entities to extract meaningful relationships (e.g., "Product X has been in storage for Y meses")
- Temporal relation detection for aging analysis
- Causal relationship identification (why items are excess)

### 4. Classification y Scoring

**Excess Inventory Score Calculation:**
```
ExcessScore = w1 * KeywordMatch + w2 * TemporalAge + w3 * SentimentScore + w4 * EntityDensity
```
Where:
- **w1-w4** = Weighted coefficients (sum to 1.0)
- **KeywordMatch** = Presence of excess-Relacionado terms (normalized 0-1)
- **TemporalAge** = Time-based aging factor from extracted dates
- **SentimentScore** = Negative sentiment intensity (0-1)
- **EntityDensity** = Concentration of relevant entities in text

**Thresholds:**
- High Excess: Score > 0.7
- Medium Excess: 0.4 < Score ≤ 0.7
- Low Excess: Score ≤ 0.4

### 5. Output Generation

**Report Synthesis:**
- Aggregates classified entities into structured inventory registros
- Generates human-readable summaries using text generation
- Creates actionable recommendations based on classification results
- Highlights key phrases y context from source text

### 6. Rendimiento Characteristics

- **Tiempo de Procesamiento:** 300-800ms for typical document (500-2000 words)
- **Rendimiento:** 100-150 documents per minute (batch processing)
- **Accuracy Metrics:**
  - Precision: 0.87-0.92 (entity extraction)
  - Recall: 0.84-0.89 (entity extraction)
  - F1 Score: 0.86-0.90 (overall classification)
- **Language Support:** English, Spanish, Portuguese with dedicated models
- **Scalability:** Horizontal scaling via message queue (SQS, RabbitMQ)

## Relacionado

### Relacionado Endpoints

- **[Inventory History Improved](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryHistoryImproved.md)** - Time series analysis of inventory movements
- **[Inventory Optimization](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryOptimization.md)** - Mathematical optimization for inventory levels
- **[NLP Analysis](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpAnalysis.md)** - General NLP analysis for inventory text
- **[Excess Inventory Report](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpOpenaiExcessInventoryReport.md)** - AI-generated comprehensive reports

### Relacionado Domain Concepts

- **Natural Language Processing (NLP):** Text analysis, entity extraction, sentiment analysis
- **Inventory Management:** Stock keeping, turnover rates, obsolescence tracking
- **Text Classification:** Multi-label classification, confidence scoring
- **Ingeniería de Características:** TF-IDF, embeddings, linguistic features

### Integration Points

- **Warehouse Management Systems (WMS):** Import inventory descriptions y Notas
- **ERP Systems:** Export structured excess inventory data
- **Business Intelligence:** Feed classified data into dashboards y reports
- **Notification Services:** Trigger alerts for high-risk excess inventory

## Preguntas Frecuentes

### Q: What types of text data can be analyzed?
**A:** The NLP engine processes unstructured text from multiple sources: product descriptions, warehouse Notas, procurement reports, supplier communications, y inventory audit comments. Supported languages are English, Spanish, y Portuguese. Text length can range from 50 to 5,000 words per document.

### Q: How accurate is the excess inventory classification?
**A:** The system achieves 87-92% precision y 84-89% recall on entity extraction tasks. Classification F1 scores typically range from 0.86-0.90. Accuracy is highest for well-structured text with clear entity markers (SKU codes, quantities, dates). Ambiguous or poorly written text may require manual review.

### Q: Can I customize the excess inventory classification thresholds?
**A:** Sí. The Por Defecto excess score threshold is 0.7 for "High Excess". You can adjust thresholds via the `options` Parámetro: set custom values for high (>0.7), medium (0.4-0.7), y low (<0.4) categories. Lowering thresholds increases sensitivity but may reduce precision.

### Q: What happens if text contains multiple languages?
**A:** The system automatically detects the primary language per document. If multiple languages are present (e.g., English Descripción with Spanish Notas), the primary language model is applied. For optimal accuracy, separate multi-language documents into single-language segments before processing.

### Q: How does the system hyle industry-specific terminology?
**A:** The NER model is pre-trained on inventory domain data including retail, manufacturing, y logistics terminology. For highly specialized industries (e.g., pharmaceuticals, aerospace), custom model fine-tuning is available upon request. Contact support for domain-specific model training.

### Q: What's the processing time for large document batches?
**A:** Processing time is 300-800ms per document (500-2000 words). Batch processing throughput is 100-150 documents per minute. For large batches (>1,000 documents), use the async Endpoint (planned Q4 2025) or split into multiple requests with 500-document batches for optimal performance.

## Guías Comerciales

### Playbook 1: Automated Excess Inventory Identification
**Objetivo:** Reduce excess inventory carrying costs by 30% through automated text analysis.

**Implementación:**
1. Export all product descriptions y warehouse Notas from WMS
2. Submit documents to NLP Endpoint in batches of 500
3. Filter results for ExcessScore > 0.7 (High Excess classification)
4. Cross-reference with current stock levels y turnover rates
5. Generate prioritized action list: markdown, liquidation, or return to supplier

**Resultados Esperados:**
- 30-40% reduction in excess inventory carrying costs
- 25-30% improvement in inventory turnover ratio
- $50,000-$200,000 in recovered capital (typical for mid-size retailer)
- 60-80% reduction in manual review time

### Playbook 2: Early Warning System for Slow-Moving Inventory
**Objetivo:** Identify slow-moving items 60-90 días earlier using NLP sentiment analysis.

**Implementación:**
1. Analyze product descriptions y buyer comments for negative sentiment patterns
2. Identify keywords indicating slow movement: "excess", "overstock", "not selling", "aging"
3. Set up weekly automated scans of new inventory Notas
4. Create alerts for items with increasing negative sentiment trends
5. Proactively implement pricing adjustments or promotional campaigns

**Resultados Esperados:**
- 60-90 day earlier detection of slow-moving items
- 20-30% reduction in write-offs y markdowns
- Improved cash flow through faster inventory turnover
- Better supplier negotiation leverage with early visibility

### Playbook 3: Root Cause Analysis for Excess Inventory
**Objetivo:** Identify systemic causes of excess inventory using text mining.

**Implementación:**
1. Collect 12 meses of excess inventory Notas y communications
2. Run NLP analysis to extract causal relationships y patterns
3. Identify top 5 root causes (e.g., "forecasting Errores", "supplier overshipment", "SKU proliferation")
4. Quantify impact of each root cause on excess inventory value
5. Implement process improvements targeting top causes

**Resultados Esperados:**
- Clear visibility into top 3-5 drivers of excess inventory
- 15-25% reduction in future excess inventory generation
- Data-driven process improvements
- Better vendor scorecarding y accountability

### Playbook 4: Intelligent Document Search for Excess Inventory
**Objetivo:** Reduce time spent searching for excess inventory information by 70%.

**Implementación:**
1. Index all inventory-Relacionado documents using NLP entity extraction
2. Build searchable database of products, quantities, dates, y locations
3. Enable semantic search: find "electronics overstocked in warehouse B" without exact keyword matching
4. Provide instant reports on excess inventory by category, location, or time period
5. Integrate search functionality into inventory management dashboard

**Resultados Esperados:**
- 70-80% reduction in time spent searching for excess inventory data
- Faster decision-making y Respuesta times
- Improved collaboration across teams with centralized information
- Better audit readiness y compliance

### Playbook 5: Multi-Language Global Inventory Optimization
**Objetivo:** Styardize excess inventory classification across global operations.

**Implementación:**
1. Deploy NLP analysis across warehouses in different countries (EN, ES, PT)
2. Styardize excess inventory scoring methodology globally
3. Create unified dashboard showing excess inventory across all regions
4. Implement cross-regional rebalancing: transfer excess from Region A to meet demy in Region B
5. Monitor y optimize global inventory allocation

**Resultados Esperados:**
- Unified global view of excess inventory
- 10-20% reduction in global excess inventory through rebalancing
- Styardized processes across regions
- Improved global supply chain efficiency

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:271`
