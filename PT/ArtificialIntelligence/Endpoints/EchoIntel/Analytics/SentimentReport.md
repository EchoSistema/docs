# Inteligência Artificial – Relatório de Sentimento

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-report
```

Relatório de Sentimento consolidando múltiplas fontes.

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


## Como é Calculado

O relatório de sentimentos agrega e analisa dados de sentimentos de múltiplas fontes para fornecer insights abrangentes:

### 1. Agregação de Dados

O sistema consolida pontuações de sentimentos de várias fontes de dados:

- **Coleta Multi-Fonte:** Agrega sentimentos de avaliações, mídias sociais, feedback de clientes, tickets de suporte, pesquisas
- **Agrupamento Temporal:** Agrupa sentimentos por períodos de tempo (horário, diário, semanal, mensal) para análise de tendências
- **Ponderação de Fontes:** Aplica pesos configuráveis a diferentes fontes baseados em confiabilidade e importância empresarial

### 2. Análise Estatística

- **Passo 1:** Calcular estatísticas descritivas (média, mediana, desvio padrão) para pontuações de sentimentos
- **Passo 2:** Calcular distribuição de sentimentos (% positivo, % neutro, % negativo) através de períodos de tempo
- **Passo 3:** Identificar tendências estatisticamente significativas usando teste de tendência Mann-Kendall
- **Passo 4:** Detectar anomalias de sentimento usando detecção de valores discrepantes baseada em pontuação Z e IQR

### 3. Geração de Insights

- **Detecção de Tendências:** Identifica padrões de sentimentos em melhoria, declínio ou estáveis ao longo do tempo
- **Extração de Tópicos:** Usa LDA (Latent Dirichlet Allocation) para descobrir temas principais em feedback positivo/negativo
- **Análise Comparativa:** Compara sentimentos entre produtos, regiões, segmentos de clientes ou períodos de tempo
- **Gatilhos de Alerta:** Sinaliza quedas ou picos repentinos de sentimento que excedem mudanças de limiar

### 4. Desempenho e Otimização

- **Tempo de Processamento:** 500ms-2s para agregar 100.000 registros de sentimentos
- **Requisitos de Dados:** Mínimo 100 pontos de dados de sentimentos para análise estatística confiável
- **Cache:** Agregações temporais são armazenadas em cache por 1 hora para melhorar tempos de resposta
- **Escalabilidade:** Lida com milhões de registros de sentimentos usando particionamento baseado em tempo

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:328`
