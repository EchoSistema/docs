# ReputationBook – Reclamações públicas

## Listar reclamações (público)

### Endpoint

```
GET /api/v1/complaints
```

Retorna reclamações publicadas para a plataforma corrente (`platform` middleware). Os resultados podem ser paginados ou limitados via parâmetros de consulta.

### Autenticação

Nenhuma. Apenas o cabeçalho `X-PUBLIC-KEY` é obrigatório para identificar a plataforma.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição | Valores |
| --------- | ---- | ----------- | --------- | ------- |
| `X-PUBLIC-KEY` | string | Sim | Identificador público da plataforma. |
| `Accept-Language` | string | Recomendado | Idioma alvo (`pt-BR`, `en`, `es`) para títulos/textos carregados. |

### Parâmetros de consulta

| Parâmetro | Tipo | Obrigatório | Descrição | Padrão/Valores |
| --------- | ---- | ----------- | --------- | -------------- |
| `page` | integer | Não | Página atual quando paginado. | `1` |
| `per_page` | integer | Não | Tamanho da página. | `15` |
| `no_paginate` | boolean | Não | Quando `true`, retorna lista simples limitada por `limit`. | `false` |
| `limit` | integer | Não | Limite máximo quando `no_paginate=true`. | `15` |
| `status` | string | Não | Filtra por status (`open`, `in_progress`, etc.). | — |
| `company` | string | Não | Aceita ID, UUID, slug ou filtros `email:` / `url:`. | — |
| `title` | string | Não | Busca por título. | — |
| `complaint` | string | Não | Busca pelo texto da reclamação. | — |
| `with_texts` | boolean | Não | Inclui textos completos na resposta. | `false` |
| `created_at`, `created_at_period[]` | date / array | Não | Filtram por data de criação (exata ou intervalo). | — |
| `purchase_date`, `purchase_date_period[]` | date / array | Não | Filtram por data de compra informada. | — |
| `month`, `year` | integer | Não | Filtram por mês/ano (exige os dois quando `month` presente). | — |

