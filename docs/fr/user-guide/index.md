# Guide d'utilisation

Apprenez à utiliser Duckling efficacement.

## Aperçu

Duckling fournit une interface complète pour la conversion de documents avec des fonctionnalités avancées comme l'OCR, l'extraction de tableaux et la segmentation RAG.

## Sections

<div class="grid cards" markdown>

-   :material-star:{ .lg .middle } __Fonctionnalités__

    ---

    Explorez toutes les capacités de Duckling

    [:octicons-arrow-right-24: Voir les fonctionnalités](features.md)

-   :material-file-document:{ .lg .middle } __Formats pris en charge__

    ---

    Référence des formats d'entrée et de sortie

    [:octicons-arrow-right-24: Guide des formats](formats.md)

-   :material-cog:{ .lg .middle } __Configuration__

    ---

    Personnalisez les paramètres OCR, tableaux, images et performances

    [:octicons-arrow-right-24: Guide de configuration](configuration.md)

</div>

## Conseils rapides

!!! tip "Traitement par lot"
    Activez le mode batch pour convertir plusieurs fichiers simultanément. Le système traite jusqu'à 2 fichiers en parallèle pour équilibrer vitesse et utilisation mémoire.

!!! tip "Sélection OCR"
    - **EasyOCR** : Meilleur pour les documents multilingues avec support GPU
    - **Tesseract** : Fiable pour les documents simples
    - **macOS Vision** : Le plus rapide sur Mac avec Apple Silicon
    - **RapidOCR** : Léger et rapide

!!! tip "Segmentation RAG"
    Activez la segmentation dans les paramètres pour générer des segments de document optimisés pour la génération augmentée par récupération. Les segments incluent des métadonnées comme les en-têtes et les numéros de page.
