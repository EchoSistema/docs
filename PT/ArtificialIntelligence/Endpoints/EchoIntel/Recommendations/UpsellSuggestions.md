# Artificial Intelligence – Upsell Product Suggestions

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/upsell
```

Suggests higher-value product alternatives e upgrades to increase average order value.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). Obrigatório if not configured on the server. |
| X-Secret           | string | Condicional | 64-caracteres de segredo. Obrigatório if not configured on the server. |
| Accept-Language    | string | Não         | Idioma de resposta (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Tipo       | string | Sim         | `application/json`. |

## Parâmetros

> **Note:** Os parâmetros aceitam tanto `snake_case` e `camelCase`.

### Corpo da Requisição

| Parâmetro | Tipo | Obrigatório | Descrição | Significado Empresarial | Padrão |
| --------- | ---- | -------- | ----------- | ---------------- | ------- |
| data | array | Sim | Dados de entrada para análise. | Dados empresariais a serem processados. | - |
| options | object | Não | Opções de configuração do algoritmo. | Parâmetros de personalização para o modelo de ML. | `{}` |
| include_metadata | boolean | Não | Incluir metadados de processamento na resposta. | Adicionar informações de diagnóstico. | `false` |

## Exemplos

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
  "https://echosistema.online/api/v1/ai/echointel/recommendations/upsell"
```

### Exemplo de Requisição (Python)

