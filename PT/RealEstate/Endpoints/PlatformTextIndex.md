# RealEstate – Listar Textos da Plataforma

## Endpoint

```
GET /api/v1/real-estate/platform-texts
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
  "https://sandbox.seu-dominio.com/api/v1/real-estate/platform-texts"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440001",
      "key": "welcome_message",
      "content": "Encontre a casa dos seus sonhos conosco",
      "usage": "homepage",
      "language": "pt-BR"
    },
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440002",
      "key": "about_us",
      "content": "Somos uma plataforma imobiliária líder...",
      "usage": "about",
      "language": "pt-BR"
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
| data[] | array | Lista de textos da plataforma |
| data[].uuid | string | Identificador único do texto |
| data[].key | string | Chave identificadora do texto para referência |
| data[].content | string | Conteúdo do texto |
| data[].usage | string | Contexto de uso do texto (homepage, about, footer, etc.) |
| data[].language | string | Código do idioma do texto |
| meta | object | Metadados de resposta |
| meta.total | integer | Número total de textos |

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

- Retorna todos os textos específicos da plataforma para personalização
- Os textos são filtrados pelo cabeçalho `X-PUBLIC-KEY`
- O conteúdo respeita o cabeçalho `Accept-Language` para traduções
- Use o campo `key` para identificar textos específicos na sua aplicação
- Os resultados não são paginados pois a lista é tipicamente pequena

## Relacionados

- [Listar Imagens da Plataforma](PlatformImageIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
