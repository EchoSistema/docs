# Learn – Exibir Grupo de Categoria Learn

## Endpoint

```
GET /api/v1/learn/category-groups/{categoryGroup}
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Não         | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| categoryGroup | string | Sim         | Identificador de recurso do grupo de categoria |

> O parâmetro `categoryGroup` usa vinculação de modelo de rota com o campo `resource`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/category-groups/nome-recurso-grupo"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "770e8400-e29b-41d4-a716-446655440002",
    "resource": "nome-recurso-grupo",
    "domain_area_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "title": {
      "language": "pt-BR",
      "value": "Nome do Grupo de Categoria",
      "is_default": false
    }
  }
}
```

## Estrutura JSON Explicada

| Campo                   | Tipo    | Descrição |
| ----------------------- | ------- | --------- |
| data                    | object  | Detalhes do grupo de categoria |
| data.uuid               | string  | Identificador único do grupo de categoria |
| data.resource           | string  | Nome do recurso do grupo de categoria |
| data.domain_area_uuid   | string  | UUID da área de domínio associada |
| data.title              | object  | Título localizado do grupo de categoria |
| data.title.value        | string  | Texto do título |
| data.title.language     | string  | Código de idioma do título |
| data.title.is_default   | boolean | Se este é o título padrão |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado - Grupo de categoria não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Grupo de categoria não encontrado"
}
```

## Notas

- Retorna um único grupo de categoria por seu identificador de recurso
- O título é localizado com base no cabeçalho `Accept-Language`
- Se um título localizado não estiver disponível, o título padrão é retornado

## Relacionados

- [Listar Grupos de Categorias Learn](./LearnCategoryGroupIndex.md)
- [Listar Categorias Learn](./LearnCategoryIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
