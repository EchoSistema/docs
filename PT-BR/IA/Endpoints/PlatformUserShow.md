# Usuários da Plataforma — Detalhar

- Método: `GET`
- Rota: `/api/v1/ia/admin/users/{user:uuid}`
- Auth: `auth:sanctum`

## Resumo
Retorna o perfil do usuário. Opcionalmente inclui os roles do usuário dentro da plataforma atual.

## Query Params
- `role` (bool) — quando verdadeiro, carrega `platformRoles` limitados à plataforma atual, ordenados por `role_id`.

## Resposta
`UserProfileResource`:

```json
{
  "uuid": "...",
  "name": "...",
  "email": "...",
  "avatar": "...",
  "roles": [
    { "id": 5, "uuid": "...", "name": "guest", "localized_name": "Convidado", "permissions": ["..."] }
  ],
  "telephone": "+55 ...",
  "gender": "...",
  "birthday": "2021-01-02T00:00:00Z",
  "address": { "city": "...", "state": "...", "country": { "name": "...", "code": "...", "flag": "..." } },
  "nationalities": [ { "uuid": "...", "country": { "name": "...", "code": "...", "flag": "..." } } ]
}
```

Notas:
- `roles` é incluído somente quando `role=true`.
- Endereço e nacionalidades são incluídos quando presentes no usuário.

