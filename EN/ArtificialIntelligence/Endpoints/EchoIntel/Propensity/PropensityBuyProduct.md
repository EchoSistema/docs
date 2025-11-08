# Inteligência Artificial – Propensão de Compra de Produto

## Endpoint

```
POST /api/v1/ai/echointel/propensity/buy-product
```

Calcula a propensão de clientes comprarem produtos específicos utilizando modelos preditivos.

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
| customers       | array  | Sim         | Lista de clientes para análise. |
| product_id      | string | Sim         | ID do produto alvo. |
| historical_data | array  | Não         | Dados históricos para melhorar precisão. |
| top_n           | int    | Não         | Número de clientes com maior propensão a retornar. Padrão: `100`. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {"customer_id": "C001", "age": 35, "income": 75000, "previous_purchases": 12},
      {"customer_id": "C002", "age": 28, "income": 55000, "previous_purchases": 5}
    ],
    "product_id": "PROD-123",
    "top_n": 50
  }' \
  "https://your-domain.com/api/v1/ai/echointel/propensity/buy-product"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "propensity_scores": [
    {
      "customer_id": "C001",
      "propensity_score": 0.87,
      "propensity_level": "high",
      "confidence": 0.92,
      "key_factors": [
        {"factor": "high previous purchases", "weight": 0.35},
        {"factor": "similar product history", "weight": 0.28},
        {"factor": "demographic match", "weight": 0.24}
      ],
      "recommended_actions": [
        "Send personalized email with product details",
        "Offer limited-time discount",
        "Highlight product benefits"
      ]
    },
    {
      "customer_id": "C002",
      "propensity_score": 0.42,
      "propensity_level": "medium",
      "confidence": 0.78,
      "key_factors": [
        {"factor": "age group match", "weight": 0.32},
        {"factor": "income bracket", "weight": 0.25}
      ],
      "recommended_actions": [
        "Include in general marketing campaign",
        "Provide educational content about product"
      ]
    }
  ],
  "product_info": {
    "product_id": "PROD-123",
    "avg_propensity": 0.645,
    "total_high_propensity": 125,
    "total_medium_propensity": 342,
    "total_low_propensity": 533
  }
}
```

## Estrutura JSON

| Campo                                       | Tipo    | Descrição |
| ------------------------------------------- | ------- | --------- |
| `propensity_scores`                         | array   | Scores de propensão por cliente. |
| `propensity_scores[].customer_id`           | string  | ID do cliente. |
| `propensity_scores[].propensity_score`      | float   | Score de propensão (0-1). |
| `propensity_scores[].propensity_level`      | string  | Nível (`low`, `medium`, `high`). |
| `propensity_scores[].confidence`            | float   | Confiança da previsão (0-1). |
| `propensity_scores[].key_factors`           | array   | Fatores-chave da propensão. |
| `propensity_scores[].recommended_actions`   | array   | Ações recomendadas. |
| `product_info`                              | object  | Informações agregadas do produto. |
| `product_info.avg_propensity`               | float   | Propensão média. |
| `product_info.total_high_propensity`        | int     | Total de clientes com alta propensão. |

## Notas

* Níveis de propensão: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* Resultados ordenados por `propensity_score` decrescente.
* Recomendações são personalizadas por nível de propensão.

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:177`
