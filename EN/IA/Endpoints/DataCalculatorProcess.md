# IA — Data Calculator: Process Data (CSV/JSON)

## Endpoint

`POST /ia/admin/data/calculator/process`

Processes CSV/TSV/NDJSON/XML/YAML (file) or JSON (string/file) to return the payload size and the extracted JSON.

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Optional locale for messages. |

---

## Request

Send either a CSV `file` or a JSON string in `json`.

### Body (multipart/form-data or JSON)
| Field        | Type      | Required | Description |
| ------------ | --------- | -------- | ----------- |
| `file`       | `file`    | Cond     | One of: CSV (`.csv`), TSV (`.tsv`), NDJSON (`.ndjson`), JSON (`.json`), XML (`.xml`), YAML (`.yaml`/`.yml`), Excel (`.xls`/`.xlsx`). Required if `json` is not provided. |
| `json`       | `string`  | Cond     | JSON string. Required if `file` is not provided. |
| `has_header` | `boolean` | No       | CSV only: if true (default), uses first row as headers. |
| `delimiter`  | `string`  | No       | CSV only: field delimiter (single char). Default: `,`. |
| `format`     | `string`  | No       | Force format detection: `csv`, `tsv`, `dsv`, `json`, `ndjson`, `xml`, `yaml`. |

> CSV without headers: keys are generated as `"1"`, `"2"`, `"3"`, ... per row.

---

## Response

```json
{
  "data": {
    "size": 1234,
    "size_formatted": "9872 bits | 1234 B | 1.21 KB | 0.00 MB | 0.00 GB | 0.00 TB | 0.00 PB",
    "json": [ { "colA": "value" } ]
  }
}
```

- `size`: payload size in bytes after normalization
- `size_formatted`: combined sizes in bits, bytes, KB, MB, GB, TB, PB
- `json`: parsed payload (array/object)

---

## Examples

### CSV (multipart/form-data)
- file: `data.csv`
- has_header: `true`
- delimiter: `,`

### JSON (application/json)
```json
{ "json": "{ \"name\": \"Alice\", \"scores\": [1,2,3] }" }
```

### cURL examples

CSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.csv" \
  -F "has_header=1" \
  -F "delimiter=," \
  https://<host>/ia/admin/data/calculator/process
```

TSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.tsv" \
  -F "format=tsv" \
  https://<host>/ia/admin/data/calculator/process
```

NDJSON
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.ndjson" \
  -F "format=ndjson" \
  https://<host>/ia/admin/data/calculator/process
```

XML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.xml" \
  -F "format=xml" \
  https://<host>/ia/admin/data/calculator/process
```

YAML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.yaml" \
  -F "format=yaml" \
  https://<host>/ia/admin/data/calculator/process
```

Excel (XLSX)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/path/data.xlsx" \
  -F "format=excel" \
  https://<host>/ia/admin/data/calculator/process
```

JSON (inline)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"json":"{ \"name\": \"Alice\", \"scores\": [1,2,3] }"}' \
  https://<host>/ia/admin/data/calculator/process
```

### Example Response (CSV with headers)
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

## Notes
- CSV BOM is stripped automatically when present in the first cell.
- For CSV/TSV without headers, numerical string keys are used starting from `"1"`.
- JSON can be provided as string body or `.json` file.
### NDJSON (multipart/form-data)
- file: `data.ndjson`

### XML (multipart/form-data)
- file: `data.xml`

### YAML (multipart/form-data)
- file: `data.yaml` (requires PHP yaml extension; otherwise returns unsupported)

### Excel (multipart/form-data)
- file: `data.xlsx` (requires PhpSpreadsheet installed on server; otherwise returns unsupported)
- NDJSON: each line must be a valid JSON object; empty lines are ignored.
- YAML requires the PHP yaml extension (`yaml_parse`). If missing, the API returns an unsupported error.
- Excel requires PhpSpreadsheet (`PhpOffice/PhpSpreadsheet`). If missing, the API returns an unsupported error.
- Size uses base 1024 for KB and above.
