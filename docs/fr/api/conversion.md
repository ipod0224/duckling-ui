# API de conversion

Endpoints pour téléverser et convertir des documents.

## Téléverser et convertir un document unique

```http
POST /api/convert
Content-Type: multipart/form-data
```

### Paramètres

| Nom | Type | Requis | Description |
|------|------|--------|-------------|
| `file` | Fichier | Oui | Document à convertir |
| `settings` | Chaîne JSON | Non | Remplacement des paramètres de conversion |

### Exemple de requête

```bash
curl -X POST http://localhost:5001/api/convert \
  -F "file=@document.pdf" \
  -F 'settings={"ocr":{"enabled":true,"language":"en"}}'
```

### Réponse (202 Accepté)

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "document.pdf",
  "input_format": "pdf",
  "status": "processing",
  "message": "Conversion started"
}
```

---

## Convertir plusieurs documents en lot

```http
POST /api/convert/batch
Content-Type: multipart/form-data
```

### Paramètres

| Nom | Type | Requis | Description |
|------|------|--------|-------------|
| `files` | Fichier[] | Oui | Documents à convertir |
| `settings` | Chaîne JSON | Non | Remplacement des paramètres de conversion |

### Exemple de requête

```bash
curl -X POST http://localhost:5001/api/convert/batch \
  -F "files=@doc1.pdf" \
  -F "files=@doc2.pdf" \
  -F "files=@image.png"
```

### Réponse (202 Accepté)

```json
{
  "jobs": [
    {
      "job_id": "550e8400-e29b-41d4-a716-446655440001",
      "filename": "doc1.pdf",
      "input_format": "pdf",
      "status": "processing"
    },
    {
      "job_id": "550e8400-e29b-41d4-a716-446655440002",
      "filename": "doc2.pdf",
      "input_format": "pdf",
      "status": "processing"
    },
    {
      "job_id": "550e8400-e29b-41d4-a716-446655440003",
      "filename": "image.png",
      "input_format": "image",
      "status": "processing"
    }
  ],
  "total": 3,
  "message": "Started 3 conversions"
}
```

---

## Convertir un document depuis une URL

```http
POST /api/convert/url
Content-Type: application/json
```

### Paramètres

| Nom | Type | Requis | Description |
|------|------|--------|-------------|
| `url` | string | Oui | URL du document à convertir |
| `settings` | object | Non | Remplacement des paramètres de conversion |

### Exemple de requête

```bash
curl -X POST http://localhost:5001/api/convert/url \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/document.pdf",
    "settings": {"ocr": {"enabled": true}}
  }'
```

### Réponse (202 Accepté)

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "document.pdf",
  "source_url": "https://example.com/document.pdf",
  "input_format": "pdf",
  "status": "processing",
  "message": "Conversion started"
}
```

---

## Convertir plusieurs documents depuis des URLs

```http
POST /api/convert/url/batch
Content-Type: application/json
```

### Paramètres

| Nom | Type | Requis | Description |
|------|------|--------|-------------|
| `urls` | string[] | Oui | Tableau d'URLs à convertir |
| `settings` | object | Non | Remplacement des paramètres de conversion |

### Exemple de requête

```bash
curl -X POST http://localhost:5001/api/convert/url/batch \
  -H "Content-Type: application/json" \
  -d '{
    "urls": [
      "https://example.com/doc1.pdf",
      "https://example.com/doc2.docx",
      "https://example.com/page.html"
    ]
  }'
```

### Réponse (202 Accepté)

```json
{
  "jobs": [
    {
      "job_id": "550e8400-e29b-41d4-a716-446655440001",
      "url": "https://example.com/doc1.pdf",
      "filename": "doc1.pdf",
      "input_format": "pdf",
      "status": "processing"
    },
    {
      "job_id": "550e8400-e29b-41d4-a716-446655440002",
      "url": "https://example.com/doc2.docx",
      "filename": "doc2.docx",
      "input_format": "docx",
      "status": "processing"
    },
    {
      "url": "https://example.com/invalid",
      "status": "rejected",
      "error": "File type not allowed"
    }
  ],
  "total": 3,
  "message": "Started 2 conversions"
}
```

---

