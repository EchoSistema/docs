# Shared – Exibir Mensagem de Contato

## Endpoint

`GET /api/v1/contacts/{message:uuid}`

## Autenticação

Exige token Bearer e cabeçalho de plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Locale para respostas. |

## Parâmetros

Obter detalhes de uma mensagem de contato específica. Permissões: `show.platform_message` ou `show.all`, exceto quando o usuário autenticado é o autor.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/contacts/{message:uuid}"
```

### Sucesso `200 OK`

```json
{
  "data": {
    "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
    "name": "Maria Silva",
    "is_read": true,
    "email": "maria.silva@example.com",
    "created_at": "2024-06-06T18:12:45+00:00",
    "type": "default",
    "language": "pt-BR",
    "phone": "+55 11912345678",
    "content": "Gostaria de saber mais sobre os planos e integrações disponíveis...",
    "ip_address": "200.200.200.5",
    "images": [ { "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122", "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png", "usage": "platform_contact" } ],
    "user": { "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4", "name": "Maria Silva", "email": "maria.silva@example.com" },
    "platform": { "uuid": "7a3b6d12-4d9f-4f5c-b41b-6ce39e04d57f", "name": "Echosistema" },
    "first_reader": { "uuid": "f39af7fa-6622-4c46-ba0d-fefb699b10f8", "name": "João Admin", "email": "joao.admin@example.com" }
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data  | `object` | Recurso de mensagem. |

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

- Obter detalhes de uma mensagem de contato específica

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
