# Shared – Verificação de Saúde

## Endpoints

Cada domínio possui sua própria rota de health check:

```
GET /api/v1/advertising/health
GET /api/v1/affiliate/health
GET /api/v1/articles/health
GET /api/v1/ai/health
GET /api/v1/bio/health
GET /api/v1/service/health              # Brick
GET /api/v1/cms/health
GET /api/v1/company/health
GET /api/v1/ecommerce/health
GET /api/v1/find-a-job/health
GET /api/v1/footprints/health
GET /api/v1/health-care/health
GET /api/v1/its-checked/health
GET /api/v1/learn/health
GET /api/v1/link-list/health
GET /api/v1/public/health               # Microservices
GET /api/v1/news/health
GET /api/v1/payment-hub/health
GET /api/v1/pigeons/health
GET /api/v1/real-estate/health
GET /api/v1/rent/health
GET /api/v1/complaints/health           # ReputationBook
GET /api/v1/university/health
```

## Autenticação

Nenhuma (apenas cabeçalho público da plataforma)

## Cabeçalhos

| Cabeçalho     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| Authorization | string | Não         | Credencial `Bearer {token}` (opcional para ambientes autenticados). |
| X-PUBLIC-KEY  | string | Sim         | UUID público da plataforma. Alternativamente, use o query param `pbk`. |
| Accept-Language | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Sem parâmetros obrigatórios. Opcionalmente, informe `pbk=<uuid>` na query string caso não consiga enviar headers.

## Exemplos

### Exemplo de requisição (curl)

```bash
# Verificar saúde de um domínio específico
curl -X GET \
  -H "X-PUBLIC-KEY: 123e4567-e89b-12d3-a456-426614174000" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/real-estate/health"
```

### Exemplo de resposta

```json
{
  "data": {
    "domain": "RealEstate",
    "slug": "real-estate",
    "status": "ok",
    "checked_at": "2025-11-19T18:43:00+00:00",
    "routes": {
      "file": "src/Domain/RealEstate/routes/api.php",
      "exists": true,
      "last_modified": "2025-11-18T20:01:00+00:00"
    }
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data | object | Objeto com informações de saúde do domínio. |
| data.domain | string | Nome do domínio (pasta em `src/Domain`). |
| data.slug | string | Slug usado na rota (`Str::kebab(domain)`). |
| data.status | string | `ok` quando o arquivo de rotas existe, `missing-routes` caso contrário. |
| data.checked_at | string | Timestamp da checagem atual (ISO 8601). |
| data.routes.file | string | Caminho relativo do arquivo `routes/api.php`. |
| data.routes.exists | boolean | Indica se o arquivo existe. |
| data.routes.last_modified | string\|null | Timestamp (ISO 8601) do último update do arquivo. |

## Status HTTP

- 200: OK
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Cada domínio possui sua própria rota de health check registrada diretamente no respectivo `routes/api.php`.
- O endpoint verifica se o arquivo de rotas do domínio existe e retorna o horário da última modificação.
- Não existe mais um endpoint agregador global `/health`. Cada domínio deve ser verificado individualmente.
- Endpoint não precisa de login, mas exige a chave pública (`X-PUBLIC-KEY` ou `pbk`) para identificar o tenant.

## Relacionados

- Ver demais endpoints de Shared

## Changelog

- 2025-11-19: Atualização para refletir as rotas específicas por domínio e o snapshot baseado nos arquivos de rota.
- 2025-10-16: Documentação inicial.
