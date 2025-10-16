# Learn – Obter Texto do Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/text/{usage}
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

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| event     | string | Sim         | Identificador de texto do evento (não o nome do recurso) |
| usage     | string | Sim         | Tipo de uso do texto (ex.: description, about, terms) |

### Parâmetros de consulta

| Parâmetro   | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ----------- | ------ | ----------- | --------- | -------------- |
| platform_id | string | Não         | UUID da plataforma para validação de propriedade | Derivado de X-PUBLIC-KEY |

> O parâmetro `event` usa vinculação de campo `text`. O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/evento-texto-123/text/description"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "usage": "description",
    "content": "Este é um curso abrangente cobrindo todos os aspectos do desenvolvimento web...",
    "language": "pt-BR",
    "is_default": false,
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-10-10T14:20:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo              | Tipo    | Descrição |
| ------------------ | ------- | --------- |
| data               | object  | Dados do conteúdo de texto |
| data.uuid          | string  | Identificador único do texto |
| data.usage         | string  | Tipo de uso do texto |
| data.content       | string  | Conteúdo do texto |
| data.language      | string  | Código de idioma do texto |
| data.is_default    | boolean | Se este é o texto padrão |
| data.created_at    | string  | Data/hora de criação (ISO 8601) |
| data.updated_at    | string  | Data/hora da última atualização (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido - A plataforma não possui este evento
- 404: Não encontrado - Evento ou texto não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Texto não encontrado"
}
```

```json
{
  "message": "Este evento não pertence à plataforma especificada"
}
```

## Notas

- Retorna um conteúdo de texto específico para um evento com base no tipo de uso
- O evento deve pertencer à plataforma que faz a requisição
- A propriedade da plataforma é validada usando o parâmetro `platform_id` ou derivado de `X-PUBLIC-KEY`
- Tipos de uso comuns incluem: `description`, `about`, `terms`, `prerequisites`, `objectives`
- O conteúdo do texto é retornado com base no cabeçalho `Accept-Language` quando disponível

## Relacionados

- [Exibir Evento](./EventShow.md)
- [Listar Eventos](./EventIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
