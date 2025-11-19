# Backoffice – Logs da Aplicação

## Endpoints

```
GET /api/v1/backoffice/logs              # lista arquivos de log disponíveis
GET /api/v1/backoffice/logs/{arquivo}    # recupera o conteúdo (tail) de um log específico
```

## Autenticação & Permissões

- Requer token Sanctum com habilidade `backoffice`.
- Gate `viewLogs`: apenas usuários com permissão `index.all` (ex.: função *master*) conseguem acessar.

## Cabeçalhos

| Cabeçalho     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| Authorization | string | Sim         | `Bearer {token}` com habilidade `backoffice`. |
| Accept        | string | Não         | `application/json`. |

## Parâmetros

### GET /api/v1/backoffice/logs
Sem parâmetros.

### GET /api/v1/backoffice/logs/{arquivo}
| Parâmetro | Tipo  | Obrigatório | Descrição |
| --------- | ----- | ----------- | --------- |
| arquivo   | string | Sim | Prefixo do log sem a extensão `.log`. Ex.: `order`, `authentication`, `logout`, `log` (mapeia para `laravel.log`). |
| lines     | int   | Não | Quantidade máxima de linhas retornadas após combinar todos os arquivos correspondentes (default 200, máximo 500). |

## Exemplo de Requisições

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  https://sandbox.echosistema.app/api/v1/backoffice/logs

curl -X GET \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.echosistema.app/api/v1/backoffice/logs/laravel.log?lines=100"
```

## Exemplo de Respostas

### Lista de logs
```json
{
  "data": [
    {
      "file": "laravel.log",
      "size": 1048576,
      "size_human": "1.00 MB",
      "last_modified": "2025-11-19T18:00:00+00:00"
    }
  ]
}
```

### Leitura de log
```json
{
  "data": {
    "query": "order",
    "files": [
      {
        "file": "order.log",
        "size": 34567,
        "size_human": "33.8 KB",
        "last_modified": "2025-11-19T18:00:00+00:00"
      },
      {
        "file": "order-2025-11-18.log",
        "size": 12345,
        "size_human": "12.1 KB",
        "last_modified": "2025-11-18T23:00:00+00:00"
      }
    ],
    "lines": [
      {
        "file": "order.log",
        "timestamp": "2025-11-19 18:00:01",
        "environment": "testing",
        "type": "INFO",
        "name": "Ewerton Daniel",
        "message": "Ewerton Daniel successfully authenticated.",
        "context": { "model": "Domain\\Shared\\Models\\User", "id": 24 }
      },
      {
        "file": "order-2025-11-18.log",
        "timestamp": "2025-11-18 23:59:59",
        "environment": "testing",
        "type": "ERROR",
        "message": "Falha",
        "context": { "details": "..." }
      }
    ]
  }
}
```

## Status HTTP

- 200: OK
- 401: Token inválido ou ausente
- 403: Sem permissão `index.all`
- 404: Arquivo não encontrado

## Notas

- Apenas arquivos em `storage/logs` são expostos.
- Informe somente o prefixo do arquivo (sem `.log`). O endpoint procura todos os arquivos que começam com esse prefixo (ex.: `order`, `order-2025-11-17.log`) e faz o *merge* das linhas.
- Cada linha é convertida em um objeto com `timestamp`, `environment`, `type`, `name` (trecho entre o `:` e o `successfully`) , `message` e `context` (quando o log estiver no formato padrão Laravel). Caso a linha não corresponda ao padrão, o campo `line` é retornado com o conteúdo bruto.
- O conteúdo retornado é um *tail* consolidado; utilize `lines` para definir o número máximo de linhas agregadas.
- O valor `log` é automaticamente convertido para `laravel.log` por retrocompatibilidade.

## Changelog

- 2025-11-19: Endpoint criado para leitura de logs no backoffice.