```python
import requests

url = "https://echosistema.online/api/v1/ai/echointel/recommendations/upsell"
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

## Resposta

### Sucesso `200 OK`

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

### Erro `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required and must contain at least one record."
}
```

### Erro `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid data format",
  "details": {
    "data.0.value": "The value field must be a number."
  }
}
```

## Estrutura JSON

| Campo | Tipo | Descrição | Significado Empresarial |
| ----- | ---- | ----------- | ---------------- |
| `results` | array | array de resultados da análise. | Saída processada para cada registro de entrada. |
| `results[].id` | string | Identificador do registro. | Vincula a saída ao registro de entrada. |
| `results[].prediction` | float | Pontuação de previsão (0-1). | Pontuação de confiança da saída do modelo. |
| `metadata` | object | Metadados de processamento. | Informações de diagnóstico e versionamento. |
| `metadata.model_version` | string | Versão do modelo de IA utilizada. | Para reprodutibilidade e rastreamento. |
| `metadata.processing_time_ms` | integer | Duração do processamento em milissegundos. | Métrica de desempenho. |

## Como é Calculado

Suggests higher-value product alternatives e upgrades to increase average order value.

### Algoritmo Principal

The system employs advanced machine learning e statistical techniques tailored for ai-powered recommendation systems:

- **Pré-processamento de Dados:** Limpeza, normalização e extração de características
- **Seleção do Modelo:** Seleção automática do algoritmo ótimo baseado nas características dos dados
- **Previsão/Análise:** Aplicação do modelo treinado para gerar insights
- **Pós-Processamento:** Formatação de resultados e aplicação de regras de negócio

### Passos de Processamento

1. **Validação de Entrada:** Verificar formato de dados, tipos e restrições empresariais
2. **Engenharia de Características:** Extrair e transformar características relevantes
3. **Inferência do Modelo:** Aplicar modelos de ML para gerar previsões/classificações
4. **Agregação de Resultados:** Compilar e formatar resultados com metadados
5. **Garantia de Qualidade:** Validar saída contra faixas e restrições esperadas

### Desempenho

- **Tempo de Processamento:** 100-500ms para cargas úteis típicas (1,000-10,000 registros)
- **Taxa de Transferência:** 50-100 requisições por minuto por tenant
- **Precisão:** Dependente do modelo, tipicamente 85-95% em conjuntos de validação
- **Requisitos de Dados:** Varia por Endpoint, mínimo 100-1,000 registros históricos para treinamento

## Status HTTP

| Código | Descrição |
|------|-------------|
| 200  | Sucesso - Análise concluída com sucesso |
| 400  | Bad Request - Parâmetros inválidos ou campos obrigatórios faltantes |
| 401  | Unauthorized - Token de autenticação inválido ou faltante |
| 403  | Forbidden - Permissões insuficientes ou tenant inválido |
| 422  | Unprocessable Entity - Erros de validação nos dados de entrada |
| 429  | Too Many Requests - Limite de taxa excedido |
| 500  | Internal Server Erro - Erro do serviço de IA ou tempo esgotado |
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

**Erro do Serviço de IA:**
```json
{
  "error": "Service error",
  "message": "Failed to process request due to AI service error.",
  "retry_after": 60
}
```

## Notas

### Melhores Práticas

- **Qualidade de Dados:** Garantir que os dados de entrada sejam limpos, completos e representativos
- **Tamanho do Lote:** Otimizar tamanhos de lote (1,000-10,000 registros) para o melhor desempenho
- **Tratamento de Erros:** Implementar lógica de retry com backoff exponencial para erros transitórios
- **Monitoramento:** Rastrear tempos de processamento e métricas de precisão em produção

### Otimização de Desempenho

- Usar processamento em lote para conjuntos de dados grees
- Cachear análises solicitadas frequentemente queo apropriado
- Minimizar tamanho da carga útil excluindo campos desnecessários
- Aproveitar compressão para requisições grees (gzip codificação)

### Considerações de Segurança

- Todos os dados são criptografados em trânsito (TLS 1.3) e em repouso
- As chaves de API e segredos devem ser rotacionados a cada 90 dias
- Os logs de auditoria são mantidos por 12 meses
- As políticas de retenção de dados estão em conformidade com GDPR e regulamentações regionais

## Perguntas Frequentes

### Q: Quão precisas são as previsões/análises?
**A:** A precisão varia por Endpoint e qualidade dos dados. A maioria dos modelos alcança 85-95% de precisão em conjuntos de dados de validação. A precisão melhora com dados de entrada de maior qualidade e conjuntos de dados de treinamento maiores. Monitore a pontuação de `confidence` nas respostas para confiabilidade por previsão.

### Q: Qual é o tamanho máximo da carga útil?
**A:** O tamanho máximo da requisição é 20MB (~250,000 registros dependendo da contagem de campos). Para conjuntos de dados maiores, use processamento em lote ou contate o suporte para opções de processamento em massa.

### Q: Com que frequência os modelos são retreinados?
**A:** Os modelos são retreinados mensalmente com dados frescos ou queo degradação significativa de precisão é detectada. O retreinamento personalizado de modelos pode ser solicitado através do suporte.

### Q: Posso usar este Endpoint em aplicações em tempo real?
**A:** Sim, os tempos de resposta típicos são 100-500ms. Para casos de uso em tempo real de alto rendimento (>1,000 req/min), contate o suporte para planejamento de capacidade dedicado.

### Q: Como a privacidade dos dados é tratada?
**A:** Todos os dados de clientes são estritamente isolados por tenant. Os dados nunca são compartilhados entre tenants ou usados para treinamento de modelos entre tenants. Estamos em conformidade com GDPR, CCPA e regulamentações específicas do setor. Os dados são retidos por 90 dias a menos que especificado de outra forma.

### Q: O que acontece se o serviço de IA estiver indisponível?
**A:** O sistema retorna um status 503 com cabeçalho `retry_after` indiceo queo tentar novamente. Implemente lógica de retry com backoff exponencial (atraso inicial: 1s, máx: 60s). O SLA de disponibilidade do serviço é 99.9% mensal.

## Manuais Comerciais

### Playbook 1: Increase average order value through upselling
**Objetivo:** Aproveitar insights de IA para alcançar resultados empresariais mensuráveis.

**Implementação:**
1. Coletar e preparar dados históricos para análise
2. Enviar dados ao Endpoint com configuração apropriada
3. Analisar resultados e identificar alvos de alta prioridade
4. Implementar ações empresariais baseadas em insights
5. Monitorar desempenho e iterar na estratégia

**Resultados Esperados:**
- 20-40% melhoria em métricas empresariais chave
- Custos operacionais reduzidos e eficiência melhorada
- Tomada de decisão baseada em dados
- ROI mensurável dentro de 3-6 meses

### Playbook 2: Improve cross-sell conversion rates
**Objetivo:** Otimizar processos empresariais useo insights preditivos.

**Implementação:**
1. Identificar métricas chave e critérios de sucesso
2. Integrar Endpoint em fluxos de trabalho existentes
3. Usar previsões para priorizar ações
4. A/B test abordagens impulsionadas por IA vs tradicionais
5. Escalar estratégias bem-sucedidas em toda a organização

**Resultados Esperados:**
- 15-30% aumento em eficiência
- Alocação de recursos melhorada
- Ciclos de decisão mais rápidos
- Vantagem competitiva através da adoção de IA

### Playbook 3: Enhance customer discovery e engagement
**Objetivo:** Impulsionar crescimento de receita através de otimização potencializada por IA.

**Implementação:**
1. Definir métricas de impacto na receita
2. Implementar insights de IA em canais voltados ao cliente
3. Personalizar experiências baseadas em previsões
4. Rastrear conversão e aumento de receita
5. Refinar continuamente baseado em feedback

**Resultados Esperados:**
- 10-25% aumento de receita
- Pontuações de satisfação do cliente mais altas
- Taxas de conversão melhoradas
- Relacionamentos com clientes mais fortes

### Playbook 4: Personalize product recommendations
**Objetivo:** Alcançar excelência operacional através de IA.

**Implementação:**
1. Estabelecer métricas de linha de base
2. Integrar insights de IA em operações diárias
3. Automatizar tomada de decisão repetitiva
4. Monitorar KPIs e ajustar limites
5. Compartilhar aprendizados entre equipes

**Resultados Esperados:**
- 25-50% redução em esforço manual
- Precisão e consistência melhoradas
- Tempo até insight mais rápido
- Processos escaláveis

## Relacionado

- Os endpoints relacionados serão listados aqui com base na categoria

## Referências

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:329`
