# Architektur

Technische Architekturdokumentation für Duckling.

## Überblick

Duckling ist eine Full-Stack-Webanwendung mit klarer Trennung zwischen Frontend und Backend:

```mermaid
graph LR
    A[Browser] --> B[React Frontend]
    B --> C[Flask Backend]
    C --> D[Docling Engine]
    D --> E[(Storage)]

    style A fill:#3b82f6,color:#fff
    style B fill:#1e3a5f,color:#fff
    style C fill:#14b8a6,color:#fff
    style D fill:#8b5cf6,color:#fff
    style E fill:#f59e0b,color:#fff
```

## Abschnitte

<div class="grid cards" markdown>

-   :material-view-dashboard:{ .lg .middle } __Systemübersicht__

    ---

    High-Level-Architektur und Datenfluss

    [:octicons-arrow-right-24: Übersicht](overview.md)

-   :material-puzzle:{ .lg .middle } __Komponenten__

    ---

    Frontend- und Backend-Komponentendetails

    [:octicons-arrow-right-24: Komponenten](components.md)

-   :material-chart-box:{ .lg .middle } __Diagramme__

    ---

    Architekturdiagramme und Flussdiagramme

    [:octicons-arrow-right-24: Diagramme](diagrams.md)

</div>

## Wichtige Designentscheidungen

### Trennung der Belange

- **Frontend**: React mit TypeScript für Typsicherheit und moderne UI
- **Backend**: Flask für Einfachheit und Zugriff auf das Python-Ökosystem
- **Engine**: Docling für Dokumentkonvertierung (IBMs Bibliothek)

### Asynchrone Verarbeitung

Die Dokumentkonvertierung wird asynchron behandelt:

1. Client lädt Datei hoch
2. Server gibt sofort Job-ID zurück
3. Client fragt Status ab
4. Server verarbeitet im Hintergrundthread
5. Ergebnisse verfügbar, wenn abgeschlossen

### Job-Warteschlange

Eine threadbasierte Job-Warteschlange verhindert Speichererschöpfung:

- Maximum 2 gleichzeitige Konvertierungen
- Jobs werden in Warteschlange gestellt, wenn Kapazität erreicht
- Automatische Bereinigung abgeschlossener Jobs

### Einstellungspersistenz

Einstellungen werden pro Benutzersitzung gespeichert und pro Konvertierung angewendet:

- Globale Standardwerte in `config.py`
- Benutzereinstellungen in der Datenbank (pro Sitzungs-ID)
- Pro-Anfrage-Überschreibungen über API

Einstellungen sind pro Benutzersitzung isoliert, sodass Multi-User-Bereitstellungen sich nicht gegenseitig beeinträchtigen.

## Technologie-Stack

### Frontend

| Technologie | Zweck |
|------------|-------|
| React 18 | UI-Framework |
| TypeScript | Typsicherheit |
| Tailwind CSS | Styling |
| Framer Motion | Animationen |
| Axios | HTTP-Client |
| Vite | Build-Tool |

### Backend

| Technologie | Zweck |
|------------|-------|
| Flask | Web-Framework |
| SQLAlchemy | Datenbank-ORM |
| SQLite | Verlaufsspeicherung |
| Docling | Dokumentkonvertierung |
| Threading | Asynchrone Verarbeitung |
