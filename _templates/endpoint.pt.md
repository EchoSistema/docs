# [DOMÍNIO] – [TÍTULO DO ENDPOINT]

## Endpoint

```
[MÉTODO] /api/v1/[CAMINHO]
[OPCIONAL OUTROS MÉTODOS/ROTAS]
```

## Autenticação

[Obrigatória – Bearer {token} com habilidade X | Nenhuma]

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Quando aplicável | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| [id]      | string | Sim         | [descrição curta] |

### Parâmetros de consulta

| Parâmetro | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| --------- | -------- | ----------- | --------- | -------------- |
| [foo]     | string   | Não         | [descrição] | [padrão] |
| [bar]     | integer  | Não         | [descrição] | [1-200] |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. Quando aplicável, informe que o endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

### Corpo da requisição (quando aplicável)

```json
{
  "[campo]": "[valor]"
}
```

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X [MÉTODO] \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/[CAMINHO]?foo=bar"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": 1
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[]       | array    | [lista de itens] |
| data[].id    | integer  | [identificador] |

## Status HTTP

- 200: Sucesso
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

Descreva erros específicos (códigos/mensagens) e exemplos de payload:

```json
{
  "message": "[mensagem]",
  "errors": {
    "campo": ["[detalhe]"]
  }
}
```

## Notas

- [Regras de negócio, defaults, paginação/ordenação, limites]

## Relacionados

- [Link relativo para endpoint correlato](../Endpoints/OutroEndpoint.md)

## Changelog

- 2025-08-31: [descrição da mudança]

