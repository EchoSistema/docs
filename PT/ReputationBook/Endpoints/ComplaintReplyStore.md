# ReputationBook – Criar resposta de reclamação

## Endpoint

```
POST /api/v1/complaints/{complaint}/replies
```

Cria uma nova resposta para a reclamação identificada pelo parâmetro de caminho `complaint`.

---

## Autenticação

Obrigatória – `Bearer {token}` autenticado via `auth:sanctum`. Nenhuma habilidade específica é exigida.

---

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição | Valores |
| --------- | ---- | ----------- | --------- | ------- |
| `Authorization` | string | Sim | Credencial `Bearer {token}`. |
| `X-PUBLIC-KEY` | string | Sim | Identifica a plataforma emissora. |
| `Accept-Language` | string | Recomendado | Idioma IETF da interface do cliente (ex.: `pt-BR`, `en`, `es`). |

---

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo | Obrigatório | Descrição | Padrão/Valores |
| --------- | ---- | ----------- | --------- | -------------- |
| `complaint` | string | Sim | Identificador da reclamação. Aceita ID numérico ou UUID. | — |

### Corpo da requisição (JSON)

| Campo | Tipo | Obrigatório | Descrição | Padrão/Valores |
| ----- | ---- | ----------- | --------- | -------------- |
| `user` | string | Sim | Referência ao autor da resposta. Aceita ID numérico, UUID ou e-mail existentes do usuário. | — |
| `company` | string | Não | Identificador da empresa quando a resposta for emitida por ela. Aceita ID, UUID ou slug. | — |
| `text` | string | Sim | Conteúdo principal da resposta (5–5000 caracteres). | — |
| `language` | string | Sim | Idioma da resposta em formato IETF (ex.: `pt-BR`). | `app.locale` |
| `is_platform_support` | boolean | Não | Quando `true`, indica que a resposta foi registrada por um agente de suporte da plataforma. | `false` |

> Os nomes dos campos são canônicos em `snake_case`, mas o endpoint aceita variantes `camelCase`, `kebab-case` e `CapitalCase`.

---

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST "https://api.sandbox.echosistema.com/api/v1/complaints/0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5/replies" \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-client-id>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "user": "cliente@example.com",
    "text": "Estamos analisando o seu caso e retornaremos em breve.",
    "language": "pt-BR",
    "is_platform_support": false
  }'
```

### Exemplo de resposta (200)

```json
{
  "data": {
    "uuid": "4f0d5906-8aa5-3f44-8eb8-0b2936ccaf8c",
    "index": 2,
    "is_support": false,
    "content": {
      "content": "Estamos analisando o seu caso e retornaremos em breve.",
      "language": "pt-BR"
    },
    "user": {
      "uuid": "b02527ac-47f4-4c0e-9157-1f917bf923c7",
      "name": "Maria Silva"
    },
    "company": {
      "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
      "name": "Loja Exemplo"
    },
    "platform": {
      "uuid": "8b3dd5bb-1d6b-48ce-84b4-3097f3d5343f",
      "name": "EchoSistema"
    }
  }
}
```

---

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data.uuid` | uuid | Identificador da resposta criada. |
| `data.index` | integer | Ordem da resposta dentro da reclamação. |
| `data.is_support` | boolean | Indica se a resposta foi marcada como suporte (plataforma). |
| `data.content.content` | string | Texto principal associado à resposta. |
| `data.content.language` | string | Idioma do texto padrão da resposta. |
| `data.user.uuid` | uuid | Identificador do usuário autor. |
| `data.user.name` | string | Nome do usuário autor. |
| `data.company.uuid` | uuid | (Opcional) Empresa vinculada à resposta. |
| `data.company.name` | string | (Opcional) Nome da empresa. |
| `data.platform.uuid` | uuid | (Opcional) Plataforma associada à reclamação. |
| `data.platform.name` | string | (Opcional) Nome da plataforma. |

---

## Status HTTP

- 200: Resposta criada com sucesso.
- 400: Não foi possível localizar a reclamação, usuário ou outro recurso relacionado.
- 401: Token ausente ou inválido.
- 404: Reclamação não encontrada para o identificador informado.
- 422: Erro de validação nos campos enviados.
- 500: Erro interno inesperado.

---

## Erros

```json
{
  "message": "usuário não encontrado"
}
```

---

## Notas

- O identificador da reclamação suporta ID numérico e UUID graças ao binding dinâmico.
- Quando `is_platform_support=false` e a reclamação possui plataforma associada, a resposta é marcada automaticamente como suporte (`is_support=true`).
- O índice (`index`) é calculado automaticamente considerando as respostas existentes da reclamação.
- Campos opcionais, como `company` e `platform`, são preenchidos apenas quando as relações estão carregadas e válidas.

---

## Relacionados

- [Visão geral do domínio](../README.md)
- [Índice de endpoints ReputationBook](README.md)

---

## Changelog

- 2025-09-26: Documento inicial para o endpoint de criação de respostas de reclamação.
