# Shared – Adicionar Item à Lista de Desejos

## Endpoint

```
POST /api/v1/wishlists
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sim         | `application/json`. |

## Parâmetros

### Corpo da requisição

```json
{
  "type": "company",
  "uuid": "550e8400-e29b-41d4-a716-446655440000"
}
```

| Campo | Tipo   | Obrigatório | Descrição |
| ----- | ------ | ----------- | --------- |
| type  | string | Sim         | Tipo do item a adicionar. Valores válidos: `bio`, `property`, `event`, `course`, `product`, `platform`, `company`, `article`, `complaint`, `review`, `country`, `state`, `city` |
| uuid  | string | Sim         | UUID do item a adicionar à lista de desejos |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "company",
    "uuid": "550e8400-e29b-41d4-a716-446655440000"
  }' \
  "https://sandbox.seu-dominio.com/api/v1/wishlists"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "user_id": 1,
    "type": "company",
    "type_id": 123,
    "created_at": "2025-10-17T00:00:00+00:00",
    "updated_at": "2025-10-17T00:00:00+00:00"
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data | object | Dados da entrada da lista de desejos |
| data.uuid | string | Identificador único da entrada |
| data.user_id | integer | ID do usuário proprietário desta entrada |
| data.type | string | Tipo do item salvo |
| data.type_id | integer | ID interno do item salvo |
| data.created_at | string | Data de criação ISO 8601 |
| data.updated_at | string | Data da última atualização ISO 8601 |

## Status HTTP

- 200: Sucesso - Item já está na lista de desejos (retorna a entrada existente)
- 201: Criado - Item adicionado à lista de desejos com sucesso
- 401: Não autenticado - Token inválido ou ausente
- 403: Proibido - Permissões insuficientes
- 404: Não encontrado - Item com o UUID fornecido não encontrado
- 422: Entidade não processável - Tipo inválido ou erro de validação
- 500: Erro interno do servidor

## Erros

### 401 Não autenticado
```json
{
  "message": "Não autenticado."
}
```

### 404 Não encontrado
```json
{
  "message": "Item não encontrado."
}
```

### 422 Entidade não processável
```json
{
  "message": "Tipo inválido.",
  "errors": {
    "type": ["O tipo selecionado é inválido."],
    "uuid": ["O campo uuid deve ser um UUID válido."]
  }
}
```

## Notas

- Se o item já estiver na lista de desejos do usuário, a entrada existente é retornada (sem duplicatas)
- O campo `type` aceita valores enum que mapeiam para diferentes tipos de recursos
- O endpoint resolve automaticamente a classe a partir do tipo e encontra o item pelo UUID
- Tanto `type` quanto `uuid` são campos obrigatórios
- O usuário autenticado se torna o proprietário da entrada da lista de desejos

## Relacionados

- [Listar Lista de Desejos do Usuário](./WishlistIndex.md)
- [Remover Item da Lista de Desejos](./WishlistDestroy.md)

## Changelog

- 2025-10-17: Versão inicial - Adicionar item à lista de desejos com tipo e UUID
