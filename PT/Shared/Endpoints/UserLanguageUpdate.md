# Shared – Atualizar idioma do usuário

## Endpoint

```
PATCH /api/v1/user/language
```

Atualiza o idioma preferido do usuário autenticado, persistindo a informação na tabela de regionalização.

## Autenticação

Obrigatória – Bearer {token} (Laravel Sanctum).

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | Credencial `Bearer {token}` válida para o usuário atual. |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Corpo da requisição

```json
{
  "language": "pt-BR"
}
```

| Campo      | Tipo   | Obrigatório | Descrição |
| ---------- | ------ | ----------- | --------- |
| language   | string | Sim         | Código da enum `LanguageEnum` (`pt-BR`, `en`, `es`, `gn`). |

## Exemplos

### Requisição (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{"language":"pt-BR"}' \
  "https://sandbox.echosistema.com/api/v1/user/language"
```

### Resposta de sucesso

```json
{
  "data": {
    "language": "pt-BR"
  },
  "meta": []
}
```

## Estrutura JSON Explicada

| Campo             | Tipo   | Descrição |
| ----------------- | ------ | --------- |
| data.language     | string | Idioma salvo para o usuário. |
| meta              | array  | Reservado para metadados (vazio). |

## Status HTTP

- 200: Idioma atualizado com sucesso.
- 401: Token ausente ou inválido.
- 422: Valor de `language` fora dos permitidos.
- 500: Erro interno inesperado.

## Erros

```json
{
  "message": "O idioma selecionado é inválido.",
  "errors": {
    "language": [
      "O campo language é inválido."
    ]
  }
}
```

## Notas

- Nome da rota: `api.v1.user.language.update`.
- O idioma é persistido em `regional_information.language` com base na enum `LanguageEnum`.
- Caso o usuário não possua registro de regionalização, ele é criado automaticamente.

## Relacionados

- [Shared – Atualizar moeda do usuário](UserCurrencyUpdate.md)
- [Shared – Listagem de idiomas disponíveis](LanguageIndex.md)

## Changelog

- 2025-10-14: Endpoint documentado.
