# ReputationBook – Reclamações do usuário autenticado

## Listar minhas reclamações

### Endpoint

```
GET /api/v1/complaint-book/my-complaints
```

Retorna as reclamações do usuário logado na plataforma atual.

### Autenticação

Obrigatória – `Bearer {token}` via `auth:sanctum`.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `Authorization` | string | Sim | Token `Bearer` válido. |
| `X-PUBLIC-KEY` | string | Sim | Plataforma corrente. |
| `Accept-Language` | string | Recomendado | Idioma para títulos/textos. |

### Parâmetros de consulta

Os mesmos suportados por `ComplaintFilterRequest`, porém a API força `user_id = Auth::id()` internamente.

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `page`, `per_page`, `no_paginate`, `limit` | integer / boolean | Controlam paginação. |
| `status` | string | Filtra por status (`open`, `resolved`, ...). |
| `created_at`, `created_at_period[]` | date / array | Filtro de período de criação. |
| `purchase_date`, `purchase_date_period[]` | date / array | Filtra por data de compra. |
| `with_texts` | boolean | Inclui textos completos. |

### Resposta (200)

Segue estrutura idêntica à listagem pública, porém inclui campos adicionais (`texts`, `images`, `user`) do recurso `ComplaintResource`.

### Status HTTP

- 200: Lista retornada.
- 401: Token ausente/expirado.
- 500: Erro interno.

---

## Exibir uma reclamação minha

### Endpoint

```
GET /api/v1/complaint-book/my-complaints/{complaint}
```

Retorna detalhes completos da reclamação pertencente ao usuário autenticado. Respeita binding por ID, UUID ou slug.

### Autenticação

Obrigatória – `Bearer {token}` (`auth:sanctum`).

### Parâmetros de caminho

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `complaint` | string | Identificador da reclamação (ID/UUID/slug). |

### Status HTTP

- 200: Reclamação encontrada.
- 403: Reclamação pertence a outro usuário.
- 404: Reclamação inexistente.

### Notas

- A resposta inclui dados de plataforma, empresa, textos completos, imagens e eventos de status (quando carregados).

