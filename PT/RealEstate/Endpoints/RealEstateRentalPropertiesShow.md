# Imobiliário — Imóveis para Locação: Detalhe

## Endpoint

```
GET /real-estate/rental-properties/{rentalProperty}
```

## Autenticação

Apenas garante que o imóvel pertence à plataforma ativa. Não há necessidade de token Bearer.

## Parâmetros de Caminho

| Parâmetro      | Tipo | Obrigatório | Descrição |
| -------------- | ---- | ----------- | --------- |
| rentalProperty | uuid | Sim         | UUID ou identificador aceito pelo binding do `RentableItem`. |

## Parâmetros de Consulta

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| language  | string | Não         | Idioma para títulos e textos (`pt-BR`, `en`, `es`, ...). |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/rental-properties/c51823c1-8d41-477e-a3b7-7108faae0001?language=pt-BR"
```

### Resposta (200)

```json
{
  "data": {
    "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
    "payment_interval": "monthly",
    "type": {
      "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
      "name": "Apartamento mobiliado"
    },
    "creator": {
      "uuid": "49287c4a-32aa-483c-8d4f-98cfd8300101",
      "name": "Administrador de Locação"
    },
    "title": "Estúdio próximo ao metrô",
    "description": "Estúdio mobiliado com contas incluídas.",
    "renter": "Skyline Rentals",
    "renter_type": "company",
    "image": {
      "usage": "card",
      "url": "https://cdn.exemplo.com/rental-properties/c51823c1/card.jpg",
      "name": "cover"
    },
    "images": [
      {
        "usage": "gallery",
        "url": "https://cdn.exemplo.com/rental-properties/c51823c1/gallery-1.jpg",
        "name": "sala",
        "width": 1920,
        "height": 1080
      }
    ],
    "address": {
      "street": "Av. Paulista, 1000",
      "district": "Bela Vista",
      "city": "São Paulo",
      "state": "SP",
      "country": "BR",
      "zipcode": "01310-100",
      "formatted": "Av. Paulista, 1000 — Bela Vista, São Paulo/SP"
    },
    "is_available": true
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 403 — Imóvel não pertence à plataforma ativa
- 404 — Imóvel não encontrado

## Notas

- O controller valida se o locador (`renter`) é a empresa da plataforma atual (`App::platform()->company`).
- Campos traduzidos respeitam `language` com fallback para o idioma padrão.
- Além da imagem de card, a resposta inclui a galeria completa quando disponível.

## Changelog

- 2025-09-25 — Documentação inicial.
