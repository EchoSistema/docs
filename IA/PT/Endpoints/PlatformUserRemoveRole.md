# Usuários da Plataforma — Remover Papel (Role)

- Método: `DELETE`
- Rota: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Autenticação: `auth:sanctum`

## Resumo
Remove um papel do usuário dentro da plataforma atual. Se for o único papel que o usuário possui, ele é substituído por `GUEST` com `status=approved` (o usuário nunca fica sem papel).

## Parâmetros de rota
- `user` — uuid
- `role` — aceita `id`, `uuid` ou `name`

## Resposta
`UserProfileResource` com `platformRoles` carregados.

## Notas
- Sem efeito se o usuário não possuir o papel especificado nesta plataforma.
