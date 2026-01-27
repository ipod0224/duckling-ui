# Déploiement

Guides pour déployer Duckling dans divers environnements.

## Aperçu

Duckling peut être déployé de plusieurs façons selon vos besoins :

<div class="grid cards" markdown>

-   :material-server:{ .lg .middle } __Production__

    ---

    Déployez avec Gunicorn, Nginx et systemd

    [:octicons-arrow-right-24: Guide de production](production.md)

-   :material-scale-balance:{ .lg .middle } __Mise à l'échelle__

    ---

    Mettez à l'échelle pour un trafic élevé avec équilibrage de charge

    [:octicons-arrow-right-24: Guide de mise à l'échelle](scaling.md)

-   :material-shield-check:{ .lg .middle } __Sécurité__

    ---

    Bonnes pratiques de sécurité et durcissement

    [:octicons-arrow-right-24: Guide de sécurité](security.md)

</div>

## Options de déploiement

| Méthode | Idéal pour | Complexité |
|---------|------------|------------|
| Docker Compose | Déploiement rapide, tests | Faible |
| Manuel + Nginx | Contrôle total, personnalisation | Moyenne |
| Kubernetes | Grande échelle, cloud-native | Élevée |

## Référence rapide

### Docker (Le plus simple)

```bash
docker-compose up -d --build
```

### Déploiement manuel

```bash
# Backend avec Gunicorn
cd backend
gunicorn -w 4 -b 0.0.0.0:5001 duckling:app

# Build du frontend
cd frontend
npm run build
# Servir dist/ avec nginx
```

## Liste de vérification de l'environnement

Avant de déployer en production :

- [ ] Définir une `SECRET_KEY` forte
- [ ] Définir `FLASK_DEBUG=false`
- [ ] Configurer CORS pour votre domaine
- [ ] Activer HTTPS
- [ ] Définir des limites de taille de fichier appropriées
- [ ] Configurer le reverse proxy
- [ ] Configurer la surveillance et les journaux
