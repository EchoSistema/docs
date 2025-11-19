# Artificial Intelligence – Cost Forecast

## Endpoint

```
POST /api/v1/ai/echointel/forecast/cost
```

Predicts future costs using time-series forecasting models (ARIMA, Prophet, ETS) based on historical cost data.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Obrigatório if not configured on the server. |
| X-Secret           | string | Condicional | 64-caracteres de segredo. Obrigatório if not configured on the server. |
| Accept-Language    | string | No         | Idioma de resposta (`en`, `es`, `pt`). Predeterminado: `en`. |
| Content-Tipo       | string | Sí         | `application/json`. |

## Parámetros

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo de la Requisição

| Parámetro | Tipo | Requerido | Descripción | Significado Empresarial | Padrão |
| --------- | ---- | -------- | ----------- | ---------------- | ------- |
| data | array | Sí | Dados de entrada para análisis. | Dados empresariais a serem processados. | - |
| options | object | No | Opções de configuração del algoritmo. | Parámetros de personalização para o modelo de ML. | `{}` |
| include_metadata | boolean | No | Incluir metadados de processamento en la resposta. | Adicionar informações de diagnóstico. | `false` |

## Ejemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "data": [
      {"id": "001", "value": 100}
    ],
    "options": {},
    "include_metadata": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/forecast/cost"
```

### Exemplo de Requisição (Python)

```python
import requests

url = "https://echosistema.online/api/v1/ai/echointel/forecast/cost"
headers = {
    "Authorization": "Bearer <token>",
    "X-Customer-Api-Id": "<tenant-uuid>",
    "X-Secret": "<secret>",
    "Accept-Language": "en",
    "Content-Type": "application/json"
}
payload = {
    "data": [{"id": "001", "value": 100}],
    "options": {},
    "include_metadata": True
}

