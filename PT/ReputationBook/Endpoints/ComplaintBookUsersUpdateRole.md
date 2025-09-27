# ReputationBook – Usuários do Livro de Reclamações (Atualizar Role)

## Endpoint

```
PUT /api/v1/complaint-book/users/{user}/roles/{role}
```

Atualiza o vínculo de role do usuário na plataforma atual ou altera o status existente.

---

## Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

---

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `Authorization` | string | Sim | Token `Bearer {token}` válido. |
| `X-PUBLIC-KEY` | string | Sim | Identificador público da plataforma. |
| `Accept-Language` | string | Recomendado | Idioma desejado para campos traduzíveis. |

---

## Parâmetros

### Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `user` | uuid | Sim | UUID do usuário. |
| `role` | uuid \| integer \| string | Sim | Identificador do role (UUID, ID ou nome canônico). |

### Corpo (JSON)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `status` | string | Não | Novo status do vínculo (`approved`, `disapproved`, `requested`). |

---

## Notas

- O método `PUT` atua como upsert: cria o vínculo usuário↔role quando inexistente e atualiza apenas o status quando já existe.
- Operações idempotentes: enviar o mesmo payload múltiplas vezes mantém o resultado.

