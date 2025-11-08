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
https://echosistema.online/api/v1/ai/echointel
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
**O que faz:** Segmenta clientes com base em padrões de compra (valor médio, frequência, categorias preferidas, sazonalidade), identificando grupos com comportamentos similares. Permite criar campanhas de marketing direcionadas e personalizadas para cada segmento, aumentando conversão e ROI. Retorna clusters de clientes com características de compra homogêneas e recomendações específicas de ação para cada grupo.

#### 2. Segmentation Report
```
POST /customer-intelligence/segmentation-report
```
**O que faz:** Gera relatórios executivos de segmentação com visualizações gráficas (distribuição de clusters, perfil demográfico, comportamental), métricas de qualidade (silhouette score, Davies-Bouldin), insights acionáveis e recomendações estratégicas para cada segmento. Ideal para apresentações gerenciais e tomada de decisão baseada em dados.

#### 3. Segment Hierarchy Chart
```
POST /customer-intelligence/segment-hierarchy-chart
```
**O que faz:** Cria hierarquia visual de segmentos em formato de árvore ou dendrograma, mostrando relações hierárquicas entre segmentos principais e sub-segmentos. Permite entender a estrutura de segmentação em múltiplos níveis (macro, meso, micro) e identificar oportunidades de segmentação mais granular ou agrupamento estratégico.

#### 4. Segment Subsegment Explore
```
POST /customer-intelligence/segment-subsegment-explore
```
**O que faz:** Permite drill-down exploratório em subsegmentos específicos dentro de segmentos maiores, revelando características detalhadas, padrões de comportamento únicos e oportunidades de micro-segmentação. Útil para análise aprofundada de nichos de mercado e personalização avançada de estratégias.

#### 5. Segment Cluster Profiles
```
POST /customer-intelligence/segment-cluster-profiles
```
**O que faz:** Gera perfis detalhados (personas) de cada cluster/segmento identificado, incluindo características demográficas (idade, gênero, localização), comportamentais (frequência de compra, ticket médio, canais preferidos), psicográficas e atitudinais. Fornece descrição narrativa de cada perfil e sugestões de estratégias de comunicação e oferta personalizadas.

#### 6. Customer Segmentation
```
POST /customer-intelligence/segmentation
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/CustomerIntelligence/CustomerSegmentation.md)

Realiza análise de segmentação automática de clientes utilizando algoritmos de clustering (K-means, Hierárquico, DBSCAN).

#### 7. Customer Features
```
POST /customer-intelligence/features
```
**O que faz:** Extrai e calcula features (características) quantitativas importantes de clientes através de feature engineering avançado: métricas RFM, padrões temporais, sazonalidade, diversidade de produtos, engagement, ciclo de vida. Features servem como input para modelos preditivos de churn, CLV, propensão e recomendação, aumentando precisão das previsões.

#### 8. Customer Loyalty
```
POST /customer-intelligence/loyalty
```
**O que faz:** Analisa e pontua o nível de lealdade/fidelidade de clientes (score 0-100) com base em comportamento histórico: recência, frequência, consistência de compra, tempo de relacionamento, diversidade de produtos, interação com programa de fidelidade. Classifica clientes em níveis (Bronze, Prata, Ouro, Platinum) e sugere ações para aumentar retenção e lifetime value.

#### 9. Customer RFM
```
POST /customer-intelligence/rfm
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/CustomerIntelligence/CustomerRFM.md)

Realiza análise RFM (Recency, Frequency, Monetary) para segmentar clientes e gerar recomendações de ação.

#### 10. Customer CLV Features
```
POST /customer-intelligence/clv-features
```
**O que faz:** Calcula features especializadas para previsão de Customer Lifetime Value (CLV): valor histórico total, média de compra, tendência de crescimento, probabilidade de recompra, taxa de retorno, margem de contribuição. Identifica clientes de alto valor potencial e atual, permitindo priorização de recursos e estratégias de retenção focadas.

