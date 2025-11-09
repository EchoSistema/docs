# Inteligência Artificial – Segmentação de Clientes

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segmentation
```

Realiza análise de segmentação de clientes utilizando técnicas de machine learning para identificar grupos de clientes com características similares.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). Requerido se não configurado no servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido se não configurado no servidor. |
| Accept-Language    | string | Não         | Idioma da resposta (`en`, `es`, `pt`). Padrão: `en`. |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

Os parâmetros variam conforme a necessidade do algoritmo de segmentação. Consulte a documentação da API EchoIntel para detalhes específicos.

| Parâmetro    | Tipo   | Obrigatório | Descrição |
| ------------ | ------ | ----------- | --------- |
| data         | array  | Sim         | Array de dados dos clientes para segmentação. |
| algorithm    | string | Não         | Algoritmo a ser utilizado (ex.: `kmeans`, `hierarchical`). |
| n_clusters   | int    | Não         | Número de clusters desejados. |
| features     | array  | Não         | Features específicas para análise. |

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
    "data": [
      {"customer_id": "123", "total_purchases": 1500, "frequency": 12},
      {"customer_id": "456", "total_purchases": 3000, "frequency": 24}
    ],
    "algorithm": "kmeans",
    "n_clusters": 3
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'pt',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    data: [
      {customer_id: '123', total_purchases: 1500, frequency: 12},
      {customer_id: '456', total_purchases: 3000, frequency: 24}
    ],
    algorithm: 'kmeans',
    n_clusters: 3
  })
});

const result = await response.json();
console.log(result);
```

### Exemplo de requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'pt',
])->post('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', [
    'data' => [
        ['customer_id' => '123', 'total_purchases' => 1500, 'frequency' => 12],
        ['customer_id' => '456', 'total_purchases' => 3000, 'frequency' => 24],
    ],
    'algorithm' => 'kmeans',
    'n_clusters' => 3,
]);

$result = $response->json();
```

## Resposta

### Sucesso `200 OK`

```json
{
  "segments": [
    {
      "segment_id": 0,
      "size": 150,
      "characteristics": {
        "avg_purchases": 1200,
        "avg_frequency": 10
      },
      "customers": ["123", "789"]
    },
    {
      "segment_id": 1,
      "size": 200,
      "characteristics": {
        "avg_purchases": 3500,
        "avg_frequency": 25
      },
      "customers": ["456"]
    }
  ],
  "metrics": {
    "silhouette_score": 0.72,
    "davies_bouldin_score": 0.45
  }
}
```

### Erro `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required."
}
```

### Erro `401 Unauthorized`

```json
{
  "error": "Unauthorized",
  "message": "Invalid authentication credentials"
}
```

### Erro `500 Internal Server Error`

```json
{
  "error": "Failed to communicate with AI service",
  "message": "Internal server error"
}
```

## Estrutura JSON

| Campo                              | Tipo    | Descrição |
| ---------------------------------- | ------- | --------- |
| `segments`                         | array   | Array de segmentos identificados. |
| `segments[].segment_id`            | int     | Identificador do segmento. |
| `segments[].size`                  | int     | Número de clientes no segmento. |
| `segments[].characteristics`       | object  | Características médias do segmento. |
| `segments[].customers`             | array   | IDs dos clientes no segmento. |
| `metrics`                          | object  | Métricas de qualidade da segmentação. |
| `metrics.silhouette_score`         | float   | Score de silhueta (0-1, maior é melhor). |
| `metrics.davies_bouldin_score`     | float   | Score Davies-Bouldin (menor é melhor). |

## Notas

* O secret deve ser rotacionado a cada 90 dias conforme política de segurança.
* O tempo de processamento pode variar conforme o volume de dados (timeout: 300 segundos).
* Os headers `X-Customer-Api-Id` e `X-Secret` podem ser configurados no servidor via `.env`.
* A resposta pode variar conforme a API EchoIntel. Consulte a documentação oficial para detalhes específicos.

## Como é Calculado

O sistema usa clustering algorithms (K-means, hierarchical clustering) para group similar customers or items into segments.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:130`
