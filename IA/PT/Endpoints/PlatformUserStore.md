# Usuários da Plataforma — Criar (Registro)

- Método: `POST`
- Rota: `/api/v1/ia/admin/users`
- Autenticação: `auth:sanctum`

## Resumo
Registra um novo usuário na plataforma atual utilizando o pipeline de registro. Retorna os dados do usuário e, por padrão neste fluxo, sem token.

## Corpo da requisição
Obrigatórios:
- `device` (string)
- `name` (string)
- `email` (email)
- `password` (string, mínimo 8)
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
- O controlador injeta flags: `recently_created=true`, `no_auth=true`, `registered_by=true`.
- Quando `roles` inclui `guest`, os campos obrigatórios de endereço são flexibilizados.

## Resposta
Formato de `UserAuthResource` (token omitido devido a `no_auth=true`):

```json
{
  "message": "User successfully registered.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "...",
      "echo_uuid": "...",
      "name": "...",
      "email": "...",
      "avatar": "...",
      "language": "en",
      "roles": [
        {
          "id": 5,
          "platform": { "uuid": "...", "name": "...", "public_key": "..." },
          "name": "guest",
          "localized_name": "Guest",
          "permissions": [ { "subject": "users", "action": "read" } ]
        }
      ],
      "registered_by": { }
    }
  }
}
```
