# Inteligência Artificial – Rotulação de Churn

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/churn-label
```

Rotulação de Churn utilizando inteligência artificial e machine learning.

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

> **Nota:** Os parâmetros aceitam tanto `snake_case` quanto `camelCase`.


## Resposta

Consulte a documentação oficial para formato de resposta completo.

## Como é Calculado

O sistema usa rule-based classification logic para assign labels to historical data for model training.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:170`
