# Verlaufs-API

Endpoints zum Zugriff auf den Konvertierungsverlauf.

## Konvertierungsverlauf abrufen

```http
GET /api/history
```

### Abfrageparameter

| Name | Typ | Standard | Beschreibung |
|------|-----|----------|--------------|
| `limit` | int | 50 | Maximale Anzahl zurückzugebender Einträge |
| `offset` | int | 0 | Anzahl zu überspringender Einträge |
| `status` | string | - | Nach Status filtern |

### Antwort

```json
{
  "entries": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "filename": "document_abc123.pdf",
      "original_filename": "My Document.pdf",
      "input_format": "pdf",
      "status": "completed",
      "confidence": 0.92,
      "file_size": 1048576,
      "created_at": "2024-01-15T10:00:00Z",
      "completed_at": "2024-01-15T10:00:30Z"
    }
  ],
  "total": 1,
  "limit": 50,
  "offset": 0
}
```

---

## Kürzlichen Verlauf abrufen

```http
GET /api/history/recent
```

### Abfrageparameter

| Name | Typ | Standard | Beschreibung |
|------|-----|----------|--------------|
| `limit` | int | 10 | Maximale Anzahl zurückzugebender Einträge |

---

## Verlaufseintrag abrufen

```http
GET /api/history/{job_id}
```

### Antwort

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "filename": "document_abc123.pdf",
  "original_filename": "My Document.pdf",
  "input_format": "pdf",
  "status": "completed",
  "confidence": 0.92,
  "error_message": null,
  "output_path": "/outputs/550e8400.../document.md",
  "settings": {
    "ocr": {"enabled": true}
  },
  "file_size": 1048576,
  "created_at": "2024-01-15T10:00:00Z",
  "completed_at": "2024-01-15T10:00:30Z"
}
```

---

## Verlaufseintrag löschen

```http
DELETE /api/history/{job_id}
```

### Antwort

```json
{
  "message": "Entry deleted",
  "job_id": "550e8400-e29b-41d4-a716-446655440000"
}
```

---

## Verlaufsstatistiken abrufen

```http
GET /api/history/stats
```

### Antwort

```json
{
  "total": 150,
  "completed": 142,
  "failed": 5,
  "pending": 2,
  "processing": 1,
  "success_rate": 94.7,
  "format_breakdown": {
    "pdf": 100,
    "docx": 30,
    "image": 20
  }
}
```

---

## Verlauf durchsuchen

```http
GET /api/history/search
```

### Abfrageparameter

| Name | Typ | Erforderlich | Beschreibung |
|------|-----|--------------|--------------|
| `q` | string | Ja | Suchanfrage |
| `limit` | int | Nein | Maximale Ergebnisse (Standard: 20) |

### Antwort

```json
{
  "entries": [...],
  "query": "invoice",
  "count": 5
}
```

---

## Verlauf exportieren

```http
GET /api/history/export
```

**Antwort**: JSON-Datei-Download mit allen Verlaufseinträgen

---

## Gesamten Verlauf löschen

```http
DELETE /api/history
```

### Antwort

```json
{
  "message": "All history entries deleted",
  "count": 150
}
```
