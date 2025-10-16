# Company – Exibir Detalhes da Oportunidade de Emprego

## Endpoint

```
GET /api/v1/company/opportunities/{jobOpportunity}
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| jobOpportunity  | string | Sim         | UUID da oportunidade de emprego. |

### Parâmetros de consulta

| Parâmetro | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------ | ----------- | --------- | -------------- |
| language  | string | Não         | Código de idioma para campos traduzíveis (ex.: `en`, `pt-BR`, `es`). | Padrão da plataforma |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company/opportunities/00000000-0000-0000-0000-000000000001?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "company": {
      "name": "Tech Solutions Inc.",
      "country": "Paraguai",
      "state": "Central",
      "city": "Assunção"
    },
    "total_job_openings": 3,
    "title": "Desenvolvedor de Software Sênior",
    "mode": "remote",
    "description": "Procuramos um desenvolvedor de software experiente para se juntar à nossa equipe e trabalhar em projetos de ponta usando tecnologias modernas.",
    "education_required": "bachelor",
    "required_skills": [
      "PHP",
      "Laravel",
      "JavaScript",
      "Vue.js",
      "Docker"
    ],
    "required_experience": [
      {
        "area": "Desenvolvimento de Software",
        "years": 5
      }
    ],
    "occupation_areas_scope": [
      {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "title": "Tecnologia da Informação"
      }
    ],
    "languages": [
      {
        "language": "Inglês",
        "level": "avançado"
      },
      {
        "language": "Espanhol",
        "level": "intermediário"
      }
    ],
    "salary_range": {
      "from": "R$ 5.000,00",
      "to": "R$ 8.000,00"
    },
    "country": "Paraguai",
    "state": "Central",
    "city": "Assunção",
    "starts_at": "2025-10-20T00:00:00+00:00",
    "finishes_at": "2025-11-30T23:59:59+00:00"
  }
}
```

## Estrutura JSON Explicada

| Campo                               | Tipo     | Descrição |
| ----------------------------------- | -------- | --------- |
| data                                | object   | Recurso de oportunidade de emprego. |
| data.uuid                           | string   | UUID da oportunidade de emprego. |
| data.company                        | object   | Informações da empresa (apenas quando não é proprietário). |
| data.company.name                   | string   | Nome da empresa. |
| data.company.country                | string   | País da empresa. |
| data.company.state                  | string   | Estado da empresa. |
| data.company.city                   | string   | Cidade da empresa. |
| data.total_job_openings             | integer  | Número de vagas abertas. |
| data.title                          | string   | Título do trabalho no idioma solicitado. |
| data.mode                           | string   | Modo de trabalho: `remote`, `hybrid`, `on-site`. |
| data.description                    | string   | Descrição do trabalho no idioma solicitado. |
| data.education_required             | string   | Nível de educação requerido. |
| data.required_skills[]              | array    | Lista de habilidades requeridas. |
| data.required_experience[]          | array    | Lista de experiência requerida. |
| data.required_experience[].area     | string   | Área de experiência. |
| data.required_experience[].years    | integer  | Anos de experiência requeridos. |
| data.occupation_areas_scope[]       | array    | Áreas de ocupação relevantes. |
| data.occupation_areas_scope[].uuid  | string   | UUID da área de ocupação. |
| data.occupation_areas_scope[].title | string   | Título da área de ocupação. |
| data.languages[]                    | array    | Idiomas requeridos. |
| data.languages[].language           | string   | Nome do idioma. |
| data.languages[].level              | string   | Nível de proficiência. |
| data.salary_range                   | object   | Informações da faixa salarial. |
| data.salary_range.from              | string   | Salário mínimo (formatado). |
| data.salary_range.to                | string   | Salário máximo (formatado). |
| data.country                        | string   | País de localização do trabalho. |
| data.state                          | string   | Estado de localização do trabalho. |
| data.city                           | string   | Cidade de localização do trabalho. |
| data.starts_at                      | datetime | Data de início da oportunidade. |
| data.finishes_at                    | datetime | Data de término da oportunidade. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Oportunidade de emprego não encontrada.",
  "errors": {}
}
```

## Notas

- Este endpoint retorna informações detalhadas sobre uma oportunidade de emprego específica.
- A oportunidade de emprego deve pertencer à empresa associada à plataforma atual.
- O campo `company` é incluído apenas quando a requisição não é do proprietário.
- As faixas salariais são formatadas de acordo com o idioma solicitado e a moeda.
- Todos os campos traduzíveis (título, descrição) são retornados no idioma solicitado, retornando ao idioma padrão se não estiver disponível.
- O endpoint valida se a oportunidade de emprego pertence à empresa antes de retornar os dados.

## Relacionados

- [Job Opportunity Index](./MeJobOpportunityIndex.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Documentação inicial.
