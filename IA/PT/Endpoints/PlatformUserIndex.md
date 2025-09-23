# Usuários da Plataforma — Índice

- Método: `GET`
- Rota: `/api/v1/ia/admin/users`
- Autenticação: `auth:sanctum`

## Resumo
Lista os usuários da plataforma atual, retornando registros de papéis da plataforma com o usuário relacionado. Suporta filtragem avançada e paginação opcional.

## Parâmetros de consulta
- Paginação: `page` (inteiro), `per_page` (inteiro, padrão 25), `no_paginate` (booleano)
- Relações: `relations[]` — por padrão inclui `user`, `user.avatarImage`, `user.telephone`
- Localização: `language` — usado ao solicitar relações de ocupação
- Filtros de papel (role):
  - `role` (id ou nome)
  - `roles[]` (array de ids ou nomes)
  - ou explícitos: `role_id`, `role_name`, `role_ids[]`, `role_names[]`
- Filtros de usuário: `name` ou `user_name` (contém), `email` ou `user_email` (contém), `user_uuid`
- Filtros de ocupação:
  - `job_occupation` (id | uuid | título) → normalizado em `job_occupation_id` | `job_occupation_uuid` | `job_occupation_title`
  - `occupation_area` (uuid | `content:usage`) ou estruturado `occupation_area[content]`, `occupation_area[usage]`
  - Flags de conveniência (derivadas automaticamente): `has_job_occupation`, `has_occupation_area`

## Comportamento
- Exclui papéis abaixo do papel mínimo do operador atual na plataforma.
- Quando `has_job_occupation` ou `has_occupation_area` são verdadeiros, carrega antecipadamente `user.jobOccupations` (apenas a padrão) com títulos localizados conforme `language`.

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
- `links`/`meta` aparecem quando paginado; são omitidos quando `no_paginate=true`.
- Cada item é um registro `platform_user_roles`, possivelmente com as relações solicitadas.

## Exemplos
- Básico: `GET /api/v1/ia/admin/users?per_page=25`
- Filtrar por nome do papel: `GET ...?role=guest`
- Filtrar por múltiplos ids de papel: `GET ...?roles[]=1&roles[]=2`
- Filtrar por email do usuário (contém): `GET ...?email=@domain.com`
- Incluir ocupação e títulos localizados: `GET ...?job_occupation=Some Title&language=en`
