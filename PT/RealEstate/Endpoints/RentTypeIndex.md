# RealEstate – Listar Tipos de Aluguel

## Endpoint

```
GET /api/v1/real-estate/rent-types
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro de consulta é necessário.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/real-estate/rent-types"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440001",
      "name": "Venda",
      "slug": "sale",
      "description": "Propriedade à venda"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440002",
      "name": "Aluguel",
      "slug": "rent",
      "description": "Propriedade para aluguel"
    },
    {
      "uuid": "aa0e8400-e29b-41d4-a716-446655440003",
      "name": "Locação",
      "slug": "lease",
      "description": "Propriedade para locação"
    }
  ],
  "meta": {
    "total": 3
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de tipos de aluguel/transação |
| data[].uuid | string | Identificador único do tipo de aluguel |
| data[].name | string | Nome do tipo de aluguel (traduzível) |
| data[].slug | string | Identificador amigável para URL do tipo de aluguel |
| data[].description | string | Descrição do tipo de aluguel |
| meta | object | Metadados de resposta |
| meta.total | integer | Número total de tipos de aluguel |

## Status HTTP

- 200: Sucesso
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Retorna todos os tipos de transação disponíveis para a plataforma (venda, aluguel, locação, etc.)
- Os nomes dos tipos de aluguel são traduzíveis e respeitam o cabeçalho `Accept-Language`
- Use o campo `slug` para filtrar propriedades por tipo de transação
- Os resultados não são paginados pois a lista é tipicamente pequena

## Relacionados

- [Listar Propriedades](PropertyIndex.md)
- [Exibir Detalhes da Propriedade](PropertyShow.md)

## Changelog

- 2025-10-16: Documentação inicial
