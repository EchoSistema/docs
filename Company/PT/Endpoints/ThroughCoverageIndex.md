# Company – Listar Empresas pela Cobertura

## Endpoint

`GET /api/v1/company/through-coverage`

Recupera empresas disponíveis para a cobertura da plataforma atual. Suporta filtros, ordenação e paginação.

---

## Autenticação

Nenhuma.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma necessária para autenticar e receber respostas. |
| **Accept-Language** | `string` | Idioma para campos traduzíveis (ex.: `en`, `pt-BR`). Opcional. |

### Parâmetros de Filtro

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `id` | `integer` | Não | ID da empresa. |
| `uuids` | `array[uuid]` | Não | Filtra por UUIDs específicos de empresas. |
| `name` | `string` | Não | Nome da empresa. |
| `slug` | `string` | Não | Slug da empresa. |
| `fiscal_document_type` | `FiscalDocumentTypeEnum` | Não | Tipo de documento fiscal. |
| `fiscal_document_value` | `string` | Não | Valor do documento fiscal. |
| `no_paginate` | `boolean` | Não | Desativa a paginação quando `true`. |
| `per_page` | `integer` | Não | Itens por página quando paginado. |
| `sort_by` | `string` | Não | Coluna para ordenação. |
| `order_by` | `string` | Não | Direção da ordenação (`ASC` ou `DESC`). |
| `with_count` | `array` | Não | Relacionamentos para agregação de contagem. |
| `no_complaints` | `boolean` | Não | Empresas sem reclamações. |
| `has_complaints` | `boolean` | Não | Empresas com reclamações. |
| `no_reviews` | `boolean` | Não | Empresas sem avaliações. |
| `has_reviews` | `boolean` | Não | Empresas com avaliações. |
| `rating` | `float` | Não | Filtro por média de avaliação. |
| `good_rating` | `boolean` | Não | Filtra empresas com boas avaliações. |
| `bad_rating` | `boolean` | Não | Filtra empresas com avaliações ruins. |

> Parâmetros aceitam variações em camelCase, snake_case, kebab-case ou CapitalCase.

---

## Exemplo de Resposta

```json
{
  "data": [
    {
      "created_by": null,
      "uuid": "5ca829a0-aab2-3991-a8de-817d33b7dbcf",
      "name": "1 Empresa XPTO",
      "assumed_name": null,
      "slug": "1-empresa-xpto",
      "created_at": "2025-06-30T05:11:58-03:00",
      "address": "General Cabañas, 233, Juan León Mallorquín, Encarnación, Itapuá, PY",
      "fiscal_documents": [
        {
          "id": 1002,
          "type": "ruc",
          "value": "123561"
        }
      ]
    }
  ]
}
```

---

## Estrutura JSON

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | `array` | Lista de resumos de empresas. |
| `data[].uuid` | `uuid` | Identificador da empresa. |
| `data[].name` | `string` | Nome da empresa. |
| `data[].slug` | `string` | Slug amigável para URL. |
| `data[].assumed_name` | `string` | Nome fantasia, se houver. |
| `data[].created_at` | `datetime` | Data de criação. |
| `data[].address` | `string` | Endereço da empresa. |
| `data[].fiscal_documents[]` | `array` | Lista de documentos fiscais. |
| `data[].fiscal_documents[].id` | `integer` | ID do documento fiscal. |
| `data[].fiscal_documents[].type` | `string` | Tipo do documento fiscal. |
| `data[].fiscal_documents[].value` | `string` | Valor do documento fiscal. |

---

## Notas

* Metadados de paginação são omitidos quando `no_paginate=true`.
* Filtros de string são correspondências parciais sem distinção entre maiúsculas e minúsculas, salvo indicação em contrário.

