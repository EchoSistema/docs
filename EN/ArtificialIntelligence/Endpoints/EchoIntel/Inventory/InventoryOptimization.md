# Artificial Intelligence – Otimização of Estoque

## Endpoint

```
POST /api/v1/ai/echointel/inventory/optimization
```

Optimizes níveis of estoque using predictive models for minimizar custos and evitar rupturas.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-character secret. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Parameters of the body

| Parameter        | Type   | Required | Description |
| ---------------- | ------ | ----------- | --------- |
| products         | array  | Yes         | List of products for otimização. |
| lead_time        | int    | Yes         | Tempo of reposição (em dias). |
| holding_cost     | float  | No         | Custo of manutenção of estoque (%). |
| stockout_cost    | float  | No         | Custo of ruptura of estoque. |
| service_level    | float  | No         | Level of serviço desejado (0-1). Default: `0.95`. |

## Examples

### Request Example (curl)

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
  "https://echosistema.online/api/v1/ai/echointel/inventory/optimization"
```

## Response

### Success `200 OK`

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

## JSON Structure

| Field                                     | Type    | Description |
| ----------------------------------------- | ------- | --------- |
| `optimization_results`                    | array   | Resultados por product. |
| `optimization_results[].product_id`       | string  | ID of product. |
| `optimization_results[].current_stock`    | int     | Estoque atual. |
| `optimization_results[].recommended_stock`| int     | Estoque recomendado. |
| `optimization_results[].reorder_point`    | int     | Ponto of reposição. |
| `optimization_results[].economic_order_quantity` | int | EOQ (Lote Econômico). |
| `optimization_results[].safety_stock`     | int     | Estoque of segurança. |
| `optimization_results[].action`           | string  | Ação recomendada. |
| `optimization_results[].quantity_to_order`| int     | Quantidade to pedir. |
| `optimization_results[].estimated_savings`| float   | Economia estimada. |
| `optimization_results[].risk_level`       | string  | Level of risco (`low`, `medium`, `high`). |
| `summary`                                 | object  | Resumo geral. |

## HTTP Status

| Status Code | Description |
|-------------|-------------|
| 200 OK | Request successful. Returns inventory optimization results. |
| 400 Bad Request | Invalid request parameters. Check parameter types and required fields. |
| 401 Unauthorized | Missing or invalid Bearer token. |
| 403 Forbidden | Valid token but insufficient permissions. |
| 422 Unprocessable Entity | Request validation failed. See response for details. |
| 429 Too Many Requests | Rate limit exceeded. Retry after cooldown period. |
| 500 Internal Server Error | Server error. Contact support if persistent. |
| 503 Service Unavailable | AI service temporarily unavailable. Retry with exponential backoff. |

## Errors

### Common Error Responses

#### Missing Required Parameters
```json
{
  "error": "Validation failed",
  "message": "Required parameter 'data' is missing",
  "code": "MISSING_PARAMETER",
  "details": {
    "parameter": "data",
    "location": "body"
  }
}
```

**Solution:** Ensure all required parameters are provided in the request body.

#### Invalid Authentication
```json
{
  "error": "Unauthorized",
  "message": "Invalid or expired authentication token",
  "code": "AUTH_FAILED"
}
```

**Solution:** Verify Bearer token is valid and not expired. Check `X-Customer-Api-Id` and `X-Secret` headers.

## Notes

* Ações possíveis: `maintain`, `increase`, `decrease`, `urgent_order`.
* Risco alto indica probabilidade of ruptura of estoque.
* EOQ considera custos of pedido and manutenção.

## How It Is Computed

The Inventory Optimization endpoint applies mathematical optimization algorithms to determine optimal inventory levels that minimize total costs while maintaining desired service levels.

### 1. Demand Forecasting

**Historical Demand Analysis:**
- Calculate average daily/weekly demand: `μ_demand = Σ(demand_i) / n`
- Compute demand standard deviation: `σ_demand = √(Σ(demand_i - μ)² / (n-1))`
- Identify demand distribution (Normal, Poisson, or empirical)
- Seasonality adjustment using seasonal indices

**Forecast Methods:**
- Simple Moving Average (SMA): `SMA = (D₁ + D₂ + ... + Dₙ) / n`
- Exponential Smoothing: `F_t+1 = α × D_t + (1-α) × F_t`
- Holt-Winters for trend and seasonality
- Machine learning models (ARIMA, Prophet, LSTM) for complex patterns

### 2. Economic Order Quantity (EOQ) Calculation

**Classic EOQ Formula:**
```
EOQ = √((2 × D × S) / H)
```
Where:
- **D** = Annual demand (units/year)
- **S** = Ordering cost per order ($)
- **H** = Holding cost per unit per year ($ or % of unit cost)

**Total Cost Calculation:**
```
TC = (D/Q) × S + (Q/2) × H + D × C
```
Where:
- **Q** = Order quantity
- **C** = Unit cost
- **TC** = Total cost (ordering + holding + purchasing)

**EOQ Variations:**
- **EPQ (Economic Production Quantity):** For manufacturing scenarios
- **Quantity Discounts:** Adjusts EOQ when bulk pricing available
- **Backorder Model:** Allows planned stockouts with penalty costs

### 3. Safety Stock Calculation

**Service Level Approach:**
```
Safety Stock = Z × σ_demand × √(Lead Time)
```
Where:
- **Z** = Z-score for desired service level (e.g., 1.65 for 95%, 1.96 for 97.5%)
- **σ_demand** = Standard deviation of demand
- **Lead Time** = Replenishment time in same units as demand

**Fixed Period Approach:**
```
Safety Stock = Z × σ_demand × √(Lead Time + Review Period)
```

**Advanced Methods:**
- **Demand-Supply Variability:** `SS = Z × √((LT × σ_d²) + (μ_d² × σ_LT²))`
- **MAD (Mean Absolute Deviation):** `SS = MAD × Z × √(Lead Time)`
- **Simulation-based:** Monte Carlo for complex demand patterns

### 4. Reorder Point (ROP) Determination

**Basic ROP Formula:**
```
ROP = (Average Daily Demand × Lead Time) + Safety Stock
```

**Dynamic ROP:**
- Adjusts for seasonal variations
- Incorporates supply chain reliability factors
- Updates based on recent demand patterns

**Multi-Echelon Considerations:**
- Downstream demand variability
- Network inventory positioning
- Bullwhip effect mitigation

### 5. Optimization Objective Function

**Cost Minimization:**
```
Minimize: TC = Ordering Cost + Holding Cost + Stockout Cost + Obsolescence Cost

