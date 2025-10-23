# Compartilhado – Obter Tipos de Endereço

## Endpoint

```
GET /api/v1/addresses/types
```

## Autenticação

Obrigatório – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). Afeta a tradução do campo `title_localized`. |

## Parâmetros

Este endpoint não aceita parâmetros.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/addresses/types"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/addresses/types', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
  }
});
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "name": "BILLING",
      "value": "billing",
      "title_localized": "Cobrança"
    },
    {
      "name": "SHIPPING",
      "value": "shipping",
      "title_localized": "Entrega"
    },
    {
      "name": "BOTH",
      "value": "both",
      "title_localized": "Ambos"
    },
    {
      "name": "FISCAL",
      "value": "fiscal",
      "title_localized": "Fiscal"
    },
    {
      "name": "ADDITIONAL",
      "value": "additional",
      "title_localized": "Adicional"
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                  | Tipo   | Descrição |
| ---------------------- | ------ | --------- |
| data                   | array  | Lista de tipos de endereço disponíveis. |
| data[].name            | string | Nome da constante enum (maiúsculas). |
| data[].value           | string | Valor do enum (minúsculas). Use este valor ao criar/atualizar endereços. |
| data[].title_localized | string | Título traduzido legível baseado no cabeçalho `Accept-Language`. |

## Status HTTP

- 200: OK
- 401: Não Autorizado
- 403: Proibido
- 429: Muitas Requisições
- 500: Erro Interno do Servidor

## Erros

### Não Autenticado

```json
{
  "message": "Não autenticado.",
  "errors": {}
}
```

### Proibido

```json
{
  "message": "Esta ação não é autorizada.",
  "errors": {}
}
```

## Notas

- Este endpoint retorna todos os tipos de endereço disponíveis do `AddressTypeEnum`.
- O campo `title_localized` é traduzido automaticamente com base no cabeçalho `Accept-Language`.
- Use o campo `value` ao criar ou atualizar endereços (ex.: `"type": "billing"`).
- Idiomas suportados: `en` (Inglês), `pt-BR` (Português Brasileiro), `es` (Espanhol), `gn` (Guarani).

## Traduções dos Tipos de Endereço

| Valor      | EN          | PT-BR     | ES           | GN           |
| ---------- | ----------- | --------- | ------------ | ------------ |
| billing    | Billing     | Cobrança  | Facturación  | Jehepyme'ã   |
| shipping   | Shipping    | Entrega   | Envío        | Mondo        |
| both       | Both        | Ambos     | Ambos        | Mokõive      |
| fiscal     | Fiscal      | Fiscal    | Fiscal       | Fiscal       |
| additional | Additional  | Adicional | Adicional    | Oĩmbýva      |

## Relacionados

- [Criar Endereço](./AddressStore.md)
- [Atualizar Endereço](./AddressUpdate.md)

## Histórico de Alterações

- 2025-10-23: Atualizado nome do campo de `title` para `title_localized`.
- 2025-10-23: Documentação inicial.
