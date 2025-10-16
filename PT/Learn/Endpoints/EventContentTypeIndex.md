# Learn – Listar Tipos de Conteúdo de Evento

## Endpoint

```
GET /api/v1/learn/events/content-types
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Não         | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro adicional necessário.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/content-types"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": "video",
      "name": "Vídeo"
    },
    {
      "id": "audio",
      "name": "Áudio"
    },
    {
      "id": "text",
      "name": "Texto"
    },
    {
      "id": "pdf",
      "name": "Documento PDF"
    },
    {
      "id": "quiz",
      "name": "Questionário"
    },
    {
      "id": "assignment",
      "name": "Tarefa"
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo       | Tipo   | Descrição |
| ----------- | ------ | --------- |
| data[]      | array  | Lista de tipos de conteúdo |
| data[].id   | string | Identificador do tipo de conteúdo |
| data[].name | string | Nome localizado do tipo de conteúdo |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Chave de plataforma inválida"
}
```

## Notas

- Retorna todos os tipos de conteúdo de evento disponíveis
- Os nomes dos tipos de conteúdo são localizados com base no cabeçalho `Accept-Language`
- A lista é ordenada alfabeticamente por nome
- Estes tipos são usados ao criar ou filtrar conteúdos de eventos
- Tipos de conteúdo comuns incluem: vídeo, áudio, texto, PDF, questionário, tarefa, interativo, sessão ao vivo

## Relacionados

- [Listar Conteúdos do Evento](./EventContentIndex.md)
- [Exibir Conteúdo do Evento](./EventContentShow.md)

## Changelog

- 2025-10-16: Documentação inicial
