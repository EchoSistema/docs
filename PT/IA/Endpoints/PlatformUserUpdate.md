# IA – Usuários Admin (Atualizar)

## Endpoint

`PUT /api/v1/ia/admin/users/{user:uuid}`

Atualiza os campos de perfil do usuário: `name`, `email`, `gender`, `birth_date` e `password`.

---

## Autenticação

Obrigatória – Bearer token com habilidade `backoffice`.

---

## Headers

| Header | Tipo | Obrigatório | Descrição |
| ------ | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traduzíveis. |

---

## Parâmetros

### Path

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `user` | `uuid` | Sim | UUID do usuário. |

### Body (JSON)

Ao menos um dos campos abaixo deve ser enviado.

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Não | Nome completo. Máx. 255. |
| `email` | `string` | Não | E-mail único; `email` e `email:dns`. |
| `gender` | `string` | Não | Um de: `m`, `f`, `o`. |
| `birth_date` | `date` | Não | Data ISO-8601. |
| `password` | `string` | Não | Mín. 8 caracteres. Requer `password_confirmation`. |
| `password_confirmation` | `string` | Não | Deve coincidir com `password`. |

