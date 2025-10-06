# Plataforma - Mensagens de Contato

## Visão Geral

Endpoints responsáveis pelo fluxo de mensagens enviadas via formulário de contato da plataforma. Permitem registrar o contato público (com validação de reCAPTCHA), anexar imagens ao ticket recém-criado e, no backoffice autenticado, listar, visualizar e marcar mensagens como lidas.

## Endpoints

| Método | Caminho | Descrição | Autenticação |
| ------ | ------- | --------- | ------------ |
| `POST` | `/api/v1/contact` | Registra uma nova mensagem de contato. | Não exige token; requer `X-PUBLIC-KEY`. |
| `POST` | `/api/v1/contact/{message}/images` | Adiciona imagem vinculada à mensagem. | Não exige token; requer `X-PUBLIC-KEY`. |
| `GET` | `/api/v1/contacts` | Lista mensagens com filtros/paginação. | `auth:sanctum` + `X-PUBLIC-KEY`. |
| `GET` | `/api/v1/contacts/{message}` | Mostra detalhes completos da mensagem. | `auth:sanctum` + `X-PUBLIC-KEY`. |
| `PATCH` | `/api/v1/contacts/{message}/toggle-read` | Alterna o status de leitura da mensagem. | `auth:sanctum` + `X-PUBLIC-KEY`. |

> **Permissões Backoffice**
> - Listagem exige `index.platform_message` ou `index.all` na role da plataforma.
> - Exibição e toggle exigem `show.platform_message` ou `show.all`, exceto quando o usuário é o autor da mensagem.
> - Usuários que não são backoffice só acessam mensagens da própria plataforma; convidados visualizam apenas as próprias submissões.

---

## Criar mensagem (`POST /api/v1/contact`)

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `X-PUBLIC-KEY` | `string` | Sim | Chave pública da plataforma (também aceita `public_key` na query). |
| `Content-Type` | `application/json` | Sim | Corpo JSON. |
| `Accept-Language` | `string` | Não | Locale para mensagens de validação (`pt-BR`, `en`, `es`, `gn`). |

