# Formato de Log IA para Grafana

Este documento descreve o formato dos logs gerados pelo sistema de IA (`storage/logs/ia.log`) para facilitar a configuração no Grafana.

## Formato JSON

Todos os logs são gravados em formato JSON (um objeto por linha), facilitando o parsing e análise no Grafana Loki ou outras ferramentas de observabilidade.

## Tipos de Log

### 1. Request Log

Registra todas as requisições enviadas à API EchoIntel.

```json
{
  "message": "EchoIntel API Request",
  "context": {
    "type": "request",
    "endpoint": "customer_segmentation",
    "method": "POST",
    "url": "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation",
    "headers": {
      "Accept": "application/json",
      "Content-Type": "application/json",
      "X-Customer-Api-Id": "***MASKED***",
      "X-Secret": "***MASKED***",
      "Accept-Language": "pt"
    },
    "payload_size": 1024,
    "execution_time": 0.0015,
    "user_id": 123,
    "platform_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "ip": "192.168.1.100",
    "user_agent": "Mozilla/5.0...",
    "timestamp": "2025-01-08T14:30:45+00:00"
  },
  "level": 200,
  "level_name": "INFO",
  "channel": "ia",
  "datetime": "2025-01-08T14:30:45.123456+00:00"
}
```

**Campos importantes para Grafana:**
- `context.type`: Tipo de log (`request`)
- `context.endpoint`: Endpoint chamado
- `context.execution_time`: Tempo de execução parcial (em segundos)
- `context.user_id`: ID do usuário que fez a requisição
- `context.platform_id`: ID da plataforma/tenant

### 2. Response Log

Registra respostas bem-sucedidas da API EchoIntel.

```json
{
  "message": "EchoIntel API Response",
  "context": {
    "type": "response",
    "endpoint": "customer_segmentation",
    "status_code": 200,
    "response_size": 5120,
    "execution_time": 2.3456,
    "success": true,
    "timestamp": "2025-01-08T14:30:47+00:00"
  },
  "level": 200,
  "level_name": "INFO",
  "channel": "ia",
  "datetime": "2025-01-08T14:30:47.456789+00:00"
}
```

**Campos importantes para Grafana:**
- `context.type`: Tipo de log (`response`)
- `context.status_code`: Código HTTP da resposta
- `context.response_size`: Tamanho da resposta em bytes
- `context.execution_time`: Tempo total de execução (em segundos)
- `context.success`: Indica se foi sucesso (true/false)

### 3. Performance Log

Registra métricas de performance para monitoramento.

```json
{
  "message": "EchoIntel Performance Metrics",
  "context": {
    "type": "performance",
    "endpoint": "customer_segmentation",
    "execution_time": 2.3456,
    "status_code": 200,
    "response_size": 5120,
    "is_slow": false,
    "timestamp": "2025-01-08T14:30:47+00:00"
  },
  "level": 200,
  "level_name": "INFO",
  "channel": "ia",
  "datetime": "2025-01-08T14:30:47.456789+00:00"
}
```

**Campos importantes para Grafana:**
- `context.type`: Tipo de log (`performance`)
- `context.execution_time`: Tempo de execução (em segundos)
- `context.is_slow`: Flag indicando requisição lenta (> 5s)
- `context.response_size`: Tamanho da resposta

### 4. Error Log

Registra erros retornados pela API EchoIntel.

```json
{
  "message": "EchoIntel API Error",
  "context": {
    "type": "api_error",
    "endpoint": "customer_segmentation",
    "status_code": 400,
    "response_body": "{\"error\":\"Invalid parameters\",\"message\":\"The data field is required.\"}",
    "execution_time": 0.5678,
    "user_id": 123,
    "platform_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "ip": "192.168.1.100",
    "timestamp": "2025-01-08T14:30:45+00:00"
  },
  "level": 400,
  "level_name": "ERROR",
  "channel": "ia",
  "datetime": "2025-01-08T14:30:45.678901+00:00"
}
```

**Campos importantes para Grafana:**
- `context.type`: Tipo de log (`api_error`)
- `context.status_code`: Código HTTP do erro
- `context.response_body`: Corpo da resposta de erro
- `context.endpoint`: Endpoint que falhou

### 5. Exception Log

Registra exceções durante a comunicação com a API.

