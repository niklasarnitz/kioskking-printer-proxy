# KioskKing Printer Proxy

Small Docker Compose setup that exposes a local CUPS server to browser-based KioskKing clients through an Nginx proxy with CORS headers.

The container listens on port `8631` and forwards IPP/CUPS requests to the host machine on port `631`.

## Usage

```sh
cp .env.example .env
docker compose up -d
```

By default, the proxy allows requests from:

- `http://localhost:3000`
- `https://kioskking.arnitz.org`

Adjust `.env` if your app runs on another origin or if CUPS is reachable through another host/port.

## Configuration

| Variable | Default | Description |
| --- | --- | --- |
| `PROXY_PORT` | `8631` | Host port exposed by Docker |
| `CUPS_HOST` | `host.docker.internal` | Hostname of the CUPS server from inside the container |
| `CUPS_PORT` | `631` | CUPS port |
| `ALLOWED_ORIGIN_LOCAL` | `http://localhost:3000` | Local development origin allowed by CORS |
| `ALLOWED_ORIGIN_PRODUCTION` | `https://kioskking.arnitz.org` | Production origin allowed by CORS |

## Requirements

- Docker Compose
- CUPS running on the host machine
- A browser/app that sends IPP requests to `http://<host>:8631`
