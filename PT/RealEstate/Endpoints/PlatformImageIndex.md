# RealEstate – Listar Imagens da Plataforma

## Endpoint

```
GET /api/v1/real-estate/platform-images
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
  "https://sandbox.seu-dominio.com/api/v1/real-estate/platform-images"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440001",
      "key": "hero_banner",
      "url": "https://example.com/images/hero-banner.jpg",
      "alt": "Banner Principal da Plataforma Imobiliária",
      "usage": "homepage",
      "width": 1920,
      "height": 1080
    },
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440002",
      "key": "logo",
      "url": "https://example.com/images/logo.png",
      "alt": "Logotipo da Plataforma",
      "usage": "branding",
      "width": 200,
      "height": 80
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de imagens da plataforma |
| data[].uuid | string | Identificador único da imagem |
| data[].key | string | Chave identificadora da imagem para referência |
| data[].url | string | URL da imagem |
| data[].alt | string | Texto alternativo para acessibilidade |
| data[].usage | string | Contexto de uso da imagem (homepage, branding, etc.) |
| data[].width | integer | Largura da imagem em pixels |
| data[].height | integer | Altura da imagem em pixels |
| meta | object | Metadados de resposta |
| meta.total | integer | Número total de imagens |

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

- Retorna todas as imagens específicas da plataforma para personalização
- As imagens são filtradas pelo cabeçalho `X-PUBLIC-KEY`
- Use o campo `key` para identificar imagens específicas na sua aplicação
- Os resultados não são paginados pois a lista é tipicamente pequena
- As imagens são otimizadas e servidas através de CDN

## Relacionados

- [Listar Textos da Plataforma](PlatformTextIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
