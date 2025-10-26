# ReputationBook – Dashboard de Reclamações

## Descrição

Este documento descreve a estrutura de métricas armazenadas no MongoDB para dashboards de reclamações do ReputationBook. Os dados são armazenados na collection `dashboards` do database `reputation-book`.

## Model

**Namespace**: `Domain\ReputationBook\Models\Mongo\ComplaintBookDashboard`

**Collection**: `dashboards`

**Database**: `reputation-book`

## Estrutura do Campo `metrics`

O campo `metrics` é um objeto JSON que contém diferentes tipos de métricas agrupadas por categorias. Cada métrica possui sua própria estrutura de dados.

---

## Estrutura de Métricas Coerentes

O objeto `metrics` contém múltiplas visões de tempo (diária, semanal, quinzenal, mensal e anual) que são matematicamente consistentes entre si. Por exemplo:
- A soma das 24 horas de um dia equivale ao total diário
- A soma de 7 dias equivale ao total semanal
- A soma de 2 semanas equivale ao total quinzenal
- A soma de 4 semanas equivale ao total mensal
- O valor de Outubro no gráfico anual é igual ao total mensal de Outubro

## Métricas Disponíveis

### 1. Visão Diária (`daily`)

Métrica com quebra por hora de um dia específico (24 horas).

#### Estrutura

```json
{
  "daily": {
    "period": {
      "type": "daily",
      "categories": ["00h", "01h", "02h", "03h", "04h", "05h", "06h", "07h", "08h", "09h", "10h", "11h", "12h", "13h", "14h", "15h", "16h", "17h", "18h", "19h", "20h", "21h", "22h", "23h"],
      "startDate": "2024-10-20",
      "endDate": "2024-10-20"
    },
    "summary": {
      "totalComplaints": 982,
      "averageHourlySolved": 14,
      "dailyChange": "+5%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 350,
        "percentage": 35.6
      },
      "pending": {
        "total": 800,
        "percentage": 48.3
      },
      "unresolved": {
        "total": 320,
        "percentage": 12.6
      },
      "spam": {
        "total": 180,
        "percentage": 3.5
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [8, 12, 15, 18, 22, 25, 28, 30, 32, 28, 25, 22, 20, 18, 15, 12, 10, 8, 6, 5, 4, 3, 2, 1]
        },
        {
          "name": "pending",
          "data": [25, 30, 35, 40, 42, 45, 48, 50, 52, 50, 48, 45, 42, 40, 38, 35, 32, 30, 28, 25, 22, 20, 18, 15]
        },
        {
          "name": "unresolved",
          "data": [10, 12, 14, 15, 16, 18, 20, 22, 24, 22, 20, 18, 16, 15, 14, 12, 10, 9, 8, 7, 6, 5, 4, 3]
        },
        {
          "name": "spam",
          "data": [5, 6, 7, 8, 9, 10, 11, 12, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 2, 1, 1, 1]
        }
      ],
      "categories": ["00h", "01h", "02h", "03h", "04h", "05h", "06h", "07h", "08h", "09h", "10h", "11h", "12h", "13h", "14h", "15h", "16h", "17h", "18h", "19h", "20h", "21h", "22h", "23h"]
    }
  }
}
```

### 2. Visão Semanal (`weekly`)

Métrica com quebra por dia da semana (7 dias).

#### Estrutura

```json
{
  "weekly": {
    "period": {
      "type": "week",
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      "startDate": "2024-10-20",
      "endDate": "2024-10-26"
    },
    "summary": {
      "totalComplaints": 11600,
      "averageDailySolved": 357,
      "weeklyChange": "+12%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 2500,
        "percentage": 21.6
      },
      "pending": {
        "total": 5600,
        "percentage": 48.3
      },
      "unresolved": {
        "total": 2240,
        "percentage": 19.3
      },
      "spam": {
        "total": 1260,
        "percentage": 10.9
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [350, 340, 360, 370, 355, 345, 380]
        },
        {
          "name": "pending",
          "data": [800, 780, 820, 810, 790, 770, 830]
        },
        {
          "name": "unresolved",
          "data": [320, 310, 330, 325, 315, 305, 335]
        },
        {
          "name": "spam",
          "data": [180, 170, 190, 185, 175, 165, 195]
        }
      ],
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
    }
  }
}
```

