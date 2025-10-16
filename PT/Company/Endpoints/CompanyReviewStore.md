# Company – Criar Avaliação de Empresa

## Endpoint

```
POST /api/v1/company/reviews
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Corpo da requisição

| Parâmetro     | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ------------- | ------- | ----------- | --------- | -------------- |
| company       | string  | Sim*        | Identificador da empresa (UUID ou ID). Obrigatório se `company_id` e `company_uuid` não forem fornecidos. | |
| company_id    | integer | Sim*        | ID numérico da empresa. Obrigatório se `company` e `company_uuid` não forem fornecidos. | |
| company_uuid  | string  | Sim*        | UUID da empresa. Obrigatório se `company` e `company_id` não forem fornecidos. | |
| review        | string  | Sim         | Conteúdo de texto da avaliação. | |
| rating        | integer | Sim         | Valor de avaliação entre 0 e 5. | 0-5 |

> *Um de `company`, `company_id`, ou `company_uuid` deve ser fornecido.

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "company_uuid": "00000000-0000-0000-0000-000000000001",
    "review": "Excelente serviço e equipe profissional. Altamente recomendado!",
    "rating": 5
  }' \
  "https://sandbox.seu-dominio.com/api/v1/company/reviews"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 123,
    "author_name": "João Silva",
    "profile_photo_url": "https://example.com/avatars/joaosilva.jpg",
    "text": "Excelente serviço e equipe profissional. Altamente recomendado!",
    "company": {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "name": "Tech Solutions Inc.",
      "logo": [
        {
          "usage": "logo",
          "url": "https://example.com/logos/techsolutions.png"
        }
      ],
      "assumed_name": "TechSol",
      "slug": "tech-solutions-inc",
      "rating": 4.7,
      "google": null
    },
    "time": "2025-10-16T15:30:00+00:00",
    "rating": 5
  }
}
```

## Estrutura JSON Explicada

| Campo                       | Tipo    | Descrição |
| --------------------------- | ------- | --------- |
| data                        | object  | Recurso de avaliação. |
| data.id                     | integer | Identificador da avaliação. |
| data.author_name            | string  | Nome do usuário que escreveu a avaliação. |
| data.profile_photo_url      | string  | URL da foto de perfil do autor. |
| data.text                   | string  | Conteúdo de texto da avaliação. |
| data.company                | object  | Informações da empresa. |
| data.company.uuid           | string  | UUID da empresa. |
| data.company.name           | string  | Nome da empresa. |
| data.company.logo[]         | array   | Array de objetos de logo da empresa. |
| data.company.logo[].usage   | string  | Tipo de uso do logo. |
| data.company.logo[].url     | string  | URL do logo. |
| data.company.assumed_name   | string  | Nome fantasia da empresa. |
| data.company.slug           | string  | Identificador amigável para URL da empresa. |
| data.company.rating         | float   | Avaliação média da empresa. |
| data.company.google         | mixed   | Dados relacionados ao Google (se disponível). |
| data.time                   | string  | Data e hora de criação da avaliação (ISO 8601). |
| data.rating                 | integer | Avaliação da resenha (0-5). |

## Status HTTP

- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "review": ["O campo review é obrigatório."],
    "rating": ["O campo rating é obrigatório.", "O rating deve estar entre 0 e 5."],
    "company": ["Pelo menos um identificador de empresa é obrigatório."]
  }
}
```

## Notas

- O usuário autenticado é automaticamente associado como o autor da avaliação.
- O parâmetro `company` aceita tanto UUID quanto ID numérico e é resolvido automaticamente.
- Os valores de avaliação devem ser inteiros entre 0 e 5 (inclusive).
- Após criar uma avaliação, a avaliação média da empresa é recalculada.
- Os usuários podem criar múltiplas avaliações para diferentes empresas, mas as regras de negócio podem limitar avaliações duplicadas para a mesma empresa.

## Relacionados

- [Company Review Index](./CompanyReviewIndex.md)
- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentação inicial.
