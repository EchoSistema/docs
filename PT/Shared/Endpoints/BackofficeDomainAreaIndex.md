# Shared – Listar Áreas de Domínio

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro adicional necessário.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "efa68660-4870-51c2-84be-736f87792f1d",
      "name": "Article",
      "slug": "article",
      "is_active": true,
      "platforms_total": 5,
      "categories_total": 12,
      "item_types_total": 8
    },
    {
      "id": 2,
      "uuid": "70f8067c-7d68-50ef-b847-c43e3d6c0878",
      "name": "Bio",
      "slug": "bio",
      "is_active": true,
      "platforms_total": 3,
      "categories_total": 0,
      "item_types_total": 0
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                      | Tipo    | Descrição                                           |
| -------------------------- | ------- | --------------------------------------------------- |
| data                       | array   | Lista de áreas de domínio                           |
| data[].id                  | integer | ID interno da área de domínio                       |
| data[].uuid                | string  | UUID único da área de domínio                       |
| data[].name                | string  | Nome da área de domínio (formatado)                 |
| data[].slug                | string  | Slug identificador da área de domínio               |
| data[].is_active           | boolean | Indica se a área de domínio está ativa              |
| data[].platforms_total     | integer | Quantidade total de plataformas associadas          |
| data[].categories_total    | integer | Quantidade total de categorias associadas           |
| data[].item_types_total    | integer | Quantidade total de tipos de item associados        |

## Status HTTP

- 200: OK
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Lista todas as áreas de domínio com contadores de plataformas, categorias e tipos de item
- Endpoint exclusivo para backoffice com permissão `index.all`
- Retorna todas as áreas de domínio de uma vez (não paginado)

## Relacionados

- [Exibir Área de Domínio](BackofficeDomainAreaShow.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Atualização da documentação com estrutura completa de resposta
- 2025-10-16: Documentação inicial