### 3. Visão Quinzenal (`bi_weekly`)

Métrica com quebra por semana (2 semanas).

#### Estrutura

```json
{
  "bi_weekly": {
    "period": {
      "type": "bi_weekly",
      "categories": ["Week 1", "Week 2"],
      "startDate": "2024-10-20",
      "endDate": "2024-11-02"
    },
    "summary": {
      "totalComplaints": 23590,
      "averageWeeklySolved": 2535,
      "biWeeklyChange": "+10%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 5070,
        "percentage": 21.5
      },
      "pending": {
        "total": 11360,
        "percentage": 48.1
      },
      "unresolved": {
        "total": 4560,
        "percentage": 19.3
      },
      "spam": {
        "total": 2600,
        "percentage": 11.0
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [2500, 2570]
        },
        {
          "name": "pending",
          "data": [5600, 5760]
        },
        {
          "name": "unresolved",
          "data": [2240, 2320]
        },
        {
          "name": "spam",
          "data": [1260, 1340]
        }
      ],
      "categories": ["Week 1", "Week 2"]
    }
  }
}
```

### 4. Visão Mensal (`monthly`)

Métrica com quebra por semana do mês (4 semanas).

#### Estrutura

```json
{
  "monthly": {
    "period": {
      "type": "month",
      "categories": ["Week 1", "Week 2", "Week 3", "Week 4"],
      "startDate": "2024-10-01",
      "endDate": "2024-10-31"
    },
    "summary": {
      "totalComplaints": 46500,
      "averageWeeklySolved": 2500,
      "monthlyChange": "+8%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 10000,
        "percentage": 21.5
      },
      "pending": {
        "total": 22400,
        "percentage": 48.2
      },
      "unresolved": {
        "total": 9000,
        "percentage": 19.4
      },
      "spam": {
        "total": 5100,
        "percentage": 11.0
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [2500, 2570, 2450, 2480]
        },
        {
          "name": "pending",
          "data": [5600, 5760, 5500, 5540]
        },
        {
          "name": "unresolved",
          "data": [2240, 2320, 2200, 2240]
        },
        {
          "name": "spam",
          "data": [1260, 1340, 1230, 1270]
        }
      ],
      "categories": ["Week 1", "Week 2", "Week 3", "Week 4"]
    }
  }
}
```

### 5. Visão Anual (`yearly`)

Métrica com quebra por mês do ano (12 meses). **Importante**: O valor de Outubro (índice 9) é igual ao total mensal.

#### Estrutura

```json
{
  "yearly": {
    "period": {
      "type": "year",
      "categories": ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
      "startDate": "2024-01-01",
      "endDate": "2024-12-31"
    },
    "summary": {
      "totalComplaints": 241400,
      "averageMonthlySolved": 9083,
      "yearlyChange": "+15%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 109000,
        "percentage": 45.2
      },
      "pending": {
        "total": 241400,
        "percentage": 48.2
      },
      "unresolved": {
        "total": 98500,
        "percentage": 19.4
      },
      "spam": {
        "total": 54300,
        "percentage": 11.0
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [8200, 8400, 8600, 8800, 9000, 9200, 9100, 9000, 8900, 10000, 9500, 9800]
        },
        {
          "name": "pending",
          "data": [18400, 18800, 19200, 19600, 20000, 20400, 20200, 20000, 19800, 22400, 21000, 21600]
        },
        {
          "name": "unresolved",
          "data": [7400, 7600, 7800, 8000, 8200, 8400, 8300, 8200, 8100, 9000, 8600, 8900]
        },
        {
          "name": "spam",
          "data": [4200, 4300, 4400, 4500, 4600, 4700, 4650, 4600, 4550, 5100, 4800, 4900]
        }
      ],
      "categories": ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
    }
  }
}
```

