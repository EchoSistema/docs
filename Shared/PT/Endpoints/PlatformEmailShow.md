# Plataforma – Endpoint de Exibição de E-mail (IMAP)

## Endpoint

`GET /platform/app/emails/{uid}`

Retorna uma mensagem de e-mail IMAP específica pelo `uid`, com foco nos campos usados pela interface. Cabeçalhos e contagem de anexos são opcionais para preservar performance.

---

## Autenticação

Obrigatória – `auth:sanctum` (Bearer Token válido para o backoffice da plataforma).

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho         | Tipo     | Descrição |
| ----------------- | -------- | --------- |
| Authorization     | `string` | `Bearer {token}`. |
| X-PUBLIC-KEY      | `string` | Chave pública da plataforma. |
| Accept-Language   | `string` | Idioma IETF (ex.: `pt-BR`, `en`, `es`). |

### Parâmetros de Query

| Parâmetro                    | Tipo      | Padrão  | Descrição |
| ---------------------------- | --------- | ------- | --------- |
| `include_headers`            | `boolean` | `false` | Quando `true`, inclui cabeçalhos crus (custo extra). |
| `include_attachment_count`   | `boolean` | `false` | Quando `true`, retorna a contagem de anexos (pode ser custoso em alguns servidores). |

---

## Resposta

A resposta segue o formato padrão com `data`.

### Exemplo (básico)

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Equipe Suporte <suporte@example.com>",
    "from": "suporte@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": null,
    "body_raw": "<html>...conteúdo...</html>",
    "body": "...conteudo em texto...",
    "attachments_count": null
  }
}
```

### Exemplo (com cabeçalhos e anexos)

Requisição: `GET /platform/app/emails/{uid}?include_headers=1&include_attachment_count=1`

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Equipe Suporte <suporte@example.com>",
    "from": "suporte@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": {
      "subject": "Bem-vindo",
      "date": "Tue, 10 Sep 2025 15:43:12 +0000",
      "message_id": "<abc@example.com>",
      "from": "Equipe Suporte <suporte@example.com>",
      "to": "usuario@cliente.com",
      "cc": "",
      "bcc": "",
      "reply_to": "",
      "sender": "Equipe Suporte <suporte@example.com>",
      "content_type": "text/html; charset=UTF-8"
    },
    "body_raw": "<html>...conteúdo...</html>",
    "body": "...conteudo em texto...",
    "attachments_count": 2
  }
}
```

---

## Campos

- `uid` `int` – Identificador IMAP da mensagem.
- `sender_full` `string|null` – Nome e e‑mail do remetente (preferencialmente `Sender`; fallback para `From`).
- `from` `string|null` – Endereço de e‑mail usado como fallback e exibido na UI.
- `date` `string|null` – Data em ISO‑8601.
- `headers` `object|null` – Cabeçalhos crus, somente quando `include_headers=1`.
- `body_raw` `string|null` – HTML (preferencial) ou texto puro quando não houver HTML.
- `body` `string|null` – Texto puro como fallback.
- `attachments_count` `int|null` – Quantidade de anexos, somente quando `include_attachment_count=1`.

---

## Notas

- Flags IMAP não são recuperadas no show para reduzir latência.
- Evite solicitar cabeçalhos e contagem de anexos quando não forem necessários.
- A normalização de data tenta manter ISO‑8601; alguns servidores podem retornar formatos diferentes.
