# Shared – Listar Mensagens de Contato

## Endpoint

`GET /api/v1/contacts`

## Autenticação

Exige token Bearer e cabeçalho de plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Locale para respostas. |

## Parâmetros

Listar mensagens de contato com filtros e paginação. Permissões: `index.platform_message` ou `index.all`. Usuários que não são backoffice se limitam à sua plataforma; convidados veem apenas seus próprios envios.

### Parâmetros de consulta

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `page` | `integer` | Número da página (padrão 1). |
| `per_page` | `integer` | Itens por página (padrão 25). |
| `no_paginate` | `boolean` | Desativa paginação. |
| `limit` | `integer` | Limite quando `no_paginate` é verdadeiro. |
| `sort_by` | `string` | Coluna de ordenação (padrão `created_at`). |
| `direction` | `string` | `ASC`/`DESC` (padrão `DESC`). |
| `uuid` | `uuid` | Identificador exato. |
| `email` | `email` | E-mail do contato. |
| `phone` | `string` | Telefone informado. |
| `name` | `string` | Nome (busca parcial). |
| `type` | `string` | `default`, `denunciation`, `report`. |
| `message_lang` | `string` | `pt-BR`, `en`, `es`, `gn`. |
| `created_at` | `date` | Data exata (`YYYY-MM-DD`). |
| `interval` | `string` | `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Faixa de datas personalizada. |
| `user` | `string` | ID, UUID, e-mail ou parte do nome do autor. |
| `platform` | `string` | ID, UUID, slug ou parte do nome da plataforma. |
| `ip_address` | `ipv4` | IP do remetente. |
| `ip_banned` | `boolean` | Apenas IPs banidos. |
| `is_robot` | `boolean` | Apenas IPs identificados como robôs. |
| `is_read` | `boolean` | Filtra por status de leitura. |
| `first_reader` | `string` | Identificador do primeiro leitor. |
| `content` | `string` | Busca no corpo da mensagem. |
| `with_images` | `boolean` | Apenas mensagens com anexos. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/contacts"
```

### Exemplo de resposta (200 OK)

```json
{
  "data": [
    {
      "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
      "name": "Maria Silva",
      "is_read": false,
      "email": "maria.silva@example.com",
      "created_at": "2024-06-06T18:12:45+00:00",
      "user": { "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4", "name": "Maria Silva", "email": "maria.silva@example.com" },
      "content": "Gostaria de saber mais sobre os planos..."
    }
  ],
  "links": { "first": "https://api.example.com/api/v1/contacts?page=1", "last": "https://api.example.com/api/v1/contacts?page=5", "prev": null, "next": "https://api.example.com/api/v1/contacts?page=2" },
  "meta": { "current_page": 1, "from": 1, "last_page": 5, "path": "https://api.example.com/api/v1/contacts", "per_page": 25, "to": 25, "total": 112 }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data  | `array` | Lista de mensagens. Metadados de paginação quando aplicável. |

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

- Listar todas as mensagens de contato (com filtros)

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
