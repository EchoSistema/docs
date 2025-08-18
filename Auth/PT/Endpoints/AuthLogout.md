# Auth – Endpoint de Logout

## Endpoint

`POST /auth/logout`

Encerra a sessão do usuário invalidando o token de acesso.

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
  "message": "Usuário :name desconectado com sucesso."
}
```

---

## Notas

* A mensagem de resposta é traduzida conforme `Accept-Language`.

