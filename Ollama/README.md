# 🧠 Ollama + Open WebUI + Ngrok Docker Environment

This Docker Compose setup provides a local LLM environment with the following services:
- **Ollama**: A self-hosted runtime for large language models.
- **Open WebUI**: A modern UI to interact with Ollama.
- **Ngrok**: Secure public tunnel to access Ollama remotely.
- **Model Preloader**: A one-time container to preload a specific LLM model.

---

## 🚀 Services

### 1. `ollama`
- The core service to run and serve LLMs.
- Exposes API on port `11434`.
- Stores models and data in the `ollama_storage` volume.
- Includes `host.docker.internal` access for seamless host-guest communication.

### 2. `open-webui`
- Frontend interface for interacting with Ollama.
- Maps port `8080` to `localhost:3000`.
- Uses `OLLAMA_BASE_URL=http://docker.for.mac.localhost:11434` for backend access.
- Stores backend data in `open-webui-data`.

### 3. `ollama-pull-models`
- One-time container that pulls the `llama3.2` model into Ollama at startup.
- Delays start slightly (`sleep 3`) to ensure Ollama is ready.
- Shares the same volume with Ollama so the model is retained.

### 4. `ngrok-ollama`
- Exposes Ollama’s port `11434` publicly using [Ngrok](https://ngrok.com).
- Uses a custom subdomain (e.g., `humble-presumably-longhorn.ngrok-free.app`).
- Requires an Ngrok authtoken (`NGROK_AUTHTOKEN`) for authentication.

---

## 🗃 Volumes

| Volume Name       | Description                         |
|-------------------|-------------------------------------|
| `ollama_storage`  | Persists models and Ollama configs  |
| `open-webui-data` | Persists user and chat data for UI  |

---

## 🌐 Network

| Name  | Purpose                  |
|-------|--------------------------|
| `demo`| Shared network for services|

Note: `ngrok-ollama` uses `host` mode for seamless tunneling.

---

## 📦 Prerequisites

- Docker + Docker Compose installed
- (Optional) Ngrok account to manage custom domains and access tokens
- Edit the environment variables in the `docker-compose.yaml`:
  - `NGROK_AUTHTOKEN`
  - `OLLAMA_BASE_URL` (adjust for your system if needed)

---

## 🧩 Usage

1. Clone the repository and navigate to its directory.
2. Run the services:
   ```bash
   docker-compose up -d

## 

## Accessing interfaces

1. Open WebUI: http://localhost:3000
2. Ollama API (local): http://localhost:11434
3. Ollama API (public): <your-static-domain-from-ngrok>
