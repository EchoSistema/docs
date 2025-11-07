# Artificial Intelligence API - EchoIntel

This document provides a complete overview of all artificial intelligence endpoints available through integration with the EchoIntel platform.

## Table of Contents

- [Overview](#overview)
- [Authentication](#authentication)
- [Endpoint Categories](#endpoint-categories)
  - [Customer Intelligence](#customer-intelligence)
  - [Propensity](#propensity)
  - [Recommendations](#recommendations)
  - [Forecast](#forecast)
  - [Inventory](#inventory)
  - [Risk](#risk)
  - [Analytics](#analytics)
- [References](#references)

---

## Overview

The Artificial Intelligence API provides **41 endpoints** organized into **7 main categories**, offering machine learning solutions, predictive analytics, optimization, and natural language processing for various business use cases.

### Base URL

```
https://your-domain.com/api/v1/ai/echointel
```

---

## Authentication

All endpoints require authentication via **Bearer token** (Sanctum) and may require additional headers:

| Header             | Required    | Description |
| ------------------ | ----------- | ----------- |
| Authorization      | Yes         | `Bearer {token}` |
| X-Customer-Api-Id  | Conditional | Tenant UUID (v4). Required if not configured on server. |
| X-Secret           | Conditional | 64-character secret. Required if not configured on server. Must be rotated every 90 days. |
| Accept-Language    | No          | Response language (`en`, `es`, `pt`). Default: `en`. |
| Content-Type       | Yes         | `application/json` |

---

## Endpoint Categories

### Customer Intelligence

Behavioral analysis and customer segmentation using machine learning.

#### 1. Purchasing Segmentation
```
POST /customer-intelligence/purchasing-segmentation
```
**What it does:** Segments customers based on purchasing patterns, identifying groups with similar behaviors for targeted campaigns.

#### 2. Segmentation Report
```
POST /customer-intelligence/segmentation-report
```
**What it does:** Generates detailed segmentation reports with visualizations and actionable insights.

#### 3. Segment Hierarchy Chart
```
POST /customer-intelligence/segment-hierarchy-chart
```
**What it does:** Creates visual hierarchy of segments showing relationships and sub-segments.

#### 4. Segment Subsegment Explore
```
POST /customer-intelligence/segment-subsegment-explore
```
**What it does:** Explores details of specific subsegments within larger segments.

#### 5. Segment Cluster Profiles
```
POST /customer-intelligence/segment-cluster-profiles
```
**What it does:** Generates detailed profiles of each identified cluster/segment, including demographic and behavioral characteristics.

#### 6. Customer Segmentation
```
POST /customer-intelligence/segmentation
```
**What it does:** [📄 See full documentation](./Endpoints/CustomerIntelligence/CustomerSegmentation.md)

Performs automatic customer segmentation analysis using clustering algorithms (K-means, Hierarchical, DBSCAN).

#### 7. Customer Features
```
POST /customer-intelligence/features
```
**What it does:** Extracts and calculates important customer features for use in predictive models.

#### 8. Customer Loyalty
```
POST /customer-intelligence/loyalty
```
**What it does:** Analyzes and scores customer loyalty/fidelity level based on historical behavior.

#### 9. Customer RFM
```
POST /customer-intelligence/rfm
```
**What it does:** [📄 See full documentation](./Endpoints/CustomerIntelligence/CustomerRFM.md)

Performs RFM (Recency, Frequency, Monetary) analysis to segment customers and generate action recommendations.

#### 10. Customer CLV Features
```
POST /customer-intelligence/clv-features
```
**What it does:** Calculates features for Customer Lifetime Value (CLV) prediction, identifying high-value customers.

#### 11. Customer CLV Forecast
```
POST /customer-intelligence/clv-forecast
```
**What it does:** Predicts customer lifetime value (CLV), estimating future revenue each customer will generate.

#### 12. Churn Risk
```
POST /customer-intelligence/churn-risk
```
**What it does:** [📄 See full documentation](./Endpoints/CustomerIntelligence/ChurnRisk.md)

Identifies customers at risk of churn (cancellation/abandonment) and provides personalized retention recommendations.

#### 13. NPS (Net Promoter Score)
```
POST /customer-intelligence/nps
```
**What it does:** Calculates and analyzes NPS, classifying customers into Promoters, Neutrals, and Detractors.

#### 14. Churn Label
```
POST /customer-intelligence/churn-label
```
**What it does:** Classifies and labels customers according to churn probability for preventive actions.

---

### Propensity

Predictive propensity models for specific actions.

#### 15. Propensity Buy Product
```
POST /propensity/buy-product
```
**What it does:** [📄 See full documentation](./Endpoints/Propensity/PropensityBuyProduct.md)

Calculates the probability of customers purchasing specific products, identifying the most likely for targeted campaigns.

#### 16. Propensity Respond Campaign
```
POST /propensity/respond-campaign
```
**What it does:** Predicts which customers are most likely to respond positively to marketing campaigns.

#### 17. Propensity Upgrade Plan
```
POST /propensity/upgrade-plan
```
**What it does:** Identifies customers with high propensity to upgrade plan/service for upselling strategies.

---

### Recommendations

Recommendation systems based on collaborative and content-based filtering.

#### 18. Recommend User Items
```
POST /recommendations/user-items
```
**What it does:** [📄 See full documentation](./Endpoints/Recommendations/RecommendUserItems.md)

Generates personalized product recommendations for specific users based on history and preferences.

#### 19. Recommend Similar Items
```
POST /recommendations/similar-items
```
**What it does:** Recommends products similar to a specific item based on characteristics and purchase behavior.

#### 20. Cross-Sell Matrix
```
POST /recommendations/cross-sell-matrix
```
**What it does:** Generates cross-selling matrix identifying products frequently bought together.

#### 21. Upsell Suggestions
```
POST /recommendations/upsell-suggestions
```
**What it does:** Suggests personalized upgrade products/services (upsell) for each customer.

#### 22. Dynamic Pricing Recommend
```
POST /recommendations/dynamic-pricing
```
**What it does:** Recommends optimized dynamic prices based on demand, competition, and customer profile.

#### 23. Uplift Model
```
POST /recommendations/uplift-model
```
**What it does:** Calculates real uplift (increment) of campaigns, identifying customers who truly respond to interventions.

---

### Forecast

Time series predictions for strategic planning.

#### 24. Forecast Revenue
```
POST /forecast/revenue
```
**What it does:** [📄 See full documentation](./Endpoints/Forecast/ForecastRevenue.md)

Predicts future revenue using ARIMA, SARIMA, and Prophet models, with confidence intervals.

#### 25. Forecast Units
```
POST /forecast/units
```
**What it does:** Predicts quantity of future units/sales for production and inventory planning.

#### 26. Forecast Units Asyncio
```
POST /forecast/units-asyncio
```
**What it does:** Asynchronous version of units forecasting, optimized for large data volumes.

#### 27. Forecast Cost
```
POST /forecast/cost
```
**What it does:** Predicts future costs (operational, production, etc.) for budget planning.

#### 28. Forecast Cost Improved
```
POST /forecast/cost-improved
```
**What it does:** Improved version of cost forecasting with more accurate algorithms and additional features.

#### 29. Forecast Cost Totus
```
POST /forecast/cost-totus
```
**What it does:** Consolidated total cost forecasting considering multiple variables and scenarios.

---

### Inventory

Intelligent inventory optimization and management.

#### 30. Inventory History Improved
```
POST /inventory/history-improved
```
**What it does:** Analyzes inventory history with improved algorithms to identify patterns and anomalies.

#### 31. Inventory Optimization
```
POST /inventory/optimization
```
**What it does:** [📄 See full documentation](./Endpoints/Inventory/InventoryOptimization.md)

Optimizes inventory levels by calculating reorder point, economic order quantity (EOQ), and safety stock.

#### 32. NLP OpenAI Excess Inventory Report
```
POST /inventory/excess-report
```
**What it does:** Generates natural language reports about excess inventory using generative AI.

#### 33. Excess Inventory NLP
```
POST /inventory/excess-nlp
```
**What it does:** Analyzes excess inventory and generates insights and recommendations in natural language.

#### 34. NLP Analysis
```
POST /inventory/nlp-analysis
```
**What it does:** General inventory analysis using NLP to extract insights from unstructured data.

#### 35. NLP Analysis EN
```
POST /inventory/nlp-analysis-en
```
**What it does:** English version of inventory NLP analysis.

---

### Risk

Various risk analysis and scoring.

#### 36. Credit Risk Score
```
POST /risk/credit-score
```
**What it does:** [📄 See full documentation](./Endpoints/Risk/CreditRiskScore.md)

Calculates credit risk score (300-850) analyzing multiple financial and behavioral variables.

#### 37. Credit Risk Explain
```
POST /risk/credit-explain
```
**What it does:** Provides detailed explanations about factors influencing credit score (explainable AI/XAI model).

#### 38. Anomaly Transactions
```
POST /risk/anomaly-transactions
```
**What it does:** Detects anomalous/fraudulent transactions using anomaly detection algorithms.

#### 39. Anomaly Accounts
```
POST /risk/anomaly-accounts
```
**What it does:** Identifies accounts with anomalous behavior that may indicate fraud or problems.

#### 40. Anomaly Graph
```
POST /risk/anomaly-graph
```
**What it does:** Detects anomalies in relationship graphs (account networks, transactions, etc.).

---

### Analytics

Advanced attribution and sentiment analytics.

#### 41. Channel Attribution
```
POST /analytics/channel-attribution
```
**What it does:** Attributes value/credit to different marketing channels in customer journey (first-touch, last-touch, multi-touch).

#### 42. Journey Markov
```
POST /analytics/journey-markov
```
**What it does:** Models customer journey using Markov chains to understand transitions between states.

#### 43. Journey Sequences
```
POST /analytics/journey-sequences
```
**What it does:** Analyzes event sequences in customer journey identifying common patterns.

#### 44. Sentiment Report
```
POST /analytics/sentiment-report
```
**What it does:** Generates consolidated sentiment analysis reports from multiple sources (reviews, social media, etc.).

#### 45. Sentiment Realtime
```
POST /analytics/sentiment-realtime
```
**What it does:** [📄 See full documentation](./Endpoints/Analytics/SentimentAnalysis.md)

Performs real-time sentiment analysis of texts using NLP, with support for multiple languages and aspect-based analysis.

---

## Configuration

### Environment Variables

Add to `.env` file:

```env
ECHOINTEL_CUSTOMER_API_ID=your-tenant-uuid-here
ECHOINTEL_SECRET=your-64-character-secret-here
```

### Service Configuration

Add to `config/services.php`:

```php
'echointel' => [
    'customer_api_id' => env('ECHOINTEL_CUSTOMER_API_ID'),
    'secret' => env('ECHOINTEL_SECRET'),
],
```

---

## Limits and Considerations

- **Timeout:** 300 seconds (5 minutes) for long-running operations
- **Secret Rotation:** Secret must be rotated every 90 days for security
- **Rate Limiting:** Consult EchoIntel API documentation for specific limits
- **Supported Languages:** Portuguese (`pt`), English (`en`), Spanish (`es`)

---

## HTTP Status Codes

| Code  | Description |
| ----- | ----------- |
| `200` | Success - Request processed successfully |
| `400` | Bad Request - Invalid or missing parameters |
| `401` | Unauthorized - Authentication failure |
| `422` | Unprocessable Entity - Validation error |
| `500` | Internal Server Error - Server or external API error |
| `503` | Service Unavailable - EchoIntel API unavailable |

---

## Generic Request Example

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Customer-Api-Id: YOUR_TENANT_UUID" \
  -H "X-Secret: YOUR_SECRET" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"your": "payload"}' \
  "https://your-domain.com/api/v1/ai/echointel/{category}/{endpoint}"
```

---

## References

- **EchoIntel Documentation:** https://github.com/EchoSistema/abintel-documentation
- **Controller:** `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php`
- **Routes:** `src/Domain/ArtificialIntelligence/routes/api.php`
- **TCT (Technical Change Ticket):** `features/2025-01-07-echointel-api-proxy/TCT.md`

---

## Support

For questions or issues:
1. Consult the specific documentation for each endpoint
2. Check logs in `storage/logs/laravel.log`
3. Review official EchoIntel documentation
4. Contact the development team

---

## Changelog

- **2025-01-07:** Complete EchoIntel API integration with 41 endpoints
- **2025-01-07:** Creation of EchoIntelProxyController
- **2025-01-07:** Initial PT documentation

---

**Last updated:** 2025-01-07
**API Version:** v1
**Controller Version:** 1.0.0
