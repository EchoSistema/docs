# Inteligência Artificial – Otimização de Estoque

## Endpoint

```
POST /api/v1/ai/echointel/inventory/optimization
```

Otimiza níveis de estoque utilizando modelos preditivos para minimizar custos e evitar rupturas.

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

| Parâmetro        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| products         | array  | Sim         | Lista de produtos para otimização. |
| lead_time        | int    | Sim         | Tempo de reposição (em dias). |
| holding_cost     | float  | Não         | Custo de manutenção de estoque (%). |
| stockout_cost    | float  | Não         | Custo de ruptura de estoque. |
| service_level    | float  | Não         | Nível de serviço desejado (0-1). Padrão: `0.95`. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "products": [
      {
        "product_id": "SKU-001",
        "current_stock": 150,
        "historical_demand": [45, 52, 48, 55, 50, 58, 53],
        "unit_cost": 25.00
      },
      {
        "product_id": "SKU-002",
        "current_stock": 80,
        "historical_demand": [120, 115, 130, 125, 118, 135, 128],
        "unit_cost": 15.00
      }
    ],
    "lead_time": 7,
    "holding_cost": 0.15,
    "service_level": 0.95
  }' \
  "https://your-domain.com/api/v1/ai/echointel/inventory/optimization"
```

## Resposta

### Sucesso `200 OK`

```json
{
  "optimization_results": [
    {
      "product_id": "SKU-001",
      "current_stock": 150,
      "recommended_stock": 175,
      "reorder_point": 95,
      "economic_order_quantity": 220,
      "safety_stock": 35,
      "action": "increase",
      "quantity_to_order": 25,
      "estimated_savings": 450.00,
      "risk_level": "low",
      "demand_forecast": {
        "next_7_days": 378,
        "std_deviation": 12.5
      }
    },
    {
      "product_id": "SKU-002",
      "current_stock": 80,
      "recommended_stock": 250,
      "reorder_point": 180,
      "economic_order_quantity": 350,
      "safety_stock": 65,
      "action": "urgent_order",
      "quantity_to_order": 170,
      "estimated_savings": 1200.00,
      "risk_level": "high",
      "demand_forecast": {
        "next_7_days": 890,
        "std_deviation": 28.3
      }
    }
  ],
  "summary": {
    "total_potential_savings": 1650.00,
    "products_at_risk": 1,
    "urgent_orders_needed": 1,
    "total_recommended_investment": 6700.00
  }
}
```

## Estrutura JSON

| Campo                                     | Tipo    | Descrição |
| ----------------------------------------- | ------- | --------- |
| `optimization_results`                    | array   | Resultados por produto. |
| `optimization_results[].product_id`       | string  | ID do produto. |
| `optimization_results[].current_stock`    | int     | Estoque atual. |
| `optimization_results[].recommended_stock`| int     | Estoque recomendado. |
| `optimization_results[].reorder_point`    | int     | Ponto de reposição. |
| `optimization_results[].economic_order_quantity` | int | EOQ (Lote Econômico). |
| `optimization_results[].safety_stock`     | int     | Estoque de segurança. |
| `optimization_results[].action`           | string  | Ação recomendada. |
| `optimization_results[].quantity_to_order`| int     | Quantidade a pedir. |
| `optimization_results[].estimated_savings`| float   | Economia estimada. |
| `optimization_results[].risk_level`       | string  | Nível de risco (`low`, `medium`, `high`). |
| `summary`                                 | object  | Resumo geral. |

## Notas

* Ações possíveis: `maintain`, `increase`, `decrease`, `urgent_order`.
* Risco alto indica probabilidade de ruptura de estoque.
* EOQ considera custos de pedido e manutenção.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:261`
