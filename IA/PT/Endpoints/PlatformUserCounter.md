# Inteligência Artificial – Contador de Usuários Admin

## Endpoint

`GET /api/v1/ia/admin/users/counter`

Retorna o número total de usuários da plataforma por cargo que correspondem aos filtros aplicados na área administrativa de Inteligência Artificial. O comportamento e os filtros são os mesmos do endpoint Shared Users Counter.

Veja também: docs/domain/shared/http/controllers/platform/platform-user-controller.counter.md

---

## Autenticação

Obrigatória – Token Bearer com habilidade `backoffice`.

---

## Parâmetros de Consulta

Aceita os mesmos parâmetros de filtro do AI Users Index (e do Shared Users Index). Consulte:

- docs/domain/artificial-intelligence/http/controllers/platform-user-controller.index.md
- docs/domain/shared/http/controllers/platform/platform-user-controller.index.md

---

## Resposta

Idêntica à resposta do Shared Users Counter. Exemplo:

```json
{
  "counter": {
    "admin": {
      "role_id": 2,
      "name": "admin",
      "localized_name": "Administrator",
      "total": 10
    }
  }
}
```

Para mais detalhes, veja:

- docs/domain/shared/http/controllers/platform/platform-user-controller.counter.md
