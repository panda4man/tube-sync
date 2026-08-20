# tube-sync

Docker Compose setup for [TubeSync](https://github.com/meeb/tubesync), a self-hosted YouTube channel/playlist downloader, run on my homelab.

## Usage

1. Copy `.env.example` to `.env` and set `TUBESYNC_PORT` if you don't want the default.
2. Ensure the external `homelab_stack` network exists (`docker network create homelab_stack`).
3. `docker compose up -d`

Web UI available at `http://<host>:${TUBESYNC_PORT:-4848}`.

## Config

- `./config` — persisted app config/db, mounted into the container.
- `/mnt/user/media-data/media/youtube` — download destination, mounted into the container.
- `TZ`, `PUID`, `PGID` — set in `docker-compose.yml` to match the host user.
