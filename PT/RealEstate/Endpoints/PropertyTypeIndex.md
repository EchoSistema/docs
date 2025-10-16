# RealEstate – Listar Tipos de Propriedade

## Endpoint

```
GET /api/v1/real-estate/property-types
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
  "https://sandbox.seu-dominio.com/api/v1/real-estate/property-types"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440001",
      "name": "Apartamento",
      "slug": "apartment",
      "description": "Edifício residencial de múltiplas unidades",
      "icon": "building"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440002",
      "name": "Casa",
      "slug": "house",
      "description": "Residência unifamiliar independente",
      "icon": "home"
    },
    {
      "uuid": "990e8400-e29b-41d4-a716-446655440003",
      "name": "Condomínio",
      "slug": "condo",
      "description": "Unidade de propriedade individual em um complexo",
      "icon": "condo"
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
| data[] | array | Lista de tipos de propriedade |
| data[].uuid | string | Identificador único do tipo de propriedade |
| data[].name | string | Nome do tipo de propriedade (traduzível) |
| data[].slug | string | Identificador amigável para URL do tipo de propriedade |
| data[].description | string | Descrição do tipo de propriedade |
| data[].icon | string | Identificador de ícone para exibição na UI |
| meta | object | Metadados de resposta |
| meta.total | integer | Número total de tipos de propriedade |

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

- Retorna todos os tipos de propriedade disponíveis para a plataforma
- Os nomes dos tipos de propriedade são traduzíveis e respeitam o cabeçalho `Accept-Language`
- Use o campo `slug` para filtrar propriedades por tipo
- Os resultados não são paginados pois a lista é tipicamente pequena

## Relacionados

- [Listar Propriedades](PropertyIndex.md)
- [Exibir Detalhes da Propriedade](PropertyShow.md)

## Changelog

- 2025-10-16: Documentação inicial
