# Usuários da Plataforma — Atualizar Role

- Método: `PUT`
- Rota: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Auth: `auth:sanctum`

## Resumo
Adiciona ou atualiza um role do usuário dentro da plataforma atual.

## Parâmetros de Caminho
- `user` — uuid
- `role` — aceita `id`, `uuid` ou `name`

## Corpo da Requisição
- `status` (opcional) — um de `approved`, `disapproved`, `requested`

## Comportamento
- Se o role ainda não existir para o usuário nesta plataforma, cria (padrão `status=requested`).
- Se já existir, atualiza o `status` (e garante que o `role_id` corresponda ao informado).

## Resposta
`UserProfileResource` com `platformRoles` carregado.

