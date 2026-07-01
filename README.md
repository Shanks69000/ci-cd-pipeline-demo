# CI/CD Pipeline Demo

Pipeline GitHub Actions multi-stage pour une API Flask : lint, tests unitaires, build Docker, push vers GitHub Container Registry (GHCR), deploy dry-run.

## Pipeline
push/PR → Lint (flake8) → Test (pytest) → Build & Push (GHCR) → Deploy (dry-run)

Le build et push Docker ne se déclenchent que sur `push` vers `main` (pas sur les PR).

## Stack

| Composant | Outil |
|---|---|
| Application | Python 3.12, Flask |
| Lint | flake8 |
| Tests | pytest |
| Conteneurisation | Docker (multi-stage, non-root, healthcheck) |
| Registry | GitHub Container Registry (GHCR) |
| CI/CD | GitHub Actions |

## Utilisation locale

```bash
# Cloner
git clone https://github.com/Shanks69000/ci-cd-pipeline-demo.git
cd ci-cd-pipeline-demo

# Installer et tester
pip install -r requirements.txt
pytest tests/ -v

# Build et run Docker
docker build -t ci-cd-pipeline-demo .
docker run -p 5000:5000 ci-cd-pipeline-demo
curl http://localhost:5000/health
```

## Pull de l'image depuis GHCR

```bash
docker pull ghcr.io/shanks69000/ci-cd-pipeline-demo:latest
docker run -p 5000:5000 ghcr.io/shanks69000/ci-cd-pipeline-demo:latest
```

## Sécurité du Dockerfile

- Image de base `python:3.12-slim` (surface d'attaque réduite)
- `USER 1000` (non-root)
- `HEALTHCHECK` intégré
- Pas de secrets dans l'image

## Ce que ce projet démontre

- Pipeline CI/CD GitHub Actions complet (lint → test → build → push → deploy)
- Conteneurisation avec bonnes pratiques Docker (slim, non-root, healthcheck)
- Tests unitaires automatisés avec pytest
- Publication automatique vers GHCR avec tags (SHA + latest)
- Séparation PR (lint+test only) vs merge main (full pipeline)