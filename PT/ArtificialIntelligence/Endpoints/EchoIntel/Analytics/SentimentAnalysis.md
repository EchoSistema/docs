# Inteligência Artificial – Análise de Sentimento em Tempo Real

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-realtime
```

Realiza análise de sentimento em tempo real de textos (reviews, comentários, feedbacks) utilizando processamento de linguagem natural (NLP).

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). |
| X-Secret           | string | Condicional | Secret de 64 caracteres. |
| Accept-Language    | string | Não         | Idioma (`en`, `es`, `pt`). |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| texts           | array  | Sim         | Textos para análise de sentimento. |
| language        | string | Não         | Idioma dos textos (`auto` para detecção automática). |
| include_entities| boolean| Não         | Extrair entidades mencionadas. Padrão: `false`. |
| include_aspects | boolean| Não         | Análise de sentimento por aspecto. Padrão: `false`. |

## Exemplos

### Exemplo de requisição (curl)

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
        "text": "Produto excelente! Entrega rápida e atendimento impecável.",
        "source": "product_review"
      },
      {
        "id": "comment_045",
        "text": "Péssima experiência. O produto chegou danificado e o suporte não resolveu.",
        "source": "customer_feedback"
      }
    ],
    "language": "pt",
    "include_entities": true,
    "include_aspects": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-realtime"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "sentiment_analysis": [
    {
      "id": "review_001",
      "text": "Produto excelente! Entrega rápida e atendimento impecável.",
      "sentiment": "positive",
      "confidence": 0.96,
      "score": 0.88,
      "aspects": [
        {"aspect": "produto", "sentiment": "positive", "score": 0.92},
        {"aspect": "entrega", "sentiment": "positive", "score": 0.85},
        {"aspect": "atendimento", "sentiment": "positive", "score": 0.90}
      ],
      "entities": [
        {"entity": "produto", "type": "PRODUCT"},
        {"entity": "entrega", "type": "SERVICE"},
        {"entity": "atendimento", "type": "SERVICE"}
      ],
      "keywords": ["excelente", "rápida", "impecável"]
    },
    {
      "id": "comment_045",
      "text": "Péssima experiência. O produto chegou danificado e o suporte não resolveu.",
      "sentiment": "negative",
      "confidence": 0.94,
      "score": -0.82,
      "aspects": [
        {"aspect": "experiência", "sentiment": "negative", "score": -0.95},
        {"aspect": "produto", "sentiment": "negative", "score": -0.78},
        {"aspect": "suporte", "sentiment": "negative", "score": -0.75}
      ],
      "entities": [
        {"entity": "produto", "type": "PRODUCT"},
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

## Estrutura JSON

| Campo                                    | Tipo    | Descrição |
| ---------------------------------------- | ------- | --------- |
| `sentiment_analysis`                     | array   | Análises por texto. |
| `sentiment_analysis[].id`                | string  | ID do texto. |
| `sentiment_analysis[].text`              | string  | Texto analisado. |
| `sentiment_analysis[].sentiment`         | string  | Sentimento (`positive`, `neutral`, `negative`). |
| `sentiment_analysis[].confidence`        | float   | Confiança da classificação (0-1). |
| `sentiment_analysis[].score`             | float   | Score de sentimento (-1 a +1). |
| `sentiment_analysis[].aspects`           | array   | Análise por aspecto (se solicitado). |
| `sentiment_analysis[].entities`          | array   | Entidades extraídas (se solicitado). |
| `sentiment_analysis[].keywords`          | array   | Palavras-chave identificadas. |
| `summary`                                | object  | Resumo geral. |
| `summary.total_analyzed`                 | int     | Total de textos analisados. |
| `summary.positive`                       | int     | Quantidade de sentimentos positivos. |
| `summary.neutral`                        | int     | Quantidade de sentimentos neutros. |
| `summary.negative`                       | int     | Quantidade de sentimentos negativos. |
| `summary.avg_score`                      | float   | Score médio. |

## Classificação de Sentimento

| Score       | Sentimento | Descrição |
| ----------- | ---------- | --------- |
| 0.5 a 1.0   | Positive   | Sentimento claramente positivo. |
| 0.1 a 0.49  | Positive   | Sentimento levemente positivo. |
| -0.1 a 0.1  | Neutral    | Sentimento neutro ou misto. |
| -0.49 a -0.1| Negative   | Sentimento levemente negativo. |
| -1.0 a -0.5 | Negative   | Sentimento claramente negativo. |

## Notas

* Suporte para múltiplos idiomas (auto-detecção disponível).
* Análise por aspecto identifica sentimentos sobre diferentes características.
* Útil para análise de reviews, feedback de clientes, redes sociais, etc.

## Como é Calculado

O sistema de análise de sentimentos usa modelos NLP de última geração para classificar sentimento de texto e extrair insights:

### 1. Arquitetura do Modelo NLP

O sistema emprega modelos baseados em transformers ajustados para classificação de sentimentos:

- **Modelos Baseados em BERT:** Usa BERT multilíngue (mBERT) ou variantes BERT específicas do idioma para detecção de sentimentos consciente do contexto
- **Sentimento Baseado em Aspectos:** Aplica extração de sentimento direcionado para aspectos/características específicos mencionados no texto
- **Reconhecimento de Entidades:** Usa Reconhecimento de Entidades Nomeadas (NER) para identificar produtos, serviços e marcas

### 2. Processo de Pontuação de Sentimento

- **Passo 1:** Pré-processamento de texto (tokenização, normalização, minúsculas)
- **Passo 2:** Geração de embeddings contextuais usando codificador BERT (vetores de 768 dimensões)
- **Passo 3:** Cabeça de classificação prediz probabilidades de sentimento [positivo, neutro, negativo]
- **Passo 4:** Converter probabilidades para pontuação contínua: pontuação = P(positivo) - P(negativo) ∈ [-1, +1]

### 3. Confiança e Calibração

- **Pontuação de Confiança:** Probabilidade máxima de classe após calibração de escalonamento de temperatura
- **Classificação Baseada em Limiares:** Zona neutra (-0.1 a +0.1) reduz classificações falso positivas/negativas
- **Agregação de Ensemble:** Combina previsões de múltiplos modelos para melhorar a precisão (opcional)

### 4. Desempenho e Otimização

- **Tempo de Processamento:** 50-100ms por texto (processamento em lote: 20 textos/segundo)
- **Precisão:** 88-92% em conjuntos de dados de referência (SST-2, IMDB, dados de domínio personalizado)
- **Idiomas Suportados:** 100+ idiomas via mBERT, otimizado para EN, ES, PT
- **Aceleração GPU:** Reduz latência em 5-10x para solicitações em lote (>50 textos)

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:333`