#### Campos Detalhados

##### `period` - Informações do Período

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| type | string | Tipo do período (week, month, year, custom) |
| categories | array | Labels das categorias/datas do período |
| startDate | string | Data inicial do período (formato: YYYY-MM-DD) |
| endDate | string | Data final do período (formato: YYYY-MM-DD) |

**Valores possíveis para `type`**:
- `daily` - Período diário (24 horas)
- `week` - Período semanal (7 dias)
- `bi_weekly` - Período quinzenal (2 semanas)
- `month` - Período mensal (4 semanas ou 30/31 dias)
- `year` - Período anual (12 meses)

##### `summary` - Resumo Geral

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| totalComplaints | integer | Total de reclamações no período |
| averageWeeklySolved | integer | Média de reclamações resolvidas por semana |
| weeklyChange | string | Variação percentual em relação à semana anterior (ex: "+12%", "-5%") |

##### `statusBreakdown` - Distribuição por Status

Objeto que contém a contagem e percentual para cada status de reclamação.

**Status disponíveis**:

| Status | Campo | Descrição |
| ------ | ----- | --------- |
| Resolvidas | solved | Reclamações solucionadas |
| Pendentes | pending | Reclamações aguardando resposta/ação |
| Não Resolvidas | unresolved | Reclamações que não foram resolvidas |
| Spam | spam | Reclamações marcadas como spam |

**Estrutura de cada status**:

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| total | integer | Total de reclamações neste status |
| percentage | float | Percentual em relação ao total |

##### `chartData` - Dados para Gráfico

Estrutura preparada para visualização em gráficos (compatível com ApexCharts, Chart.js, etc).

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| series | array | Array de séries de dados |
| series[].name | string | Nome da série (status) |
| series[].data | array | Array de valores numéricos correspondentes às categorias |
| categories | array | Labels do eixo X (dias, meses, etc) |

---

## Tipos de Período

### Semanal (`week`)
- **categories**: `["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]`
- **Duração**: 7 dias
- **Uso**: Análise diária da semana

### Mensal (`month`)
- **categories**: `["Week 1", "Week 2", "Week 3", "Week 4"]` ou `["1", "2", ..., "31"]`
- **Duração**: 1 mês
- **Uso**: Análise semanal ou diária do mês

### Anual (`year`)
- **categories**: `["Jan", "Feb", "Mar", ..., "Dec"]`
- **Duração**: 12 meses
- **Uso**: Análise mensal do ano

### Customizado (`custom`)
- **categories**: Definido pelo usuário
- **Duração**: Período personalizado
- **Uso**: Análises específicas com datas customizadas

---

## Status de Reclamações

### Ciclo de Vida

1. **Initiated** - Reclamação iniciada
2. **Pending** - Aguardando análise/resposta
3. **Approved** - Aprovada para publicação
4. **Replied** - Empresa respondeu
5. **Solved** (Amicably/Legally) - Resolvida
6. **Unresolved** - Não resolvida
7. **Spam** - Marcada como spam
8. **Archived** - Arquivada
9. **Canceled** - Cancelada

### Agrupamento para Dashboard

- **Solved**: `amicably_resolved`, `legally_resolved`
- **Pending**: `initiated`, `pending`, `approved`, `replied`
- **Unresolved**: `unresolved`
- **Spam**: `spam`

---

## Cálculos e Fórmulas

### Percentual por Status

```
percentage = (total_status / total_complaints) * 100
```

### Variação Semanal

```
weeklyChange = ((current_week - previous_week) / previous_week) * 100
```

Exemplo:
- Semana Anterior: 1340 resolvidas
- Semana Atual: 1500 resolvidas
- Variação: `((1500 - 1340) / 1340) * 100 = +11.94%` ≈ `+12%`

### Média Semanal

```
averageWeeklySolved = sum(solved_per_week) / total_weeks
```

---

## Casos de Uso

