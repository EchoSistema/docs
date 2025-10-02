# Microservices – Países

Coleção de endpoints públicos para consulta de países, estados e cidades, além da operação autenticada que força a atualização dos metadados no catálogo local.

## 1. Listar países

```
GET /api/v1/public/countries
```

### Autenticação

Nenhuma.

### Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição                                             |
| --------------- | ------ | ----------- | ----------------------------------------------------- |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma consumidora.              |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`). Controla mensagens. |

### Parâmetros de consulta

| Parâmetro    | Tipo    | Obrigatório | Descrição                                                                 | Padrão/Valores                  |
| ------------ | ------- | ----------- | ------------------------------------------------------------------------- | ------------------------------- |
| name         | string  | Não         | Nome comum do país (`raw.name`). Aceita variantes `camelCase`/`kebab-case`. | —                               |
| code         | string  | Não         | Código ISO alpha-2 (2 letras).                                            | —                               |
| cca3         | string  | Não         | Código ISO alpha-3 (3 letras).                                            | —                               |
| ccn3         | string  | Não         | Código ISO numérico.                                                      | —                               |
| cioc         | string  | Não         | Código do Comitê Olímpico Internacional.                                  | —                               |
| capital      | string  | Não         | Nome da capital registrada.                                               | —                               |
| currency     | string  | Não         | Código de moeda (ex.: `USD`, `BRL`).                                      | —                               |
| languages    | string  | Não         | Código de idioma (ISO 639‑1).                                             | —                               |
| timezone     | string  | Não         | Identificador de fuso horário (ex.: `America/Sao_Paulo`).                 | —                               |
| minimum      | bool    | Não         | Quando `true`, omite `details` no payload.                                | `false`                         |
| no_paginate  | bool    | Não         | Retorna todos os registros em uma única lista.                            | `false`                         |
| per_page     | int     | Não         | Tamanho da página (quando paginado).                                      | `25`                            |
| order_by     | string  | Não         | Campo usado na ordenação ASC. Aceita `orderBy`.                            | `name`                          |

### Exemplo de requisição

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <public-key>" \
  "https://api.sandbox.echosistema.online/api/v1/public/countries?currency=EUR&minimum=true&per_page=10"
```

### Exemplo de resposta (paginada)

```json
{
  "data": [
    {
      "id": 33,
      "uuid": "f5b1f5d2-3e7e-4cd9-b7d2-271f7b567890",
      "code": "PT",
      "name": "Portugal",
      "official_name": "República Portuguesa"
    }
  ],
  "links": {
    "first": "https://api.sandbox.echosistema.online/api/v1/public/countries?page=1",
    "last": "https://api.sandbox.echosistema.online/api/v1/public/countries?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "path": "https://api.sandbox.echosistema.online/api/v1/public/countries",
    "per_page": 10,
    "to": 1,
    "total": 1
  }
}
```

Quando `minimum=false`, cada item inclui `details` com o JSON bruto retornado pela RestCountries.

### Estrutura JSON

| Campo                      | Tipo     | Descrição                                                                    |
| -------------------------- | -------- | ---------------------------------------------------------------------------- |
| data[]                     | array    | Lista de países compatível com `CountryResource`.                            |
| data[].id                  | integer  | Identificador interno.                                                       |
| data[].uuid                | string   | UUID derivado do código ISO.                                                 |
| data[].code                | string   | Código ISO alpha-2.                                                          |
| data[].name                | string   | Nome amigável (ajustado para UTF-8).                                         |
| data[].official_name       | string   | Nome oficial.                                                                |
| data[].details             | object   | (Opcional) Payload bruto no formato RestCountries quando `minimum=false`.    |
| links, meta                | object   | Metadados de paginação padrão do Laravel.                                   |

### Status HTTP

- 200 – Sucesso.
- 400 – Parâmetros de filtro inválidos.
- 429 – Limite de requests excedido.
- 500 – Erro interno.

### Notas

- Parâmetros aceitam variações em `snake_case`, `camelCase` e `kebab-case`.
- `no_paginate=true` ignora `per_page` e remove `links/meta`.

---

## 2. Detalhar país

```
GET /api/v1/public/countries/{country}
```

### Autenticação

Nenhuma.

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição                                                                       |
| --------- | ------ | ----------- | ------------------------------------------------------------------------------- |
| country   | string | Sim         | Identificador do país: ID numérico, UUID, código ISO (alpha-2) ou nome comum. |

### Exemplo de resposta

```json
{
  "data": {
    "id": 33,
    "uuid": "f5b1f5d2-3e7e-4cd9-b7d2-271f7b567890",
    "code": "PT",
    "name": "Portugal",
    "official_name": "República Portuguesa",
    "details": {
      "capital": ["Lisboa"],
      "region": "Europe"
    }
  }
}
```

