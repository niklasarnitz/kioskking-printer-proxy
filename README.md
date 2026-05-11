# KioskKing Printer Proxy

Small Docker Compose setup that exposes a local CUPS server to browser-based KioskKing clients through an Nginx proxy with CORS headers.

The container listens on port `8631` and forwards IPP/CUPS requests to the host machine on port `631`.

## Usage

On macOS / Docker Desktop:

```sh
cp .env.example .env
docker compose up -d
```

On native Linux, including Arch and Debian, use the Linux compose file:

```sh
cp .env.example .env
docker compose -f docker-compose.linux.yml up -d
```

Native Linux CUPS rejects requests from Docker bridge networks when the
HTTP `Host` header claims `localhost:631`. The Linux compose file uses host
networking and proxies to `127.0.0.1:631`, so CUPS accepts the request as a
local one.

By default, the proxy allows requests from:

- `http://localhost:3000`
- `https://kioskking.arnitz.org`

Adjust `.env` if your app runs on another origin or, for the default
Docker Desktop setup, if CUPS is reachable through another host/port.

## Configuration

| Variable | Default | Description |
| --- | --- | --- |
| `PROXY_PORT` | `8631` | Host port exposed by Docker |
| `CUPS_HOST` | `host.docker.internal` | Hostname of the CUPS server from inside the container |
| `CUPS_PORT` | `631` | CUPS port |
| `ALLOWED_ORIGIN_LOCAL` | `http://localhost:3000` | Local development origin allowed by CORS |
| `ALLOWED_ORIGIN_PRODUCTION` | `https://kioskking.arnitz.org` | Production origin allowed by CORS |

`PROXY_PORT` and `CUPS_HOST` are used by the default Docker bridge setup.
The Linux host-network setup always listens on `8631` and proxies to
`127.0.0.1:${CUPS_PORT}`.

## Requirements

- Docker Compose
- CUPS running on the host machine
- A browser/app that sends IPP requests to `http://<host>:8631`
