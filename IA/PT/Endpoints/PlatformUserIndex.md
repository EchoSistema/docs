# IA – Usuários Admin (Listagem)

## Endpoint

`GET /api/v1/ia/admin/users`

Lista usuários da plataforma com filtros por cargo, ocupação e área, na área administrativa de IA.

---

## Autenticação

Obrigatória – Token Bearer com habilidade `backoffice`.

---

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traduzíveis (ex.: `pt-BR`, `en`, `es`). |

---

## Parâmetros de Consulta

Aceita os mesmos filtros documentados em Shared Platform User Index. Consulte:

- docs/Shared/PT/Endpoints/PlatformUserIndex.md

Parâmetros suportados incluem, entre outros: `role`, `roles[]`, `role_id`, `role_name`, `user_name`, `user_email`, `user_uuid`, `job_occupation` (e variações), `occupation_area` (e variações), `has_job_occupation`, `has_occupation_area`, `no_paginate`, `per_page`.

> Observação: parâmetros podem ser enviados em `snake_case` (canônico) e variações `camelCase`/`kebab-case`.

---

## Exemplo de Resposta

A resposta é idêntica à do Shared Platform User Index. Exemplo e estrutura detalhada em:

- docs/Shared/PT/Endpoints/PlatformUserIndex.md

---

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 422: Erro de validação

---

## Notas

- Retorna apenas usuários com cargos inferiores ao menor cargo do usuário autenticado na plataforma atual.
- Quando `has_job_occupation` ou `has_occupation_area` são verdadeiros, títulos são carregados com base em `Accept-Language`.
- Quando `no_paginate=true`, retorna todos os resultados sem metadados de paginação.

---

## Relacionados

- docs/IA/PT/Endpoints/PlatformUserCounter.md
- docs/IA/PT/Endpoints/PlatformUserShow.md
- docs/IA/PT/Endpoints/PlatformUserStore.md

