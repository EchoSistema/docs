# Chat – Estatísticas de Dispositivos

## Endpoint

`GET /api/v1/chat/stats/devices`

Recupera estatísticas agregadas sobre os tipos de dispositivos utilizados para enviar mensagens nas conversas do usuário autenticado.

---

## Autenticação

**Obrigatória** – Token Bearer.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho          | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| **Authorization**  | `string` | Credencial `Bearer {token}`. **Obrigatório**. |
| **X-PUBLIC-KEY**   | `string` | Chave pública que identifica a plataforma solicitante. **Obrigatório**. |

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/chat/stats/devices"
```

### Exemplo de Resposta

```json
{
  "data": [
    {
      "_id": "web",
      "count": 125
    },
    {
      "_id": "mobile",
      "count": 87
    },
    {
      "_id": "desktop",
      "count": 43
    }
  ]
}
```

---

## Explicação da Estrutura JSON

### Resposta de Sucesso

| Campo    | Tipo    | Descrição |
| -------- | ------- | --------- |
| `data[]` | `array` | Lista de estatísticas agregadas por tipo de dispositivo. |

### `data[]` – Estatística de Dispositivo

| Campo   | Tipo      | Descrição |
| ------- | --------- | --------- |
| `_id`   | `string`  | Tipo de dispositivo: `web`, `mobile`, `desktop`. |
| `count` | `integer` | Quantidade total de mensagens enviadas deste tipo de dispositivo. |

---

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 500: Erro interno

---

## Notas

* As estatísticas incluem apenas mensagens das conversas nas quais o usuário autenticado é participante.
* A agregação é realizada usando o MongoDB Aggregation Framework.
* O campo `_id` corresponde ao campo `device.type` armazenado em cada mensagem.
* Se o usuário não tiver conversas, um array vazio é retornado.
* Esta API é útil para análises de uso e comportamento dos usuários, permitindo identificar quais dispositivos são mais utilizados.
* Os dados podem ser utilizados para otimizar a experiência do usuário em diferentes plataformas.

---

## Relacionados

- [Listar Conversas](./ChatConversationsList.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
