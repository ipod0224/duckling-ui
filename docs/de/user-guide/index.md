# Benutzerhandbuch

Erfahren Sie, wie Sie Duckling effektiv nutzen.

## Überblick

Duckling bietet eine umfassende Oberfläche für die Dokumentkonvertierung mit erweiterten Funktionen wie OCR, Tabellenextraktion und RAG-Chunking.

## Abschnitte

<div class="grid cards" markdown>

-   :material-star:{ .lg .middle } __Funktionen__

    ---

    Erkunden Sie alle Funktionen von Duckling

    [:octicons-arrow-right-24: Funktionen anzeigen](features.md)

-   :material-file-document:{ .lg .middle } __Unterstützte Formate__

    ---

    Referenz für Eingabe- und Ausgabeformate

    [:octicons-arrow-right-24: Format-Anleitung](formats.md)

-   :material-cog:{ .lg .middle } __Konfiguration__

    ---

    Passen Sie OCR-, Tabellen-, Bild- und Leistungseinstellungen an

    [:octicons-arrow-right-24: Konfigurationsanleitung](configuration.md)

</div>

## Schnelle Tipps

!!! tip "Stapelverarbeitung"
    Aktivieren Sie den Batch-Modus, um mehrere Dateien gleichzeitig zu konvertieren. Das System verarbeitet bis zu 2 Dateien parallel, um Geschwindigkeit und Speichernutzung auszugleichen.

!!! tip "OCR-Auswahl"
    - **EasyOCR**: Am besten für mehrsprachige Dokumente mit GPU-Unterstützung
    - **Tesseract**: Zuverlässig für einfache Dokumente
    - **macOS Vision**: Schnellste auf Mac mit Apple Silicon
    - **RapidOCR**: Leichtgewichtig und schnell

!!! tip "RAG-Chunking"
    Aktivieren Sie das Chunking in den Einstellungen, um Dokument-Segmente zu generieren, die für retrieval-augmented generation optimiert sind. Chunks enthalten Metadaten wie Überschriften und Seitennummern.