Subject to:
- Service Level ≥ Target Service Level
- Inventory ≥ Safety Stock
- Cash Flow ≤ Budget Constraint
- Warehouse Capacity ≤ Max Capacity
```

**Multi-Objective Optimization:**
- Pareto frontier analysis for cost vs. service level tradeoffs
- Weighted sum method: `Objective = w₁×Cost + w₂×(1-ServiceLevel) + w₃×Cash`
- Constraint handling using penalty functions or barrier methods

**Optimization Algorithms:**
- **Linear Programming (LP):** For simple linear constraints
- **Mixed-Integer Programming (MIP):** When quantities must be integers
- **Genetic Algorithms:** For complex non-linear scenarios
- **Gradient Descent:** For continuous optimization problems

### 6. Risk Assessment

**Risk Level Classification:**
- **High Risk:** Current stock < Safety Stock OR ROP already triggered
- **Medium Risk:** Safety Stock ≤ Stock < ROP
- **Low Risk:** Stock > ROP with adequate buffer

**Risk Metrics:**
- **Stockout Probability:** P(Demand during Lead Time > Current Stock)
- **Expected Shortfall:** Average units short when stockout occurs
- **Service Level:** `SL = 1 - (Expected Stockouts / Total Demand)`

### 7. Action Recommendations

**Decision Rules:**
- **Urgent Order:** Stock < Safety Stock (immediate replenishment needed)
- **Regular Order:** Stock < ROP but > Safety Stock (normal reorder)
- **Increase:** Demand trending up, adjust ROP and EOQ upward
- **Decrease:** Excess inventory or declining demand, reduce order quantities
- **Maintain:** Stock levels optimal, no action required

**Order Quantity Calculation:**
```
Order Quantity = max(EOQ, ROP + EOQ - Current Stock)
```

### 8. Performance Characteristics

- **Processing Time:** 400-900ms per product, 2-5s for batch of 50 products
- **Optimization Accuracy:**
  - Cost Reduction: 15-25% vs. ad-hoc ordering
  - Service Level Achievement: 95-99% target hit rate
  - Forecast MAPE: 12-18% for stable demand, 20-30% for volatile items
- **Scalability:** Handles 500+ SKUs per request with parallel processing
- **Update Frequency:** Recommendations updated daily or on-demand
- **Model Retraining:** Quarterly or when MAE exceeds threshold (>20%)

## Typical Workflow

### 1. Initial Setup
- Configure inventory parameters (lead times, costs, service levels)
- Import historical sales and inventory data (minimum 6-12 months)
- Define product hierarchies and relationships

### 2. Data Preparation
- System validates and cleanses input data
- Identifies missing values and outliers
- Aggregates demand history at appropriate granularity

### 3. Optimization Execution
- Call endpoint with current inventory levels and product list
- System computes EOQ, safety stock, and ROP for each product
- Risk assessment performed based on current stock vs. optimal levels

### 4. Review Recommendations
- Review optimization results: recommended stock levels, order quantities, actions
- Analyze risk levels and prioritize urgent orders
- Evaluate estimated cost savings and investment requirements

### 5. Implementation
- Generate purchase orders for recommended quantities
- Update inventory management system with new parameters
- Schedule orders based on lead times and priorities

### 6. Monitoring and Adjustment
- Track actual vs. forecasted demand
- Monitor service levels and stockout occurrences
- Re-run optimization periodically (weekly/monthly) to adapt to changing conditions
- Fine-tune parameters (holding costs, service levels) based on business needs

## Related

### Related Endpoints

- **[Inventory History Improved](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/InventoryHistoryImproved.md)** - Historical analysis feeding optimization
- **[Excess Inventory NLP](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Inventory/ExcessInventoryNlp.md)** - Identifies items to reduce
- **[Demand Forecasting](/docs/EN/ArtificialIntelligence/Endpoints/EchoIntel/Forecasting/)** - Future demand predictions

### Related Domain Concepts

- **Operations Research:** EOQ, safety stock, reorder point optimization
- **Supply Chain Management:** Lead time management, service levels, inventory policies
- **Cost Accounting:** Holding costs, ordering costs, stockout costs
- **Statistical Forecasting:** Demand prediction, uncertainty quantification

### Integration Points

- **ERP Systems:** Import product data, costs, and demand history
- **Procurement Systems:** Generate automated purchase orders
- **Warehouse Management Systems:** Update stock levels and reorder points
- **Financial Systems:** Track inventory carrying costs and cash flow impact

### Key Business Metrics

- **Total Inventory Cost Reduction:** 15-25% typical savings
- **Service Level Improvement:** Maintain 95-98% fill rates
- **Cash Flow Optimization:** Reduce capital tied up in excess inventory
- **Warehouse Utilization:** Optimize storage space usage

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:261`
