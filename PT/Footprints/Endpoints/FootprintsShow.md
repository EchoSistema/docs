# Footprints – Detalhes de um Footprint

## Endpoint

`GET /api/v1/footprints/{footprint}`

Recupera informações detalhadas sobre um footprint específico.

---

## Autenticação

O endpoint requer autenticação.

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --- | --- | --- |
| **Authorization** | `string` | Token Bearer usado para autenticação. |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma. |
| **Accept-Language** | `string` | Idioma das mensagens de erro (opcional). |

---

## Requisição

### Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `footprint` | `integer` | Sim | Identificador do footprint. |

### Parâmetros de Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `is_index` | `boolean` | Não | Se `true`, retorna o formato simplificado. Valor padrão `false`. |

---

## Respostas

### Sucesso `200 OK`

```json
{
  "data": {
    "id": 1,
    "session_id": "11111111-2222-3333-4444-555555555555",
    "event": "page_view",
    "event_time": "2024-01-01T12:00:00-03:00",
    "page": "https://example.com/messages",
    "title": "Messages Page",
    "timezone_offset": 180,
    "language": "en",
    "connection_type": null,
    "created_at": "2024-01-01T09:00:00-03:00",
    "ip_address": "203.0.113.42",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "name": "Example Platform",
      "public_key": "example-public-key",
      "domain_area": {
        "uuid": "11111111-2222-3333-4444-555555555555",
        "name": "Example Domain",
        "slug": "example-domain",
        "is_active": 1
      },
      "regional_information": {
        "currency": 1,
        "language": "en-US"
      }
    },
    "user": {
      "uuid": "ffffffff-1111-2222-3333-444444444444",
      "name": "Jane Doe",
      "age": 30,
      "regional_information": {
        "currency": 1,
        "language": "en-US"
      }
    },
    "os_name": "Linux",
    "os_version": null,
    "browser_name": "Firefox",
    "browser_version": "128.0",
    "device_type": "desktop",
    "device_brand": "Generic",
    "device_model": null,
    "screen": null,
    "viewport": null
  }
}
```

### Sucesso (Formato Index) `200 OK`

Retornado quando `is_index=true`.

```json
{
  "data": {
    "id": 1,
    "event_time": "2024-01-01T12:00:00-03:00",
    "event": "page_view",
    "page": "https://example.com/messages",
    "title": "Messages Page",
    "session_id": "11111111-2222-3333-4444-555555555555",
    "ip_address": "203.0.113.42",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "name": "Example Platform"
    },
    "user": {
      "uuid": "ffffffff-1111-2222-3333-444444444444",
      "name": "Jane Doe"
    },
    "created_at": "2024-01-01T09:00:00-03:00"
  }
}
```

### Erro `404 Não Encontrado`

Retornado quando o footprint não existe.

### Erro `401 Não Autorizado`

Retornado quando a autenticação falha.

