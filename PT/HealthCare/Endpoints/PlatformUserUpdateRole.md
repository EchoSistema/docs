# Saúde – Usuários (Atualizar Papel)

## Endpoint

`PUT /api/v1/health-care/users/{user}/roles/{role}`

Atualiza o papel do usuário na plataforma atual e/ou o status do papel.

---

## Autenticação

Obrigatória – Bearer token com habilidade `backoffice` e `domain:healthcare`.

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

## Exemplo

```bash
curl -X PUT \
  "/api/v1/health-care/users/123/roles/admin" \
  -H "Authorization: Bearer {token}" \
  -H "X-PUBLIC-KEY: {public_key}" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{"status": "approved"}'
```

---

## Status HTTP

- 200, 401, 403, 404, 409, 422

---

## Notas

- Usa PUT e funciona como upsert: cria o vínculo usuário↔papel quando ausente; atualiza o status quando presente. Idempotente para o mesmo payload.
