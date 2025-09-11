# Plataforma – Endpoint de Listagem de E-mails (IMAP)

## Endpoint

`GET /platform/app/emails`

Retorna mensagens de e-mail IMAP da caixa configurada na plataforma. Suporta paginação, ordenação e projeção de campos para melhor performance.

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

| Parâmetro                    | Tipo       | Padrão | Descrição |
| ---------------------------- | ---------- | ------ | --------- |
| `page`                       | `integer`  | `1`    | Página atual. |
| `per_page`                   | `integer`  | `10`   | Itens por página (1–100). |
| `order`                      | `string`   | `desc` | Ordenação cronológica: `asc` ou `desc`. |
| `fields`                     | `string`   | —      | Lista de campos separados por vírgula para projeção (ex.: `subject,sender_name,sender_full,date,uid,seen`). Recomendada para performance. |
| `include_flags`              | `boolean`  | `true` | Inclui flags IMAP quando necessário. É automaticamente inferido quando algum campo de flag for solicitado via `fields` (ex.: `seen`). |
| `include_body`               | `boolean`  | `false`| Inclui corpo do e-mail (custo alto; use apenas quando necessário). |
| `include_body_preview`       | `boolean`  | `false`| Inclui prévia de 300 chars (texto simples). |
| `include_attachment_count`   | `boolean`  | `false`| Conta anexos (pode ser caro em alguns servidores). |
| `include_headers`            | `boolean`  | `false`| Inclui cabeçalhos brutos (custo extra). |

---

## Resposta

A resposta segue o formato padrão com `data` e `meta`.

### Exemplo (projeção enxuta)

Recomendado para o index: `?fields=subject,sender_name,sender_full,date,uid,seen`.

```json
{
  "data": [
    {
      "subject": "Bem-vindo",
      "sender_name": "Equipe Suporte",
      "sender_full": "Equipe Suporte <suporte@example.com>",
      "date": "2025-09-10T15:43:12+00:00",
      "uid": 12345,
      "seen": false
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 10,
    "has_more": true,
    "order": "desc"
  }
}
```

### Exemplo (payload completo)

Sem `fields`, mais campos podem ser retornados (to/cc/bcc/reply_to, tamanho, flags etc.). Use apenas se necessário devido ao custo de parse.

---

## Campos Comuns

- Identidade
  - `subject` `string`
  - `sender` `string|null` – e-mail do remetente (quando solicitado)
  - `sender_full` `string|null` – nome + e-mail
  - `sender_name` `string|null` – nome do remetente
  - `from`/`from_full`/`from_name` – variantes do cabeçalho From (quando solicitados)
- Metadados
  - `date` `string|null` – ISO‑8601
  - `uid` `int|null`
  - `msgno`, `message_id`, `size` (quando solicitados)
- Flags (quando `include_flags` ou solicitadas via `fields`)
  - `flags` `array|null`
  - `seen`, `answered`, `flagged`, `draft`, `recent`, `deleted` `boolean|null`
- Conteúdo (custo alto; solicite apenas quando necessário)
  - `body_preview` `string|null`
  - `body_raw` `string|null`
  - `body` `string|null`
  - `headers` `object|null`
- Anexos (custo variável)
  - `has_attachments` `boolean|null`
  - `attachments_count` `int|null`

---

## Notas

- Utilize `fields` para reduzir latência e uso de memória no servidor e no IMAP.
- `seen` e demais flags só são obtidos quando requisitados, economizando chamadas IMAP.
- `attachments_count` pode ser custoso; ative somente quando necessário.
- Datas são normalizadas para ISO‑8601 quando possível.

