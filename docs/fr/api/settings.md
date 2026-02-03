# API des paramètres

Endpoints pour gérer les paramètres de conversion.

!!! note "Stockage basé sur les sessions"
    Les paramètres sont stockés par session utilisateur dans la base de données. Les paramètres de chaque utilisateur sont isolés et n'affectent pas les autres utilisateurs, ce qui rend Duckling sûr pour les déploiements multi-utilisateurs.

## Obtenir tous les paramètres

```http
GET /api/settings
```

### Réponse

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

## Mettre à jour les paramètres

```http
PUT /api/settings
Content-Type: application/json
```

### Corps de la requête

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

### Réponse

Retourne l'objet de paramètres mis à jour.

---

## Réinitialiser les paramètres aux valeurs par défaut

```http
POST /api/settings/reset
```

### Réponse

Retourne l'objet de paramètres par défaut.

---

## Obtenir les formats pris en charge

```http
GET /api/settings/formats
```

### Réponse

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

## Paramètres OCR

### Obtenir les paramètres OCR

```http
GET /api/settings/ocr
```

### Mettre à jour les paramètres OCR

```http
PUT /api/settings/ocr
Content-Type: application/json
```

**Paramètres de requête :**

| Paramètre | Type | Description |
|-----------|------|-------------|
| `auto_install` | boolean | Si `true`, installe automatiquement les backends installables via pip |

### Réponse/Requête

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
    {"id": "easyocr", "name": "EasyOCR", "description": "OCR polyvalent avec support GPU"},
    {"id": "tesseract", "name": "Tesseract", "description": "Moteur OCR classique"},
    {"id": "ocrmac", "name": "macOS Vision", "description": "OCR natif macOS (Mac uniquement)"},
    {"id": "rapidocr", "name": "RapidOCR", "description": "OCR rapide avec runtime ONNX"}
  ]
}
```

---

## Gestion des backends OCR

### Obtenir le statut de tous les backends

```http
GET /api/settings/ocr/backends
```

Retourne le statut d'installation pour tous les backends OCR.

### Réponse

```json
{
  "backends": [
    {
      "id": "easyocr",
      "name": "EasyOCR",
      "description": "OCR polyvalent avec support GPU",
      "installed": true,
      "available": true,
      "error": null,
      "pip_installable": true,
      "requires_system_install": false,
      "platform": null,
      "note": "Le premier lancement téléchargera les modèles de langue (~100MB par langue)"
    },
    {
      "id": "tesseract",
      "name": "Tesseract",
      "description": "Moteur OCR classique",
      "installed": false,
      "available": false,
      "error": "Package not installed",
      "pip_installable": true,
      "requires_system_install": true,
      "platform": null,
      "note": "Nécessite que Tesseract soit installé sur votre système"
    }
  ],
  "current_platform": "darwin"
}
```

### Vérifier un backend spécifique

```http
GET /api/settings/ocr/backends/{backend_id}/check
```

### Réponse

```json
{
  "backend": "easyocr",
  "installed": true,
  "available": true,
  "error": null,
  "pip_installable": true,
  "requires_system_install": false,
  "note": "Le premier lancement téléchargera les modèles de langue"
}
```

### Installer un backend

```http
POST /api/settings/ocr/backends/{backend_id}/install
```

Installe un backend OCR installable via pip.

### Réponse (Succès)

```json
{
  "message": "Successfully installed easyocr",
  "success": true,
  "installed": true,
  "available": true,
  "note": "Le premier lancement téléchargera les modèles de langue"
}
```

### Réponse (Déjà installé)

```json
{
  "message": "easyocr is already installed and available",
  "already_installed": true
}
```

### Réponse (Nécessite une installation système)

```json
{
  "message": "Failed to install tesseract",
  "success": false,
  "error": "tesseract requires system-level installation",
  "requires_system_install": true
}
```

---

## Paramètres des tableaux

### Obtenir les paramètres des tableaux

```http
GET /api/settings/tables
```

### Mettre à jour les paramètres des tableaux

```http
PUT /api/settings/tables
Content-Type: application/json
```

### Requête/Réponse

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

## Paramètres des images

### Obtenir les paramètres des images

```http
GET /api/settings/images
```

### Mettre à jour les paramètres des images

```http
PUT /api/settings/images
Content-Type: application/json
```

### Requête/Réponse

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

## Paramètres d'enrichissement

### Obtenir les paramètres d'enrichissement

```http
GET /api/settings/enrichment
```

### Réponse

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
      "description": "Améliorer les blocs de code avec détection de langage et coloration syntaxique",
      "default": false,
      "note": "Peut augmenter le temps de traitement"
    },
    "formula_enrichment": {
      "description": "Extraire les représentations LaTeX des formules mathématiques",
      "default": false,
      "note": "Permet un meilleur rendu des formules dans les exports"
    },
    "picture_classification": {
      "description": "Classer les images par type (figure, graphique, diagramme, photo, etc.)",
      "default": false,
      "note": "Ajoute des balises sémantiques aux images extraites"
    },
    "picture_description": {
      "description": "Générer des légendes descriptives pour les images en utilisant des modèles de vision IA",
      "default": false,
      "note": "Nécessite un téléchargement de modèle supplémentaire, augmente considérablement le temps de traitement"
    }
  }
}
```

### Mettre à jour les paramètres d'enrichissement

```http
PUT /api/settings/enrichment
Content-Type: application/json
```

### Requête

```json
{
  "code_enrichment": true,
  "formula_enrichment": true
}
```

### Réponse

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

| Champ | Type | Description |
|-------|------|-------------|
| `code_enrichment` | boolean | Améliorer les blocs de code avec détection de langage |
| `formula_enrichment` | boolean | Extraire LaTeX des formules mathématiques |
| `picture_classification` | boolean | Classer les images par type sémantique |
| `picture_description` | boolean | Générer des légendes IA pour les images |

!!! warning "Temps de traitement"
    L'activation de `formula_enrichment` et surtout de `picture_description` peut considérablement augmenter le temps de traitement des documents.

---

## Paramètres de performance

### Obtenir les paramètres de performance

```http
GET /api/settings/performance
```

### Mettre à jour les paramètres de performance

```http
PUT /api/settings/performance
Content-Type: application/json
```

### Requête/Réponse

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

## Paramètres de segmentation

### Obtenir les paramètres de segmentation

```http
GET /api/settings/chunking
```

### Mettre à jour les paramètres de segmentation

```http
PUT /api/settings/chunking
Content-Type: application/json
```

### Requête/Réponse

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

## Paramètres de sortie

### Obtenir les paramètres de sortie

```http
GET /api/settings/output
```

### Mettre à jour les paramètres de sortie

```http
PUT /api/settings/output
Content-Type: application/json
```

### Requête/Réponse

```json
{
  "output": {
    "default_format": "markdown"
  }
}
```
