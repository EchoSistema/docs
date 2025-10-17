# Shared – Listar Lista de Desejos do Usuário

## Endpoint

```
GET /api/v1/wishlists
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------- | ----------- | --------- | -------------- |
| per_page  | integer | Não         | Número de itens por página | 25 |
| page      | integer | Não         | Número da página | 1 |

> O endpoint aceita variantes do nome do parâmetro: `per_page`, `perPage`, `per-page`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/wishlists?per_page=25&page=1"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "user_id": 1,
      "type": "company",
      "type_id": 123,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "user_id": 1,
      "type": "product",
      "type_id": 456,
      "created_at": "2025-10-17T00:00:00+00:00",
      "updated_at": "2025-10-17T00:00:00+00:00"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/wishlists?page=1",
    "last": "https://api.example.com/api/v1/wishlists?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/wishlists?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 120
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data | array | Array de itens da lista de desejos |
| data[].uuid | string | Identificador único da entrada da lista de desejos |
| data[].user_id | integer | ID do usuário proprietário desta entrada |
| data[].type | string | Tipo do item na lista de desejos (bio, property, event, course, product, platform, company, article, complaint, review, country, state, city) |
| data[].type_id | integer | ID interno do item salvo |
| data[].created_at | string | Data de criação ISO 8601 |
| data[].updated_at | string | Data da última atualização ISO 8601 |
| links | object | Links de paginação |
| links.first | string | URL para a primeira página |
| links.last | string | URL para a última página |
| links.prev | string\|null | URL para a página anterior |
| links.next | string\|null | URL para a próxima página |
| meta | object | Metadados de paginação |
| meta.current_page | integer | Número da página atual |
| meta.from | integer | Número do primeiro item na página atual |
| meta.last_page | integer | Número total de páginas |
| meta.per_page | integer | Itens por página |
| meta.to | integer | Número do último item na página atual |
| meta.total | integer | Número total de itens na lista de desejos |

## Status HTTP

- 200: Sucesso - Lista de desejos recuperada com sucesso
- 401: Não autenticado - Token inválido ou ausente
- 403: Proibido - Permissões insuficientes
- 500: Erro interno do servidor

## Erros

### 401 Não autenticado
```json
{
  "message": "Não autenticado."
}
```

### 403 Proibido
```json
{
  "message": "Esta ação não está autorizada."
}
```

## Notas

- Retorna apenas as listas de desejos do usuário autenticado
- Os resultados são ordenados por mais recentes primeiro (latest)
- A paginação padrão é de 25 itens por página
- Itens excluídos (soft-deleted) não são incluídos nos resultados

## Relacionados

- [Adicionar Item à Lista de Desejos](./WishlistStore.md)
- [Remover Item da Lista de Desejos](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Versão inicial - Listar lista de desejos do usuário com paginação
