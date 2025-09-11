# Usuários da Plataforma — Criar (Registrar)

- Método: `POST`
- Rota: `/api/v1/ia/admin/users`
- Auth: `auth:sanctum`

## Resumo
Registra um novo usuário na plataforma atual usando o pipeline de registro. Retorna os dados do usuário e, por padrão neste fluxo, sem token.

## Corpo da Requisição
Obrigatório:
- `device` (string)
- `name` (string)
- `email` (email)
- `password` (string, min 8)
- `password_confirmation` (string)

Opcional (comum):
- `language` (enum)
- `currency` (enum)

Opcional (quando `collaborator=true`):
- `collaborator` (bool)
- `gender` (enum)
- `birth_date` (date)
- `roles[]` (array de nomes; ex.: `guest`, `support`)
- `address{...}` (ver regras de validação)
- `contacts[]` com `type`/`value`/`country_code`/`number`
- `nationalities[]`

Notas:
- O controller injeta flags: `recently_created=true`, `no_auth=true`, `registered_by=true`.
- Quando `roles` inclui `guest`, os campos required de endereço são relaxados.

## Resposta
`UserAuthResource` (token omitido por `no_auth=true`):

```json
{
  "message": "Usuário registrado com sucesso.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "...",
      "echo_uuid": "...",
      "name": "...",
      "email": "...",
      "avatar": "...",
      "language": "pt-BR",
      "roles": [
        {
          "id": 5,
          "platform": { "uuid": "...", "name": "...", "public_key": "..." },
          "name": "guest",
          "localized_name": "Convidado",
          "permissions": [ { "subject": "users", "action": "read" } ]
        }
      ],
      "registered_by": { }
    }
  }
}
```