#### 11. Customer CLV Forecast
```
POST /customer-intelligence/clv-forecast
```
**O que faz:** Prevê o valor vitalício (CLV) de clientes nos próximos 12, 24 ou 36 meses, estimando receita futura total que cada cliente gerará. Utiliza modelos probabilísticos (BG/NBD, Pareto/NBD) e machine learning para calcular probabilidade de estar ativo, frequência esperada de compra e valor monetário futuro. Essencial para CAC (Customer Acquisition Cost) viável e alocação de budget de marketing.

#### 12. Churn Risk
```
POST /customer-intelligence/churn-risk
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/CustomerIntelligence/ChurnRisk.md)

Identifica clientes em risco de churn (cancelamento/abandono) e fornece recomendações de retenção personalizadas.

#### 13. NPS (Net Promoter Score)
```
POST /customer-intelligence/nps
```
**O que faz:** Calcula e analisa NPS (Net Promoter Score) baseado em respostas de pesquisas ou comportamento inferido, classificando clientes em Promotores (9-10), Neutros (7-8) e Detratores (0-6). Fornece score geral, distribuição por segmento, evolução temporal e correlação com métricas de negócio. Identifica drivers de satisfação/insatisfação e ações para melhorar experiência do cliente.

#### 14. Churn Label
```
POST /customer-intelligence/churn-label
```
**O que faz:** Classifica e rotula clientes conforme probabilidade de churn em categorias (Muito Alto, Alto, Médio, Baixo) para ações preventivas segmentadas. Além do score, atribui labels descritivos e recomendações específicas de retenção por nível de risco. Permite criação de campanhas automatizadas e workflows de retenção baseados em labels.

---

### Propensity

Modelos preditivos de propensão para ações específicas.

#### 15. Propensity Buy Product
```
POST /propensity/buy-product
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Propensity/PropensityBuyProduct.md)

Calcula a probabilidade de clientes comprarem produtos específicos, identificando os mais propensos para campanhas direcionadas.

#### 16. Propensity Respond Campaign
```
POST /propensity/respond-campaign
```
**O que faz:** Prevê quais clientes têm maior probabilidade de responder positivamente (abrir, clicar, converter) a campanhas de marketing específicas. Analisa histórico de engajamento, timing ideal, canal preferido e tipo de mensagem mais efetiva. Permite segmentação inteligente de audiências, reduzindo desperdício de budget e fadiga de comunicação, otimizando ROI de campanhas.

#### 17. Propensity Upgrade Plan
```
POST /propensity/upgrade-plan
```
**O que faz:** Identifica clientes com alta propensão a fazer upgrade de plano/serviço baseado em padrões de uso (limites atingidos, features premium tentadas), crescimento de negócio, satisfação e ciclo de vida. Fornece score de propensão, momento ideal para abordagem, incentivos recomendados (desconto, período trial) e argumentos de venda específicos. Maximiza receita recorrente (MRR) através de upselling estratégico.

---

### Recommendations

Sistemas de recomendação baseados em collaborative e content-based filtering.

