# Usuários da Plataforma — Remover Role

- Método: `DELETE`
- Rota: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Auth: `auth:sanctum`

## Resumo
Remove um role do usuário dentro da plataforma atual. Se for o único role do usuário, ele é substituído por `GUEST` com `status=approved` (o usuário nunca fica sem role).

## Parâmetros de Caminho
- `user` — uuid
- `role` — aceita `id`, `uuid` ou `name`

## Resposta
`UserProfileResource` com `platformRoles` carregado.

## Notas
- No-op se o usuário não possuir o role informado nesta plataforma.

