# Usuários da Plataforma — Atualizar Papel (Role)

- Método: `PUT`
- Rota: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Autenticação: `auth:sanctum`

## Resumo
Adiciona ou atualiza um papel do usuário dentro da plataforma atual.

## Parâmetros de rota
- `user` — uuid
- `role` — aceita `id`, `uuid` ou `name`

## Corpo da requisição
- `status` (opcional) — um de `approved`, `disapproved`, `requested`

## Comportamento
- Se o papel não existir para o usuário nesta plataforma, ele é criado (padrão `status=requested`).
- Se já existir, atualiza o `status` (e garante que `role_id` corresponde ao papel informado).

## Resposta
`UserProfileResource` com `platformRoles` carregados.
