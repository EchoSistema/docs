# Company – Exibir Detalhes da Empresa

## Endpoint

```
GET /api/v1/company
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------ | ----------- | --------- | -------------- |
| language  | string | Não         | Código de idioma para campos traduzíveis. | Padrão da plataforma |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "name": "Tech Solutions Inc.",
    "slug": "tech-solutions-inc",
    "rating": 4.5,
    "reviews": 150,
    "country": "Paraguai",
    "state": "Central",
    "city": "Assunção",
    "email": "contato@techsolutions.com",
    "favicon": "https://example.com/favicon.ico",
    "logo": {
      "main": "https://example.com/logo.png",
      "square": "https://example.com/logo-quadrado.png",
      "rounded": "https://example.com/logo-arredondado.png",
      "rectangular": "https://example.com/logo-retangular.png"
    },
    "domain_area": {
      "uuid": "00000000-0000-0000-0000-000000000010",
      "name": "Tecnologia"
    },
    "open_to_register": true
  }
}
```

## Estrutura JSON Explicada

| Campo                  | Tipo    | Descrição |
| ---------------------- | ------- | --------- |
| data                   | object  | Recurso de empresa. |
| data.uuid              | string  | UUID da empresa. |
| data.name              | string  | Nome legal da empresa. |
| data.slug              | string  | Identificador amigável para URL. |
| data.rating            | float   | Avaliação média (0-5). |
| data.reviews           | integer | Número total de avaliações. |
| data.country           | string  | Nome do país onde a empresa está localizada. |
| data.state             | string  | Nome do estado. |
| data.city              | string  | Nome da cidade. |
| data.email             | string  | Email de contato da empresa (da plataforma). |
| data.favicon           | string  | URL do favicon da plataforma. |
| data.logo              | object  | Variantes do logo da plataforma. |
| data.logo.main         | string  | URL do logo principal. |
| data.logo.square       | string  | URL do logo quadrado. |
| data.logo.rounded      | string  | URL do logo arredondado. |
| data.logo.rectangular  | string  | URL do logo retangular. |
| data.domain_area       | object  | Informações da área de domínio empresarial. |
| data.domain_area.uuid  | string  | UUID da área de domínio. |
| data.domain_area.name  | string  | Nome da área de domínio. |
| data.open_to_register  | boolean | Se a empresa aceita novos registros. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Não autenticado.",
  "errors": {}
}
```

## Notas

- Este endpoint retorna a empresa associada à plataforma do usuário autenticado.
- O usuário autenticado deve possuir a habilidade `backoffice`.
- A avaliação e as avaliações são calculadas a partir de todas as avaliações da empresa.
- As informações de endereço (país, estado, cidade) vêm do endereço principal da empresa.
- Os campos relacionados à plataforma (email, logo, favicon, domain_area, open_to_register) são carregados do relacionamento de plataforma associado.

## Relacionados

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)
- [Company Address Store](./CompanyAddressStore.md)
- [Company Review Store](./CompanyReviewStore.md)

## Changelog

- 2025-10-16: Documentação inicial.
