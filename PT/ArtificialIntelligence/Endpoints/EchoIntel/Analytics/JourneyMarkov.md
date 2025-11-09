# Inteligência Artificial – Jornada do Cliente (Markov)

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-markov
```

Jornada do Cliente (Markov) com cadeias de Markov.

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

A análise de jornada usa modelagem de cadeias de Markov para entender probabilidades de caminhos de clientes e probabilidade de conversão:

### 1. Construção de Cadeia de Markov

O sistema constrói uma máquina de estados probabilística a partir de dados de jornada do cliente:

- **Identificação de Estados:** Cada ponto de contato único (canal, página, ação) se torna um estado na cadeia de Markov
- **Matriz de Transição:** Calcula probabilidade P(estado_j | estado_i) para todos os pares de estados baseado em sequências de jornada observadas
- **Estados Absorventes:** Define estados terminais (conversão, abandono) que os clientes não podem deixar uma vez que entraram

### 2. Cálculo de Probabilidade de Transição

- **Passo 1:** Contar todas as transições observadas do estado i para o estado j através de todas as jornadas de clientes
- **Passo 2:** Normalizar pelo total de transições do estado i para obter P(j|i) = count(i→j) / sum(count(i→*))
- **Passo 3:** Lidar com dados esparsos usando suavização de Laplace (suavização add-one) para transições raras

### 3. Análise de Jornada

- **Probabilidade de Conversão:** Calcular probabilidade de alcançar conversão a partir de qualquer estado usando análise de cadeia absorvente
- **Comprimento de Caminho Esperado:** Calcular número médio de passos até conversão ou abandono
- **Caminhos Críticos:** Identificar sequências de alta probabilidade que levam à conversão usando algoritmo Viterbi

### 4. Desempenho e Otimização

- **Tempo de Processamento:** 200-400ms para 50.000 sequências de jornada
- **Requisitos de Dados:** Mínimo 500 jornadas completas para probabilidades de transição estáveis
- **Eficiência de Memória:** Usa representação de matriz esparsa para espaços de estado grandes (10.000+ estados)

## Referências

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:303`