#### 18. Recommend User Items
```
POST /recommendations/user-items
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Recommendations/RecommendUserItems.md)

Gera recomendações personalizadas de produtos para usuários específicos baseado em histórico e preferências.

#### 19. Recommend Similar Items
```
POST /recommendations/similar-items
```
**O que faz:** Recomenda produtos similares a um item específico baseado em características (categoria, atributos, preço, marca), comportamento de compra (co-visualização, co-compra) e similaridade de conteúdo (descrição, tags). Ideal para páginas de detalhe de produto ("Produtos Similares"), substituição de items indisponíveis e diversificação de catálogo. Utiliza content-based filtering e collaborative filtering.

#### 20. Cross-Sell Matrix
```
POST /recommendations/cross-sell-matrix
```
**O que faz:** Gera matriz de afinidade de cross-selling identificando pares e conjuntos de produtos frequentemente comprados juntos através de análise de cestas de compra (market basket analysis). Calcula métricas de associação (support, confidence, lift) para cada combinação. Permite criar bundles estratégicos, recomendações "Frequentemente Comprados Juntos" e aumentar ticket médio.

#### 21. Upsell Suggestions
```
POST /recommendations/upsell-suggestions
```
**O que faz:** Sugere produtos/serviços de upgrade (upsell) personalizados para cada cliente baseado em histórico de compra, propensão a pagar mais, estágio do ciclo de vida e padrão de consumo. Identifica versões premium, modelos superiores ou adicionais de maior valor que fazem sentido para o perfil do cliente. Maximiza valor da transação (AOV - Average Order Value) com recomendações relevantes.

#### 22. Dynamic Pricing Recommend
```
POST /recommendations/dynamic-pricing
```
**O que faz:** Recomenda preços dinâmicos otimizados em tempo real baseados em múltiplos fatores: demanda atual e prevista, preços da concorrência, elasticidade-preço do produto, sazonalidade, nível de estoque, perfil e sensibilidade do cliente (price sensitivity). Utiliza reinforcement learning e otimização para maximizar receita ou margem de lucro. Essencial para e-commerce, hotelaria, transporte e marketplaces.

#### 23. Uplift Model
```
POST /recommendations/uplift-model
```
**O que faz:** Calcula uplift (incremento causal) real de campanhas através de uplift modeling, identificando clientes que realmente respondem a intervenções de marketing versus aqueles que comprariam de qualquer forma (sure things) ou nunca comprarão (lost causes). Segmenta audiência em Persuadables, Sure Things, Lost Causes e Sleeping Dogs. Evita desperdício de budget focando apenas em quem pode ser influenciado positivamente pela campanha.

---

### Forecast

Previsões de séries temporais para planejamento estratégico.

#### 24. Forecast Revenue
```
POST /forecast/revenue
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Forecast/ForecastRevenue.md)

Prevê receita futura utilizando modelos ARIMA, SARIMA e Prophet, com intervalos de confiança.

#### 25. Forecast Units
```
POST /forecast/units
```
**O que faz:** Prevê quantidade de unidades/vendas futuras (diária, semanal, mensal) para planejamento de produção, compras e gestão de estoque. Utiliza modelos de séries temporais (ARIMA, Prophet, XGBoost) considerando sazonalidade, tendências, eventos especiais e variáveis exógenas. Fornece previsão pontual e intervalos de confiança. Essencial para evitar stockout e excesso de inventory.

#### 26. Forecast Units Asyncio
```
POST /forecast/units-asyncio
```
**O que faz:** Versão assíncrona e otimizada de previsão de unidades, processando múltiplos SKUs/produtos em paralelo através de computação distribuída. Ideal para catálogos extensos (milhares de SKUs), reduzindo tempo de processamento de horas para minutos. Retorna job ID para consulta posterior dos resultados via polling ou webhook.

#### 27. Forecast Cost
```
POST /forecast/cost
```
**O que faz:** Prevê custos futuros (operacionais, produção, matéria-prima, logística, marketing) para planejamento orçamentário e financeiro. Analisa histórico de custos, correlação com volume de vendas, sazonalidade, inflação e variáveis econômicas. Permite criação de cenários (otimista, realista, pessimista) e identificação de oportunidades de redução de custos. Fundamental para gestão de margem e lucratividade.

#### 28. Forecast Cost Improved
```
POST /forecast/cost-improved
```
**O que faz:** Versão aprimorada de previsão de custos utilizando algoritmos de última geração (LSTM, Transformer, ensemble methods) e features adicionais: variáveis macroeconômicas, índices setoriais, commodities, câmbio. Inclui detecção automática de anomalias, análise de drivers de custo e simulação what-if. Precisão superior para planejamento estratégico de médio/longo prazo.

#### 29. Forecast Cost Totus
```
POST /forecast/cost-totus
```
**O que faz:** Previsão de custos totais consolidados (TOTUS - Total Operating Unified System) agregando todas categorias de custo (COGS, OPEX, CAPEX, overhead) em visão unificada. Considera interdependências entre centros de custo, alocações, rateios e múltiplos cenários de negócio. Fornece breakdown detalhado por categoria, departamento e driver, facilitando decisões de otimização holística de custos e pricing estratégico.

---

### Inventory

Otimização e gestão inteligente de estoque.

