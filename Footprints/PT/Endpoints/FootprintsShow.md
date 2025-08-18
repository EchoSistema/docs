# Footprints – Detalhes de um Footprint

## Endpoint

`GET /footprints/{footprint}`

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
    "id": 813,
    "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
    "event": "page_view",
    "event_time": "2025-08-17T00:44:24-03:00",
    "page": "http://localhost:5173/my-messages",
    "title": "Libro de Quejas - Mensajes",
    "timezone_offset": 180,
    "language": "es",
    "connection_type": null,
    "created_at": "2025-08-16T21:44:24-03:00",
    "ip_address": "181.120.47.200",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "9bcf5ae4-f7e6-3276-9755-3b3ef52ef03d",
      "name": "Defensa del Consumidor - Encarnación",
      "public_key": "c43864ce-efae-31c9-bb66-82dceefbc996",
      "domain_area": {
        "uuid": "9a87934f-4adb-56a5-8c3f-34efc1b91881",
        "name": "ReputationBook",
        "slug": "reputationbook",
        "is_active": 1
      },
      "regional_information": {
        "currency": 1,
        "language": "pt-BR"
      }
    },
    "user": {
      "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
      "name": "Ewerton Girardi",
      "age": 39,
      "regional_information": {
        "currency": 1,
        "language": "pt-BR"
      }
    },
    "os_name": "Linux",
    "os_version": null,
    "browser_name": "Firefox",
    "browser_version": "128.0",
    "device_type": "desktop",
    "device_brand": "0",
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
    "id": 813,
    "event_time": "2025-08-17T00:44:24-03:00",
    "event": "page_view",
    "page": "http://localhost:5173/my-messages",
    "title": "Libro de Quejas - Mensajes",
    "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
    "ip_address": "181.120.47.200",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "9bcf5ae4-f7e6-3276-9755-3b3ef52ef03d",
      "name": "Defensa del Consumidor - Encarnación"
    },
    "user": {
      "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
      "name": "Ewerton Girardi"
    },
    "created_at": "2025-08-16T21:44:24-03:00"
  }
}
```

### Erro `404 Não Encontrado`

Retornado quando o footprint não existe.

### Erro `401 Não Autorizado`

Retornado quando a autenticação falha.

