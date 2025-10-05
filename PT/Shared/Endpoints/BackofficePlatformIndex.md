# Plataformas — Listagem e Detalhe (Backoffice)

## Endpoint

```
GET /backoffice/platforms
GET /backoffice/platforms/{platform:uuid}
```

Retorna a lista de plataformas disponíveis para o operador backoffice, bem como o detalhe de uma plataforma específica.

---

## Autenticação

**Obrigatória** – Token Bearer com habilidade `backoffice`.

Além da habilidade, o usuário deve possuir a permissão `index.all` para consumir a listagem.

---

## Cabeçalhos

| Cabeçalho       | Tipo     | Obrigatório | Descrição |
| --------------- | -------- | ----------- | --------- |
| Authorization   | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY    | string   | Sim         | Chave pública que identifica a plataforma solicitante. |
| Accept-Language | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). Determina a localização de campos traduzíveis, quando aplicável. |

---

## Parâmetros

### Parâmetros de caminho

| Parâmetro        | Tipo | Obrigatório | Descrição |
| ---------------- | ---- | ----------- | --------- |
| `platform:uuid`  | uuid | Sim         | UUID da plataforma consultada no endpoint de detalhe. |

### Parâmetros de consulta (`GET /backoffice/platforms`)

| Parâmetro     | Tipo      | Obrigatório | Descrição | Padrão/Valores |
| ------------- | --------- | ----------- | --------- | -------------- |
| `per_page`    | integer   | Não         | Número de itens por página quando a paginação está ativa. | `25` |
| `page`        | integer   | Não         | Página atual quando paginando. | `1` |
| `no_paginate` | boolean   | Não         | Quando `true`, desabilita paginação e retorna todos os registros. | `false` |

> Os parâmetros aceitam variantes em `snake_case`, `camelCase` e `kebab-case`.

---

## Exemplos

