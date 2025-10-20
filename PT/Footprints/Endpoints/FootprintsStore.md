# Footprints – Criar Footprint

## Endpoint

`POST /api/v1/footprints`

Registra um rastro de navegação ou interação do visitante.

---

## Autenticação

Nenhuma. Ainda assim, o cabeçalho `X-PUBLIC-KEY` deve ser enviado.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma necessária para autenticar e receber respostas. |
| **Accept-Language** | `string` | Idioma para mensagens de erro (ex.: `pt-BR`). Opcional. |

### Corpo

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `session_id` | `string` | Sim | Identificador da sessão; utiliza o cabeçalho `x-correlation-id` se ausente. |
| `event` | `FootprintEventEnum` | Sim | Tipo do evento. |
| `event_time` | `date` | Sim | Momento em que o evento ocorreu. |
| `page` | `string` | Sim | URL da página visitada. |
| `visitor_ip` | `ip` | Não | IP do visitante; detectado automaticamente. |
| `echo_id` | `string` | Não | Identificador de espelhamento. |
| `user` | `string` | Não | Referência ao usuário. |
| `referrer` | `string` | Não | URL de origem. |
| `title` | `string` | Não | Título da página. |
| `utm.source` | `string` | Não | Fonte da campanha. |
| `utm.medium` | `string` | Não | Meio da campanha. |
| `utm.campaign` | `string` | Não | Nome da campanha. |
| `utm.term` | `string` | Não | Termo da campanha. |
| `utm.content` | `string` | Não | Conteúdo da campanha. |
| `utms` | `array` | Não | Dados UTM adicionais. |
| `clids` | `array` | Não | Identificadores de clique. |
| `browser.name` | `string` | Não | Nome do navegador. |
| `browser.version` | `string` | Não | Versão do navegador. |
| `os.name` | `string` | Não | Sistema operacional. |
| `os.version` | `string` | Não | Versão do sistema. |
| `device.type` | `string` | Não | Tipo do dispositivo. |
| `device.brand` | `string` | Não | Marca do dispositivo. |
| `device.model` | `string` | Não | Modelo do dispositivo. |
| `screen.w` | `integer` | Não | Largura da tela. |
| `screen.h` | `integer` | Não | Altura da tela. |
| `screen.dpr` | `numeric` | Não | Densidade de pixels. |
| `viewport.w` | `integer` | Não | Largura do viewport. |
| `viewport.h` | `integer` | Não | Altura do viewport. |
| `timezone_offset` | `integer` | Não | Deslocamento do fuso horário em minutos. |
| `connection_type` | `string` | Não | Tipo de conexão. |
| `language` | `string` | Não | Idioma do navegador. |
| `public_key` | `uuid` | Não | Chave pública associada. |
| `platform_id` | `integer` | Não | ID da plataforma. |
| `platform_uuid` | `uuid` | Não | UUID da plataforma. |
| `platform_language` | `string` | Não | Idioma da plataforma. |
| `platform_currency` | `string` | Não | Moeda da plataforma. |

### Exemplo de Corpo

```json
{
  "echo_id": null,
  "session_id": "00000000-0000-0000-0000-000000000000",
  "event": "page_view",
  "event_time": "2025-01-01T00:00:00.000Z",
  "page": "https://example.com/laws",
  "referrer": null,
  "title": "Título de Página Exemplo",
  "utm": {
    "source": null,
    "medium": null,
    "campaign": null,
    "term": null,
    "content": null
  },
  "browser": {
    "name": "Edge",
    "version": "140.0.0.0"
  },
  "os": {
    "name": "Linux",
    "version": null
  },
  "device": {
    "type": "desktop",
    "brand": null,
    "model": null
  },
  "screen": {
    "w": 1920,
    "h": 1080,
    "dpr": 1
  },
  "viewport": {
    "w": 1912,
    "h": 964
  },
  "timezone_offset": 180,
  "connection_type": "4g",
  "language": "pt-BR",
  "platform_id": 1,
  "platform_uuid": "00000000-0000-0000-0000-000000000000",
  "platform_language": "pt-BR",
  "platform_currency": "BRL",
  "visitor_ip": "203.0.113.1"
}
```

---

## Respostas

### Sucesso `201 Created`

```json
["success"]
```

### Erro `400 Bad Request`

Retorna um objeto com a mensagem de erro.

---

## Observações

* Localização: mensagens de erro podem seguir o `Accept-Language` quando fornecido.

### Valores de Evento (FootprintEventEnum)

Valores permitidos para o campo obrigatório `event`:

- Analytics & Tracking: `ad_click`, `ad_view`, `first_visit`, `page_view`, `page_redirect`, `session_start`, `user_engagement`, `view_search_results`
- E-commerce & Payments: `add_payment_info`, `add_shipping_info`, `add_to_cart`, `add_to_wishlist`, `begin_checkout`, `purchase`, `refund`, `remove_from_cart`, `select_item`, `select_promotion`, `view_cart`, `view_item`, `view_item_list`, `view_promotion`
- Leads & Conversions: `close_convert_lead`, `close_unconvert_lead`, `disqualify_lead`, `emailCapture`, `generate_lead`, `lead`, `qualify_lead`, `register_event`, `working_lead`
- User Actions: `apply_job`, `email_view`, `contact`, `login`, `logout`, `sign_up`, `chat`, `video_call`, `audio_call`
- UI Interactions: `button_click`, `click`, `file_download`, `form_submission`, `input_focus`, `lightbox_open`, `modal_open`, `scroll`, `tooltip_click`
- Media Events: `video_complete`, `video_play`, `video_progress`, `video_start`
- Learning & Tutorials: `tutorial_begin`, `tutorial_complete`
