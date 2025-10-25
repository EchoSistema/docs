# Shared – Exibir Detalhes da Plataforma

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de URL

| Parâmetro | Tipo | Obrigatório | Descrição |
| ----------- | ------- | ----------- | ----------- |
| platform    | string  | Sim         | UUID da plataforma a ser exibida |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "total": {
      "users": 150
    },
    "domain": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "E-commerce"
    },
    "name": "Plataforma Demo",
    "email": "contato@plataforma.com",
    "open_to_register": true,
    "public_key": "pk_test_123456789",
    "logo": {
      "main": "https://cdn.example.com/logos/main.png",
      "square": "https://cdn.example.com/logos/square.png",
      "rounded": "https://cdn.example.com/logos/rounded.png",
      "rectangular": "https://cdn.example.com/logos/rectangular.png"
    },
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | object  | Dados da plataforma |
| data.uuid   | string  | Identificador único da plataforma |
| data.total  | object  | Totalizadores da plataforma |
| data.total.users | integer | Total de usuários na plataforma |
| data.domain | object  | Informações da área de domínio |
| data.domain.uuid | string  | Identificador único da área de domínio |
| data.domain.name | string  | Nome da área de domínio |
| data.name   | string  | Nome da plataforma |
| data.email  | string  | Email de contato da plataforma |
| data.open_to_register | boolean | Indica se a plataforma está aberta para novos registros |
| data.public_key | string  | Chave pública da plataforma |
| data.logo   | object  | URLs dos logos da plataforma |
| data.logo.main | string  | URL do logo principal |
| data.logo.square | string  | URL do logo quadrado |
| data.logo.rounded | string  | URL do logo arredondado |
| data.logo.rectangular | string  | URL do logo retangular |
| data.created_at | string  | Data de criação (ISO 8601) |

## Status HTTP

- 200: OK
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Este endpoint retorna os detalhes de uma plataforma específica
- Apenas usuários com permissão de backoffice podem acessar
- Inclui contagem de usuários da plataforma (users_count)
- O parâmetro {platform} aceita o UUID da plataforma

## Relacionados

- [Listar Plataformas](BackofficePlatformIndex.md)
- [Listar Usuários da Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Atualização completa da documentação com estrutura detalhada
- 2025-10-16: Documentação inicial
