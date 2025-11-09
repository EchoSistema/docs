# Inteligência Artificial – Atribuição de Canais

## Endpoint

```
POST /api/v1/ai/echointel/analytics/channel-attribution
```

Atribuição de Canais utilizando modelos de atribuição.

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

O sistema de atribuição de canais usa múltiplos modelos de atribuição para atribuir crédito por conversões através de pontos de contato de marketing:

### 1. Modelos de Atribuição

O sistema suporta cinco modelos padrão de atribuição:

- **Atribuição Primeiro Toque:** Atribui 100% do crédito ao primeiro ponto de contato na jornada do cliente
- **Atribuição Último Toque:** Atribui 100% do crédito ao ponto de contato final antes da conversão
- **Atribuição Linear:** Distribui crédito igualmente entre todos os pontos de contato na jornada
- **Atribuição Decaimento Temporal:** Atribui crédito exponencialmente crescente a pontos de contato mais próximos da conversão
- **Atribuição Valor Shapley:** Usa teoria dos jogos para calcular distribuição justa de crédito baseada em contribuição marginal

### 2. Processo de Computação

- **Passo 1:** Agregar caminhos de jornada do cliente de todos os eventos de conversão e não conversão
- **Passo 2:** Para cada modelo, calcular pesos de atribuição baseados em posições e timestamps de pontos de contato
- **Passo 3:** Aplicar pesos aos valores de conversão para determinar contribuição do canal
- **Passo 4:** Normalizar resultados para garantir que a atribuição total seja 100% para cada caminho de conversão

### 3. Desempenho e Otimização

- **Tempo de Processamento:** 150-300ms para 10.000 caminhos de jornada
- **Requisitos de Dados:** Mínimo 100 jornadas completas de clientes para atribuição confiável
- **Escalabilidade:** Lida com até 1 milhão de pontos de contato por requisição usando computação distribuída

### 4. Seleção de Modelo

- **Valor Shapley** é recomendado para a atribuição estatisticamente mais rigorosa mas tem maior custo computacional (O(n!))
- **Decaimento Temporal** fornece um bom equilíbrio entre precisão e desempenho para a maioria dos casos de uso
- **Primeiro/Último Toque** são úteis para comparações de linha base e cenários de atribuição simples

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:298`
