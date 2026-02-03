# Einstellungs-API

Endpoints zur Verwaltung von Konvertierungseinstellungen.

!!! note "Sitzungsbasierte Speicherung"
    Einstellungen werden pro Benutzersitzung in der Datenbank gespeichert. Die Einstellungen jedes Benutzers sind isoliert und beeinträchtigen andere Benutzer nicht, was Duckling für Multi-User-Bereitstellungen sicher macht.

## Alle Einstellungen abrufen

```http
GET /api/settings
```

### Antwort

```json
{
  "ocr": {
    "enabled": true,
    "language": "en",
    "force_full_page_ocr": false,
    "backend": "easyocr",
    "use_gpu": false,
    "confidence_threshold": 0.5,
    "bitmap_area_threshold": 0.05
  },
  "tables": {
    "enabled": true,
    "structure_extraction": true,
    "mode": "accurate",
    "do_cell_matching": true
  },
  "images": {
    "extract": true,
    "classify": true,
    "generate_page_images": false,
    "generate_picture_images": true,
    "generate_table_images": true,
    "images_scale": 1.0
  },
  "enrichment": {
    "code_enrichment": false,
    "formula_enrichment": false,
    "picture_classification": false,
    "picture_description": false
  },
  "output": {
    "default_format": "markdown"
  },
  "performance": {
    "device": "auto",
    "num_threads": 4,
    "document_timeout": null
  },
  "chunking": {
    "enabled": false,
    "max_tokens": 512,
    "merge_peers": true
  }
}
```

---

## Einstellungen aktualisieren

```http
PUT /api/settings
Content-Type: application/json
```

### Anfrage-Body

```json
{
  "ocr": {
    "language": "de",
    "backend": "tesseract"
  },
  "tables": {
    "mode": "fast"
  }
}
```

### Antwort

Gibt das aktualisierte Einstellungsobjekt zurück.

---

## Einstellungen auf Standardwerte zurücksetzen

```http
POST /api/settings/reset
```

### Antwort

Gibt das Standard-Einstellungsobjekt zurück.

---

## Unterstützte Formate abrufen

```http
GET /api/settings/formats
```

### Antwort

```json
{
  "input_formats": [
    {"id": "pdf", "name": "PDF Document", "extensions": [".pdf"], "icon": "document"},
    {"id": "docx", "name": "Microsoft Word", "extensions": [".docx"], "icon": "document"},
    {"id": "image", "name": "Image", "extensions": [".png", ".jpg", ".jpeg", ".tiff"], "icon": "image"}
  ],
  "output_formats": [
    {"id": "markdown", "name": "Markdown", "extension": ".md", "mime_type": "text/markdown"},
    {"id": "html", "name": "HTML", "extension": ".html", "mime_type": "text/html"},
    {"id": "json", "name": "JSON", "extension": ".json", "mime_type": "application/json"}
  ]
}
```

---

## OCR-Einstellungen

### OCR-Einstellungen abrufen

```http
GET /api/settings/ocr
```

### OCR-Einstellungen aktualisieren

```http
PUT /api/settings/ocr
Content-Type: application/json
```

**Abfrageparameter:**

| Parameter | Typ | Beschreibung |
|-----------|-----|--------------|
| `auto_install` | boolean | Wenn `true`, werden pip-installierbare Backends automatisch installiert |

### Antwort/Anfrage

```json
{
  "ocr": {
    "enabled": true,
    "language": "en",
    "force_full_page_ocr": false,
    "backend": "easyocr",
    "use_gpu": false,
    "confidence_threshold": 0.5,
    "bitmap_area_threshold": 0.05
  },
  "available_languages": [
    {"code": "en", "name": "English"},
    {"code": "de", "name": "German"},
    {"code": "fr", "name": "French"}
  ],
  "available_backends": [
    {"id": "easyocr", "name": "EasyOCR", "description": "Allzweck-OCR mit GPU-Unterstützung"},
    {"id": "tesseract", "name": "Tesseract", "description": "Klassische OCR-Engine"},
    {"id": "ocrmac", "name": "macOS Vision", "description": "Native macOS OCR (nur Mac)"},
    {"id": "rapidocr", "name": "RapidOCR", "description": "Schnelle OCR mit ONNX Runtime"}
  ]
}
```

---

## OCR-Backend-Verwaltung

### Status aller Backends abrufen

```http
GET /api/settings/ocr/backends
```

Gibt den Installationsstatus für alle OCR-Backends zurück.

### Antwort

```json
{
  "backends": [
    {
      "id": "easyocr",
      "name": "EasyOCR",
      "description": "Allzweck-OCR mit GPU-Unterstützung",
      "installed": true,
      "available": true,
      "error": null,
      "pip_installable": true,
      "requires_system_install": false,
      "platform": null,
      "note": "Beim ersten Lauf werden Sprachmodelle heruntergeladen (~100MB pro Sprache)"
    },
    {
      "id": "tesseract",
      "name": "Tesseract",
      "description": "Klassische OCR-Engine",
      "installed": false,
      "available": false,
      "error": "Package not installed",
      "pip_installable": true,
      "requires_system_install": true,
      "platform": null,
      "note": "Erfordert die Installation von Tesseract auf Ihrem System"
    }
  ],
  "current_platform": "darwin"
}
```

### Spezifisches Backend prüfen

