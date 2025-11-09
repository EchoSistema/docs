# Inteligência Artificial – Sequências de Jornada

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-sequences
```

Sequências de Jornada identificando padrões.

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

A análise de sequências de jornada usa algoritmos de mineração de padrões sequenciais para descobrir padrões de comportamento de clientes frequentes e significativos:

### 1. Mineração de Padrões Sequenciais

O sistema emprega algoritmos avançados de descoberta de padrões:

- **Mineração Baseada em Apriori:** Usa algoritmo Apriori sequencial para encontrar subsequências frequentes que atendem limiar mínimo de suporte
- **Algoritmo Prefix Span:** Minera eficientemente padrões sequenciais usando metodologia de crescimento de padrões para grandes conjuntos de dados
- **Filtragem Baseada em Restrições:** Aplica restrições de lacuna, janelas de tempo e filtros de comprimento mínimo/máximo

### 2. Processo de Descoberta de Padrões

- **Passo 1:** Converter dados de jornada brutos em sequências ordenadas de eventos com timestamps
- **Passo 2:** Minerar sequências frequentes usando suporte mínimo (padrão: 1% de todas as jornadas)
- **Passo 3:** Identificar sequências fechadas (padrões maximais que subsumem padrões menores)
- **Passo 4:** Classificar padrões por métricas de suporte, confiança e lift

### 3. Análise de Sequências

- **Análise de Frequência:** Calcular suporte absoluto e relativo para cada padrão descoberto
- **Padrões Temporais:** Identificar padrões baseados em tempo (tendências horárias, diárias, sazonais em sequências)
- **Detecção de Divergência:** Encontrar sequências que diferenciam conversores de não conversores

### 4. Desempenho e Otimização

- **Tempo de Processamento:** 300-600ms para 100.000 sequências de jornada
- **Requisitos de Dados:** Mínimo 1.000 jornadas para descoberta significativa de padrões
- **Escalabilidade:** Mineração paralela para conjuntos de dados que excedem 1 milhão de sequências
- **Poda de Padrões:** Remove automaticamente padrões redundantes e estatisticamente insignificantes

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:308`
