# Inteligência Artificial – Recomendação de Itens para Usuário

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/user-items
```

Gera recomendações personalizadas de produtos/itens para usuários específicos baseado em collaborative filtering e content-based filtering.

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

| Parâmetro     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| user_id       | string | Sim         | ID do usuário. |
| n_recommendations | int | Não       | Número de recomendações. Padrão: `10`. |
| exclude_purchased | boolean | Não   | Excluir itens já comprados. Padrão: `true`. |
| filters       | object | Não         | Filtros adicionais (categoria, preço, etc.). |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "U001",
    "n_recommendations": 5,
    "exclude_purchased": true,
    "filters": {
      "category": "electronics",
      "max_price": 1000
    }
  }' \
  "https://your-domain.com/api/v1/ai/echointel/recommendations/user-items"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "user_id": "U001",
  "recommendations": [
    {
      "item_id": "ITEM-789",
      "score": 0.94,
      "rank": 1,
      "reason": "Based on your recent purchases and similar users",
      "item_details": {
        "name": "Wireless Headphones Pro",
        "category": "electronics",
        "price": 299.99
      }
    },
    {
      "item_id": "ITEM-456",
      "score": 0.88,
      "rank": 2,
      "reason": "Frequently bought together with your previous items",
      "item_details": {
        "name": "Smartphone Case Premium",
        "category": "accessories",
        "price": 49.99
      }
    }
  ],
  "algorithm_used": "hybrid_collaborative_content",
  "confidence": 0.91
}
```

## Estrutura JSON

| Campo                              | Tipo    | Descrição |
| ---------------------------------- | ------- | --------- |
| `user_id`                          | string  | ID do usuário. |
| `recommendations`                  | array   | Lista de recomendações. |
| `recommendations[].item_id`        | string  | ID do item recomendado. |
| `recommendations[].score`          | float   | Score de relevância (0-1). |
| `recommendations[].rank`           | int     | Ranking da recomendação. |
| `recommendations[].reason`         | string  | Razão da recomendação. |
| `recommendations[].item_details`   | object  | Detalhes do item. |
| `algorithm_used`                   | string  | Algoritmo utilizado. |
| `confidence`                       | float   | Confiança geral (0-1). |

## Notas

* Recomendações são ordenadas por score decrescente.
* Algoritmos disponíveis: `collaborative`, `content_based`, `hybrid`.
* Recomendações são atualizadas em tempo real conforme novas interações.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:192`
