# Shared – Dashboard Executivo do Backoffice

## Endpoint

```
GET /api/v1/backoffice/executive-dashboard
```

## Autenticação

Requer token Bearer válido.

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Permissões

- Gate `viewExecutiveDashboard`: Requer permissão `backoffice.all` ou ser usuário backoffice
- Usuários backoffice têm acesso automático
- Usuários de plataforma precisam ter a permissão `backoffice.all` em suas roles

## Descrição

Retorna o último snapshot consolidado do Executive Dashboard com visão de saúde do sistema, métricas de negócio e ações rápidas.

**Importante:** Os dados são gerados automaticamente a cada 10 minutos pelo job `ExecutiveDashboardDataGenerateJob`. O endpoint **não gera dados sob demanda** - apenas retorna dados do cache Redis ou da persistência MongoDB mais recente.

## Parâmetros

Este endpoint não possui parâmetros.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/backoffice/executive-dashboard"
```

### Exemplo de resposta

```json
{
  "data": {
    "generated_at": "2025-11-19T14:30:00.000000Z",
    "system_health": {
      "domain_areas": [
        {
          "domain_area": "ecommerce",
          "name": "E-commerce",
          "users": 1250,
          "status": "operational"
        },
        {
          "domain_area": "real-estate",
          "name": "Real Estate",
          "users": 850,
          "status": "operational"
        }
      ],
      "critical_alerts": {
        "total_unread": 3,
        "latest": [
          {
            "uuid": "alert-uuid-1",
            "type": "bug",
            "platform_id": 1,
            "reporter": "João Silva",
            "created_at": "2025-11-19T14:25:00.000000Z"
          }
        ]
      },
      "uptime": {
        "last_24h": null,
        "last_7d": null,
        "last_30d": null
      },
      "resource_utilization": {
        "cpu_percentage": 45.23,
        "memory": {
          "used": 536870912,
          "limit": 2147483648,
          "percentage": 25.0
        },
        "disk": {
          "used": 107374182400,
          "total": 536870912000,
          "percentage": 20.0
        }
      }
    },
    "business_metrics": {
      "active_users": 5420,
      "new_users_month": 320,
      "growth_rate_mom": 6.25,
      "revenue_total_cents": 1500000000,
      "revenue_month_cents": 85000000,
      "platform_distribution": [
        {
          "domain_area": "ecommerce",
          "platforms": ["Loja Principal", "Marketplace"],
          "total": 2
        },
        {
          "domain_area": "real-estate",
          "platforms": ["Imobiliária A", "Imobiliária B"],
          "total": 2
        }
      ]
    },
    "quick_actions": {
      "deploy_status": {
        "stage": "Production",
        "environment": "production",
        "laravel_version": "10.48.0",
        "php_version": "8.3.0"
      },
      "pending_approvals": 12,
      "system_notifications": {
        "error_count": 5,
        "warning_count": 12,
        "critical_count": 1,
        "latest_errors": [
          {
            "level": "ERROR",
            "message": "Connection timeout on external API",
            "timestamp": "2025-11-19T14:20:00.000000Z"
          },
          {
            "level": "CRITICAL",
            "message": "Database connection pool exhausted",
            "timestamp": "2025-11-19T14:15:00.000000Z"
          }
        ]
      }
    }
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data | object | Dados do dashboard executivo |
| data.generated_at | string | Data e hora de geração do snapshot (ISO 8601) |
| data.system_health | object | Métricas de saúde do sistema |
| data.system_health.domain_areas | array | Contagem de usuários por área de domínio |
| data.system_health.domain_areas[].domain_area | string | Slug da área de domínio |
| data.system_health.domain_areas[].name | string | Nome da área de domínio |
| data.system_health.domain_areas[].users | integer | Quantidade de usuários na área |
| data.system_health.domain_areas[].status | string | Status operacional ("operational") |
| data.system_health.critical_alerts | object | Alertas críticos não lidos |
| data.system_health.critical_alerts.total_unread | integer | Total de mensagens não lidas |
| data.system_health.critical_alerts.latest | array | Últimas 5 mensagens não lidas |
| data.system_health.uptime | object | Métricas de uptime (null se não disponível) |
| data.system_health.uptime.last_24h | float\|null | Uptime últimas 24 horas (%) |
| data.system_health.uptime.last_7d | float\|null | Uptime últimos 7 dias (%) |
| data.system_health.uptime.last_30d | float\|null | Uptime últimos 30 dias (%) |
| data.system_health.resource_utilization | object | Utilização de recursos do servidor |
| data.system_health.resource_utilization.cpu_percentage | float | Uso de CPU (%) |
| data.system_health.resource_utilization.memory | object | Uso de memória |
| data.system_health.resource_utilization.memory.used | integer | Memória usada (bytes) |
| data.system_health.resource_utilization.memory.limit | integer | Limite de memória (bytes) |
| data.system_health.resource_utilization.memory.percentage | float | Percentual de memória usada |
| data.system_health.resource_utilization.disk | object | Uso de disco |
| data.system_health.resource_utilization.disk.used | integer | Espaço usado (bytes) |
| data.system_health.resource_utilization.disk.total | integer | Espaço total (bytes) |
| data.system_health.resource_utilization.disk.percentage | float | Percentual de disco usado |
| data.business_metrics | object | Métricas de negócio |
| data.business_metrics.active_users | integer | Total de usuários ativos |
| data.business_metrics.new_users_month | integer | Novos usuários no mês atual |
| data.business_metrics.growth_rate_mom | float | Taxa de crescimento mês a mês (%) |
| data.business_metrics.revenue_total_cents | integer | Receita total em centavos |
| data.business_metrics.revenue_month_cents | integer | Receita do mês em centavos |
| data.business_metrics.platform_distribution | array | Distribuição de plataformas por área |
| data.business_metrics.platform_distribution[].domain_area | string | Área de domínio |
| data.business_metrics.platform_distribution[].platforms | array | Nomes das plataformas |
| data.business_metrics.platform_distribution[].total | integer | Total de plataformas |
| data.quick_actions | object | Ações rápidas e informações do sistema |
| data.quick_actions.deploy_status | object | Status do ambiente de deploy |
| data.quick_actions.deploy_status.stage | string | Estágio (Production/Stage) |
| data.quick_actions.deploy_status.environment | string | Nome do ambiente |
| data.quick_actions.deploy_status.laravel_version | string | Versão do Laravel |
| data.quick_actions.deploy_status.php_version | string | Versão do PHP |
| data.quick_actions.pending_approvals | integer | Total de aprovações pendentes (solicitações de função) |
| data.quick_actions.system_notifications | object | Notificações do sistema (erros de log) |
| data.quick_actions.system_notifications.error_count | integer | Quantidade de erros no log |
| data.quick_actions.system_notifications.warning_count | integer | Quantidade de avisos no log |
| data.quick_actions.system_notifications.critical_count | integer | Quantidade de erros críticos |
| data.quick_actions.system_notifications.latest_errors | array | Últimos 5 erros do sistema |
| data.quick_actions.system_notifications.latest_errors[].level | string | Nível do erro (ERROR/WARNING/CRITICAL) |
| data.quick_actions.system_notifications.latest_errors[].message | string | Mensagem do erro (max 200 chars) |
| data.quick_actions.system_notifications.latest_errors[].timestamp | string\|null | Data e hora do erro (ISO 8601) |

## Status HTTP

- 200: Sucesso - Dashboard recuperado com sucesso
- 401: Não autenticado - Token inválido ou ausente
- 403: Proibido - Permissão `backoffice.all` necessária
- 500: Erro interno

## Erros

### 401 Não autenticado
```json
{
  "message": "Não autenticado."
}
```

### 403 Proibido
```json
{
  "message": "Esta ação não está autorizada."
}
```

## Notas

### Atualização de Dados
- Os dados são gerados a cada **10 minutos** via job agendado (`ExecutiveDashboardDataGenerateJob`)
- Cache mantido por 10 minutos usando Redis
- Persistência em MongoDB para fallback caso cache expire
- Evento `executive-dashboard-updated` é broadcast no canal `backoffice` após cada atualização

### Permissões
- Requer permissão `backoffice.all` ou ser usuário do tipo backoffice
- Validado via Policy `BackofficePolicy::viewExecutiveDashboard()`
- Usuários backoffice (identificados via método `isBackoffice()`) têm acesso automático
- Usuários de plataforma precisam ter `backoffice.all` nas permissões de suas roles

### Fontes de Dados
- **Usuários por Área**: Agregação de `users` → `platforms` → `platform_domain_areas`
- **Alertas Críticos**: Mensagens não lidas em `platform_contact_messages`
- **Notificações do Sistema**: Análise das últimas 500 linhas de `storage/logs/laravel.log`
- **Receita**: Soma de `payment_orders` com `paid_at` não nulo
- **Recursos**: Métricas do servidor via `sys_getloadavg()`, `memory_get_usage()`, `disk_free_space()`

### Uptime Monitoring
- Campos `uptime` retornam `null` até integração com sistema de monitoramento
- Não utilize dados falsos - melhor null do que impreciso

### Performance
- Endpoint otimizado para leitura de cache
- Não gera dados sob demanda (somente via job agendado)
- Queries otimizadas com JOINs e agregações no banco

### WebSocket/Broadcasting
- Frontend pode se inscrever no canal `backoffice` para receber atualizações em tempo real
- Evento: `executive-dashboard-updated`
- Payload do evento broadcast:
  ```json
  {
    "uuid": "platform-uuid",
    "generated_at": "2025-11-19T14:30:00.000000Z"
  }
  ```
- O evento notifica quando novos dados estão disponíveis, permitindo que o frontend recarregue o dashboard
- Útil para dashboards que ficam abertos por longos períodos

## Relacionados

- [Backoffice - Logs do Sistema](./BackofficeLogs.md)
- [Backoffice - Índice de Plataformas](./BackofficePlatformIndex.md)
- [Backoffice - Índice de Usuários](./BackofficePlatformUserIndex.md)

## Changelog

- 2025-11-19: Endpoint criado com métricas de sistema, negócio e ações rápidas
- 2025-11-19: Adicionada análise de logs do sistema para notificações
- 2025-11-19: Implementado agendamento a cada 10 minutos e broadcasting de eventos
