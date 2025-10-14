# Shared – Perfil do usuário autenticado

## Endpoint

```
GET /api/v1/me/profile
```

Retorna detalhes de perfil do usuário autenticado, incluindo informações pessoais, endereço, idioma/moeda preferidos e nacionalidades associadas.

## Autenticação

Obrigatória – Bearer {token} (Laravel Sanctum).

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | Credencial `Bearer {token}` do usuário logado. |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma solicitante. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro de caminho, consulta ou corpo é necessário.

## Exemplos

### Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.echosistema.com/api/v1/me/profile"
```

### Resposta de sucesso

```json
{
  "data": {
    "uuid": "86b77a09-5f27-4c37-bb8f-5d0ba1e93f9d",
    "name": "Maria Silva",
    "email": "maria.silva@example.com",
    "avatar": "https://cdn.echosistema.com/avatars/maria.webp",
    "roles": [
      {
        "id": 2,
        "uuid": "1bdf9f1d-22c4-4248-8a7a-5b19b0e45f26",
        "name": "ADMIN",
        "localized_name": "Administrador",
        "permissions": ["platform.manage", "user.read"]
      }
    ],
    "telephone": "+55 11 91234-5678",
    "language": "pt-BR",
    "currency": "BRL",
    "gender": "female",
    "birthday": "1990-01-20T00:00:00+00:00",
    "address": {
      "uuid": "5f2e4f7b-6d9a-4f8a-9648-2f6f79d2cb6e",
      "postal_code": "01310-000",
      "street": "Av. Paulista",
      "number": "1000",
      "district": "Bela Vista",
      "city": "São Paulo",
      "state": "SP",
      "country": "Brasil"
    },
    "nationalities": [
      {
        "uuid": "d5e1f2e9-27d5-4bd8-90c2-1887a57c6510",
        "country": {
          "name": "Brasil",
          "code": "BR",
          "flag": "🇧🇷"
        }
      }
    ]
  }
}
```

## Estrutura JSON Explicada

| Campo                        | Tipo     | Descrição |
| ---------------------------- | -------- | --------- |
| data.uuid                    | uuid     | Identificador do usuário. |
| data.name                    | string   | Nome completo do usuário. |
| data.email                   | string   | Endereço de e-mail cadastrado. |
| data.avatar                  | string   | URL da imagem de avatar. Pode ser `null` se não existir. |
| data.roles[]                 | array    | Presente quando a relação `platformRoles` foi carregada. Cada item expõe `id`, `uuid`, `name`, `localized_name` e `permissions`. |
| data.telephone               | string   | Telefone principal do usuário, quando informado. |
| data.language                | string   | Idioma preferido derivado da regionalização (`LanguageEnum`). |
| data.currency                | string   | Moeda preferida derivada da regionalização (`CurrencyEnum`). |
| data.gender                  | string   | Gênero cadastrado. |
| data.birthday                | string   | Data de nascimento em ISO-8601 (`YYYY-MM-DDTHH:MM:SSZ`). Pode ser `null`. |
| data.address                 | object   | Endereço principal quando a relação `address` foi carregada. Estrutura definida por `AddressResource`. |
| data.nationalities[]         | array    | Lista de nacionalidades associadas ao usuário. Cada item inclui `uuid` e dados do país (`name`, `code`, `flag`). |

## Status HTTP

- 200: Perfil retornado com sucesso.
- 401: Não autenticado (token ausente ou inválido).
- 500: Erro interno inesperado.

## Erros

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Nome da rota: `api.v1.profile`.
- O controlador carrega automaticamente `address`, `avatarImage` e `telephone`. Outros relacionamentos (ex.: `platformRoles`, `nationalities`) só aparecem se estiverem previamente carregados via eager loading em pipelines anteriores.
- O avatar é resolvido por `UserProfileResource::getAvatar()` e pode depender de integrações de armazenamento (S3).

## Relacionados

- [Shared – Atualizar idioma do usuário](UserLanguageUpdate.md)
- [Shared – Atualizar moeda do usuário](UserCurrencyUpdate.md)

## Changelog

- 2025-10-14: Documentação criada para o perfil do usuário.
