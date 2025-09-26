# Imobiliário — Imagens da Plataforma: Listagem

## Endpoint

```
GET /real-estate/platform-images
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
curl -X GET "https://api.exemplo.com/real-estate/platform-images?public_key=realestate-demo"
```

### Resposta (200)

```json
{
  "data": [
    {
      "base": "card",
      "data": {
        "url": "https://cdn.exemplo.com/real-estate/card-default.png",
        "alt": "Imagem padrão"
      },
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 1
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Plataforma não encontrada

## Notas

- A estrutura de `data` pode variar (URL, atributos extras) pois vem da coleção MongoDB `platform_images`.
- A resposta retorna todos os registros associados ao `public_key` de uma só vez.

## Changelog

- 2025-09-25 — Documentação inicial.
