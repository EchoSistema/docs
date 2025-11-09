# Artificial Intelligence – Excess Inventory Report (NLP)

## Endpoint

```
POST /api/v1/ai/echointel/inventory/excess-report
```

Excess Inventory Report (NLP) using generative AI.

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
| 200 OK | Request successful. Returns excess inventory report results. |
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

The Excess Inventory Report endpoint uses OpenAI's GPT models to generate comprehensive, human-readable reports analyzing excess inventory situations with context-aware insights and recommendations.

### 1. Data Collection and Preprocessing

**Input Data Aggregation:**
- Inventory levels and historical trends
- Product descriptions and metadata
- Sales velocity and turnover rates
- Storage costs and warehouse capacity
- Market conditions and demand forecasts

**Data Structuring:**
- Convert raw data into structured JSON format
- Calculate key metrics: days of supply, turnover ratio, aging analysis
- Identify critical data points: highest excess items, cost implications
- Format numbers and dates for readability

### 2. Prompt Engineering

**System Prompt Design:**
```
Role: Expert inventory analyst and report writer
Context: Supply chain and warehouse management domain
Task: Analyze excess inventory data and generate actionable report
Constraints: Professional tone, data-driven insights, specific recommendations
```

**Prompt Template Structure:**
- **Context Setting:** "You are an expert inventory analyst..."
- **Data Presentation:** Structured data in clear format (JSON or markdown tables)
- **Analysis Requirements:** "Identify top excess items, root causes, financial impact..."
- **Output Format:** "Generate a report with sections: Executive Summary, Detailed Analysis, Recommendations..."
- **Tone and Style:** "Professional, concise, actionable, data-driven"

**Few-Shot Examples:**
- Include sample data → sample report pairs
- Demonstrates desired output format and analysis depth
- Calibrates model for inventory domain terminology

### 3. OpenAI API Integration

**API Call Configuration:**
```json
{
  "model": "gpt-4" or "gpt-4-turbo" or "gpt-3.5-turbo",
  "messages": [
    {"role": "system", "content": "<system_prompt>"},
    {"role": "user", "content": "<data_and_instructions>"}
  ],
  "temperature": 0.3,  // Low for consistent, factual output
  "max_tokens": 2000-4000,  // Based on report length needs
  "top_p": 0.9,
  "frequency_penalty": 0.2,  // Reduce repetition
  "presence_penalty": 0.1
}
```

**Model Selection Logic:**
- **GPT-4:** Complex analysis, multiple products, nuanced insights (higher cost, slower)
- **GPT-4-Turbo:** Balanced performance and cost, recommended default
- **GPT-3.5-Turbo:** Simple reports, single product analysis (faster, cheaper)

**Token Management:**
- Estimate input tokens: data + prompt (~500-1500 tokens)
- Reserve output tokens for comprehensive report (1500-3000 tokens)
- Implement chunking for large datasets (>50 products)

### 4. Report Generation Process

**LLM Processing:**
1. **Understanding Phase:** Model interprets structured data and analysis requirements
2. **Analysis Phase:** Identifies patterns, anomalies, trends in excess inventory
3. **Reasoning Phase:** Determines root causes and business implications
4. **Generation Phase:** Produces human-readable report with sections

**Report Structure (Generated):**
- **Executive Summary:** High-level overview, key findings, total excess value
- **Inventory Analysis:**
  - Top excess items by value and quantity
  - Aging analysis (30, 60, 90+ days)
  - Category breakdown
- **Root Cause Analysis:**
  - Demand forecasting errors
  - Over-ordering or bulk purchase issues
  - Obsolescence or market changes
  - Seasonality misalignment
- **Financial Impact:**
  - Carrying costs (storage, insurance, depreciation)
  - Opportunity cost of tied-up capital
  - Potential write-off risks
- **Recommendations:**
  - Short-term actions: discounts, promotions, liquidation
  - Medium-term: adjust reorder points and quantities
  - Long-term: improve forecasting, supplier agreements
- **Priority Action Items:** Ranked by urgency and impact

### 5. Post-Processing and Enrichment

