# Learn – Listar Categorias Learn

## Endpoint

```
GET /api/v1/learn/categories
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

### Parâmetros de consulta

| Parâmetro | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------ | ----------- | --------- | -------------- |
| language  | string | Não         | Código de idioma para títulos localizados | Padrão da plataforma |

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/categories?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "resource": "nome-recurso-categoria",
      "domain_area": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "Learn"
      },
      "group": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "resource": "nome-recurso-grupo",
        "title": {
          "language": "pt-BR",
          "value": "Nome do Grupo de Categoria"
        }
      },
      "title": {
        "language": "pt-BR",
        "value": "Nome da Categoria",
        "is_default": false
      }
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                | Tipo    | Descrição |
| -------------------- | ------- | --------- |
| data[]               | array   | Lista de categorias |
| data[].uuid          | string  | Identificador único da categoria |
| data[].resource      | string  | Nome do recurso da categoria |
| data[].domain_area   | object  | Informações da área de domínio |
| data[].group         | object  | Informações do grupo de categoria |
| data[].group.title   | object  | Título localizado do grupo |
| data[].title         | object  | Título localizado da categoria |
| data[].title.value   | string  | Texto do título |
| data[].title.language| string  | Código de idioma do título |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Chave de plataforma inválida"
}
```

## Notas

- Retorna todas as categorias para a área de domínio da plataforma
- As categorias incluem seu grupo associado e área de domínio
- Os títulos são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se um título localizado não estiver disponível, o título padrão é retornado

## Relacionados

- [Listar Grupos de Categorias Learn](./LearnCategoryGroupIndex.md)
- [Exibir Grupo de Categoria Learn](./LearnCategoryGroupShow.md)

## Changelog

- 2025-10-16: Documentação inicial
