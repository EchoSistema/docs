# Shared – Exibir Área de Domínio

## Endpoint

```
GET /api/v1/backoffice/domain-areas/{domainArea}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição                                    |
| ---------------- | ------ | ----------- | -------------------------------------------- |
| Authorization    | string | Sim         | Credencial `Bearer {token}`.                 |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma.                 |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`).     |

## Parâmetros de Rota

| Parâmetro    | Tipo   | Obrigatório | Descrição                           |
| ------------ | ------ | ----------- | ----------------------------------- |
| domainArea   | string | Sim         | UUID da área de domínio.            |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas/efa68660-4870-51c2-84be-736f87792f1d"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
    "name": "Article",
    "slug": "article",
    "is_active": true,
    "platforms": [
      {
        "uuid": "550e8400-e29b-41d4-a716-446655440000",
        "domain": {
          "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
          "name": "Article"
        },
        "name": "My Platform",
        "email": "platform@example.com",
        "open_to_register": true,
        "public_key": "pk_test_123456",
        "logo": {
          "main": "https://cdn.example.com/logo.png",
          "square": "https://cdn.example.com/logo-square.png",
          "rounded": "https://cdn.example.com/logo-rounded.png",
          "rectangular": "https://cdn.example.com/logo-rect.png"
        },
        "created_at": "2024-01-01T00:00:00+00:00"
      }
    ],
    "categories": [
      {
        "uuid": "123e4567-e89b-12d3-a456-426614174000",
        "title": "Technology",
        "title_is_default": true,
        "title_language": "en",
        "slug": "technology"
      }
    ],
    "item_types": [
      {
        "uuid": "987fcdeb-51a2-43f7-8543-123456789012",
        "default_title": true,
        "language": "en",
        "title": "Blog Post",
        "slug": "blog-post"
      }
    ]
  }
}
```

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição                                           |
| ------------------------------ | ------- | --------------------------------------------------- |
| data                           | object  | Dados da área de domínio                            |
| data.id                        | integer | ID interno da área de domínio                       |
| data.uuid                      | string  | UUID único da área de domínio                       |
| data.name                      | string  | Nome da área de domínio (formatado)                 |
| data.slug                      | string  | Slug identificador da área de domínio               |
| data.is_active                 | boolean | Indica se a área de domínio está ativa              |
| data.platforms                 | array   | Lista de plataformas associadas                     |
| data.platforms[].uuid          | string  | UUID único da plataforma                            |
| data.platforms[].domain        | object  | Dados do domínio da plataforma                      |
| data.platforms[].name          | string  | Nome da plataforma                                  |
| data.platforms[].email         | string  | Email da plataforma                                 |
| data.platforms[].open_to_register | boolean | Indica se está aberta para registro             |
| data.platforms[].public_key    | string  | Chave pública da plataforma                         |
| data.platforms[].logo          | object  | URLs dos logos em diferentes formatos               |
| data.platforms[].created_at    | string  | Data de criação (ISO 8601)                          |
| data.categories                | array   | Lista de categorias da área de domínio              |
| data.categories[].uuid         | string  | UUID único da categoria                             |
| data.categories[].title        | string  | Título da categoria                                 |
| data.categories[].title_is_default | boolean | Indica se é o título padrão                    |
| data.categories[].title_language | string | Idioma do título                                   |
| data.categories[].slug         | string  | Slug da categoria                                   |
| data.item_types                | array   | Lista de tipos de item da área de domínio           |
| data.item_types[].uuid         | string  | UUID único do tipo de item                          |
| data.item_types[].default_title | boolean | Indica se é o título padrão                        |
| data.item_types[].language     | string  | Idioma do título                                    |
| data.item_types[].title        | string  | Título do tipo de item                              |
| data.item_types[].slug         | string  | Slug do tipo de item                                |

## Status HTTP

- 200: OK
- 401: Não autenticado
- 403: Proibido (sem permissões adequadas)
- 404: Área de domínio não encontrada
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Exibe uma área de domínio específica com todas as plataformas, categorias e tipos de item associados
- Requer permissão `show.all` no contexto de backoffice
- O parâmetro `{domainArea}` deve ser o UUID da área de domínio
- As categorias e tipos de item incluem seus títulos localizados

## Relacionados

- [Listar Áreas de Domínio](BackofficeDomainAreaIndex.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Documentação inicial
