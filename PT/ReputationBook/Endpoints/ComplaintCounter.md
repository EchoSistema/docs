# ReputationBook – Métricas de reclamações

## Resumo de contadores

### Endpoint

```
GET /api/v1/complaint-book/complaints/counter
```

Retorna contagem total de reclamações e distribuição por status baseada nos filtros informados.

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Parâmetros de consulta

Aceita os mesmos filtros de `ComplaintFilterRequest` (status, company, período, etc.).

### Exemplo de resposta (200)

```json
{
  "data": {
    "total": 128,
    "status": {
      "open": 64,
      "in_progress": 40,
      "resolved": 24
    },
    "month": 12
  }
}
```

> A chave `month` só aparece quando o filtro `month` é enviado.

### Status HTTP

- 200: Dados retornados.
- 401/403: Sem permissão.

---

## Relatório por intervalo

### Endpoint

```
GET /api/v1/complaint-book/complaints/report/{interval}
```

Gera série temporal das últimas mudanças de status.

### Parâmetros de caminho

| Parâmetro | Valores aceitos |
| --------- | ---------------- |
| `interval` | `annual`, `semi-annual`, `tri-monthly`, `bi-monthly`, `monthly`, `weekly` (qualquer outro assume o ano corrente). |

### Resposta

```json
{
  "success": true,
  "counter": {
    "2025-06": {
      "open": 12,
      "resolved": 4
    },
    "2025-07": {
      "open": 8,
      "in_progress": 6,
      "resolved": 9
    }
  },
  "interval": 6
}
```

- `counter`: mapa `YYYY-MM` → totais por status (apenas o último evento de cada reclamação no período é considerado).
- `interval`: número de meses abrangidos pelo recorte.

### Status HTTP

- 200: Relatório gerado.
- 401/403: Sem permissão.