> Os nomes aceitam `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

### Exemplo de resposta (200)

```json
{
  "data": [
    {
      "uuid": "0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5",
      "is_public": true,
      "title": {
        "title": "Entrega atrasada",
        "slug": "entrega-atrasada",
        "language": "pt-BR",
        "is_default": true
      },
      "text": "O pedido chegou com 7 dias de atraso...",
      "company": {
        "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
        "name": "Loja Exemplo",
        "slug": "loja-exemplo"
      },
      "status": "open",
      "purchase_date": "2025-08-12",
      "created_at": "2025-08-13T17:01:22+00:00"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 42
  }
}
```

### Estrutura JSON explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | array | Lista de reclamações públicas. |
| `data[].uuid` | uuid | Identificador da reclamação. |
| `data[].title` | objeto | Título localizado da reclamação. |
| `data[].text` | string | Trecho do texto principal (limitado quando público). |
| `data[].company` | objeto | Empresa associada. |
| `data[].status` | string | Status atual (`open`, `resolved`, ...). |
| `meta` | objeto | Metadados de paginação quando aplicável. |

### Status HTTP

- 200: Listagem retornada com sucesso.
- 400: Filtros inválidos.
- 429: Limite de requisições excedido.
- 500: Erro interno.

---

## Criar reclamação

### Endpoint

```
POST /api/v1/complaints
```

Cria uma nova reclamação em nome do usuário autenticado.

### Autenticação

Obrigatória – `Bearer {token}` via `auth:sanctum` com habilidades: `backoffice` **e** `domain:reputationbook`.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `Authorization` | string | Sim | `Bearer {token}` válido. |
| `X-PUBLIC-KEY` | string | Sim | Plataforma emissora. |
| `Accept-Language` | string | Recomendado | Idioma padrão para título/texto. |
| `Content-Type` | string | Sim | `application/json`. |

### Corpo (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `company_uuid` | uuid | Condicional | UUID da empresa. Requerido se `company_id` ausente. |
| `company_id` | integer | Condicional | ID interno da empresa (incompatível com `company_uuid`). |
| `purchase_date` | date | Sim | Data da compra (<= hoje). |
| `title` | string | Sim | Título da reclamação. |
| `complaint` | string | Sim | Texto completo da reclamação. |
| `is_public` | boolean | Não | Define se a reclamação é pública. |
| `invoice_number` | string | Não | Número da nota. |
| `attendants_name` | string | Não | Nome(s) dos atendentes envolvidos. |
| `language` | string | Não | Idioma do conteúdo (`pt-BR`, `en`, etc.). |

### Exemplo de requisição

```json
{
  "company_uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
  "purchase_date": "2025-08-12",
  "title": "Entrega atrasada",
  "complaint": "O pedido deveria chegar em 48 horas...",
  "is_public": true,
  "invoice_number": "NF-12345",
  "language": "pt-BR"
}
```

### Exemplo de resposta (201)

```json
{
  "data": {
    "uuid": "31db8d4a-0d0f-4d38-9c6a-c4c0ad9c8d41",
    "is_public": true,
    "platform": {
      "uuid": "c0aec1f4-d5ae-4ffd-8d24-8e55e9a3aca7",
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Defesa do Consumidor",
        "slug": "defesa-consumidor"
      }
    },
    "title": {
      "title": "Entrega atrasada",
      "language": "pt-BR",
      "is_default": true
    },
    "texts": [
      {
        "content": "O pedido deveria chegar em 48 horas...",
        "usage": "complaint_text",
        "language": "pt-BR",
        "is_default": true
      }
    ],
    "company": {
      "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
      "name": "Loja Exemplo",
      "slug": "loja-exemplo"
    },
    "status": "open",
    "purchase_date": "2025-08-12",
    "invoice_number": "NF-12345",
    "created_at": "2025-08-13T17:01:22+00:00"
  }
}
```

### Status HTTP

- 201: Reclamação criada.
- 400: Campos obrigatórios ausentes ou conflito entre `company_uuid` / `company_id`.
- 401: Token inválido.
- 403: Usuário sem permissão (`ability`).
- 422: Erros de validação.

### Notas

- A política de autorização exige permissão `create` sobre `Complaint`.
- Títulos e textos adicionais são gerenciados via endpoint de `add-text`.

---

## Visualizar reclamação por UUID

### Endpoint

```
GET /api/v1/complaints/{complaint}
```

Retorna uma reclamação específica. O parâmetro aceita ID, UUID ou slug conforme heurística de binding.

### Autenticação

Nenhuma. Usuários autenticados recebem campos extras (texto completo, usuário, invoice, imagens).

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `X-PUBLIC-KEY` | string | Sim | Plataforma emissora. |
| `Authorization` | string | Opcional | Amplia dados retornados quando presente. |

### Parâmetros de caminho

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `complaint` | string | ID numérico, UUID ou slug da reclamação. |

### Status HTTP

- 200: Reclamação localizada.
- 404: Reclamação inexistente.

---

## Atualizar reclamação

### Endpoint

```
PUT /api/v1/complaints/{complaint}
```

Atualiza dados básicos da reclamação.

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Corpo (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `company_uuid` | uuid | Não | Atualiza empresa (incompatível com `company_id`). |
| `company_id` | integer | Não | ID da empresa. |
| `purchase_date` | date | Não | Nova data de compra. |
| `title` | string | Não | Novo título (exige `language`). |
| `complaint` | string | Não | Novo texto principal (exige `language`). |
| `language` | string | Condicional | Obrigatório quando `title` ou `complaint` informados. |
| `is_public` | boolean | Não | Atualiza visibilidade. |
| `invoice_number` | string | Não | Atualiza nota fiscal. |

> A request deve conter pelo menos um campo mutável; caso contrário retorna `400` (`validation.empty_request`).

### Status HTTP

- 200: Atualização aplicada.
- 400: Corpo vazio.
- 401/403: Ausência de permissão.
- 404: Reclamação inexistente.
- 422: Dados inválidos.

---

## Atualizar status da reclamação

### Endpoint

```
PATCH /api/v1/complaints/{complaint}/status
```

### Corpo

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `status` | string | Sim | Novo status (enum `ComplaintStatusEnum`). |

### Notas

- Requer autorização (`update`) sobre a reclamação.
- O usuário autenticado é registrado como responsável pela mudança.

### Status HTTP

- 200: Status atualizado.
- 400: Status inválido.
- 401/403: Usuário sem permissão.
- 404: Reclamação inexistente.

---

## Remover reclamação

### Endpoint

```
DELETE /api/v1/complaints/{complaint}
```

Remove a reclamação e registra o usuário autenticado como responsável.

### Autenticação

Obrigatória – `Bearer {token}` (`auth:sanctum`) com habilidades `backoffice` e `domain:reputationbook`.

### Status HTTP

- 204: Remoção concluída.
- 401/403: Usuário sem permissão (`delete`).
- 404: Reclamação não encontrada.

---

## Adicionar texto complementar

### Endpoint

```
POST /api/v1/complaints/{complaint}/add-text
```

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Corpo (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `content` | string | Sim | Texto adicional (até 5000 caracteres). |
| `language` | string | Não | Idioma do texto (`LanguageEnum`). |
| `usage` | string | Não | Uso específico (default `complaint_text`). |

### Status HTTP

- 200: Texto anexado e recurso retornado.
- 401/403: Usuário sem permissão.
- 404: Reclamação inexistente.
- 422: Conteúdo inválido.

### Notas

- O texto é adicionado à coleção `texts`; use `all_texts=true` na resposta para receber tudo.