### 1. Gráfico de Área Empilhada

Mostra a evolução de cada status ao longo do tempo de forma empilhada.

**Dados**: `chartData.series`

**Eixo X**: `chartData.categories`

**Eixo Y**: Número de reclamações

### 2. Gráfico de Pizza/Donut

Mostra a distribuição atual por status.

**Dados**: `statusBreakdown` (usar os valores `total` ou `percentage`)

**Labels**: Nomes dos status (solved, pending, unresolved, spam)

### 3. Cards de Resumo

Exibir métricas principais em cards no dashboard.

**Dados**: `summary`
- Total de Reclamações
- Média Semanal Resolvida
- Variação da Semana

### 4. Tabela de Status

Lista detalhada de cada status com total e percentual.

**Dados**: `statusBreakdown`

---

## Exemplo de Consulta MongoDB

### Buscar Dashboard da Semana Atual

```javascript
db.dashboards.findOne({
  "metrics.complaint_by_status.period.type": "week",
  "metrics.complaint_by_status.period.startDate": "2024-10-20",
  "metrics.complaint_by_status.period.endDate": "2024-10-26"
})
```

### Buscar Dashboard por Plataforma

```javascript
db.dashboards.findOne({
  "platform_id": 1,
  "period": "2024-10-20_2024-10-26"
})
```

### Agregação de Múltiplas Plataformas

```javascript
db.dashboards.aggregate([
  {
    $match: {
      "metrics.complaint_by_status.period.type": "week",
      "createdAt": { $gte: ISODate("2024-10-20") }
    }
  },
  {
    $group: {
      _id: "$metrics.complaint_by_status.period.type",
      totalComplaints: { $sum: "$metrics.complaint_by_status.summary.totalComplaints" },
      totalSolved: { $sum: "$metrics.complaint_by_status.statusBreakdown.solved.total" }
    }
  }
])
```

---

## Boas Práticas

### 1. Atualização de Dados
- Atualizar métricas diariamente via cron/job
- Manter histórico de períodos anteriores
- Usar `updateOrCreate` para evitar duplicatas

### 2. Performance
- Indexar por `platform_id`, `company_id` e `period`
- Armazenar dados pré-calculados
- Evitar cálculos em tempo real no frontend

### 3. Versionamento
- Incluir campo `version` para migração de estrutura
- Manter compatibilidade retroativa
- Documentar mudanças de estrutura

### 4. Validação
- Validar que `categories` e `series[].data` tenham mesmo tamanho
- Garantir que percentuais somem ~100%
- Verificar datas de início/fim do período

---

## Estrutura Completa do Document

```json
{
  "_id": "ObjectId",
  "platform_id": 1,
  "company_id": null,
  "period": "2024-10-20_2024-10-26",
  "metrics": {
    "complaint_by_status": {
      "period": { ... },
      "summary": { ... },
      "statusBreakdown": { ... },
      "chartData": { ... }
    },
    "complaint_by_company": { ... },
    "complaint_resolution_time": { ... },
    "complaint_satisfaction": { ... }
  },
  "createdAt": "2024-10-20T00:00:00.000Z",
  "updatedAt": "2024-10-26T23:59:59.000Z"
}
```

---

## Métricas Futuras (Planejadas)

### `complaint_by_company`
Distribuição de reclamações por empresa no período.

### `complaint_resolution_time`
Tempo médio de resolução de reclamações por status.

### `complaint_satisfaction`
Índice de satisfação dos usuários com as resoluções.

### `complaint_by_category`
Distribuição de reclamações por categoria de produto/serviço.

### `complaint_by_platform`
Análise comparativa entre diferentes plataformas.

---

## Relacionados

- [ComplaintBookDashboard Model](../Models/Mongo/ComplaintBookDashboard.md)
- [Complaint Model](../Models/Complaint.md)
- [Dashboard API Endpoints](../Endpoints/BackofficeComplaintBookDashboard.md)

## Changelog

- 2025-10-26: Documentação inicial com estrutura de `complaint_by_status`
