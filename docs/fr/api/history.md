# API de l'historique

Endpoints pour accéder à l'historique des conversions.

## Obtenir l'historique des conversions

```http
GET /api/history
```

### Paramètres de requête

| Nom | Type | Par défaut | Description |
|------|------|-----------|-------------|
| `limit` | int | 50 | Nombre maximum d'entrées à retourner |
| `offset` | int | 0 | Nombre d'entrées à ignorer |
| `status` | string | - | Filtrer par statut |

### Réponse

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

## Obtenir l'historique récent

```http
GET /api/history/recent
```

### Paramètres de requête

| Nom | Type | Par défaut | Description |
|------|------|-----------|-------------|
| `limit` | int | 10 | Nombre maximum d'entrées à retourner |

---

## Obtenir une entrée d'historique

```http
GET /api/history/{job_id}
```

### Réponse

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

## Supprimer une entrée d'historique

```http
DELETE /api/history/{job_id}
```

### Réponse

```json
{
  "message": "Entry deleted",
  "job_id": "550e8400-e29b-41d4-a716-446655440000"
}
```

---

## Obtenir les statistiques de l'historique

```http
GET /api/history/stats
```

### Réponse

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

## Rechercher dans l'historique

```http
GET /api/history/search
```

### Paramètres de requête

| Nom | Type | Requis | Description |
|------|------|--------|-------------|
| `q` | string | Oui | Requête de recherche |
| `limit` | int | Non | Résultats maximum (par défaut : 20) |

### Réponse

```json
{
  "entries": [...],
  "query": "invoice",
  "count": 5
}
```

---

## Exporter l'historique

```http
GET /api/history/export
```

**Réponse** : Téléchargement d'un fichier JSON avec toutes les entrées d'historique

---

## Effacer tout l'historique

```http
DELETE /api/history
```

### Réponse

```json
{
  "message": "All history entries deleted",
  "count": 150
}
```
