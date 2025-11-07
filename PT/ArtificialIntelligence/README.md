# API de Inteligência Artificial - EchoIntel

Este documento fornece uma visão geral completa de todos os endpoints de inteligência artificial disponíveis através da integração com a plataforma EchoIntel.

## Índice

- [Visão Geral](#visão-geral)
- [Autenticação](#autenticação)
- [Categorias de Endpoints](#categorias-de-endpoints)
  - [Customer Intelligence (Inteligência de Clientes)](#customer-intelligence)
  - [Propensity (Propensão)](#propensity)
  - [Recommendations (Recomendações)](#recommendations)
  - [Forecast (Previsões)](#forecast)
  - [Inventory (Estoque)](#inventory)
  - [Risk (Risco)](#risk)
  - [Analytics (Análises)](#analytics)
- [Referências](#referências)

---

## Visão Geral

A API de Inteligência Artificial fornece **41 endpoints** organizados em **7 categorias principais**, oferecendo soluções de machine learning, análise preditiva, otimização e processamento de linguagem natural para diversos casos de uso empresariais.

### Base URL

```
https://your-domain.com/api/v1/ai/echointel
```

---

## Autenticação

Todos os endpoints requerem autenticação via **Bearer token** (Sanctum) e podem requerer headers adicionais:

| Header             | Obrigatório | Descrição |
| ------------------ | ----------- | --------- |
| Authorization      | Sim         | `Bearer {token}` |
| X-Customer-Api-Id  | Condicional | UUID do tenant (v4). Requerido se não configurado no servidor. |
| X-Secret           | Condicional | Secret de 64 caracteres. Requerido se não configurado no servidor. Deve ser rotacionado a cada 90 dias. |
| Accept-Language    | Não         | Idioma da resposta (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Type       | Sim         | `application/json` |

---

## Categorias de Endpoints

### Customer Intelligence

Análise comportamental e segmentação de clientes utilizando machine learning.

#### 1. Purchasing Segmentation
```
POST /customer-intelligence/purchasing-segmentation
```
**O que faz:** Segmenta clientes com base em padrões de compra, identificando grupos com comportamentos similares para campanhas direcionadas.

#### 2. Segmentation Report
```
POST /customer-intelligence/segmentation-report
```
**O que faz:** Gera relatórios detalhados de segmentação com visualizações e insights acionáveis.

#### 3. Segment Hierarchy Chart
```
POST /customer-intelligence/segment-hierarchy-chart
```
**O que faz:** Cria hierarquia visual de segmentos mostrando relações e sub-segmentos.

#### 4. Segment Subsegment Explore
```
POST /customer-intelligence/segment-subsegment-explore
```
**O que faz:** Explora detalhes de subsegmentos específicos dentro de segmentos maiores.

#### 5. Segment Cluster Profiles
```
POST /customer-intelligence/segment-cluster-profiles
```
**O que faz:** Gera perfis detalhados de cada cluster/segmento identificado, incluindo características demográficas e comportamentais.

#### 6. Customer Segmentation
```
POST /customer-intelligence/segmentation
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/CustomerIntelligence/CustomerSegmentation.md)

Realiza análise de segmentação automática de clientes utilizando algoritmos de clustering (K-means, Hierárquico, DBSCAN).

#### 7. Customer Features
```
POST /customer-intelligence/features
```
**O que faz:** Extrai e calcula features (características) importantes de clientes para uso em modelos preditivos.

#### 8. Customer Loyalty
```
POST /customer-intelligence/loyalty
```
**O que faz:** Analisa e pontua o nível de lealdade/fidelidade de clientes com base em comportamento histórico.

#### 9. Customer RFM
```
POST /customer-intelligence/rfm
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/CustomerIntelligence/CustomerRFM.md)

Realiza análise RFM (Recency, Frequency, Monetary) para segmentar clientes e gerar recomendações de ação.

#### 10. Customer CLV Features
```
POST /customer-intelligence/clv-features
```
**O que faz:** Calcula features para previsão de Customer Lifetime Value (CLV), identificando clientes de alto valor.

#### 11. Customer CLV Forecast
```
POST /customer-intelligence/clv-forecast
```
**O que faz:** Prevê o valor vitalício (CLV) de clientes, estimando receita futura que cada cliente gerará.

#### 12. Churn Risk
```
POST /customer-intelligence/churn-risk
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/CustomerIntelligence/ChurnRisk.md)

Identifica clientes em risco de churn (cancelamento/abandono) e fornece recomendações de retenção personalizadas.

#### 13. NPS (Net Promoter Score)
```
POST /customer-intelligence/nps
```
**O que faz:** Calcula e analisa NPS, classificando clientes em Promotores, Neutros e Detratores.

#### 14. Churn Label
```
POST /customer-intelligence/churn-label
```
**O que faz:** Classifica e rotula clientes conforme probabilidade de churn para ações preventivas.

---

### Propensity

Modelos preditivos de propensão para ações específicas.

#### 15. Propensity Buy Product
```
POST /propensity/buy-product
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Propensity/PropensityBuyProduct.md)

Calcula a probabilidade de clientes comprarem produtos específicos, identificando os mais propensos para campanhas direcionadas.

#### 16. Propensity Respond Campaign
```
POST /propensity/respond-campaign
```
**O que faz:** Prevê quais clientes têm maior probabilidade de responder positivamente a campanhas de marketing.

#### 17. Propensity Upgrade Plan
```
POST /propensity/upgrade-plan
```
**O que faz:** Identifica clientes com alta propensão a fazer upgrade de plano/serviço para estratégias de upselling.

---

### Recommendations

Sistemas de recomendação baseados em collaborative e content-based filtering.

#### 18. Recommend User Items
```
POST /recommendations/user-items
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Recommendations/RecommendUserItems.md)

Gera recomendações personalizadas de produtos para usuários específicos baseado em histórico e preferências.

#### 19. Recommend Similar Items
```
POST /recommendations/similar-items
```
**O que faz:** Recomenda produtos similares a um item específico baseado em características e comportamento de compra.

#### 20. Cross-Sell Matrix
```
POST /recommendations/cross-sell-matrix
```
**O que faz:** Gera matriz de cross-selling identificando produtos frequentemente comprados juntos.

#### 21. Upsell Suggestions
```
POST /recommendations/upsell-suggestions
```
**O que faz:** Sugere produtos/serviços de upgrade (upsell) personalizados para cada cliente.

#### 22. Dynamic Pricing Recommend
```
POST /recommendations/dynamic-pricing
```
**O que faz:** Recomenda preços dinâmicos otimizados baseados em demanda, concorrência e perfil do cliente.

#### 23. Uplift Model
```
POST /recommendations/uplift-model
```
**O que faz:** Calcula uplift (incremento) real de campanhas, identificando clientes que realmente respondem a intervenções.

---

### Forecast

Previsões de séries temporais para planejamento estratégico.

#### 24. Forecast Revenue
```
POST /forecast/revenue
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Forecast/ForecastRevenue.md)

Prevê receita futura utilizando modelos ARIMA, SARIMA e Prophet, com intervalos de confiança.

#### 25. Forecast Units
```
POST /forecast/units
```
**O que faz:** Prevê quantidade de unidades/vendas futuras para planejamento de produção e estoque.

#### 26. Forecast Units Asyncio
```
POST /forecast/units-asyncio
```
**O que faz:** Versão assíncrona de previsão de unidades, otimizada para grandes volumes de dados.

#### 27. Forecast Cost
```
POST /forecast/cost
```
**O que faz:** Prevê custos futuros (operacionais, produção, etc.) para planejamento orçamentário.

#### 28. Forecast Cost Improved
```
POST /forecast/cost-improved
```
**O que faz:** Versão melhorada de previsão de custos com algoritmos mais precisos e features adicionais.

#### 29. Forecast Cost Totus
```
POST /forecast/cost-totus
```
**O que faz:** Previsão de custos totais consolidados considerando múltiplas variáveis e cenários.

---

### Inventory

Otimização e gestão inteligente de estoque.

#### 30. Inventory History Improved
```
POST /inventory/history-improved
```
**O que faz:** Analisa histórico de estoque com algoritmos melhorados para identificar padrões e anomalias.

#### 31. Inventory Optimization
```
POST /inventory/optimization
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Inventory/InventoryOptimization.md)

Otimiza níveis de estoque calculando ponto de reposição, lote econômico (EOQ) e estoque de segurança.

#### 32. NLP OpenAI Excess Inventory Report
```
POST /inventory/excess-report
```
**O que faz:** Gera relatórios em linguagem natural sobre estoque excedente utilizando IA generativa.

#### 33. Excess Inventory NLP
```
POST /inventory/excess-nlp
```
**O que faz:** Analisa estoque excedente e gera insights e recomendações em linguagem natural.

#### 34. NLP Analysis
```
POST /inventory/nlp-analysis
```
**O que faz:** Análise geral de estoque utilizando NLP para extrair insights de dados não estruturados.

#### 35. NLP Analysis EN
```
POST /inventory/nlp-analysis-en
```
**O que faz:** Versão em inglês da análise NLP de estoque.

---

### Risk

Análise e scoring de riscos diversos.

#### 36. Credit Risk Score
```
POST /risk/credit-score
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Risk/CreditRiskScore.md)

Calcula score de risco de crédito (300-850) analisando múltiplas variáveis financeiras e comportamentais.

#### 37. Credit Risk Explain
```
POST /risk/credit-explain
```
**O que faz:** Fornece explicações detalhadas sobre fatores que influenciam o score de crédito (modelo explicável/XAI).

#### 38. Anomaly Transactions
```
POST /risk/anomaly-transactions
```
**O que faz:** Detecta transações anômalas/fraudulentas utilizando algoritmos de detecção de anomalias.

#### 39. Anomaly Accounts
```
POST /risk/anomaly-accounts
```
**O que faz:** Identifica contas com comportamento anômalo que podem indicar fraude ou problemas.

#### 40. Anomaly Graph
```
POST /risk/anomaly-graph
```
**O que faz:** Detecta anomalias em grafos de relacionamentos (redes de contas, transações, etc.).

---

### Analytics

Análises avançadas de atribuição e sentimento.

#### 41. Channel Attribution
```
POST /analytics/channel-attribution
```
**O que faz:** Atribui valor/crédito a diferentes canais de marketing na jornada do cliente (first-touch, last-touch, multi-touch).

#### 42. Journey Markov
```
POST /analytics/journey-markov
```
**O que faz:** Modela jornada do cliente usando cadeias de Markov para entender transições entre estados.

#### 43. Journey Sequences
```
POST /analytics/journey-sequences
```
**O que faz:** Analisa sequências de eventos na jornada do cliente identificando padrões comuns.

#### 44. Sentiment Report
```
POST /analytics/sentiment-report
```
**O que faz:** Gera relatórios consolidados de análise de sentimento de múltiplas fontes (reviews, redes sociais, etc.).

#### 45. Sentiment Realtime
```
POST /analytics/sentiment-realtime
```
**O que faz:** [📄 Ver documentação completa](./Endpoints/Analytics/SentimentAnalysis.md)

Realiza análise de sentimento em tempo real de textos utilizando NLP, com suporte a múltiplos idiomas e análise por aspecto.

---

## Configuração

### Variáveis de Ambiente

Adicione ao arquivo `.env`:

```env
ECHOINTEL_CUSTOMER_API_ID=your-tenant-uuid-here
ECHOINTEL_SECRET=your-64-character-secret-here
```

### Configuração de Serviço

Adicione ao `config/services.php`:

```php
'echointel' => [
    'customer_api_id' => env('ECHOINTEL_CUSTOMER_API_ID'),
    'secret' => env('ECHOINTEL_SECRET'),
],
```

---

## Limites e Considerações

- **Timeout:** 300 segundos (5 minutos) para operações de longa duração
- **Secret Rotation:** O secret deve ser rotacionado a cada 90 dias por segurança
- **Rate Limiting:** Consulte documentação da API EchoIntel para limites específicos
- **Idiomas Suportados:** Português (`pt`), Inglês (`en`), Espanhol (`es`)

---

## Códigos de Status HTTP

| Código | Descrição |
| ------ | --------- |
| `200`  | Sucesso - Requisição processada com sucesso |
| `400`  | Bad Request - Parâmetros inválidos ou ausentes |
| `401`  | Unauthorized - Falha na autenticação |
| `422`  | Unprocessable Entity - Erro de validação |
| `500`  | Internal Server Error - Erro no servidor ou na API externa |
| `503`  | Service Unavailable - API EchoIntel indisponível |

---

## Exemplo de Requisição Genérica

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Customer-Api-Id: YOUR_TENANT_UUID" \
  -H "X-Secret: YOUR_SECRET" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{"your": "payload"}' \
  "https://your-domain.com/api/v1/ai/echointel/{categoria}/{endpoint}"
```

---

## Referências

- **Documentação EchoIntel:** https://github.com/EchoSistema/abintel-documentation
- **Controller:** `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php`
- **Rotas:** `src/Domain/ArtificialIntelligence/routes/api.php`
- **TCT (Technical Change Ticket):** `features/2025-01-07-echointel-api-proxy/TCT.md`

---

## Suporte

Para dúvidas ou problemas:
1. Consulte a documentação específica de cada endpoint
2. Verifique os logs em `storage/logs/laravel.log`
3. Revise a documentação oficial do EchoIntel
4. Entre em contato com o time de desenvolvimento

---

## Changelog

- **2025-01-07:** Integração completa da API EchoIntel com 41 endpoints
- **2025-01-07:** Criação do EchoIntelProxyController
- **2025-01-07:** Documentação inicial em PT

---

**Última atualização:** 2025-01-07
**Versão da API:** v1
**Versão do Controller:** 1.0.0