#### 30. Inventory History Improved
```
POST /inventory/history-improved
```
**O que faz:** Analisa histórico de estoque com algoritmos avançados de detecção de padrões, identificando anomalias (picos, quedas abruptas), sazonalidade, tendências de longo prazo e comportamento irregular. Detecta problemas como stockouts recorrentes, excesso crônico, variabilidade excessiva e ciclos não otimizados. Fornece visualizações interativas e recomendações de melhoria baseadas em análise histórica profunda.

#### 31. Inventory Optimization
```
POST /inventory/optimization
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Inventory/InventoryOptimization.md)

Otimiza níveis de estoque calculando ponto de reposição, lote econômico (EOQ) e estoque de segurança.

#### 32. NLP OpenAI Excess Inventory Report
```
POST /inventory/excess-report
```
**O que faz:** Gera relatórios executivos em linguagem natural sobre estoque excedente utilizando IA generativa (GPT). Analisa produtos parados, slow-movers e overstocking, gerando narrativa compreensível com contexto, causas prováveis (sazonalidade, mudança de demanda, erro de compra), impacto financeiro (capital parado, obsolescência) e recomendações acionáveis (promoções, liquidação, redirecionamento). Facilita comunicação com stakeholders não-técnicos.

#### 33. Excess Inventory NLP
```
POST /inventory/excess-nlp
```
**O que faz:** Analisa estoque excedente através de processamento de linguagem natural, extraindo insights de dados estruturados (níveis de inventory) e não-estruturados (notas de compra, feedback de vendas, descrições). Gera recomendações em linguagem natural priorizadas por impacto financeiro: estratégias de clearance, bundling, desconto progressivo, redirecionamento entre canais/lojas. Identifica padrões sistemáticos de overstock para correção de processos de compra.

#### 34. NLP Analysis
```
POST /inventory/nlp-analysis
```
**O que faz:** Análise geral e abrangente de estoque utilizando NLP para extrair insights de dados não estruturados (descrições de produtos, comentários de compradores, notas de transferência, emails de fornecedores). Identifica problemas de qualidade, inconsistências de dados, oportunidades de categorização melhorada e gaps de informação. Gera sumário executivo em português sobre saúde do inventário e pontos de atenção prioritários.

#### 35. NLP Analysis EN
```
POST /inventory/nlp-analysis-en
```
**O que faz:** Versão em inglês da análise NLP de estoque, otimizada para mercados internacionais e empresas multinacionais. Processa dados em inglês (descrições, documentação, comunicações) e gera relatórios executivos em inglês seguindo padrões e terminologia internacional de supply chain e inventory management. Ideal para consolidação de análises multi-região e reporting corporativo global.

---

### Risk

Análise e scoring de riscos diversos.

#### 36. Credit Risk Score
```
POST /risk/credit-score
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Risk/CreditRiskScore.md)

Calcula score de risco de crédito (300-850) analisando múltiplas variáveis financeiras e comportamentais.

#### 37. Credit Risk Explain
```
POST /risk/credit-explain
```
**O que faz:** Fornece explicações detalhadas e interpretáveis sobre fatores que influenciam o score de crédito através de Explainable AI (XAI). Utiliza técnicas como SHAP, LIME para decompor o score em contribuições individuais de cada feature (histórico de pagamento, utilização de crédito, idade do crédito). Essencial para compliance regulatório (LGPD, GDPR), transparência com clientes e identificação de ações para melhoria de score.

#### 38. Anomaly Transactions
```
POST /risk/anomaly-transactions
```
**O que faz:** Detecta transações anômalas/fraudulentas em tempo real utilizando algoritmos avançados de detecção de anomalias (Isolation Forest, Autoencoders, LOF). Identifica padrões suspeitos: valores atípicos, horários incomuns, localização inconsistente, sequência irregular, múltiplas tentativas. Atribui score de anomalia (0-100) e motivos da suspeita. Crítico para prevenção de fraude, chargebacks e segurança financeira.

