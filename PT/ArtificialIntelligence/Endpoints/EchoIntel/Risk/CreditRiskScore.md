# Inteligência Artificial – Score de Risco de Crédito

## Endpoint

```
POST /api/v1/ai/echointel/risk/credit-score
```

Calcula score de risco de crédito para clientes utilizando modelos de machine learning que analisam múltiplas variáveis financeiras e comportamentais.

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

| Parâmetro          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| applicants         | array  | Sim         | Dados dos solicitantes de crédito. |
| credit_amount      | float  | Sim         | Valor do crédito solicitado. |
| include_explanation| boolean| Não         | Incluir explicação detalhada. Padrão: `false`. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "applicants": [
      {
        "applicant_id": "A001",
        "age": 35,
        "annual_income": 75000,
        "employment_length": 8,
        "debt_to_income": 0.28,
        "credit_history_length": 12,
        "previous_defaults": 0,
        "open_credit_lines": 3
      }
    ],
    "credit_amount": 25000,
    "include_explanation": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/risk/credit-score"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "credit_scores": [
    {
      "applicant_id": "A001",
      "credit_score": 742,
      "risk_category": "low",
      "approval_recommendation": "approve",
      "default_probability": 0.08,
      "suggested_terms": {
        "max_amount": 30000,
        "interest_rate": 0.065,
        "term_months": 60
      },
      "risk_factors": [
        {
          "factor": "stable employment",
          "impact": "positive",
          "weight": 0.25
        },
        {
          "factor": "low debt-to-income ratio",
          "impact": "positive",
          "weight": 0.22
        },
        {
          "factor": "no previous defaults",
          "impact": "positive",
          "weight": 0.20
        }
      ]
    }
  ],
  "model_version": "v2.5.3",
  "evaluation_date": "2025-01-07T15:30:00Z"
}
```

## Estrutura JSON

| Campo                                      | Tipo    | Descrição |
| ------------------------------------------ | ------- | --------- |
| `credit_scores`                            | array   | Scores por solicitante. |
| `credit_scores[].applicant_id`             | string  | ID do solicitante. |
| `credit_scores[].credit_score`             | int     | Score de crédito (300-850). |
| `credit_scores[].risk_category`            | string  | Categoria de risco (`low`, `medium`, `high`). |
| `credit_scores[].approval_recommendation`  | string  | Recomendação (`approve`, `review`, `reject`). |
| `credit_scores[].default_probability`      | float   | Probabilidade de default (0-1). |
| `credit_scores[].suggested_terms`          | object  | Termos sugeridos. |
| `credit_scores[].suggested_terms.max_amount` | float | Valor máximo recomendado. |
| `credit_scores[].suggested_terms.interest_rate` | float | Taxa de juros sugerida. |
| `credit_scores[].suggested_terms.term_months` | int  | Prazo em meses. |
| `credit_scores[].risk_factors`             | array   | Fatores de risco. |

## Categorias de Risco

| Score      | Categoria | Descrição |
| ---------- | --------- | --------- |
| 750-850    | Low       | Risco muito baixo, excelente histórico. |
| 650-749    | Medium-Low| Risco baixo, bom histórico. |
| 550-649    | Medium    | Risco moderado, requer análise. |
| 450-549    | Medium-High| Risco elevado, análise criteriosa necessária. |
| 300-449    | High      | Risco muito alto, aprovação não recomendada. |

## Notas

* Scores mais altos indicam menor risco de crédito.
* Fatores considerados: histórico de crédito, renda, emprego, dívidas, etc.
* Para explicações detalhadas, use `include_explanation: true`.

## Como é Calculado

O sistema usa risk scoring and classification models para assess credit risk and default probability.

### 1. Algoritmo Principal

- Usa técnicas de aprendizado de máquina padrão da indústria
- Treinado em padrões de dados históricos
- Otimizado para precisão e desempenho

### 2. Etapas de Processamento

- **Passo 1:** Pré-processamento de dados e extração de características
- **Passo 2:** Treinamento ou inferência do modelo
- **Passo 3:** Geração e validação de resultados
- **Passo 4:** Formatação e entrega de saída

### 3. Desempenho

- **Tempo de Processamento:** Otimizado para resposta sub-segundo (típico: 200-500ms)
- **Escalabilidade:** Lida com grandes conjuntos de dados eficientemente
- **Precisão:** Validado contra conjuntos de dados de referência

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:288`
