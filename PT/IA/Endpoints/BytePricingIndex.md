# IA — Preço por Byte: Listagem

Lista produtos precificados por byte com conteúdo localizado e preço padrão.

## Endpoint

```
GET /ia/admin/pricing/bytes
```

## Autenticação

Obrigatória — Bearer {token} com Sanctum.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}` |
| Accept-Language  | string | Não         | Locale IETF (`pt-BR`, `en`, `es`, ...) |

## Parâmetros

### Parâmetros de caminho

Nenhum.

### Parâmetros de consulta

| Parâmetro | Tipo | Obrigatório | Descrição        | Padrão |
| --------- | ---- | ----------- | ---------------- | ------ |
| page      | int  | Não         | Número da página | 1      |

## Exemplos

### Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes?page=1"
```

### Resposta (200)

```json
{
  "data": [
    {
      "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
      "measurement_type": {
        "id": "byte",
        "name": "BYTE",
        "title": "Byte"
      },
      "title": "Preço por Byte",
      "slug": "byte_price",
      "description": null,
      "language": "pt-BR",
      "price": 29900,
      "currency": "BRL",
      "formatted_price": "R$\u00a0299,00",
      "created_at": "2025-09-26T04:46:04-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes?page=1",
    "last": "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "links": [
      {
        "url": null,
        "label": "\u00ab Anterior",
        "active": false
      },
      {
        "url": "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Pr\u00f3ximo \u00bb",
        "active": false
      }
    ],
    "path": "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## Estrutura JSON Explicada

| Campo                              | Tipo        | Descrição |
| ---------------------------------- | ----------- | --------- |
| data[]                             | array       | Lista paginada de produtos precificados por byte |
| data[].uuid                        | string      | Identificador do produto |
| data[].measurement_type            | object      | Metadados do tipo de medida |
| data[].measurement_type.id         | string      | Identificador do enum (`byte`) |
| data[].measurement_type.name       | string      | Nome do enum (`BYTE`) |
| data[].measurement_type.title      | string      | Rótulo legível |
| data[].title                       | string|null | Título localizado do produto |
| data[].slug                        | string|null | Slug usado internamente (esperado `byte_price`) |
| data[].description                 | string|null | Descrição localizada opcional |
| data[].language                    | string|null | Locale associado ao título padrão |
| data[].price                       | integer|null | Preço padrão na menor unidade (ex.: centavos) |
| data[].currency                    | string|null | Código ISO da moeda |
| data[].formatted_price             | string|null | Preço formatado |
| data[].created_at                  | string|null | Data de criação (ISO 8601) |
| links                              | object      | Links de paginação |
| meta                               | object      | Metadados de paginação |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 429: Limite de requests excedido
- 500: Erro interno

## Notas

- Retorna apenas produtos com `measurement_type = byte`.
- Os rótulos dos links de paginação seguem o locale configurado no backend.
- O tamanho da página é fixo em 25 registros.
- A coleção pode incluir chaves adicionais quando o atributo `raw` do produto estiver preenchido.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alinhado ao modelo de preço único e título imutável.
- 2025-09-26: Exemplo de resposta e tabela de campos revisados.
