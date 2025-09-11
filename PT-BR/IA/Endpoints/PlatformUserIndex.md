# Usuários da Plataforma — Listar

- Método: `GET`
- Rota: `/api/v1/ia/admin/users`
- Auth: `auth:sanctum`

## Resumo
Lista usuários da plataforma atual retornando registros de `platform_user_roles` com o usuário relacionado. Suporta filtros ricos e paginação opcional.

## Query Params
- Paginação: `page` (int), `per_page` (int, padrão 25), `no_paginate` (bool)
- Relações: `relations[]` — padrão inclui `user`, `user.avatarImage`, `user.telephone`
- Localização: `language` — usada ao requisitar relações de ocupação
- Filtros de role:
  - `role` (id ou name)
  - `roles[]` (array de ids ou names)
  - ou explícitos: `role_id`, `role_name`, `role_ids[]`, `role_names[]`
- Filtros de usuário: `name` ou `user_name` (contém), `email` ou `user_email` (contém), `user_uuid`
- Filtros de ocupação:
  - `job_occupation` (id | uuid | título) → normalizado em `job_occupation_id` | `job_occupation_uuid` | `job_occupation_title`
  - `occupation_area` (uuid | `content:usage`) ou estruturado `occupation_area[content]`, `occupation_area[usage]`
  - Flags de conveniência (auto-derivadas): `has_job_occupation`, `has_occupation_area`

## Comportamento
- Exclui roles abaixo do mínimo do operador atual na plataforma.
- Quando `has_job_occupation` ou `has_occupation_area` são verdade, faz eager load de `user.jobOccupations` (apenas o default) com títulos localizados conforme `language`.

## Resposta
Retorna uma coleção no formato padrão:

```json
{
  "data": [
    {
      "platform_id": 1,
      "user_id": 10,
      "role_id": 5,
      "status": "approved",
      "user": { "uuid": "...", "name": "...", "email": "...", "avatar": "..." }
    }
  ],
  "links": { "first": "...", "last": "...", "prev": null, "next": "..." },
  "meta": { "current_page": 1, "per_page": 25, "total": 123 }
}
```

Notas:
- `links`/`meta` aparecem quando paginado; omitidos quando `no_paginate=true`.
- Cada item é um registro de `platform_user_roles` possivelmente com relações solicitadas.

## Exemplos
- Básico: `GET /api/v1/ia/admin/users?per_page=25`
- Filtrar por nome de role: `GET ...?role=guest`
- Filtrar por múltiplos role ids: `GET ...?roles[]=1&roles[]=2`
- Filtrar por email do usuário contendo: `GET ...?email=@dominio.com`
- Incluir ocupação e títulos localizados: `GET ...?job_occupation=Algum Título&language=pt-BR`

