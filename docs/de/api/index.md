# API-Referenz

Vollständige API-Dokumentation für das Duckling-Backend.

## Basis-URL

```
http://localhost:5001/api
```

## Authentifizierung

Derzeit erfordert die API keine Authentifizierung. Für Produktionsbereitstellungen sollten Sie Authentifizierungs-Middleware hinzufügen.

## Abschnitte

<div class="grid cards" markdown>

-   :material-file-document-multiple:{ .lg .middle } __Konvertierung__

    ---

    Dokumente hochladen und konvertieren

    [:octicons-arrow-right-24: Konvertierungs-API](conversion.md)

-   :material-cog:{ .lg .middle } __Einstellungen__

    ---

    Konfiguration abrufen und aktualisieren

    [:octicons-arrow-right-24: Einstellungs-API](settings.md)

-   :material-history:{ .lg .middle } __Verlauf__

    ---

    Auf Konvertierungsverlauf zugreifen

    [:octicons-arrow-right-24: Verlaufs-API](history.md)

</div>

## Schnellreferenz

### Konvertierungs-Endpoints

| Endpoint | Methode | Beschreibung |
|----------|--------|-------------|
| `/convert` | POST | Dokument hochladen und konvertieren |
| `/convert/batch` | POST | Mehrere Dokumente stapelweise konvertieren |
| `/convert/{job_id}/status` | GET | Konvertierungsstatus abrufen |
| `/convert/{job_id}/result` | GET | Konvertierungsergebnis abrufen |
| `/convert/{job_id}/images` | GET | Extrahierte Bilder auflisten |
| `/convert/{job_id}/images/{id}` | GET | Extrahierte Bilder herunterladen |
| `/convert/{job_id}/tables` | GET | Extrahierte Tabellen auflisten |
| `/convert/{job_id}/tables/{id}/csv` | GET | Tabelle als CSV herunterladen |
| `/convert/{job_id}/chunks` | GET | Dokument-Segmente abrufen |
| `/export/{job_id}/{format}` | GET | Konvertierte Datei herunterladen |

### Einstellungs-Endpoints

| Endpoint | Methode | Beschreibung |
|----------|--------|-------------|
| `/settings` | GET/PUT | Alle Einstellungen abrufen/aktualisieren |
| `/settings/reset` | POST | Auf Standardwerte zurücksetzen |
| `/settings/formats` | GET | Unterstützte Formate auflisten |
| `/settings/ocr` | GET/PUT | OCR-Einstellungen |
| `/settings/tables` | GET/PUT | Tabelleneinstellungen |
| `/settings/images` | GET/PUT | Bildeinstellungen |
| `/settings/performance` | GET/PUT | Leistungseinstellungen |
| `/settings/chunking` | GET/PUT | Chunking-Einstellungen |

### Verlaufs-Endpoints

| Endpoint | Methode | Beschreibung |
|----------|--------|-------------|
| `/history` | GET | Konvertierungsverlauf auflisten |
| `/history/{job_id}` | GET | Verlaufseintrag abrufen |
| `/history/stats` | GET | Konvertierungsstatistiken abrufen |
| `/history/search` | GET | Verlauf durchsuchen |

## Gesundheitsprüfung

```http
GET /health
```

**Antwort**

```json
{
  "status": "healthy",
  "service": "duckling-backend"
}
```

## Fehlerantworten

Alle Endpoints können Fehlerantworten im folgenden Format zurückgeben:

```json
{
  "error": "Fehlertyp",
  "message": "Detaillierte Fehlermeldung"
}
```

### HTTP-Statuscodes

| Code | Beschreibung |
|------|-------------|
| 200 | Erfolg |
| 202 | Akzeptiert (asynchrone Operation gestartet) |
| 400 | Ungültige Anfrage (ungültige Eingabe) |
| 404 | Nicht gefunden |
| 413 | Nutzlast zu groß |
| 500 | Interner Serverfehler |

## Rate Limiting

Derzeit ist keine Rate-Limitierung implementiert. Für Produktionsbereitstellungen sollten Sie Rate-Limiting-Middleware hinzufügen.

## CORS

Die API erlaubt Cross-Origin-Anfragen von der konfigurierten Frontend-Herkunft (Standard: `http://localhost:3000`).
