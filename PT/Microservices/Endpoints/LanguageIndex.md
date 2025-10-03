# Microservices – Lista de Idiomas

## Endpoint

```
GET /api/v1/public/languages
```

Retorna as linguagens mais conhecidas do mundo com base no arquivo `public/storage/documents/languages.json`. Pode ser utilizado por frontends publicos que precisem preencher seletores de idioma.

---

## Autenticacao

Nenhuma.

---

## Cabecalhos

| Cabecalho       | Tipo   | Obrigatorio | Descricao |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Nao         | Locale IETF (`pt-BR`, `en`, `es`). |

---

## Parametros

Nenhum parametro.

---

## Exemplos

### Requisicao (curl)

```bash
curl -X GET "https://sandbox.exemplo.com/api/v1/public/languages"
```

### Resposta

```json
{
  "data": [
    {"name": "English", "code": "en"},
    {"name": "Spanish", "code": "es"},
    {"name": "Portuguese", "code": "pt"}
  ]
}
```

---

## Estrutura JSON Explicada

| Campo         | Tipo   | Descricao |
| ------------- | ------ | --------- |
| `data[]`      | array  | Lista de idiomas suportados. |
| `data[].name` | string | Nome do idioma. |
| `data[].code` | string | Codigo ISO-639-1 do idioma. |

---

## Notas

- O endpoint expoe linguagens amplamente utilizadas globalmente.
- Novos idiomas podem ser adicionados editando `public/storage/documents/languages.json`.
- O repositorio `LanguageRepository` realiza o parse desse JSON e ignora erros de leitura.

---

## Changelog

- 2025-10-03: Documentacao inicial do endpoint de linguagens publicas.
