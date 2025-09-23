# Usuários da Plataforma — Atualizar

- Método: `PUT`
- Rota: `/api/v1/ia/admin/users/{user:uuid}`
- Autenticação: `auth:sanctum`

## Resumo
Atualiza campos básicos do usuário.

## Corpo da requisição (qualquer um dos)
- `name` (string)
- `email` (email, único)
- `gender` (enum)
- `birth_date` (date)
- `password` + `password_confirmation` (string, mínimo 8)

## Resposta
`UserProfileResource` — mesmo formato de Show.
