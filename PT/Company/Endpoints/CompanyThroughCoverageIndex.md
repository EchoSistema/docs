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
| `relations` | `array` | Não | Inclui relações adicionais: `logos`, `contacts`, `fiscalDocuments`, `address`. |
| `no_paginate` | `boolean` | Não | Desativa a paginação. Aceita `true`, `false`, `0` ou `1`. |
| `per_page` | `integer` | Não | Itens por página quando paginado. |
| `page` | `integer` | Não | Número da página quando paginado. |
| `sort_by` | `string` | Não | Coluna para ordenação. |
| `order_by` | `string` | Não | Direção da ordenação (`ASC` ou `DESC`). |
| `with_count` | `array` | Não | Relações para contagem (ex.: `complaints`, `reviews`). |
| `with_exists` | `array` | Não | Relações para verificar existência (ex.: `complaints`, `reviews`). |
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
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "Empresa Exemplo",
      "assumed_name": "Nome Fantasia Exemplo",
      "slug": "empresa-exemplo",
      "created_at": "2025-01-01T00:00:00-03:00",
      "address": "Rua Exemplo 123, Cidade Exemplo, EX",
      "fiscal_documents": [
        {
          "id": 1,
          "type": "ruc",
          "value": "00000001"
        }
      ],
      "logos": [
        {
          "unique_id": "123456",
          "usage": "logo",
          "url": "https://example.com/logo.svg",
          "name": "logo.svg",
          "slug": "logo",
          "width": 250,
          "height": 250,
          "creation_date": "2025-01-01 00:00:00"
        }
      ],
      "contacts": [
        {
          "id": 1,
          "type": "email",
          "email": "contato@exemplo.com"
        },
        {
          "id": 2,
          "type": "telephone",
          "phone": "+1 234 567 8900",
          "country_code": "+1",
          "number": "234 567 8900"
        }
      ],
      "complaints_count": 0,
      "reviews_count": 0,
      "complaints_exists": false,
      "reviews_exists": false
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
| `data[].address` | `string` | Endereço da empresa quando `relations` inclui `address`. |
| `data[].fiscal_documents[]` | `array` | Documentos fiscais quando `relations` inclui `fiscalDocuments`. |
| `data[].fiscal_documents[].id` | `integer` | ID do documento fiscal. |
| `data[].fiscal_documents[].type` | `string` | Tipo do documento fiscal. |
| `data[].fiscal_documents[].value` | `string` | Valor do documento fiscal. |
| `data[].logos[]` | `array` | Logos da empresa quando `relations` inclui `logos`. |
| `data[].contacts[]` | `array` | Contatos da empresa quando `relations` inclui `contacts`. |
| `data[].complaints_count` | `integer` | Contagem de reclamações quando `with_count` inclui `complaints`. |
| `data[].reviews_count` | `integer` | Contagem de avaliações quando `with_count` inclui `reviews`. |
| `data[].complaints_exists` | `boolean` | Indica se há reclamações quando `with_exists` inclui `complaints`. |
| `data[].reviews_exists` | `boolean` | Indica se há avaliações quando `with_exists` inclui `reviews`. |

---

## Notas

* Metadados de paginação são omitidos quando `no_paginate` for verdadeiro.
* Relações, contagens e verificações de existência aparecem apenas quando solicitadas.
* Filtros de string são correspondências parciais sem distinção entre maiúsculas e minúsculas, salvo indicação em contrário.

