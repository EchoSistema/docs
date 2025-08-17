# Footprints – Criar Footprint

## Endpoint

`POST /footprints`

Registra um rastro de navegação ou interação do visitante.

---

## Autenticação

Nenhuma.

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
  "session_id": "abc123",
  "event": "page_view",
  "event_time": "2025-01-01T12:00:00Z",
  "page": "/home"
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
