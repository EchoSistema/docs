# Imobiliário — Tipos de Imóvel para Locação: Listagem

## Endpoint

```
GET /real-estate/rental-properties/types
```

## Autenticação

Não é necessária.

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Não         | Auxilia a devolver o título no idioma desejado. |

## Parâmetros de Consulta

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| language  | string | Não         | Idioma para buscar os títulos (fallback para o padrão). |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/rental-properties/types?language=pt-BR"
```

### Resposta (200)

```json
{
  "data": [
    {
      "uuid": "c1927cd4-40c1-4d92-ab2e-4c05bb560001",
      "slug": "apartment",
      "name": "Apartamento"
    },
    {
      "uuid": "f0278cbe-2665-42c3-94c3-05c4371c0002",
      "slug": "loft",
      "name": "Loft"
    }
  ],
  "meta": {
    "domainArea": {
      "uuid": "df742b1d-1be8-4c73-9c5f-3c7076000101",
      "name": "Imobiliário"
    }
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Área de domínio `realestate` não configurada

## Notas

- O endpoint carrega a domain area `realestate` e retorna os tipos associados, respeitando o idioma informado ou o padrão.
- Caso a área não seja encontrada, retorna 404 com a mensagem traduzida `common.not_found`.

## Changelog

- 2025-09-25 — Documentação inicial.
