# Imobiliário — Tipos de Locação: Listagem

## Endpoint

```
GET /real-estate/rent-types
```

## Autenticação

Não é necessária.

## Parâmetros de Consulta

| Parâmetro  | Tipo   | Obrigatório | Descrição |
| ---------- | ------ | ----------- | --------- |
| public_key | string | Sim         | Chave pública da plataforma (`pbk`). |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/rent-types?public_key=realestate-demo"
```

### Resposta (200)

```json
{
  "data": [
    {
      "title": "Diária",
      "value": "daily",
      "pbk": "realestate-demo"
    },
    {
      "title": "Mensal",
      "value": "monthly",
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Plataforma não encontrada

## Notas

- Dados provenientes da coleção MongoDB `rent_types`.
- A resposta retorna todos os registros sem paginação.

## Changelog

- 2025-09-25 — Documentação inicial.
