# Newsletter — Inscrição

## Endpoint

`POST /api/v1/newsletters`

Recebe solicitações públicas de assinatura de newsletter e armazena os registros associados à plataforma informada.

---

## Autenticação

Sem Bearer token. Requer cabeçalho da plataforma para identificar o tenant.

### Cabeçalhos Obrigatórios
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| `X-PUBLIC-KEY` | `string` | Identificador público da plataforma. |

### Cabeçalhos Opcionais
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| `Accept-Language` | `string` | Define o locale; usado para preencher `language`. |

---

## Corpo (JSON)
| Campo            | Tipo     | Req. | Descrição |
| ---------------- | -------- | ---- | --------- |
| `name`           | `string` | Não  | Nome do assinante. Opcional. |
| `email`          | `email`  | Sim  | Endereço de e-mail do assinante. |
| `g_recaptcha`    | `string` | Sim  | Token reCAPTCHA validado pelo `ReCaptchaRule`. |
| `platform_uuid`  | `uuid`   | Sim  | UUID da plataforma que receberá a inscrição. |
| `is_active`      | `bool`   | Auto | Fixado como `true` pelo backend; valores enviados são ignorados. |
| `language`       | `string` | Auto | Definido a partir do locale atual (`Accept-Language` ou padrão da aplicação). |

### Exemplo de Requisição
```json
{
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "g_recaptcha": "token-recaptcha",
  "platform_uuid": "11111111-1111-1111-1111-111111111111"
}
```

---

## Respostas

### Sucesso — 201 Created
```json
{
  "message": "O e-mail foi salvo com sucesso"
}
```

### Erro de Validação — 422 Unprocessable Entity
```json
{
  "message": "O e-mail não foi salvo",
  "errors": {
    "email": ["O campo email é obrigatório."],
    "g_recaptcha": ["Falha ao validar o reCAPTCHA."]
  }
}
```

### Bloqueio por Robô — 402 Payment Required
Enviado quando o agente de usuário identificado corresponde a um robô.

---

## Notas
- Nome da rota: `api.v1.newsletters.store`.
- Cada inscrição dispara uma notificação para o Slack definido em `config('backoffice.slack.channel.default')`.
- Os atributos adicionais (`language`, `is_active`) são calculados internamente através de `NewsletterStoreRequest`.
- Os registros são persistidos por `Newsletter::createWithAttributes`, após preparação via `PrepareNewsletterData`.
