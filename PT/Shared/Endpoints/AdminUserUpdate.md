# Shared – Atualizar Usuário (Admin)

## Endpoint

`PUT /api/v1/users/{user:uuid}`

## Autenticação

Requer token Bearer com habilidade `backoffice` e permissões na plataforma.

Permissões
- Uma das: `update.guest`, `update.collaborator` ou `update.all` no papel atual da plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Locale para mensagens de validação. |

## Parâmetros de Rota

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `user` | `uuid` | Sim | Identificador do usuário alvo. |

## Corpo (application/json)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Não | Nome completo (máx. 255). |
| `gender` | `string` | Não | Aceita `m`/`f`/`o` ou `male`/`female`/`other` (normalizado). |
| `birth_date` | `date` | Não | Data de nascimento (ISO 8601). |
| `email` | `email` | Não | E-mail (máx. 255). |

Notas
- O servidor normaliza `gender`: `male`→`m`, `female`→`f`, demais→`o`.
- Apenas campos enviados são atualizados; os demais permanecem inalterados.

## Exemplo de Requisição

```json
{
  "name": "Maria Admin",
  "gender": "female",
  "birth_date": "1988-09-20",
  "email": "maria.admin@example.com"
}
```

## Sucesso `200 OK`

Retorna o recurso completo de perfil do usuário (mesma estrutura de “Obter Perfil do Usuário”).

```json
{
  "data": {
    "uuid": "11111111-2222-3333-4444-555555555555",
    "echo_uuid": "encoded_uuid",
    "name": "Maria Admin",
    "email": "maria.admin@example.com",
    "email_verified_at": null,
    "slug": "maria-admin",
    "avatar": {"url": "https://cdn.example.com/users/maria/avatar.webp", "usage": "avatar"},
    "profile_image": null,
    "age": 36,
    "gender": "f",
    "gender_name": "female",
    "birthday": "1988-09-20T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "pt-BR",
    "currency": "BRL",
    "created_at": "2024-02-10T12:00:00+00:00",
    "roles": [
      {"id": 2, "uuid": "role-admin-uuid", "name": "administrator", "localized_name": "Administrador", "permissions": ["index.platform_message", "show.platform_message", "update.all"]}
    ],
    "telephone": "+55 11 98888-0000",
    "contacts": {"email": {"uuid": "c1", "name": "email", "email": "maria.admin@example.com", "phone": null}},
    "social_medias": {"linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/maria"}},
    "address": {"uuid": "addr-uuid", "main": true, "zipcode": "01000-000", "one": "Av. Paulista", "formatted": "Av. Paulista, São Paulo - SP, 01000-000, BR"},
    "nationalities": [{"id": 76, "country_id": 76, "country": {"name": "Brasil", "code": "BR", "flag": "🇧🇷"}}],
    "biography": {"uuid": "bio-uuid", "name": "Maria Admin", "summary": "..."},
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": null,
    "identities": []
  }
}
```

## Estrutura JSON (destaques)

Igual ao endpoint “Atualizar Meu Perfil”. Consulte também “Obter Perfil do Usuário”.

## Erros

- 401 Unauthorized — token ausente/inválido
- 403 Forbidden — permissões insuficientes ou plataforma divergente
- 404 Not Found — usuário não encontrado
- 422 Unprocessable Entity — erros de validação
- 500 Internal Server Error

### Exemplo 422

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "email": ["O campo email deve ser um endereço de e-mail válido."],
    "gender": ["O valor selecionado para gender é inválido."],
    "birth_date": ["O campo birth date não é uma data válida."]
  }
}
```

## Relacionados

- [Obter Perfil do Usuário](./UserProfile.md)
- [Atualizar Meu Perfil](./UserProfileUpdate.md)
