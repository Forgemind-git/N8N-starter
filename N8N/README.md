# n8n starter Docker Environment

This repository provides a Docker Compose setup to orchestrate the following services:
- **n8n** (workflow automation tool)
- **PostgreSQL** (as n8n's database)
- **Qdrant** (vector search engine for AI-related tasks)
- **n8n-import** (a one-time service to import workflows and credentials)

---

## 🧱 Services

### 1. `n8n`
- The core automation platform.
- Exposes port `5678`.
- Mounts volumes for persistent storage, workflow/credential backups, and shared data.
- Depends on:
  - `postgres` (healthy)
  - `n8n-import` (successful completion)

### 2. `postgres`
- Lightweight PostgreSQL database (v16-alpine).
- Stores data in the `postgres_storage` volume.
- Includes a health check to ensure it's ready before other services connect.

### 3. `n8n-import`
- A short-lived container to import existing credentials and workflows from the `/n8n/backup` directory.
- Uses the same configuration as the main `n8n` service.
- Runs once, before the main `n8n` container starts.

### 4. `qdrant`
- A high-performance vector database used for semantic search or AI-powered features.
- Exposes port `6333` for API access.
- Persists data in `qdrant_storage`.

---

## 🔄 Volumes

| Name              | Purpose                                 |
|-------------------|-----------------------------------------|
| `n8n_storage`     | Stores n8n's internal configuration and workflow data |
| `postgres_storage`| Stores PostgreSQL data                  |
| `qdrant_storage`  | Stores Qdrant data                      |

---

## 🌐 Networks

| Name   | Purpose               |
|--------|-----------------------|
| `demo` | Shared network between all services |

---

## ⚙️ Environment Variables

Make sure to define the following variables in a `.env` file or your shell environment:
- `POSTGRES_USER`
- `POSTGRES_PASSWORD`
- `POSTGRES_DB`
- `N8N_ENCRYPTION_KEY`
- `N8N_USER_MANAGEMENT_JWT_SECRET`

---

## 🚀 Usage

1. Create your `.env` file with the required variables.
2. Place your credentials and workflows in `n8n/backup`.
3. Run the stack:
   ```bash
   docker-compose up -d

## Steps to bring up container

1. Copy the docker compose to seperate directory.
2. use `docker compose up -d` to start the container.
3. Ensure you have docker command line installed.
4. use `docker compose down` to stop the container.
