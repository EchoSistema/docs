# Shared – Endpoint de Healthcheck

## Endpoint

`GET /api/v1/health`

Healthcheck simples para balanceadores e monitoramento.

---

## Autenticação

Sem Bearer token. Requer cabeçalho da plataforma.

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| `X-PUBLIC-KEY` | `string` | Chave pública da plataforma, necessária para autenticar e receber respostas. |
| `Accept-Language` | `string` | Localidade opcional para mensagens. |

---

## Exemplo de Resposta

```json
{
  "success": true
}
```

---

## Notas

* Nome da rota: `api.v1.health`.
* `X-PUBLIC-KEY` ausente ou inválido é rejeitado pelo middleware `platform` (401/403 conforme configuração).
