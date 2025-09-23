# Usuários da Plataforma — Mostrar

- Método: `GET`
- Rota: `/api/v1/ia/admin/users/{user:uuid}`
- Autenticação: `auth:sanctum`

## Resumo
Retorna o perfil do usuário. Opcionalmente inclui os papéis do usuário dentro da plataforma atual.

## Parâmetros de consulta
- `role` (booleano) — quando verdadeiro, carrega `platformRoles` limitados à plataforma atual, ordenados por `role_id`.

## Resposta
Campos de `UserProfileResource`:

```json
{
  "uuid": "...",
  "name": "...",
  "email": "...",
  "avatar": "...",
  "roles": [
    { "id": 5, "uuid": "...", "name": "guest", "localized_name": "Guest", "permissions": ["..."] }
  ],
  "telephone": "+1 ...",
  "gender": "...",
  "birthday": "2021-01-02T00:00:00Z",
  "address": { "city": "...", "state": "...", "country": { "name": "...", "code": "...", "flag": "..." } },
  "nationalities": [ { "uuid": "...", "country": { "name": "...", "code": "...", "flag": "..." } } ]
}
```

Notas:
- `roles` é incluído apenas quando `role=true`.
- Endereço e nacionalidades são incluídos quando presentes no usuário.