### Notas

- A resposta segue o mesmo formato de `CountryResource` (com `details`, salvo uso do parâmetro `minimum` na listagem).
- Retorna 404 quando o país não existe ou não é encontrado na RestCountries.

---

## 3. Detalhar estado de um país

```
GET /api/v1/public/countries/{country}/states/{state}
```

### Autenticação

Nenhuma.

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição                                                                                   |
| --------- | ------ | ----------- | ------------------------------------------------------------------------------------------- |
| country   | string | Sim         | ID, UUID, código ISO (2 letras) ou nome do país.                                            |
| state     | string | Sim         | ID, UUID, `slug` ou nome do estado. Correspondência de nome e slug é case-insensitive.      |

### Exemplo de resposta

```json
{
  "data": {
    "id": 489,
    "uuid": "910c19f0-8d6a-4c5c-96f0-e0b8d1234567",
    "country_id": 33,
    "name": "Lisboa",
    "abbreviation": "LX",
    "region": "Lisboa",
    "country": {
      "uuid": "f5b1f5d2-3e7e-4cd9-b7d2-271f7b567890",
      "name": "Portugal",
      "official_name": "República Portuguesa",
      "code": "PT"
    }
  }
}
```

### Notas

- Retorna 404 se o estado não pertencer ao país informado.
- Campos `cities` podem ser carregados em fluxos internos quando o repositório adiciona o relacionamento.

---

## 4. Detalhar cidade

```
GET /api/v1/public/countries/{country}/states/{state}/cities/{city}
```

### Autenticação

Nenhuma.

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição                                                                                         |
| --------- | ------ | ----------- | ------------------------------------------------------------------------------------------------- |
| country   | string | Sim         | ID, UUID, código ISO (2 letras) ou nome do país.                                                  |
| state     | string | Sim         | ID, UUID, `slug` ou nome do estado (busca case-insensitive para nome/slug).                       |
| city      | string | Sim         | ID, UUID, `slug` ou nome da cidade (busca case-insensitive para nome/slug).                       |

### Exemplo de resposta

```json
{
  "data": {
    "id": 20491,
    "uuid": "4e7c1bde-43b8-4c9c-9c1e-18fcbabc1234",
    "state_id": 489,
    "country_id": 33,
    "name": "Sintra",
    "abbreviation": null,
    "country": {
      "uuid": "f5b1f5d2-3e7e-4cd9-b7d2-271f7b567890",
      "name": "Portugal",
      "official_name": "República Portuguesa",
      "code": "PT"
    },
    "state": {
      "uuid": "910c19f0-8d6a-4c5c-96f0-e0b8d1234567",
      "name": "Lisboa",
      "abbreviation": "LX",
      "region": "Lisboa"
    }
  }
}
```

### Notas

- O endpoint usa `LIKE` case-insensitive quando `state` ou `city` aparentam ser nomes/`slug`.
- Retorna 404 se a cidade não for encontrada dentro do estado/país informados.

---

## 5. Atualizar metadados de um país

```
PUT /api/v1/public/countries/{filter}
```

### Autenticação

Obrigatória (Sanctum). O token deve possuir a habilidade que permite acionar microservices administrativos.

### Cabeçalhos adicionais

| Cabeçalho     | Tipo   | Obrigatório | Descrição                           |
| ------------- | ------ | ----------- | ----------------------------------- |
| Authorization | string | Sim         | Token `Bearer {token}` válido.      |

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição                                                                                     |
| --------- | ------ | ----------- | --------------------------------------------------------------------------------------------- |
| filter    | string | Sim         | ID, UUID, código ISO alpha-2 ou nome do país a ser sincronizado com a RestCountries API.      |

### Corpo da requisição

Sem corpo. O serviço busca os dados atualizados na RestCountries e salva/atualiza o país localmente.

### Exemplo de resposta

```json
{
  "data": {
    "id": 33,
    "uuid": "f5b1f5d2-3e7e-4cd9-b7d2-271f7b567890",
    "code": "PT",
    "name": "Portugal",
    "official_name": "República Portuguesa",
    "details": {
      "capital": ["Lisboa"],
      "currencies": {
        "EUR": {
          "code": "EUR"
        }
      }
    }
  }
}
```

### Status HTTP

- 200 – País sincronizado localmente.
- 401 – Token ausente ou inválido.
- 404 – País não encontrado na base local nem na RestCountries.
- 429 – Limite de requests ou rate-limit da RestCountries excedido.
- 500 – Erro interno.

### Notas

- A sincronização dispara jobs internos para atualizar estados/cidades associados.
- Recomenda-se evitar chamadas em lote sem intervalo devido ao rate limit da RestCountries.

---

## Changelog

- 2025-10-02: Documento inicial dos endpoints públicos de países.
