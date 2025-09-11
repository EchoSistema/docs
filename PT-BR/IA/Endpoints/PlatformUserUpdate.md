# Usuários da Plataforma — Atualizar

- Método: `PUT`
- Rota: `/api/v1/ia/admin/users/{user:uuid}`
- Auth: `auth:sanctum`

## Resumo
Atualiza campos básicos do usuário.

## Corpo da Requisição (qualquer dos)
- `name` (string)
- `email` (email, único)
- `gender` (enum)
- `birth_date` (date)
- `password` + `password_confirmation` (string, min 8)

## Resposta
`UserProfileResource` — mesmo formato do Detalhar.