### Exemplo de requisição (Listagem)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave-publica>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/backoffice/platforms?per_page=25"
```

### Exemplo de resposta — Listagem Paginada (`200`)

```json
{
  "data": [
    {
      "uuid": "cc4f6a61-8db6-4d9a-9c4b-09d1c8b0a731",
      "total": {
        "users": 128
      },
      "domain": {
        "uuid": "45a620f4-5a3d-4c54-8546-a4fea1d73139",
        "name": "Educação"
      },
      "name": "Echo Educação",
      "email": "contato@echo-educacao.com",
      "open_to_register": true,
      "public_key": "echo-edu-public-key",
      "logo": {
        "main": "https://cdn.example.com/platforms/echo-edu/main.webp",
        "square": "https://cdn.example.com/platforms/echo-edu/square.webp",
        "rounded": "https://cdn.example.com/platforms/echo-edu/rounded.webp",
        "rectangular": "https://cdn.example.com/platforms/echo-edu/rectangular.webp"
      },
      "created_at": "2024-08-12T14:03:22+00:00",
      "company_uuid": "84e6f2bc-a0a7-4da8-a465-5f0c60ec4c3d",
      "slug": "echo-educacao",
      "country": "Brasil",
      "state": "São Paulo",
      "city": "São Paulo",
      "company_logos": [
        { "full": "https://cdn.example.com/platforms/echo-edu/company/full.webp" }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/backoffice/platforms?page=1",
    "last": "https://sandbox.seu-dominio.com/backoffice/platforms?page=5",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/backoffice/platforms?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.seu-dominio.com/backoffice/platforms",
    "per_page": 25,
    "to": 25,
    "total": 118
  }
}
```

### Exemplo de resposta — Sem paginação (`no_paginate=true`)

Retorna apenas o array `data` sem `links` e `meta`.

### Exemplo de resposta — Detalhe (`200`)

```json
{
  "data": {
    "uuid": "cc4f6a61-8db6-4d9a-9c4b-09d1c8b0a731",
    "total": {
      "users": 128
    },
    "domain": {
      "uuid": "45a620f4-5a3d-4c54-8546-a4fea1d73139",
      "name": "Educação"
    },
    "name": "Echo Educação",
    "email": "contato@echo-educacao.com",
    "open_to_register": true,
    "public_key": "echo-edu-public-key",
    "logo": {
      "main": "https://cdn.example.com/platforms/echo-edu/main.webp",
      "square": "https://cdn.example.com/platforms/echo-edu/square.webp",
      "rounded": "https://cdn.example.com/platforms/echo-edu/rounded.webp",
      "rectangular": "https://cdn.example.com/platforms/echo-edu/rectangular.webp"
    },
    "created_at": "2024-08-12T14:03:22+00:00",
    "company_uuid": "84e6f2bc-a0a7-4da8-a465-5f0c60ec4c3d",
    "slug": "echo-educacao",
    "country": "Brasil",
    "state": "São Paulo",
    "city": "São Paulo",
    "company_logos": [
      { "full": "https://cdn.example.com/platforms/echo-edu/company/full.webp" }
    ]
  }
}
```

---

## Estrutura JSON Explicada

### Campos de plataforma (`data[]` ou `data`)

| Campo                | Tipo      | Descrição |
| -------------------- | --------- | --------- |
| `uuid`               | uuid      | Identificador único da plataforma. |
| `total.users`        | integer   | Quantidade de usuários associados à plataforma. |
| `is_parent`          | boolean   | Presente somente quando a plataforma é matriz. |
| `domain.uuid`        | uuid      | UUID do domínio ao qual a plataforma pertence. |
| `domain.name`        | string    | Nome do domínio/segmento. |
| `name`               | string    | Nome da plataforma. |
| `email`              | string    | E-mail de contato principal. |
| `open_to_register`   | boolean   | Indica se a plataforma aceita novos cadastros públicos. |
| `public_key`         | string    | Chave pública utilizada por integrações. |
| `logo.main`          | string    | URL do logo principal (proporção livre). |
| `logo.square`        | string    | URL do logo quadrado. |
| `logo.rounded`       | string    | URL do logo com cantos arredondados. |
| `logo.rectangular`   | string    | URL do logo retangular. |
| `created_at`         | string    | Data de criação em formato ISO-8601. |
| `company_uuid`       | uuid      | UUID da empresa vinculada, quando disponível. |
| `slug`               | string    | Slug público da empresa ou plataforma. |
| `country`            | string    | País da empresa associado. |
| `state`              | string    | Estado/província da empresa. |
| `city`               | string    | Cidade da empresa. |
| `company_logos[]`    | object    | Lista de logos adicionais (`usage` → URL). |
| `is_main`            | boolean   | Presente quando a relação `platformAffiliated` é carregada e indica se é a plataforma principal do afiliado. |
| `is_banned`          | boolean   | Presente quando `platformAffiliated` mostra banimento. |
| `platform_transactions.in`  | number | Entradas financeiras associadas (quando `platformAffiliated` está carregado). |
| `platform_transactions.out` | number | Saídas financeiras associadas (quando `platformAffiliated` está carregado). |

---

## Status HTTP

- `200` – Sucesso na listagem ou exibição.
- `401` – Token ausente ou inválido.
- `403` – Usuário autenticado sem permissão `index.all`.
- `404` – Plataforma não encontrada (endpoint de detalhe).
- `429` – Limite de requisições excedido.
- `500` – Erro interno não tratado.

---

## Erros

### Exemplo 403 — Usuário sem `index.all`

```json
{
  "message": "You are not allowed to perform this action."
}
```

### Exemplo 404 — Plataforma inexistente

```json
{
  "message": "No query results for model [Platform]"
}
```

---

## Notas

- A paginação segue o padrão do Laravel (`links` e `meta`).
- Quando `no_paginate=true`, filas maiores podem consumir mais memória. Use apenas para exports/controlados.
- `users_count` é carregado via `withCount('users')` e reflete o total atual de usuários da plataforma.
- Os logos adicionais da empresa são retornados como objetos `{ usage => url }` conforme armazenado em `company.logos`.

---

## Relacionados

- [Plataforma – Listagem de Usuários](PlatformUserIndex.md)

---

## Changelog

- 2025-10-04: Documento inicial.

