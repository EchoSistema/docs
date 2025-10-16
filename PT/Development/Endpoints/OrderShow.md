# Development – OrderShow

## Endpoint

```
GET /api/orders/{uuid}
```

## Autenticação

Nenhuma

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parâmetros

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid | string | Yes | Uuid identifier |

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| per_page  | integer | No       | Results per page | 10 (1-100) |
| page      | integer | No       | Page number | 1 |

## Exemplos

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/orders/{uuid}"
```

### Response example

```json
{
  "data": []
}
```

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Notas

- Refer to controller implementation for specific business rules
- Pagination is available for list endpoints
- All timestamps are in ISO 8601 format

## Relacionados

- [Development Domínio](../README.md)

## Changelog

- 2025-10-16: Initial documentation
