# ReputationBook – Backoffice Dashboard de Reclamações

## Descrição

Endpoint do backoffice para obter métricas de dashboard do livro de reclamações. Retorna dados agregados com múltiplas visualizações temporais (diária, semanal, quinzenal, mensal e anual) matematicamente consistentes entre si.

## Endpoint

**GET** `/api/v1/backoffice/reputation-book/dashboards/title/{title}`

## Autenticação

Requer autenticação via Bearer Token.

## Permissões Necessárias

O usuário autenticado deve possuir uma das seguintes permissões:
- `show.all`
- `show.dashboards`

## Parâmetros de URL

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| title | string | Sim | Título/identificador do dashboard desejado |

### Valores Possíveis para `title`

- `general-metrics` - Métricas gerais de reclamações
- Outros títulos customizados conforme necessidade

## Resposta de Sucesso

**Código:** `200 OK`

### Estrutura da Resposta

```json
{
  "data": {
    "_id": "ObjectId",
    "domain_area_id": 1,
    "platform_id": null,
    "company_id": null,
    "title": "general-metrics",
    "metrics": {
      "complaint_by_status": {
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
      },
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
      },
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
      },
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
      },
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
    },
    "createdAt": "2024-10-20T00:00:00.000Z",
    "updatedAt": "2024-10-26T23:59:59.000Z"
  }
}
```

## Respostas de Erro

### 404 Not Found - Domain Area não encontrado

**Condição:** A área de domínio "ReputationBook" não foi encontrada no sistema.

```json
{
  "message": "Área de domínio não encontrada"
}
```

### 404 Not Found - Dashboard não encontrado

**Condição:** Nenhum dashboard foi encontrado com o título especificado para a área ReputationBook.

```json
{
  "message": "Dashboard não encontrado"
}
```

### 403 Forbidden

**Condição:** Usuário não possui permissões necessárias (`show.all` ou `show.dashboards`).

```json
{
  "message": "Você não tem permissão para realizar esta ação"
}
```

### 401 Unauthorized

**Condição:** Token de autenticação ausente ou inválido.

```json
{
  "message": "Não autenticado"
}
```

## Estrutura de Métricas

### Coerência Matemática

Todas as visualizações temporais são matematicamente consistentes:
- A soma das 24 horas = total diário
- A soma de 7 dias = total semanal
- A soma de 2 semanas = total quinzenal
- A soma de 4 semanas = total mensal
- O valor de Outubro no anual = total mensal de Outubro

### Tipos de Período

| Tipo | Granularidade | Categorias | Uso |
|------|---------------|------------|-----|
| `daily` | Horas (24h) | 00h-23h | Análise horária de um dia |
| `weekly` | Dias (7d) | Mon-Sun | Análise diária da semana |
| `bi_weekly` | Semanas (2) | Week 1-2 | Comparação quinzenal |
| `monthly` | Semanas (4) | Week 1-4 | Análise semanal do mês |
| `yearly` | Meses (12) | Jan-Dec | Análise mensal do ano |

### Status de Reclamações

| Status | Descrição |
|--------|-----------|
| `solved` | Reclamações resolvidas (amigavelmente ou legalmente) |
| `pending` | Reclamações pendentes (iniciadas, em análise, aprovadas, respondidas) |
| `unresolved` | Reclamações não resolvidas |
| `spam` | Reclamações marcadas como spam |

## Exemplos de Requisição

### cURL

```bash
curl -X GET "https://api.example.com/api/v1/backoffice/reputation-book/dashboards/title/general-metrics" \
  -H "Authorization: Bearer {seu-token}" \
  -H "Accept: application/json"
```

### JavaScript (Fetch)

```javascript
fetch('https://api.example.com/api/v1/backoffice/reputation-book/dashboards/title/general-metrics', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer {seu-token}',
    'Accept': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```

### PHP (Guzzle)

```php
use GuzzleHttp\Client;

$client = new Client();
$response = $client->get('https://api.example.com/api/v1/backoffice/reputation-book/dashboards/title/general-metrics', [
    'headers' => [
        'Authorization' => 'Bearer {seu-token}',
        'Accept' => 'application/json',
    ]
]);

$data = json_decode($response->getBody(), true);
```

## Casos de Uso

### 1. Dashboard Administrativo

Exibir métricas gerais de reclamações com múltiplas visualizações temporais para análise administrativa.

### 2. Relatórios Executivos

Gerar relatórios executivos com dados agregados de diferentes períodos.

### 3. Gráficos Interativos

Criar gráficos interativos (ApexCharts, Chart.js) usando os dados de `chartData.series` e `chartData.categories`.

### 4. KPIs de Desempenho

Monitorar KPIs como taxa de resolução, tempo médio, e variações percentuais entre períodos.

## Notas Importantes

1. **Permissões**: O endpoint verifica se o usuário possui permissão `show.all` OU `show.dashboards`
2. **Domain Area**: O dashboard está vinculado à área de domínio "ReputationBook"
3. **MongoDB**: Os dados são armazenados em MongoDB na collection `dashboards`
4. **Cache**: Considere implementar cache para melhorar performance em consultas frequentes
5. **Dados Pré-calculados**: Todas as métricas são pré-calculadas e armazenadas, não sendo computadas em tempo real

## Relacionados

- [Estrutura de Métricas do ComplaintBook Dashboard](../Dashboards/ComplaintBook.md)
- [Modelo ComplaintBookDashboard](../Models/Mongo/ComplaintBookDashboard.md)
- [Backoffice Complaint Index](./BackofficeComplaintIndex.md)
- [Backoffice Complaint Show](./BackofficeComplaintShow.md)

## Changelog

- 2025-10-26: Documentação inicial do endpoint com estrutura de métricas coerentes