### Corpo (`application/json`)

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| `type` | `enum` | Sim | `default`, `denunciation` ou `report`. |
| `language` | `enum` | Sim | Idioma do formulário: `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Sim | Token verificado via `ReCaptchaRule`. |
| `email` | `email` | Condicional | Obrigatório quando `phone` não é enviado. |
| `phone` | `string` | Condicional | Obrigatório quando `email` não é enviado. |
| `content` | `string` | Sim | Mensagem (máx. 5000 caracteres). |
| `name` | `string` | Não | Nome de quem enviou. |
| `user_uuid` | `uuid` | Não | Associa a mensagem a um usuário existente. |

> O IP do visitante é inferido automaticamente (`ip_address`). Se `anonymous=true` for informado, o serviço usa `anonymous@pigeons.email` como e-mail padrão.

### Exemplo de requisição

```json
{
    "type": "default",
    "language": "pt-BR",
    "g_recaptcha": "token_do_recaptcha",
    "email": "maria.silva@example.com",
    "content": "Gostaria de saber mais sobre os planos.",
    "name": "Maria Silva"
}
```

### Respostas

- **201 Created**
  ```json
  {
      "data": {
          "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e"
      }
  }
  ```
- **422 Unprocessable Entity** - falha de validação (campos obrigatórios ou reCAPTCHA).
- **403 Forbidden** - `X-PUBLIC-KEY` inválido/inexistente.

---

## Anexar imagem (`POST /api/v1/contact/{message}/images`)

Permite anexar arquivos ao contato logo após a criação. O identificador `{message}` corresponde ao `uuid` retornado na criação.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `X-PUBLIC-KEY` | `string` | Sim | Chave pública da plataforma. |
| `Content-Type` | `multipart/form-data` | Condicional | Necessário ao enviar `image_file`; pode ser `application/json` ao usar URLs/Base64. |

### Corpo (campos principais)

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| `usage` | `string` | Sim | Finalidade da imagem (ex.: `platform_contact`). |
| `name` | `string` | Condicional | Requerido quando `image_url` não é informado. |
| `image_url` | `url` | Condicional | URL pública da imagem (`<= 500` chars). |
| `image_encoded` | `string` | Condicional | Conteúdo Base64 da imagem. |
| `image_file` | `file` | Condicional | Upload (`jpeg,png,jpg,gif,svg,webp,heic,heif` até 2 MB). |
| `unique_id` | `string` | Não | Identificador opcional para deduplicação. |
| `type` | `string` | Não | Categoria livre da imagem. |

> É obrigatório prover **exatamente um** dos campos `image_url`, `image_encoded` ou `image_file`.

### Resposta de sucesso (201 Created)

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

- **404 Not Found** - quando o `uuid` não pertence à plataforma.
- **422 Unprocessable Entity** - payload inválido ou tipo de arquivo não suportado.

---

## Listar mensagens (`GET /api/v1/contacts`)

Retorna coleção paginada, limitada às permissões do usuário autenticado.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `Authorization` | `Bearer <token>` | Sim | Token Sanctum válido. |
| `X-PUBLIC-KEY` | `string` | Sim | Plataforma atual. |
| `Accept-Language` | `string` | Não | Locale das mensagens. |

### Parâmetros de query

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `page` | `integer` | Página atual (padrão `1`). |
| `per_page` | `integer` | Itens por página (padrão `25`). |
| `no_paginate` | `boolean` | Quando `true`, retorna coleção simples. |
| `limit` | `integer` | Limite de itens quando `no_paginate=true`. |
| `sort_by` | `string` | Coluna de ordenação (padrão `created_at`). |
| `direction` | `string` | Direção `ASC`/`DESC` (padrão `DESC`). |
| `uuid` | `uuid` | Filtra por identificador exato. |
| `email` | `email` | E-mail do contato. |
| `phone` | `string` | Telefone informado. |
| `name` | `string` | Nome (like). |
| `type` | `string` | Tipo (`default`, `denunciation`, `report`). |
| `message_lang` | `string` | Idioma da mensagem (`pt-BR`, `en`, `es`, `gn`). |
| `created_at` | `date` | Data exata (`YYYY-MM-DD`). |
| `interval` | `string` | Intervalos rápidos: `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Faixa personalizada por data. |
| `user` | `string` | ID, UUID, e-mail ou trecho do nome do autor. |
| `platform` | `string` | ID, UUID, slug ou trecho do nome da plataforma. |
| `ip_address` | `ipv4` | IP do remetente. |
| `ip_banned` | `boolean` | Apenas IPs marcados como banidos. |
| `is_robot` | `boolean` | Apenas IPs identificados como robôs. |
| `is_read` | `boolean` | Filtra por status de leitura. |
| `first_reader` | `string` | Identificador do primeiro leitor. |
| `content` | `string` | Busca textual no conteúdo. |
| `with_images` | `boolean` | Apenas mensagens com anexos. |

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
            "user": {
                "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
                "name": "Maria Silva",
                "email": "maria.silva@example.com"
            },
            "content": "Gostaria de saber mais sobre os planos..."
        }
    ],
    "links": {
        "first": "https://api.example.com/api/v1/contacts?page=1",
        "last": "https://api.example.com/api/v1/contacts?page=5",
        "prev": null,
        "next": "https://api.example.com/api/v1/contacts?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 5,
        "path": "https://api.example.com/api/v1/contacts",
        "per_page": 25,
        "to": 25,
        "total": 112
    }
}
```

> O campo `content` é truncado (220 caracteres) na listagem. Use o endpoint de detalhe para obter o conteúdo completo.

- **403 Forbidden** - usuário sem permissão na plataforma.

---

## Detalhar mensagem (`GET /api/v1/contacts/{message}`)

Retorna todos os campos da mensagem especificada.

### Resposta de sucesso (200 OK)

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
        "images": [
            {
                "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
                "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
                "usage": "platform_contact"
            }
        ],
        "user": {
            "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
            "name": "Maria Silva",
            "email": "maria.silva@example.com"
        },
        "platform": {
            "uuid": "7a3b6d12-4d9f-4f5c-b41b-6ce39e04d57f",
            "name": "Echosistema"
        },
        "first_reader": {
            "uuid": "f39af7fa-6622-4c46-ba0d-fefb699b10f8",
            "name": "João Admin",
            "email": "joao.admin@example.com"
        }
    }
}
```

- **403 Forbidden** - usuário sem permissão e que não é autor.
- **404 Not Found** - mensagem inexistente na plataforma.

---

## Alternar leitura (`PATCH /api/v1/contacts/{message}/toggle-read`)

Inverte o estado `is_read` da mensagem e registra o primeiro leitor (`first_reader`) quando aplicável. Retorna o mesmo payload do endpoint de detalhe.

- **200 OK** - mensagem atualizada.
- **403 Forbidden** - sem permissão ou plataforma divergente.

---

## Boas práticas e considerações

- Sempre enviar `X-PUBLIC-KEY` ou `public_key` com o valor correto da plataforma.
- Após criar a mensagem, aguarde o `uuid` da resposta antes de anexar imagens.
- Anexos são limitados a 2 MB por arquivo. Utilize `image_url` para anexos maiores já hospedados.
- Use filtros (`is_read=false`, `with_images=true`, etc.) para priorizar filas importantes no backoffice.
- Trate respostas 403 como ausência de permissão na role atual; revise as permissões atribuídas ao usuário.