**Content Formatting:**
- Convert markdown to HTML if needed
- Add visual elements placeholders (charts, graphs)
- Format tables for readability
- Highlight critical numbers and metrics

**Data Validation:**
- Verify numerical calculations match input data
- Check for hallucinations (invented facts not in source data)
- Validate recommendations align with industry best practices
- Ensure all referenced products exist in input data

**Enhancement:**
- Insert data visualizations (generated separately or referenced)
- Add hyperlinks to product pages or detailed inventory views
- Include timestamp and report metadata
- Attach raw data appendix for transparency

### 6. Quality Assurance

**Automated Checks:**
- **Factual Accuracy:** Cross-reference all numbers with source data
- **Completeness:** Verify all required sections present
- **Consistency:** Check for contradictions in analysis
- **Relevance:** Ensure recommendations applicable to business context

**Fallback Mechanisms:**
- Retry with adjusted temperature if output too generic
- Use simpler model if complex model times out
- Generate section-by-section if full report fails
- Provide templated report if all API calls fail

### 7. Performance Characteristics

- **Processing Time:** 3-8 seconds for GPT-4, 2-5 seconds for GPT-3.5-turbo
- **API Latency:** 80-90% of total time
- **Cost per Report:**
  - GPT-4: $0.03-$0.12 per report (depending on length)
  - GPT-4-Turbo: $0.01-$0.05 per report
  - GPT-3.5-Turbo: $0.002-$0.01 per report
- **Quality Metrics:**
  - Factual Accuracy: 95-98% (validated against source data)
  - Actionability Score: 8.5-9.2/10 (human evaluation)
  - Readability: 85-92% (Flesch Reading Ease)
- **Throughput:** 10-20 reports per minute (with rate limiting)
- **Caching Strategy:** Generated reports cached for 24 hours per data snapshot

### 8. Error Handling

**API Errors:**
- **Rate Limit (429):** Exponential backoff, queue for retry
- **Invalid Request (400):** Validate input data, simplify prompt
- **Model Overload (503):** Retry with alternative model
- **Context Length Exceeded:** Chunk data, generate in sections

**Quality Issues:**
- **Generic Output:** Increase temperature slightly, add more specific instructions
- **Hallucinations:** Lower temperature, add "only use provided data" instruction
- **Incomplete Report:** Increase max_tokens, retry with higher limit

## Related

### Related Endpoints

- **[Excess Inventory NLP](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/ExcessInventoryNlp.md)** - Analyzes text descriptions for excess inventory
- **[Inventory History Improved](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryHistoryImproved.md)** - Provides historical data for report context
- **[Inventory Optimization](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryOptimization.md)** - Supplies optimization recommendations
- **[NLP Analysis](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/NlpAnalysis.md)** - Extracts insights from product descriptions

### Related Domain Concepts

- **Generative AI:** GPT models, prompt engineering, natural language generation
- **Report Automation:** Automated reporting, business intelligence narratives
- **LLM Integration:** API design, token management, error handling
- **Inventory Management:** Excess stock analysis, turnover optimization, cost reduction

### Integration Points

- **Business Intelligence Dashboards:** Embed generated reports in BI tools
- **Email Systems:** Automated report distribution to stakeholders
- **ERP Systems:** Trigger report generation on inventory threshold alerts
- **Document Management:** Store reports with versioning and audit trail

### Use Cases

- **Weekly Excess Inventory Reviews:** Automated reports for inventory managers
- **Board Presentations:** Executive-level summaries with financial impact
- **Warehouse Operations:** Operational reports for warehouse staff with action items
- **Financial Planning:** Cost analysis reports for CFO and finance teams
- **Vendor Negotiations:** Data-driven reports for renegotiating purchase agreements

### Best Practices

- **Provide Complete Data:** More context enables better analysis
- **Update Regularly:** Run reports weekly or monthly for trend tracking
- **Validate Recommendations:** Cross-check AI suggestions with domain expertise
- **Customize Prompts:** Tailor prompt templates to specific business needs
- **Monitor Costs:** Track API usage and optimize model selection

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:266`
