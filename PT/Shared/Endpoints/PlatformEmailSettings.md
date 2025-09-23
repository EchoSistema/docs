# Plataforma — Endpoint de Configurações de E‑mail (IMAP)

## Endpoints

- `GET /platform/app/emails/settings`
- `POST /platform/app/emails/settings`
- `PUT /platform/app/emails/settings`

Gerencia as configurações IMAP da plataforma (host, porta, criptografia, validação, credenciais e pasta padrão).

---

## Autenticação

Obrigatória — `auth:sanctum` (Bearer Token válido para o backoffice da plataforma).

---

## GET /platform/app/emails/settings

Retorna as configurações IMAP atuais da plataforma.

- Respostas
  - `200 OK` com objeto de configurações (sem a senha)
  - `204 No Content` quando não há configuração salva

### Exemplo de Resposta
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "default_folder": "INBOX",
  "timeout": 30
}
```

---

## POST /platform/app/emails/settings

Cria a configuração IMAP para a plataforma (se ainda não existir).

- Regras
  - Falha com `422 Unprocessable Entity` se já houver configuração
  - Campos obrigatórios conforme validação

### Corpo (JSON)
| Campo            | Tipo       | Obrig. | Descrição |
| ---------------- | ---------- | ------ | --------- |
| `host`           | `string`   | Sim    | Host IMAP. |
| `port`           | `integer`  | Sim    | Porta (1–65535). |
| `encryption`     | `string|null` | Não | `ssl`, `tls` ou `null`. |
| `validate_cert`  | `boolean`  | Sim    | Valida certificado TLS. |
| `username`       | `string`   | Sim    | Usuário IMAP. |
| `password`       | `string`   | Sim    | Senha IMAP. |
| `default_folder` | `string`   | Sim    | Pasta padrão (ex.: `INBOX`). |
| `timeout`        | `integer`  | Não    | Timeout em segundos (1–600). Padrão: 30. |

### Exemplo de Requisição
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "password": "secret",
  "default_folder": "INBOX",
  "timeout": 45
}
```

### Respostas
- `201 Created` com mensagem de sucesso
- `422 Unprocessable Entity` se já existir

---

## PUT /platform/app/emails/settings

Atualiza (ou cria, se inexistente) a configuração IMAP da plataforma.

### Corpo (JSON)
Mesmos campos do POST; `password` é opcional (só atualiza se fornecido).

### Respostas
- `200 OK` com mensagem de sucesso
- `422 Unprocessable Entity` se violar validação

---

## Notas

- A senha nunca é retornada por segurança.
- `timeout` padrão é 30s quando omitido.
- Para performance e segurança, prefira usar `INBOX` ou pastas específicas como `All Mail` apenas quando necessário.
- Valores permitidos para `encryption`: `ssl`, `tls` ou `null` (sem criptografia — não recomendado).
