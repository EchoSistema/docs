# [DOMAIN] – [ENDPOINT TITLE]

## Endpoint

```
[METHOD] /api/v1/[PATH]
[OPTIONAL OTHER METHODS/PATHS]
```

## Authentication

[Required – Bearer {token} with ability X | None]

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | When applicable | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| [id]      | string | Yes      | [short description] |

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| [foo]     | string  | No       | [description] | [default] |
| [bar]     | integer | No       | [description] | [1-200] |

> Document the canonical parameter names in `snake_case`. When applicable, mention that variants are accepted (`camelCase`, `kebab-case`, `CapitalCase`).

### Request body (when applicable)

```json
{
  "[field]": "[value]"
}
```

## Examples

### Request example (curl)

```bash
curl -X [METHOD] \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/[PATH]?foo=bar"
```

### Response example

```json
{
  "data": [
    {
      "id": 1
    }
  ]
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[]      | array   | [list of items] |
| data[].id   | integer | [identifier] |

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

## Errors

Describe domain-specific errors (codes/messages) and payload examples:

```json
{
  "message": "[message]",
  "errors": {
    "field": ["[detail]"]
  }
}
```

## Notes

- [Business rules, defaults, pagination/sorting, limits]

## Related

- [Relative link to related endpoint](../Endpoints/AnotherEndpoint.md)

## Changelog

- 2025-08-31: [change description]

