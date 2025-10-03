# Compartilhado – Listagem de Idiomas Disponiveis

## Endpoint

`GET /api/v1/languages`

Retorna os idiomas suportados pelo backoffice a partir do enum `LanguageEnum`, incluindo o codigo (`code`) e o nome exibido (`native_name`).

---

## Autenticacao

Nenhuma.

---

## Requisicao

Nao ha parametros.

---

## Exemplo de Resposta

```json
{
  "data": [
    {
      "name": "PT_BR",
      "code": "pt-BR",
      "native_name": "Portugues (Brasil)"
    },
    {
      "name": "EN",
      "code": "en",
      "native_name": "English"
    }
  ]
}
```

---

## Estrutura JSON Explicada

| Campo | Tipo | Descricao |
| ----- | ---- | --------- |
| `data[]` | `array` | Lista de idiomas suportados. |
| `data[].name` | `string` | Nome do enum (`PT_BR`, `EN`, `ES`, `GN`). |
| `data[].code` | `string` | Codigo ISO associado ao idioma. |
| `data[].native_name` | `string` | Nome apresentado do idioma. |

---

## Notas

* A ordem segue a declaracao do enum `LanguageEnum`.

---

## Changelog

- 2025-10-03: Documentacao inicial do endpoint `/api/v1/languages`.
