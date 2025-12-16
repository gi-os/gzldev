# Building BasilNet: My Home Server Stack

After months of iteration, I've finally settled on a Docker Compose setup that handles all my media needs without breaking a sweat. Here's the full breakdown.

## The Stack

BasilNet runs on an old Dell OptiPlex with 32GB RAM and a 4TB NAS mounted over NFS. The core services:

- **Plex** - Media streaming
- **Sonarr/Radarr/Lidarr** - Automated content management
- **qBittorrent** - Downloads (routed through Gluetun VPN)
- **Prowlarr** - Indexer management
- **Overseerr** - Request management
- **Tautulli** - Plex analytics

## VPN Configuration

The key to this setup is Gluetun. All torrent traffic routes through a Mullvad VPN container:

```yaml
gluetun:
  image: qmcgaw/gluetun
  cap_add:
    - NET_ADMIN
  environment:
    - VPN_SERVICE_PROVIDER=mullvad
    - VPN_TYPE=wireguard
    - WIREGUARD_PRIVATE_KEY=${MULLVAD_KEY}
    - SERVER_COUNTRIES=Switzerland
  ports:
    - 8080:8080  # qBittorrent WebUI
```

qBittorrent then uses `network_mode: "service:gluetun"` to route all its traffic through the VPN container.

## Home Assistant Integration

The real magic is the Home Assistant integration. I've got automations that:

1. Pause downloads when streaming to save bandwidth
2. Send notifications when new content is available
3. Display "Now Playing" on my wall-mounted dashboard

## What's Next

Planning to add:
- Jellyfin as a Plex backup
- Calibre-web for ebooks
- Immich for photos

The full docker-compose.yml is on my GitHub if you want to steal it.
