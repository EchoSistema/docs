# Compartilhado – Endpoint de Listagem de Roles

## Endpoint

`GET /api/v1/roles`

Lista os roles disponíveis. Os roles dependem do domínio da plataforma solicitante. Parâmetros opcionais permitem incluir ou excluir roles específicos.

---

## Autenticação

Nenhuma.

---

## Request

### Parâmetros de consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `roles` | `array` | Não | Inclui apenas estes roles. Os valores devem ser nomes de roles válidos. |
| `except` | `array` | Não | Exclui estes roles do resultado. |
| `permissions` | `boolean` | Não | Quando `true`, inclui as permissões do role na resposta. Valor padrão `false`. |

> Os parâmetros aceitam variantes camelCase, snake_case, kebab-case ou CapitalCase.

---

## Exemplo de Resposta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "admin",
      "title": "Administrador",
      "permissions": ["role.read", "role.write"]
    }
  ]
}
```

---

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | `array` | Lista de roles. |
| `data[].id` | `integer` | Identificador do role. |
| `data[].uuid` | `uuid` | Identificador único do role. |
| `data[].name` | `string` | Nome técnico do role. |
| `data[].title` | `string` | Título localizado do role. |
| `data[].permissions[]` | `array` | Permissões do role quando `permissions=true`. |

---

## Notas

* Os roles retornados dependem do domínio da plataforma solicitante.
* O parâmetro `permissions` tem valor padrão `false`.
