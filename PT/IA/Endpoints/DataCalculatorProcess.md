# IA — Calculadora de Dados: Processar (CSV/JSON)

## Endpoint

`POST /ia/admin/data/calculator/process`

Processa CSV/TSV/NDJSON/XML/YAML (arquivo) ou JSON (string/arquivo) e retorna o tamanho do payload e o JSON extraído.

> Branch: master

---

## Autenticação

Obrigatória — `auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale opcional para mensagens. |

---

## Requisição

Envie um `file` (CSV) OU um `json` (string JSON).

### Corpo (multipart/form-data ou JSON)
| Campo        | Tipo      | Obrigatório | Descrição |
| ------------ | --------- | ----------- | --------- |
| `file`       | `file`    | Cond        | Um de: CSV (`.csv`), TSV (`.tsv`), NDJSON (`.ndjson`), JSON (`.json`), XML (`.xml`), YAML (`.yaml`/`.yml`), Excel (`.xls`/`.xlsx`). Obrigatório se `json` não for enviado. |
| `json`       | `string`  | Cond        | String JSON. Obrigatório se `file` não for enviado. |
| `has_header` | `boolean` | Não         | CSV: se `true` (padrão), usa a primeira linha como cabeçalhos. |
| `delimiter`  | `string`  | Não         | CSV: delimitador (1 caractere). Padrão: `,`. |
| `format`     | `string`  | Não         | Forçar detecção: `csv`, `tsv`, `dsv`, `json`, `ndjson`, `xml`, `yaml`, `excel`. |
| `currency`   | `string`  | Não         | Código de moeda para formatar totais; usa o preço padrão se omitido. |

> CSV sem cabeçalhos: chaves geradas como `"1"`, `"2"`, `"3"`, ... por linha.

---

## Resposta

```json
{
  "data": {
    "size": 1234,
    "size_formatted": "9872 bits | 1234 B | 1.21 KB | 0.00 MB | 0.00 GB | 0.00 TB | 0.00 PB",
    "json": [ { "colA": "valor" } ],
    "price": {
      "value": "0.0100",
      "raw_value": 100,
      "precision": 4,
      "float_value": 0.01,
      "formatted_value": "R$\u00a00,0100",
      "currency": "BRL"
    },
    "total_value": {
      "value": 1234,
      "float_value": 12.34,
      "raw_value": "12.3400",
      "formatted_value": "R$\u00a012,3400",
      "currency": "BRL",
      "precision": 4
    }
  }
}
```

- `size`: tamanho em bytes após normalização
- `size_formatted`: tamanhos combinados em bits, bytes, KB, MB, GB, TB, PB
- `json`: payload parseado (array/objeto)
- `price`: preço unitário por byte com precisão de quatro casas decimais; inclui o valor bruto (`raw_value`) e a moeda aplicada.
- `total_value`: valor total calculado para o tamanho encontrado, preservando representações inteira (`value`) e decimal (`raw_value`) com quatro casas decimais.

### Resposta de erro (422)

Retornada quando o formato enviado não é suportado ou bibliotecas obrigatórias (ex.: PhpSpreadsheet para Excel) não estão instaladas.

```json
{
  "success": false,
  "message": "O excel não é suportado."
}
```

- A mensagem utiliza a tradução de `validation.unsupported` para o atributo rejeitado.
- Um log detalhado é enviado ao Discord para facilitar a observabilidade do erro.

---

## Exemplos

### CSV (multipart/form-data)
- file: `dados.csv`
- has_header: `true`
- delimiter: `,`

### JSON (application/json)
```json
{ "json": "{ \"nome\": \"Alice\", \"pontuacoes\": [1,2,3] }" }
```

### Exemplos cURL

CSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.csv" \
  -F "has_header=1" \
  -F "delimiter=," \
  https://<host>/ia/admin/data/calculator/process
```

TSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.tsv" \
  -F "format=tsv" \
  https://<host>/ia/admin/data/calculator/process
```

NDJSON
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.ndjson" \
  -F "format=ndjson" \
  https://<host>/ia/admin/data/calculator/process
```

XML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.xml" \
  -F "format=xml" \
  https://<host>/ia/admin/data/calculator/process
```

YAML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.yaml" \
  -F "format=yaml" \
  https://<host>/ia/admin/data/calculator/process
```

Excel (XLSX)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/caminho/dados.xlsx" \
  -F "format=excel" \
  https://<host>/ia/admin/data/calculator/process
```

JSON (inline)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"json":"{ \"nome\": \"Alice\", \"pontuacoes\": [1,2,3] }"}' \
  https://<host>/ia/admin/data/calculator/process
```

### Exemplo de Resposta (CSV com cabeçalho)
```json
{
  "data": {
    "size": 57,
    "size_formatted": "456 bits | 57 B | 0.06 KB | 0.00 MB | 0.00 GB | 0.00 TB | 0.00 PB",
    "json": [
      { "name": "Alice", "age": "30" },
      { "name": "Bob",   "age": "25" }
    ]
  }
}
```

---

## Notas
- Remove BOM do CSV automaticamente quando presente na primeira célula.
- Para CSV/TSV sem cabeçalhos, chaves numéricas em string a partir de `"1"`.
- JSON pode ser enviado como string no corpo ou arquivo `.json`.
### NDJSON (multipart/form-data)
- file: `dados.ndjson`

### XML (multipart/form-data)
- file: `dados.xml`

### YAML (multipart/form-data)
- file: `dados.yaml` (requer extensão PHP yaml; caso contrário retorna não suportado)

### Excel (multipart/form-data)
- file: `dados.xlsx` (requer PhpSpreadsheet no servidor; caso contrário retorna não suportado)
- NDJSON: cada linha deve ser um objeto JSON válido; linhas vazias são ignoradas.
- YAML requer extensão PHP `yaml_parse`. Quando ausente, a API retorna erro de não suportado.
- Excel requer PhpSpreadsheet (`PhpOffice/PhpSpreadsheet`). Quando ausente, a API retorna erro de não suportado.
- KB e acima usam base 1024.
