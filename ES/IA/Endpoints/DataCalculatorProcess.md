# IA — Calculadora de Datos: Procesar (CSV/JSON)

## Endpoint

`POST /ia/admin/data/calculator/process`

Procesa CSV/TSV/NDJSON/XML/YAML (archivo) o JSON (cadena/archivo) y devuelve el tamaño del payload y el JSON extraído.

> Branch: master

---

## Autenticación

Obligatoria — `auth:sanctum`.

### Encabezados Requeridos
| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` requerido. |
| Accept-Language | `string` | Idioma opcional para mensajes. |

---

## Solicitud

Envíe un `file` (CSV) O un `json` (cadena JSON).

### Cuerpo (multipart/form-data o JSON)
| Campo        | Tipo      | Obligatorio | Descripción |
| ------------ | --------- | ----------- | ----------- |
| `file`       | `file`    | Cond        | Uno de: CSV (`.csv`), TSV (`.tsv`), NDJSON (`.ndjson`), JSON (`.json`), XML (`.xml`), YAML (`.yaml`/`.yml`), Excel (`.xls`/`.xlsx`). Obligatorio si no se envía `json`. |
| `json`       | `string`  | Cond        | Cadena JSON. Obligatorio si no se envía `file`. |
| `has_header` | `boolean` | No          | CSV: si `true` (por defecto), usa la primera fila como encabezados. |
| `delimiter`  | `string`  | No          | CSV: delimitador (un solo carácter). Predeterminado: `,`. |
| `format`     | `string`  | No          | Forzar detección: `csv`, `tsv`, `dsv`, `json`, `ndjson`, `xml`, `yaml`, `excel`. |
| `currency`   | `string`  | No          | Código de moneda para formatear totales; usa el precio predeterminado si se omite. |

> CSV sin encabezados: claves generadas como `"1"`, `"2"`, `"3"`, ... por fila.

---

## Respuesta

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
      "formatted_value": "$0.0100",
      "currency": "USD"
    },
    "total_value": {
      "value": 1234,
      "float_value": 12.34,
      "raw_value": "12.3400",
      "formatted_value": "$12.3400",
      "currency": "USD",
      "precision": 4
    }
  }
}
```

- `size`: tamaño en bytes tras normalización
- `size_formatted`: tamaños combinados en bits, bytes, KB, MB, GB, TB, PB
- `json`: payload parseado (array/objeto)
- `price`: precio unitario por byte con precisión de cuatro decimales; incluye el valor bruto (`raw_value`) y la moneda retornada.
- `total_value`: valor total calculado para el tamaño detectado, preservando representaciones entera (`value`) y decimal (`raw_value`) con cuatro decimales.

---

## Ejemplos

### CSV (multipart/form-data)
- file: `datos.csv`
- has_header: `true`
- delimiter: `,`

### JSON (application/json)
```json
{ "json": "{ \"nombre\": \"Alice\", \"puntuaciones\": [1,2,3] }" }
```

### Ejemplos cURL

CSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.csv" \
  -F "has_header=1" \
  -F "delimiter=," \
  https://<host>/ia/admin/data/calculator/process
```

TSV
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.tsv" \
  -F "format=tsv" \
  https://<host>/ia/admin/data/calculator/process
```

NDJSON
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.ndjson" \
  -F "format=ndjson" \
  https://<host>/ia/admin/data/calculator/process
```

XML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.xml" \
  -F "format=xml" \
  https://<host>/ia/admin/data/calculator/process
```

YAML
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.yaml" \
  -F "format=yaml" \
  https://<host>/ia/admin/data/calculator/process
```

Excel (XLSX)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -F "file=@/ruta/datos.xlsx" \
  -F "format=excel" \
  https://<host>/ia/admin/data/calculator/process
```

JSON (inline)
```bash
curl -X POST \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"json":"{ \"nombre\": \"Alice\", \"puntuaciones\": [1,2,3] }"}' \
  https://<host>/ia/admin/data/calculator/process
```

### Respuesta de Ejemplo (CSV con encabezado)
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
- El BOM del CSV se elimina automáticamente cuando está presente en la primera celda.
- Para CSV/TSV sin encabezados, claves numéricas en cadena a partir de `"1"`.
- JSON puede enviarse como cadena o archivo `.json`.
### NDJSON (multipart/form-data)
- file: `datos.ndjson`

### XML (multipart/form-data)
- file: `datos.xml`

### YAML (multipart/form-data)
- file: `datos.yaml` (requiere extensión PHP yaml; en caso contrario retorna no soportado)

### Excel (multipart/form-data)
- file: `datos.xlsx` (requiere PhpSpreadsheet en el servidor; de lo contrario retorna no soportado)
- NDJSON: cada línea debe ser un objeto JSON válido; se ignoran líneas vacías.
- YAML requiere la extensión PHP `yaml_parse`. Si falta, la API devuelve “no soportado”.
- Excel requiere PhpSpreadsheet (`PhpOffice/PhpSpreadsheet`). Si falta, la API devuelve “no soportado”.
- KB y superiores usan base 1024.