```http
GET /api/settings/ocr/backends/{backend_id}/check
```

### Antwort

```json
{
  "backend": "easyocr",
  "installed": true,
  "available": true,
  "error": null,
  "pip_installable": true,
  "requires_system_install": false,
  "note": "Beim ersten Lauf werden Sprachmodelle heruntergeladen"
}
```

### Backend installieren

```http
POST /api/settings/ocr/backends/{backend_id}/install
```

Installiert ein pip-installierbares OCR-Backend.

### Antwort (Erfolg)

```json
{
  "message": "Successfully installed easyocr",
  "success": true,
  "installed": true,
  "available": true,
  "note": "Beim ersten Lauf werden Sprachmodelle heruntergeladen"
}
```

### Antwort (Bereits installiert)

```json
{
  "message": "easyocr is already installed and available",
  "already_installed": true
}
```

### Antwort (Erfordert Systeminstallation)

```json
{
  "message": "Failed to install tesseract",
  "success": false,
  "error": "tesseract requires system-level installation",
  "requires_system_install": true
}
```

---

## Tabelleneinstellungen

### Tabelleneinstellungen abrufen

```http
GET /api/settings/tables
```

### Tabelleneinstellungen aktualisieren

```http
PUT /api/settings/tables
Content-Type: application/json
```

### Anfrage/Antwort

```json
{
  "tables": {
    "enabled": true,
    "structure_extraction": true,
    "mode": "accurate",
    "do_cell_matching": true
  }
}
```

---

## Bildeinstellungen

### Bildeinstellungen abrufen

```http
GET /api/settings/images
```

### Bildeinstellungen aktualisieren

```http
PUT /api/settings/images
Content-Type: application/json
```

### Anfrage/Antwort

```json
{
  "images": {
    "extract": true,
    "classify": true,
    "generate_page_images": false,
    "generate_picture_images": true,
    "generate_table_images": true,
    "images_scale": 1.0
  }
}
```

---

## Anreicherungseinstellungen

### Anreicherungseinstellungen abrufen

```http
GET /api/settings/enrichment
```

### Antwort

```json
{
  "enrichment": {
    "code_enrichment": false,
    "formula_enrichment": false,
    "picture_classification": false,
    "picture_description": false
  },
  "options": {
    "code_enrichment": {
      "description": "Codeblöcke mit Spracherkennung und Syntaxhervorhebung verbessern",
      "default": false,
      "note": "Kann die Verarbeitungszeit erhöhen"
    },
    "formula_enrichment": {
      "description": "LaTeX-Darstellungen aus mathematischen Formeln extrahieren",
      "default": false,
      "note": "Ermöglicht besseres Formel-Rendering in Exporten"
    },
    "picture_classification": {
      "description": "Bilder nach Typ klassifizieren (Abbildung, Diagramm, Grafik, Foto, etc.)",
      "default": false,
      "note": "Fügt semantische Tags zu extrahierten Bildern hinzu"
    },
    "picture_description": {
      "description": "Beschreibende Bildunterschriften mit KI-Visionsmodellen generieren",
      "default": false,
      "note": "Erfordert zusätzlichen Modelldownload, erhöht die Verarbeitungszeit erheblich"
    }
  }
}
```

### Anreicherungseinstellungen aktualisieren

```http
PUT /api/settings/enrichment
Content-Type: application/json
```

### Anfrage

```json
{
  "code_enrichment": true,
  "formula_enrichment": true
}
```

### Antwort

```json
{
  "message": "Enrichment settings updated",
  "enrichment": {
    "code_enrichment": true,
    "formula_enrichment": true,
    "picture_classification": false,
    "picture_description": false
  }
}
```

| Feld | Typ | Beschreibung |
|------|-----|--------------|
| `code_enrichment` | boolean | Codeblöcke mit Spracherkennung verbessern |
| `formula_enrichment` | boolean | LaTeX aus mathematischen Formeln extrahieren |
| `picture_classification` | boolean | Bilder nach semantischem Typ klassifizieren |
| `picture_description` | boolean | KI-Bildunterschriften generieren |

!!! warning "Verarbeitungszeit"
    Die Aktivierung von `formula_enrichment` und insbesondere `picture_description` kann die Dokumentverarbeitungszeit erheblich erhöhen.

---

## Leistungseinstellungen

### Leistungseinstellungen abrufen

```http
GET /api/settings/performance
```

### Leistungseinstellungen aktualisieren

```http
PUT /api/settings/performance
Content-Type: application/json
```

### Anfrage/Antwort

```json
{
  "performance": {
    "device": "auto",
    "num_threads": 4,
    "document_timeout": null
  }
}
```

---

## Chunking-Einstellungen

### Chunking-Einstellungen abrufen

```http
GET /api/settings/chunking
```

### Chunking-Einstellungen aktualisieren

```http
PUT /api/settings/chunking
Content-Type: application/json
```

### Anfrage/Antwort

```json
{
  "chunking": {
    "enabled": false,
    "max_tokens": 512,
    "merge_peers": true
  }
}
```

---

## Ausgabeeinstellungen

### Ausgabeeinstellungen abrufen

```http
GET /api/settings/output
```

### Ausgabeeinstellungen aktualisieren

```http
PUT /api/settings/output
Content-Type: application/json
```

### Anfrage/Antwort

```json
{
  "output": {
    "default_format": "markdown"
  }
}
```
