# Nanobot Railway Template

One-click deploy [nanobot](https://github.com/nano-bot/nanobot) on [Railway](https://railway.app) with a modern, dark-themed web configuration UI and status dashboard.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/template/nanobot)

## Features

- **Modern Web UI**: A sleek, dark-themed interface with sidebar navigation for easy management.
- **Overview Dashboard**:
    - **System Status**: Real-time visual indicator of the Gateway state.
    - **Setup Workflow**: A step-by-step visual guide to help you get started (Add Provider -> Add Channel -> Save & Start).
    - **Live Counters**: Monitor active providers and enabled channels at a glance.
- **AI Providers Management**: Configure API keys for Anthropic, OpenAI, OpenRouter, DeepSeek, Groq, Gemini, Zhipu, and vLLM via a card-based interface.
- **Channel Configuration**: Easily enable and configure messaging channels like Telegram, WhatsApp, and Feishu with toggle switches.
- **System Controls**: Start, stop, and restart the Nanobot gateway directly from the UI.
- **Live Logs**: Built-in terminal viewer to monitor system logs in real-time.
- **Secure**: Password-protected admin panel with Basic Auth.

## Quick Start

### Deploy to Railway

1.  Click the "Deploy on Railway" button above.
2.  Set the `ADMIN_PASSWORD` environment variable (or a random one will be generated).
3.  Attach a volume mounted at `/data` for persistent storage.
4.  **Open your app URL**: Log in with your credentials (default username: `admin`).
5.  **Follow the On-screen Setup Workflow**:
    -   **Step 1**: Go to **AI Providers** and configure at least one API key.
    -   **Step 2**: Go to **Channels** and enable a messaging platform (e.g., Telegram).
    -   **Step 3**: Click **Save Changes** and then **Start** to launch the gateway.
    -   **Step 4**: Verify by sending a message to your bot.

### Run Locally

You can run the template locally using Docker:

```bash
docker build -t nanobot .
docker run --rm -it -p 8080:8080 -e PORT=8080 -e ADMIN_PASSWORD=changeme -v nanobot-data:/data nanobot
```

Open `http://localhost:8080` and log in with username `admin` and password `changeme`.

## Environment Variables

| Variable | Default | Description |
| :--- | :--- | :--- |
| `PORT` | `8080` | Web server port. |
| `ADMIN_USERNAME` | `admin` | Basic auth username. |
| `ADMIN_PASSWORD` | *(generated)* | Basic auth password. If unset, a random password is generated and printed to stdout. |

## Architecture

The application runs a lightweight Python web server using Starlette and Uvicorn, which manages the Nanobot gateway as a subprocess.

-   **Frontend**: HTML5 + Tailwind CSS + Alpine.js (Single `index.html` template).
-   **Backend**: Python Starlette server handling API requests and managing the Gateway process.
-   **Storage**: Configuration is saved to JSON files in the `/data` volume.

## API Endpoints

The Web UI communicates with the backend via a REST API:

| Method | Path | Description |
| :--- | :--- | :--- |
| `GET` | `/api/config` | Get current configuration (secrets masked). |
| `PUT` | `/api/config` | Save new configuration. |
| `GET` | `/api/status` | Get gateway, provider, and channel status. |
| `GET` | `/api/logs` | Fetch recent log lines. |
| `POST` | `/api/gateway/start` | Start the gateway process. |
| `POST` | `/api/gateway/stop` | Stop the gateway process. |
| `POST` | `/api/gateway/restart` | Restart the gateway process. |

## Supported Integrations

**AI Providers**: Anthropic, OpenAI, OpenRouter, DeepSeek, Groq, Gemini, Zhipu, vLLM.

**Channels**: Telegram, WhatsApp (via bridge), Feishu/Lark.
