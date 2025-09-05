# Livro de Reclamações – Usuários (Atualizar Papel)

## Endpoint

`PUT /api/v1/complaint-book/users/{user}/roles/{role}`

Atualiza o papel do usuário na plataforma atual e/ou o status do papel.

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

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `user` | `int|uuid|string` | Sim | Identificador do usuário. |
| `role` | `int|uuid|string` | Sim | Identificador do papel (ID, UUID ou nome). |

### Body (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `status` | `string` | Não | Um de: `approved`, `disapproved`, `requested`. |

---

## Notas

- Usa PUT e funciona como upsert: cria o vínculo usuário↔papel quando ausente; atualiza o status quando presente. Idempotente para o mesmo payload.
