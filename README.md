# 🚀 Jarvis Backend (Ollama)

This repository contains the Docker configuration for the Jarvis AI backend. It leverages [Ollama](https://ollama.com) to run large language models (LLMs) locally, specifically optimized for the `controller_phone` Flutter application.

---

## 📋 Table of Contents
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Managing Models](#-managing-models)
- [Connecting Your Phone](#-connecting-your-phone)
- [API Documentation (Testing)](#-api-documentation-testing)
- [Service Details](#-service-details)
- [Performance Optimization](#-performance-optimization)
- [Troubleshooting](#-troubleshooting)

---

## 🛠 Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.
- At least 4GB of RAM allocated to Docker.
- A stable internet connection (for the initial model download).

---

## 🚀 Getting Started

### 1. Start the Backend
Run the following command to start the Ollama engine:

```bash
docker compose up -d
```

### 2. Pull a Model
Ollama starts empty. You must manually pull the model you want to use. We recommend `qwen2.5:0.5b` for the Jarvis app:

```bash
docker compose exec ollama ollama pull qwen2.5:0.5b
```

### 3. Verify the Installation
Check if the container is running:
```bash
docker compose ps
```

Verify that your model is ready:
```bash
docker compose exec ollama ollama list
```

---

## 🧠 Managing Models

Ollama allows you to run many different LLMs. You can browse available models at [ollama.com/library](https://ollama.com/library).

### Download a Model
```bash
docker compose exec ollama ollama pull <model-name>
```

### List Your Models
```bash
docker compose exec ollama ollama list
```

### Delete a Model
```bash
docker compose exec ollama ollama rm <model-name>
```

---

## 📱 Connecting Your Phone

To allow your mobile device to communicate with this backend:

1. **Network**: Ensure your Mac and Phone are connected to the **same Wi-Fi network**.
2. **Identify IP**: Find your Mac's local IP address:
   ```bash
   ipconfig getifaddr en0
   ```
3. **App Configuration**: 
   - Open the `controller_phone` app.
   - Navigate to **Settings**.
   - Update the **Server IP** to match your Mac's IP (e.g., `192.168.1.XX`).
   - Ensure the port is set to `3017`.

---

## 🔌 API Documentation (Testing)

You can test the backend directly from your terminal using `curl`.

### Generate a Response
```bash
curl http://localhost:3017/api/generate -d '{
  "model": "qwen2.5:0.5b",
  "prompt": "Why is the sky blue?",
  "stream": false
}'
```

### Chat Completion
```bash
curl http://localhost:3017/api/chat -d '{
  "model": "qwen2.5:0.5b",
  "messages": [
    { "role": "user", "content": "Hello, Jarvis!" }
  ],
  "stream": false
}'
```

---

## ⚙️ Service Details

- **Ollama**: The core inference engine running on port `3017`.
- **Data Persistence**: Models and configurations are stored in the `./ollama_data` directory, so they persist across restarts.

---

## ⚡ Performance Optimization

> [!TIP]
> **GPU Acceleration**: Docker on macOS currently runs Ollama via CPU-only inference, which can be slow. 
> For significantly faster performance (using Apple Silicon GPU/Metal), we recommend:
> 1. Stopping the Docker containers: `docker compose down`
> 2. Downloading the [Native Ollama App](https://ollama.com/download/mac).
> 3. Running `ollama run qwen2.5:0.5b` directly from your terminal.

---

## 💡 Troubleshooting

| Issue | Solution |
| :--- | :--- |
| **Connection Refused** | Ensure Docker is running and the `ollama` container is active. |
| **Phone can't connect** | Check your Mac's Firewall settings (System Settings > Network > Firewall) and ensure port 3017 is open. |
| **Model not found** | Run `docker compose exec ollama ollama pull qwen2.5:0.5b` manually. |
| **High Resource Usage** | The `qwen2.5:0.5b` model is very lightweight, but ensure no other heavy LLMs are running. |