response = requests.post(url, headers=headers, json=payload)
result = response.json()
```

## Respuesta

### Éxito `200 OK`

```json
{
  "results": [
    {
      "id": "001",
      "prediction": 0.85,
      "confidence": 0.92
    }
  ],
  "metadata": {
    "model_version": "v2.1.0",
    "processing_time_ms": 145
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required and must contain at least one record."
}
```

### Error `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid data format",
  "details": {
    "data.0.value": "The value field must be a number."
  }
}
```

## Estructura JSON

| Campo | Tipo | Descripción | Significado Empresarial |
| ----- | ---- | ----------- | ---------------- |
| `results` | array | array de resultados de la análisis. | Saída processada para cada registro de entrada. |
| `results[].id` | string | Identificador del registro. | Vincula a saída ao registro de entrada. |
| `results[].prediction` | float | Pontuação de previsão (0-1). | Pontuação de confiança de la saída del modelo. |
| `metadata` | object | Metadados de processamento. | Informações de diagnóstico y versionamento. |
| `metadata.model_version` | string | Versão del modelo de IA utilizada. | Para reprodutibilidade y rastreamento. |
| `metadata.processing_time_ms` | integer | Duração del processamento em milissegundos. | Métrica de desempenho. |

## Como é Calculado

Predicts future costs using time-series forecasting models (ARIMA, Prophet, ETS) based on historical cost data.

### Algoritmo Principal

The system employs advanced machine learning y statistical techniques tailored for predictive forecasting y planning:

- **Pré-processamento de Dados:** Limpeza, normalização y extração de características
- **Seleção del Modelo:** Seleção automática del algoritmo ótimo baseado en las características de los dados
- **Previsão/Análise:** Aplicação del modelo treinado para gerar insights
- **Pós-Processamento:** Formatação de resultados y aplicação de regras de negócio

### Passos de Processamento

1. **Validação de Entrada:** Verificar formato de dados, tipos y restrições empresariais
2. **Engenharia de Características:** Extrair y transformar características relevantes
3. **Inferência del Modelo:** Aplicar modelos de ML para gerar previsões/classificações
4. **Agregação de Resultados:** Compilar y formatar resultados con metadados
5. **Garantia de Qualidade:** Validar saída contra faixas y restrições esperadas

### Desempenho

- **Tempo de Processamento:** 100-500ms para cargas úteis típicas (1,000-10,000 registros)
- **Taxa de Transferência:** 50-100 requisições por minuto por tenant
- **Precisão:** Dependente del modelo, tipicamente 85-95% em conjuntos de validação
- **Requisitos de Dados:** Varia por Endpoint, mínimo 100-1,000 registros históricos para treinamento

## Status HTTP

| Código | Descripción |
|------|-------------|
| 200  | Sucesso - Análise concluída con sucesso |
| 400  | Bad Request - Parâmetros inválidos o campos obrigatórios faltantes |
| 401  | Unauthorized - Token de autenticação inválido o faltante |
| 403  | Forbidden - Permissões insuficientes o tenant inválido |
| 422  | Unprocessable Entity - Erros de validação en los dados de entrada |
| 429  | Too Many Requests - Limite de taxa excedido |
| 500  | Internal Server Erro - Erro del serviço de IA o tempo esgotado |
| 503  | Service Unavailable - Serviço de IA temporariamente indisponível |

## Erros

**Campos Obrigatórios Faltantes:**
```json
{
  "error": "Validation failed",
  "message": "Required fields missing",
  "details": {
    "data": "The data field is required and must be an array."
  }
}
```

**Formato de Dados Inválido:**
```json
{
  "error": "Invalid format",
  "message": "Data format does not match expected schema.",
  "expected_format": "Array of objects with required fields"
}
```

**Erro del Serviço de IA:**
```json
{
  "error": "Service error",
  "message": "Failed to process request due to AI service error.",
  "retry_after": 60
}
```

## Notas

### Melhores Práticas

- **Qualidade de Dados:** Garantir que os dados de entrada sejam limpos, completos y representativos
- **Tamanho del Lote:** Otimizar tamanhos de lote (1,000-10,000 registros) para o melhor desempenho
- **Tratamento de Erros:** Implementar lógica de retry con backoff exponencial para erros transitórios
- **Monitoramento:** Rastrear tempos de processamento y métricas de precisión em produção

### Otimização de Desempenho

- Usar processamento em lote para conjuntos de dados grees
- Cachear análisiss solicitadas frequentemente queo apropriado
- Minimizar tamanho de la carga útil excluindo campos desnecessários
- Aproveitar compressão para requisições grees (gzip codificação)

### Considerações de Segurança

- Todos os dados são criptografados em trânsito (TLS 1.3) y em repouso
- As chaves de API y segredos devem ser rotacionados a cada 90 dias
- Os logs de auditoria são mantidos por 12 meses
- As políticas de retenção de dados estão em conformidade con GDPR y regulamentações regionais

## Perguntas Frequentes

### Q: Quão precisas são as previsões/análisiss?
**A:** A precisión varia por Endpoint y qualidade de los dados. A mayoria de los modelos alcança 85-95% de precisión em conjuntos de dados de validação. A precisión melhora con dados de entrada de mayor qualidade y conjuntos de dados de treinamento mayores. Monitore a pontuação de `confidence` en las respostas para confiabilidade por previsão.

### Q: Qual é o tamanho máximo de la carga útil?
**A:** O tamanho máximo de la requisição é 20MB (~250,000 registros dependendo de la contagem de campos). Para conjuntos de dados mayores, use processamento em lote o contate o suporte para opções de processamento em massa.

### Q: Com que frequência os modelos são retreinados?
**A:** Os modelos são retreinados mensalmente con dados frescos o queo degradação significativa de precisión é detectada. O retreinamento personalizado de modelos pode ser solicitado através del suporte.

### Q: Posso usar este Endpoint em aplicações em tempo real?
**A:** Sim, os tempos de resposta típicos são 100-500ms. Para casos de uso em tempo real de alto rendimento (>1,000 req/min), contate o suporte para planejamento de capacidade dedicado.

### Q: Como a privacidade de los dados é tratada?
**A:** Todos os dados de clientes são estritamente isolados por tenant. Os dados nunca são compartilhados entre tenants o usados para treinamento de modelos entre tenants. Estamos em conformidade con GDPR, CCPA y regulamentações específicas del setor. Os dados são retidos por 90 dias a menos que especificado de outra forma.

### Q: O que acontece se o serviço de IA estiver indisponível?
**A:** O sistema retorna um status 503 con cabeçalho `retry_after` indiceo queo tentar novamente. Implemente lógica de retry con backoff exponencial (atraso inicial: 1s, máx: 60s). O SLA de disponibilidade del serviço é 99.9% mensal.

## Manuais Comerciais

### Playbook 1: Budget planning y financial forecasting
**Objetivo:** Aproveitar insights de IA para alcançar resultados empresariais mensuráveis.

**Implementação:**
1. Coletar y preparar dados históricos para análisis
2. Enviar dados ao Endpoint con configuração apropriada
3. Analisar resultados y identificar objetivos de alta prioridade
4. Implementar ações empresariais baseadas em insights
5. Monitorar desempenho y iterar en la estratégia

**Resultados Esperados:**
- 20-40% melhoria em métricas empresariais chave
- Custos operacionais reduzidos y eficiência melhorada
- Tomada de decisão baseada em dados
- ROI mensurável dentro de 3-6 meses

### Playbook 2: Deme prediction for inventory management
**Objetivo:** Otimizar processos empresariais useo insights preditivos.

**Implementação:**
1. Identificar métricas chave y critérios de sucesso
2. Integrar Endpoint em fluxos de trabalho existentes
3. Usar previsões para priorizar ações
4. A/B test abordagens impulsionadas por IA vs tradicionais
5. Escalar estratégias bem-sucedidas em toda a organização

**Resultados Esperados:**
- 15-30% aumento em eficiência
- Alocação de recursos melhorada
- Ciclos de decisão mais rápidos
- Vantagem competitiva através de la adoção de IA

### Playbook 3: Revenue y cost projection
**Objetivo:** Impulsionar crescimento de receita através de otimização potencializada por IA.

**Implementação:**
1. Definir métricas de impacto en la receita
2. Implementar insights de IA em canais voltados ao cliente
3. Personalizar experiências baseadas em previsões
4. Rastrear conversão y aumento de receita
5. Refinar continuamente baseado em feedback

**Resultados Esperados:**
- 10-25% aumento de receita
- Pontuações de satisfação del cliente mais altas
- Taxas de conversão melhoradas
- Relacionamentos con clientes mais fortes

### Playbook 4: Capacity planning y resource allocation
**Objetivo:** Alcançar excelência operacional através de IA.

**Implementação:**
1. Estabelecer métricas de linha de base
2. Integrar insights de IA em operações diárias
3. Automatizar tomada de decisão repetitiva
4. Monitorar KPIs y ajustar limites
5. Compartilhar aprendizados entre equipes

**Resultados Esperados:**
- 25-50% redução em esforço manual
- Precisão y consistência melhoradas
- Tempo até insight mais rápido
- Processos escaláveis

## Relacionado

- Os endpoints relacionados serão listados aqui con base en la categoria

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:239`
