# Shared – Obter Perfil do Usuário

## Endpoint

```
GET /api/v1/me/profile
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

Este endpoint não possui parâmetros.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/me/profile"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "echo_uuid": "uuid_codificado",
    "name": "João Silva",
    "email": "joao@exemplo.com",
    "slug": "joao-silva",
    "avatar": "https://exemplo.com/avatar.jpg",
    "age": 30,
    "gender": "M",
    "gender_name": "Masculino",
    "language": "pt-BR",
    "currency": "BRL",
    "roles": [
      {
        "id": 1,
        "uuid": "role-uuid",
        "name": "admin",
        "localized_name": "Administrador",
        "permissions": ["view", "create"]
      }
    ],
    "contacts": {
      "TELEPHONE": {"phone": "+5511987654321"}
    },
    "biography": {
      "uuid": "bio-uuid",
      "slug": "joao-silva",
      "summary": "Desenvolvedor de software..."
    }
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data | object | Dados do perfil do usuário |
| data.uuid | string | Identificador único do usuário |
| data.echo_uuid | string | UUID codificado do EchoSistema |
| data.name | string | Nome completo do usuário |
| data.email | string | Endereço de e-mail |
| data.slug | string\|null | Slug do usuário para URL |
| data.avatar | string | URL da imagem de avatar |
| data.profile_image | string\|null | URL da imagem de perfil |
| data.age | integer\|null | Idade do usuário |
| data.gender | string | Código de gênero (M/F/O) |
| data.gender_name | string | Nome de gênero localizado |
| data.birthday | string\|null | Data de nascimento (ISO 8601) |
| data.is_banned | boolean | Se o usuário está banido |
| data.language | string | Idioma preferido do usuário |
| data.currency | string | Moeda preferida do usuário |
| data.roles | array\|null | Papéis do usuário com permissões |
| data.telephone | string\|null | Número de telefone principal |
| data.contacts | object\|null | Todos os contatos indexados por tipo |
| data.social_medias | object\|null | Links de redes sociais indexados por plataforma |
| data.address | object\|null | Detalhes do endereço do usuário |
| data.nationalities | array | Nacionalidades do usuário |
| data.biography | object\|null | Recurso completo de biografia |
| data.job_occupation | object\|null | Ocupação profissional atual |
| data.platform | object\|null | Informações da plataforma |
| data.affiliate | object\|null | Dados de afiliado |
| data.identities | array\|null | Documentos de identidade do usuário |
| data.raw | object\|null | Dados personalizados específicos da plataforma |

## Status HTTP

- 200: Sucesso - Perfil recuperado com sucesso
- 401: Não autenticado - Token inválido ou ausente
- 403: Proibido - Permissões insuficientes
- 404: Não encontrado - Usuário não encontrado
- 500: Erro interno

## Erros

### 401 Não autenticado
```json
{
  "message": "Não autenticado."
}
```

### 403 Proibido
```json
{
  "message": "Esta ação não está autorizada."
}
```

## Notas

- Retorna o perfil completo do usuário autenticado
- Todos os relacionamentos são carregados antecipadamente para desempenho otimizado
- Os dados de biografia incluem relacionamentos aninhados (educações, habilidades, certificações, etc.)
- Contatos e redes sociais são indexados por tipo/nome para fácil acesso
- Use o cabeçalho `Accept-Language` para obter valores de campos localizados

## Relacionados

- [Atualizar Idioma do Usuário](./UserRegionalInformationLanguageUpdate.md)
- [Atualizar Moeda do Usuário](./UserRegionalInformationCurrencyUpdate.md)

## Changelog

- 2025-10-16: Perfil expandido com biografia, contatos, redes sociais, identidades e dados de afiliado
