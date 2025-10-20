# Shared – Enviar Imagem de Mensagem de Contato

## Endpoint

`POST /api/v1/contact/{message}/images`

## Autenticação

Nenhuma (público). Requer cabeçalho de plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| X-PUBLIC-KEY | `string` | Sim | Chave pública da plataforma. |
| Content-Type | `multipart/form-data` | Condicional | Necessário ao enviar `image_file`; pode ser `application/json` ao usar URLs/Base64. |

## Parâmetros

Enviar uma imagem anexada a uma mensagem de contato.

Parâmetro de rota:
- `message` (uuid) — identificador da mensagem retornado na criação.

### Corpo

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `usage` | `string` | Sim | Finalidade da imagem (ex.: `platform_contact`). |
| `name` | `string` | Obrigatório sem `image_url` | Nome original do arquivo. |
| `image_url` | `url` | Obrigatório sem `image_file`/`image_encoded` | URL pública (máx. 500). |
| `image_encoded` | `string` | Obrigatório sem `image_file`/`image_url` | Imagem codificada em Base64. |
| `image_file` | `file` | Obrigatório sem `image_url`/`image_encoded` | `jpeg,png,jpg,gif,svg,webp,heic,heif` até 2 MB. |
| `unique_id` | `string` | Não | Chave opcional de deduplicação. |
| `type` | `string` | Não | Categoria livre.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/contact/{message}/images"
```

### Sucesso `200 OK`

```json
{
  "data": {
    "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
    "unique_id": "5f2c19f4",
    "usage": "platform_contact",
    "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
    "name": "comprovante.png",
    "slug": "comprovante-png",
    "width": 1280,
    "height": 720,
    "creation_date": "2024-06-01T13:45:00+00:00"
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data  | `object` | Dados do recurso de imagem. |

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

- Enviar uma imagem anexada a uma mensagem de contato

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