## Obtenir le statut de conversion

```http
GET /api/convert/{job_id}/status
```

### Réponse (En cours)

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "processing",
  "progress": 45,
  "message": "Analyzing document with OCR (easyocr, en)..."
}
```

### Réponse (Terminé)

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "completed",
  "progress": 100,
  "message": "Conversion completed successfully",
  "confidence": 0.92,
  "formats_available": ["markdown", "html", "json", "text", "doctags"],
  "images_count": 3,
  "tables_count": 2,
  "chunks_count": 0,
  "preview": "# Document Title\n\nFirst paragraph..."
}
```

### Réponse (Échec)

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "failed",
  "progress": 0,
  "message": "Conversion failed: Invalid PDF format",
  "error": "Invalid PDF format"
}
```

---

## Obtenir le résultat de conversion

```http
GET /api/convert/{job_id}/result
```

### Réponse

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "completed",
  "confidence": 0.92,
  "formats_available": ["markdown", "html", "json", "text", "doctags", "document_tokens"],
  "result": {
    "markdown_preview": "# Document Title\n\nContent preview...",
    "formats_available": ["markdown", "html", "json", "text", "doctags"],
    "page_count": 5,
    "images_count": 3,
    "tables_count": 2,
    "chunks_count": 0,
    "warnings": []
  },
  "images_count": 3,
  "tables_count": 2,
  "chunks_count": 0,
  "completed_at": "2024-01-15T10:30:00Z"
}
```

---

## Obtenir les images extraites

```http
GET /api/convert/{job_id}/images
```

### Réponse

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "images": [
    {
      "id": 1,
      "filename": "image_1.png",
      "path": "/outputs/job_id/images/image_1.png",
      "caption": "Figure 1: Architecture diagram",
      "label": "figure"
    },
    {
      "id": 2,
      "filename": "image_2.png",
      "path": "/outputs/job_id/images/image_2.png",
      "caption": "",
      "label": "picture"
    }
  ],
  "count": 2
}
```

---

## Télécharger une image extraite

```http
GET /api/convert/{job_id}/images/{image_id}
```

**Réponse** : Fichier image binaire (PNG)

---

## Obtenir les tableaux extraits

```http
GET /api/convert/{job_id}/tables
```

### Réponse

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "tables": [
    {
      "id": 1,
      "label": "table",
      "caption": "Table 1: Sales Data",
      "rows": [
        ["Product", "Q1", "Q2", "Q3", "Q4"],
        ["Widget A", "100", "150", "200", "175"]
      ],
      "csv_path": "/outputs/job_id/tables/table_1.csv",
      "image_path": "/outputs/job_id/tables/table_1.png"
    }
  ],
  "count": 1
}
```

---

## Télécharger un tableau en CSV

```http
GET /api/convert/{job_id}/tables/{table_id}/csv
```

**Réponse** : Fichier CSV

---

## Télécharger un tableau en image

```http
GET /api/convert/{job_id}/tables/{table_id}/image
```

**Réponse** : Fichier image binaire (PNG)

---

## Obtenir les segments du document

```http
GET /api/convert/{job_id}/chunks
```

### Réponse

```json
{
  "job_id": "550e8400-e29b-41d4-a716-446655440000",
  "chunks": [
    {
      "id": 1,
      "text": "This is the first chunk of text from the document...",
      "meta": {
        "headings": ["Introduction"],
        "page": 1
      }
    },
    {
      "id": 2,
      "text": "Second chunk continues the content...",
      "meta": {
        "headings": ["Introduction", "Background"],
        "page": 1
      }
    }
  ],
  "count": 2
}
```

---

## Exporter le document

```http
GET /api/export/{job_id}/{format}
```

### Formats pris en charge

- `markdown`
- `html`
- `json`
- `text`
- `doctags`
- `document_tokens`
- `chunks`

**Réponse** : Téléchargement de fichier avec le type MIME approprié

---

## Supprimer un job

```http
DELETE /api/convert/{job_id}
```

### Réponse

```json
{
  "message": "Job 550e8400-e29b-41d4-a716-446655440000 deleted",
  "job_id": "550e8400-e29b-41d4-a716-446655440000"
}
```
