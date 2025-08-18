# Footprints – Endpoint de Listagem

## Endpoint

`GET /api/v1/footprints`

Recupera uma lista de footprints com filtragem e paginação opcionais.

---

## Autenticação

Requer um token Sanctum válido.

### Cabeçalhos Necessários

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **Authorization** | `string` | `Bearer <token>` obrigatório. |
| **Accept-Language** | `string` | Locale para campos traduzíveis. Opcional. |

---

## Requisição

### Parâmetros de Filtro

#### Footprint IP

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `ip_address` | `ip` | Endereço IP do visitante. |
| `is_banned` | `boolean` | Filtra IPs banidos. |
| `is_robot` | `boolean` | Filtra flag de robôs. |

#### Plataforma

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `platform_identifier` | `integer` | Identificador da plataforma. |
| `platform_name` | `string` | Nome da plataforma. |
| `platform_email` | `email` | Email da plataforma. |
| `platform_open` | `boolean` | Plataforma aberta. |
| `platform_is_parent` | `boolean` | Plataforma é parent. |
| `domain_area_id` | `integer` | ID da área de domínio. |
| `platform_currency_id` | `integer` | ID de moeda da plataforma. |
| `platform_language` | `string` | Idioma da plataforma. |
| `platform_has_company` | `boolean` | Plataforma possui empresa. |
| `platform_country` | `string` | País da plataforma. |
| `platform_region` | `string` | Região da plataforma. |
| `platform_city` | `string` | Cidade da plataforma. |
| `platform_user` | `string` | Usuário da plataforma. |

#### Usuário

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `user_id` | `integer` | ID do usuário. |
| `user_uuid` | `uuid` | UUID do usuário. |
| `user_email` | `email` | Email do usuário. |
| `user_name` | `string` | Nome do usuário. |
| `user_gender` | `string` | Gênero do usuário. |
| `user_birth_date` | `date` | Data de nascimento. |
| `user_birth_from` | `date` | Data de nascimento a partir. |
| `user_birth_to` | `date` | Data de nascimento até. |
| `user_email_verified` | `boolean` | Email verificado. |

#### Footprint

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `footprint_ip_id` | `integer` | ID do IP do footprint. |
| `session_id` | `string` | ID da sessão. |
| `event` | `string` | Evento único. |
| `events` | `array` | Lista de eventos. |
| `events[]` | `string` | Valor do evento. |
| `event_time` | `date` | Data/hora do evento. |
| `event_from` | `date` | Eventos a partir de. |
| `event_to` | `date` | Eventos até. |
| `interval` | `string` | Intervalo (`hour`, `day`, etc.). |
| `period` | `array` | Período com `from`/`to`. |
| `period.from` | `date` | Data inicial. |
| `period.to` | `date` | Data final. |
| `page_url` | `string` | URL da página. |
| `referrer` | `string` | URL de referência. |
| `title` | `string` | Título da página. |
| `timezone_offset` | `integer` | Fuso horário. |
| `connection_type` | `string` | Tipo de conexão. |
| `language` | `string` | Idioma. |
| `is_hidden` | `boolean` | Footprint oculto. |

#### Parâmetros de Query (UTM e similares)

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `utm_source` | `string` | Fonte UTM. |
| `utm_medium` | `string` | Meio UTM. |
| `utm_campaign` | `string` | Campanha UTM. |
| `utm_term` | `string` | Termo UTM. |
| `utm_content` | `string` | Conteúdo UTM. |
| `utms` | `string` | UTMs agrupados. |
| `clids` | `string` | IDs de campanha. |

#### Dispositivo

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `device_os_name` | `string` | Nome do sistema operacional. |
| `device_os_version` | `string` | Versão do sistema operacional. |
| `device_browser_name` | `string` | Nome do navegador. |
| `device_browser_version` | `string` | Versão do navegador. |
| `device_type` | `string` | Tipo de dispositivo. |
| `device_brand` | `string` | Marca do dispositivo. |
| `device_model` | `string` | Modelo do dispositivo. |
| `screen_w` | `integer` | Largura da tela. |
| `screen_h` | `integer` | Altura da tela. |
| `screen_dpr` | `numeric` | Densidade de pixels. |
| `viewport_w` | `integer` | Largura do viewport. |
| `viewport_h` | `integer` | Altura do viewport. |

#### Paginação e Ordenação

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `page` | `integer` | Número da página (padrão 1). |
| `per_page` | `integer` | Itens por página (1-200, padrão 25). |
| `sort` | `string` | Coluna para ordenar (`id`, `event_time`, etc.). |
| `direction` | `string` | Direção da ordenação (`ASC` ou `DESC`). |
| `no_paginate` | `boolean` | Desativa a paginação. |
| `limit` | `integer` | Máximo de itens quando `no_paginate` é verdadeiro. |

> Os parâmetros aceitam variantes camelCase, snake_case, kebab-case ou CapitalCase.

---

## Exemplo de Resposta

```json
{
  "data": [
    {
      "id": 813,
      "event_time": "2025-08-17T00:44:24-03:00",
      "event": "page_view",
      "page": "https://example.com/my-messages",
      "title": "Messages",
      "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
      "ip_address": "203.0.113.10",
      "is_banned": 0,
      "is_robot": 0,
      "platform": {
        "uuid": "00000000-0000-0000-0000-000000000000",
        "name": "Example Platform"
      },
      "user": {
        "uuid": "11111111-1111-1111-1111-111111111111",
        "name": "John Doe"
      },
      "created_at": "2025-08-16T21:44:24-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.echosistema.online/api/v1/footprints?page=1",
    "last": "https://sandbox.echosistema.online/api/v1/footprints?page=32",
    "prev": null,
    "next": "https://sandbox.echosistema.online/api/v1/footprints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 32,
    "path": "https://sandbox.echosistema.online/api/v1/footprints",
    "per_page": 25,
    "to": 1,
    "total": 798
  }
}
```

---

## Explicação da Estrutura JSON

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | `array` | Lista de footprints. |
| `data[].id` | `integer` | ID do footprint. |
| `data[].event_time` | `datetime` | Timestamp do evento. |
| `data[].event` | `string` | Tipo do evento. |
| `data[].page` | `string` | URL da página. |
| `data[].title` | `string` | Título da página. |
| `data[].session_id` | `string` | Identificador da sessão. |
| `data[].ip_address` | `ip` | Endereço IP do visitante. |
| `data[].is_banned` | `boolean` | Indica IP banido. |
| `data[].is_robot` | `boolean` | Indica acesso de robô. |
| `data[].platform` | `object` | Resumo da plataforma. |
| `data[].platform.uuid` | `uuid` | UUID da plataforma. |
| `data[].platform.name` | `string` | Nome da plataforma. |
| `data[].user` | `object` | Resumo do usuário. |
| `data[].user.uuid` | `uuid` | UUID do usuário. |
| `data[].user.name` | `string` | Nome do usuário. |
| `data[].created_at` | `datetime` | Data de criação do registro. |
| `links` | `object` | Links de paginação. |
| `meta` | `object` | Metadados de paginação. |

---

## Observações

* Footprints ocultos são retornados apenas para usuários master.
* Metadados de paginação são omitidos quando `no_paginate` é verdadeiro.

