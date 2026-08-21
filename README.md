# tube-sync

Docker Compose setup for [TubeSync](https://github.com/meeb/tubesync), a self-hosted YouTube channel/playlist downloader, run on my homelab. Routed through a dedicated [gluetun](https://github.com/qdm12/gluetun) WireGuard tunnel (ProtonVPN) so a dropped VPN connection kills tubesync's network instead of leaking traffic on the host IP.

## Usage

1. Copy `.env.example` to `.env`, set `TUBESYNC_PORT` if you don't want the default, and set `PROTONVPN_TUBESYNC_WIREGUARD_PRIVATE_KEY` (generate a dedicated WireGuard key for this stack in the ProtonVPN dashboard — don't reuse the servarr stack's key).
2. Ensure the external `homelab_stack` network exists (`docker network create homelab_stack`).
3. `docker compose up -d`

Web UI available at `http://<host>:${TUBESYNC_PORT:-4848}` (published on the `gluetun-tubesync` container since tubesync shares its network namespace).

## Config

- `./config` — persisted app config/db, mounted into the container.
- `/mnt/user/media-data/media/youtube` — download destination, mounted into the container.
- `./gluetun` — gluetun state dir (WireGuard connection state, etc).
- `TZ`, `PUID`, `PGID` — set in `docker-compose.yml` to match the host user.
- `gluetun-tubesync` — VPN sidecar; tubesync runs with `network_mode: service:gluetun-tubesync`, so all its traffic goes through the tunnel and dies with it if the VPN drops.