```json
{
  "message": "EchoIntel Proxy Exception",
  "context": {
    "type": "exception",
    "endpoint": "customer_segmentation",
    "exception_class": "Illuminate\\Http\\Client\\ConnectionException",
    "exception_message": "Connection timeout",
    "exception_code": 0,
    "file": "/var/www/app/src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php",
    "line": 45,
    "trace": [
      {
        "file": "/var/www/app/vendor/laravel/framework/src/Illuminate/Http/Client/PendingRequest.php",
        "line": 123,
        "function": "send",
        "class": "Illuminate\\Http\\Client\\PendingRequest"
      }
    ],
    "execution_time": 5.0000,
    "user_id": 123,
    "platform_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "ip": "192.168.1.100",
    "timestamp": "2025-01-08T14:30:50+00:00"
  },
  "level": 550,
  "level_name": "CRITICAL",
  "channel": "ia",
  "datetime": "2025-01-08T14:30:50.000000+00:00"
}
```

**Campos importantes para Grafana:**
- `context.type`: Tipo de log (`exception`)
- `context.exception_class`: Classe da exceção
- `context.exception_message`: Mensagem de erro
- `context.file` e `context.line`: Localização do erro

## Queries Sugeridas para Grafana

### Dashboard de Performance

**Tempo médio de resposta por endpoint:**
```logql
avg_over_time({channel="ia"} | json | type="performance" | unwrap execution_time [5m]) by (endpoint)
```

**Total de requisições por endpoint:**
```logql
sum(count_over_time({channel="ia"} | json | type="request" [5m])) by (endpoint)
```

**Taxa de erro:**
```logql
sum(count_over_time({channel="ia"} | json | type="api_error" [5m]))
/
sum(count_over_time({channel="ia"} | json | type="request" [5m]))
```

**Requisições lentas (> 5s):**
```logql
{channel="ia"} | json | type="performance" | is_slow="true"
```

### Dashboard de Erros

**Erros por status code:**
```logql
sum(count_over_time({channel="ia"} | json | type="api_error" [5m])) by (status_code)
```

**Exceções por tipo:**
```logql
sum(count_over_time({channel="ia"} | json | type="exception" [5m])) by (exception_class)
```

**Erros por endpoint:**
```logql
sum(count_over_time({channel="ia"} | json | level_name="ERROR" [5m])) by (endpoint)
```

### Dashboard de Uso

**Requisições por usuário:**
```logql
sum(count_over_time({channel="ia"} | json | type="request" [1h])) by (user_id)
```

**Requisições por plataforma/tenant:**
```logql
sum(count_over_time({channel="ia"} | json | type="request" [1h])) by (platform_id)
```

**Tamanho médio de resposta:**
```logql
avg_over_time({channel="ia"} | json | type="performance" | unwrap response_size [5m])
```

## Configuração de Alertas

### Alerta de Alta Taxa de Erro

```yaml
- alert: HighIAErrorRate
  expr: |
    (
      sum(rate({channel="ia"} | json | type="api_error" [5m]))
      /
      sum(rate({channel="ia"} | json | type="request" [5m]))
    ) > 0.1
  for: 5m
  annotations:
    summary: "Alta taxa de erro na API de IA (> 10%)"
```

### Alerta de Requisições Lentas

```yaml
- alert: SlowIARequests
  expr: |
    sum(rate({channel="ia"} | json | type="performance" | is_slow="true" [5m])) > 5
  for: 2m
  annotations:
    summary: "Múltiplas requisições lentas detectadas (> 5s)"
```

### Alerta de Exceções Críticas

```yaml
- alert: CriticalIAException
  expr: |
    sum(rate({channel="ia"} | json | type="exception" [1m])) > 0
  for: 1m
  annotations:
    summary: "Exceção crítica detectada no serviço de IA"
```

## Rotação de Logs

Os logs são automaticamente rotacionados diariamente e mantidos por 30 dias conforme configuração em `config/logging.php`:

```php
'ia' => [
    'driver' => 'daily',
    'path' => storage_path('logs/ia.log'),
    'days' => 30,
    // ...
]
```

## Notas de Segurança

- Headers sensíveis (`X-Secret`, `Authorization`) são automaticamente mascarados nos logs
- IPs e User Agents são registrados para auditoria
- Stack traces são limitados aos primeiros 5 níveis para reduzir tamanho
- Respostas longas são truncadas em 1000 caracteres

---

**Localização do arquivo de log:** `storage/logs/ia.log` ou `storage/logs/ia-YYYY-MM-DD.log`
