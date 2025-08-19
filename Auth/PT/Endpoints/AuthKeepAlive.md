# Auth – Endpoint Manter Sessão

## Endpoint

`GET /api/v1/auth/keep-alive`

Verifica se o token fornecido é válido e mantém a sessão do usuário ativa.

---

## Autenticação

Requer um token Bearer válido.

### Cabeçalhos Necessários

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **Authorization** | `string` | `Bearer <token>` obrigatório. |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma necessária para autenticar e receber respostas. |
| **Accept-Language** | `string` | Locale para mensagens de resposta. |

---

## Exemplo de Resposta

```json
{
  "success": true,
  "message": "Sessão do usuário mantida ativa."
}
```

---

## Notas

* A mensagem de resposta é traduzida conforme `Accept-Language`.

