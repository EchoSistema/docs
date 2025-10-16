# ReputationBook – Criar Reclamação

## Endpoint

```
POST /api/v1/complaints
```

Cria uma nova reclamação contra uma empresa.

## Autenticação

**Obrigatória** – Token Bearer com habilidade `backoffice` e escopo `domain:reputationbook`.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sim         | Deve ser `application/json`. |

## Parâmetros

### Corpo da requisição

```json
{
  "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
  "purchase_date": "2025-10-01",
  "title": "Problema de qualidade do produto",
  "complaint": "O produto recebido estava defeituoso e não correspondia à descrição.",
  "is_public": true,
  "invoice_number": "INV-2025-001",
  "attendants_name": "Jane Doe",
  "language": "pt-BR"
}
```

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| company_uuid | uuid | Sim* | UUID da empresa. Obrigatório se `company_id` não for fornecido. |
| company_id | integer | Sim* | ID da empresa. Obrigatório se `company_uuid` não for fornecido. |
| purchase_date | date | Sim | Data da compra (formato: YYYY-MM-DD). Deve ser hoje ou anterior. |
| title | string | Sim | Título da reclamação. |
| complaint | string | Sim | Descrição detalhada da reclamação. |
| is_public | boolean | Não | Se a reclamação deve ser visível publicamente. Padrão `false`. |
| invoice_number | string | Não | Número da fatura ou recibo. |
| attendants_name | string | Não | Nome do atendente que atendeu. |
| language | string | Não | Código de idioma para a reclamação (ex.: `en`, `es`, `pt-BR`). |

> Deve ser fornecido `company_uuid` ou `company_id`, mas não ambos.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Content-Type: application/json" \
  -H "Accept-Language: pt-BR" \
  -d '{
    "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
    "purchase_date": "2025-10-01",
    "title": "Problema de qualidade do produto",
    "complaint": "O produto recebido estava defeituoso.",
    "is_public": true
  }' \
  "https://sandbox.seu-dominio.com/api/v1/complaints"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
    "user": {
      "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
      "name": "João Silva",
      "gender": "male"
    },
    "company": {
      "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
      "name": "Empresa ABC",
      "slug": "empresa-abc"
    },
    "titles": [
      {
        "content": "Problema de qualidade do produto",
        "language": "pt-BR",
        "is_default": true
      }
    ],
    "texts": [
      {
        "content": "O produto recebido estava defeituoso.",
        "language": "pt-BR",
        "is_default": true
      }
    ],
    "images": [],
    "status": "pending",
    "is_public": true,
    "purchase_date": "2025-10-01",
    "invoice_number": "INV-2025-001",
    "attendants_name": "Jane Doe",
    "created_at": "2025-10-15T14:30:00.000000Z",
    "updated_at": "2025-10-15T14:30:00.000000Z"
  }
}
```

## Estrutura JSON Explicada

| Campo       | Tipo    | Descrição |
| ----------- | ------- | --------- |
| data        | object  | O objeto de reclamação criado. |
| data.uuid   | uuid    | Identificador único da reclamação. |
| data.user   | object  | Usuário que criou a reclamação. |
| data.company | object | Empresa sobre a qual se reclama. |
| data.titles[] | array | Títulos de reclamação localizados. |
| data.texts[] | array  | Conteúdo de texto da reclamação localizado. |
| data.images[] | array | Imagens anexadas (vazio na criação). |
| data.status | string  | Status inicial (sempre `pending`). |
| data.is_public | boolean | Configuração de visibilidade pública. |
| data.purchase_date | date | Data de compra. |
| data.invoice_number | string | Número da fatura (se fornecido). |
| data.attendants_name | string | Nome do atendente (se fornecido). |
| data.created_at | string | Marca de tempo de criação (ISO-8601). |
| data.updated_at | string | Marca de tempo de última atualização (ISO-8601). |

## Status HTTP

- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado (empresa não encontrada)
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Erro de validação",
  "errors": {
    "company_uuid": ["O campo company uuid é obrigatório quando company id não está presente."],
    "purchase_date": ["O campo purchase date é obrigatório."],
    "title": ["O campo title é obrigatório."],
    "complaint": ["O campo complaint é obrigatório."]
  }
}
```

## Notas

- Requer autenticação com habilidade `backoffice` e escopo `domain:reputationbook`.
- A reclamação é criada com status `pending` por padrão.
- A autorização do usuário é verificada antes de criar a reclamação.
- O campo `language` determina o idioma do título e do conteúdo do texto.
- Imagens podem ser adicionadas separadamente após a criação da reclamação.

## Relacionados

- [Índice de Reclamações](ComplaintIndex.md)
- [Mostrar Reclamação](ComplaintShow.md)
- [Atualizar Reclamação](ComplaintUpdate.md)
- [Adicionar Texto à Reclamação](ComplaintAddText.md)
