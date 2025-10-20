# Shared – Atualizar Meu Perfil

## Endpoint

`PUT /api/v1/me/profile`

## Autenticação

Requer token Bearer válido.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Locale para mensagens de validação. |

## Corpo (application/json)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Não | Nome completo (máx. 255). |
| `gender` | `string` | Não | Aceita `m`/`f`/`o` ou valores normalizados `male`/`female`/`other`. |
| `birth_date` | `date` | Não | Data de nascimento (ISO 8601). |
| `email` | `email` | Não | E-mail (máx. 255). |

Notas
- O servidor normaliza `gender`: `male`→`m`, `female`→`f`, demais→`o`.
- Apenas campos enviados são atualizados; os demais permanecem inalterados.

## Exemplo de Requisição

```json
{
  "name": "João Atualizado",
  "gender": "male",
  "birth_date": "1990-05-12",
  "email": "joao.atualizado@example.com"
}
```

## Sucesso `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "encoded_uuid",
    "name": "João Atualizado",
    "email": "joao.atualizado@example.com",
    "email_verified_at": null,
    "slug": "joao-atualizado",
    "avatar": {"url": "https://cdn.example.com/users/joao/avatar.webp", "usage": "avatar"},
    "profile_image": null,
    "age": 35,
    "gender": "m",
    "gender_name": "male",
    "birthday": "1990-05-12T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "pt-BR",
    "currency": "BRL",
    "created_at": "2024-01-01T12:00:00+00:00",
    "roles": [
      {"id": 9, "uuid": "role-guest-uuid", "name": "guest", "localized_name": "Convidado", "permissions": ["store.complaint"]}
    ],
    "telephone": "+55 11 99999-0000",
    "contacts": {"email": {"uuid": "c1", "name": "email", "email": "joao.atualizado@example.com", "phone": null}},
    "social_medias": {"linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/joao"}},
    "address": {"uuid": "addr-uuid", "main": true, "zipcode": "01000-000", "one": "Av. Paulista", "formatted": "Av. Paulista, São Paulo - SP, 01000-000, BR"},
    "nationalities": [{"id": 76, "country_id": 76, "country": {"name": "Brasil", "code": "BR", "flag": "🇧🇷"}}],
    "biography": {"uuid": "bio-uuid", "name": "João Atualizado", "summary": "Desenvolvedor de software..."},
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": null,
    "identities": []
  }
}
```

## Estrutura JSON (destaques)

Segue a mesma estrutura de “Obter Perfil do Usuário”. Campos principais exibidos acima.

## Erros

- 401 Unauthorized — token ausente/inválido
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
- [Atualizar Idioma do Usuário](./UserLanguageUpdate.md)
- [Atualizar Moeda do Usuário](./UserCurrencyUpdate.md)
