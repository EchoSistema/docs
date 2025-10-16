# Learn – Listar Grupos de Categorias Learn

## Endpoint

```
GET /api/v1/learn/category-groups
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

| Parâmetro  | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ---------- | ------- | ----------- | --------- | -------------- |
| language   | string  | Não         | Código de idioma para títulos localizados | Padrão da plataforma |
| categories | boolean | Não         | Incluir categorias na resposta | false |

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/category-groups?language=pt-BR&categories=true"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "resource": "nome-recurso-grupo",
      "domain_area_uuid": "660e8400-e29b-41d4-a716-446655440001",
      "title": {
        "language": "pt-BR",
        "value": "Nome do Grupo de Categoria",
        "is_default": false
      },
      "group": {
        "category": [
          {
            "uuid": "550e8400-e29b-41d4-a716-446655440000",
            "resource": "nome-recurso-categoria",
            "title": {
              "language": "pt-BR",
              "value": "Nome da Categoria"
            }
          }
        ]
      }
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                     | Tipo    | Descrição |
| ------------------------- | ------- | --------- |
| data[]                    | array   | Lista de grupos de categorias |
| data[].uuid               | string  | Identificador único do grupo de categoria |
| data[].resource           | string  | Nome do recurso do grupo de categoria |
| data[].domain_area_uuid   | string  | UUID da área de domínio associada |
| data[].title              | object  | Título localizado do grupo de categoria |
| data[].title.value        | string  | Texto do título |
| data[].title.language     | string  | Código de idioma do título |
| data[].group              | object  | Informações do grupo (quando categories=true) |
| data[].group.category[]   | array   | Lista de categorias neste grupo |

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

- Retorna todos os grupos de categorias para a área de domínio da plataforma
- Use `categories=true` para incluir categorias relacionadas na resposta
- Os títulos são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se um título localizado não estiver disponível, o título padrão é retornado

## Relacionados

- [Listar Categorias Learn](./LearnCategoryIndex.md)
- [Exibir Grupo de Categoria Learn](./LearnCategoryGroupShow.md)

## Changelog

- 2025-10-16: Documentação inicial
