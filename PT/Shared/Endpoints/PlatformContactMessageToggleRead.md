# Shared – Alternar Status de Leitura da Mensagem

## Endpoint

`PATCH /api/v1/contacts/{message:uuid}/toggle-read`

## Autenticação

Exige token Bearer e cabeçalho de plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Locale para respostas. |

## Parâmetros

Alternar o status de leitura de uma mensagem de contato. Permissões: `show.platform_message` ou `show.all`, exceto quando o usuário autenticado é o autor. Usuários que não são backoffice só podem alternar mensagens da própria plataforma.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/contacts/{message:uuid}/toggle-read"
```

### Sucesso `200 OK`

```json
{
  "data": {
    "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
    "name": "Maria Silva",
    "is_read": true,
    "email": "maria.silva@example.com",
    "created_at": "2024-06-06T18:12:45+00:00"
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data  | `object` | Recurso de mensagem (mesmo do endpoint de detalhe). |

## Status HTTP

- 200: OK
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

- Alternar o status de leitura de uma mensagem de contato

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
