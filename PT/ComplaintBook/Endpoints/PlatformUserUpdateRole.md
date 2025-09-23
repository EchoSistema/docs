# Livro de Reclamações – Usuários (Atualizar Role)

## Endpoint

`PUT /api/v1/complaint-book/users/{user}/roles/{role}`

Atualiza o role do usuário na plataforma atual e/ou o status do role.

---

## Autenticação

Obrigatória – Bearer token com habilidade `backoffice` e `domain:reputationbook`.

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

| Parâmetro | Tipo                | Obrigatório  | Descrição |
| --------- |---------------------|--------------| --------- |
| `user` | `uuid`              | Sim          | UUID do usuário. |
| `role` | `uuid, int, string` | Sim | Pode ser o UUID, ID ou o nome. |

### Body (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `status` | `string` | Não | Um de: `approved`, `disapproved`, `requested`. |

---

## Notas

- Usa PUT e funciona como upsert: cria o vínculo usuário↔role quando ausente; atualiza o status quando presente. Idempotente para o mesmo payload.
