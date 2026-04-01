# VitalSync — CI/CD conteneurisée

Application minimaliste de suivi médical/sportif, utilisée comme support pour mettre en place une chaîne **CI/CD conteneurisée**.

## Architecture

- **Frontend**: page HTML servie par **Nginx** (port hôte `8080` → conteneur `80`)
- **Backend**: API **Node.js/Express** (port conteneur `3000`)
- **Database**: **PostgreSQL** (volume persistant)

## Prérequis

- Docker Desktop (ou Docker Engine) + Docker Compose v2
- Git
- Node.js 20+ (optionnel, seulement si exécution locale sans Docker)

## Lancer en local (Docker Compose)

```bash
cd vitalsync
cp .env.example .env
docker compose up --build
```

Accès:
- Frontend: `http://localhost:8080`
- API via proxy: `http://localhost:8080/api/health`

## Pipeline CI/CD (GitHub Actions)

La pipeline `.github/workflows/ci-cd.yml` exécute:
- **Lint & tests** (ESLint + Jest) sur le backend
- **Build & push** des images backend/frontend vers **GHCR**, taggées avec le **SHA** du commit
- **Déploiement staging simulé** via `docker compose` + **health check** HTTP; la pipeline échoue si le check échoue

## Schéma (Mermaid)

```mermaid
flowchart LR
  U[Utilisateur] -->|HTTP :8080| FE[Nginx Frontend]
  FE -->|proxy /api/*| BE[Node/Express :3000]
  BE -->|TCP 5432| DB[(PostgreSQL)]
```