#### 39. Anomaly Accounts
```
POST /risk/anomaly-accounts
```
**O que faz:** Identifica contas de usuários com comportamento anômalo através de análise comportamental contínua. Detecta sinais de comprometimento (account takeover), uso indevido, bots, múltiplas identidades, lavagem de dinheiro. Analisa padrões de login, dispositivos, geolocalização, velocidade de transações, mudanças súbitas de comportamento. Permite ação proativa antes de perdas significativas ou violações de segurança.

#### 40. Anomaly Graph
```
POST /risk/anomaly-graph
```
**O que faz:** Detecta anomalias em grafos de relacionamentos (redes de contas, transações, entidades) utilizando graph analytics e algoritmos de detecção de comunidades anômalas. Identifica padrões suspeitos: anéis de fraude (fraud rings), esquemas de lavagem em múltiplas camadas, networks de contas sintéticas, centralidade anormal. Visualiza subgrafos suspeitos e calcula métricas de risco de rede. Fundamental para combate a fraudes organizadas e análise forense.

---

### Analytics

Análises avançadas de atribuição e sentimento.

#### 41. Channel Attribution
```
POST /analytics/channel-attribution
```
**O que faz:** Atribui valor/crédito proporcional a diferentes canais de marketing (paid search, organic, social, email, display) na jornada do cliente utilizando modelos de atribuição: first-touch, last-touch, linear, time-decay, position-based e data-driven (algorítmico). Quantifica contribuição real de cada touchpoint para conversão, permitindo otimização de budget de marketing e ROI por canal. Resolve o problema de sub/super-valorização de canais.

#### 42. Journey Markov
```
POST /analytics/journey-markov
```
**O que faz:** Modela jornada do cliente usando cadeias de Markov para entender probabilidades de transição entre estados (visitante > lead > prospect > cliente > churn). Calcula probabilidade de conversão em cada estágio, tempo médio de permanência, caminhos mais prováveis e pontos de abandono críticos. Permite identificar gargalos na jornada, otimizar funil de vendas e simular impacto de intervenções em diferentes estágios.

#### 43. Journey Sequences
```
POST /analytics/journey-sequences
```
**O que faz:** Analisa sequências de eventos na jornada do cliente através de sequence mining, identificando padrões sequenciais comuns (frequent patterns), caminhos de sucesso e caminhos de falha. Descobre sequências típicas de conversores vs. não-conversores, timing ideal entre touchpoints e ordem ótima de interações. Permite otimização de campanhas multi-etapa (drip campaigns, nurturing) baseada em padrões comportamentais reais.

#### 44. Sentiment Report
```
POST /analytics/sentiment-report
```
**O que faz:** Gera relatórios consolidados e executivos de análise de sentimento agregando múltiplas fontes (reviews de produtos, redes sociais, pesquisas NPS, tickets de suporte, comentários em blog). Apresenta distribuição de sentimento (positivo/neutro/negativo) ao longo do tempo, por produto/categoria, por canal. Identifica trending topics, crises emergentes, oportunidades de melhoria e drivers de satisfação/insatisfação. Inclui word clouds, gráficos de evolução temporal e alertas de sentimento negativo crescente.

#### 45. Sentiment Realtime
```
POST /analytics/sentiment-realtime
```
**O que faz:** [Ver documentação completa](Endpoints/EchoIntel/Analytics/SentimentAnalysis.md)

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
  "https://echosistema.online/api/v1/ai/echointel/{categoria}/{endpoint}"
```

---

## Referências

- **Controller:** `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php`
- **Rotas:** `src/Domain/ArtificialIntelligence/routes/api.php`
- **TCT (Technical Change Ticket):** `features/2025-01-07-echointel-api-proxy/TCT.md`

---

## Suporte

Para dúvidas ou problemas:
1. Consulte a documentação específica de cada endpoint
2. Verifique os logs em `storage/logs/ia.log`
3. Entre em contato com o time de desenvolvimento

---

## Changelog

- **2025-01-07:** Integração completa da API EchoIntel com 41 endpoints
- **2025-01-07:** Criação do EchoIntelProxyController
- **2025-01-07:** Documentação inicial em PT

---

**Última atualização:** 2025-01-07
**Versão da API:** v1
**Versão do Controller:** 1.0.0
