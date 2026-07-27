## Jellyfin, Plex, and Servarr Docker Setup

```bash
/docker
├── jellyfin
│   └── config
├── plex
│   ├── overseer
│   │   └── config
│   ├── plex
│   └── tautulli
└── servarr
    ├── bazarr
    ├── gluetun
    ├── jellyseerr
    ├── lidarr
    ├── nzbget
    ├── prowlarr
    ├── qbittorrent
    ├── radarr
    └── sonarr

/data
├── downloads
│   ├── nzbget
│   │   ├── completed
│   │   ├── intermediate
│   │   ├── nzb
│   │   ├── queue
│   │   └── tmp
│   └── qbittorrent
│       ├── completed
│       ├── incomplete
│       └── torrents
├── movies
├── music
└── shows
```

## Configuration

Each stack directory contains a `.env.example` file. Copy it to `.env` and fill in your values before starting:

```bash
cp .env.example .env
```

`.env` files are gitignored and never committed.

## Arr Stack (`media/arr`)

The arr stack supports an optional Gluetun VPN. Two compose files are provided:

| File | Purpose |
|------|---------|
| `compose.yml` | Base stack, no VPN |
| `compose.gluetun.yml` | Override that adds Gluetun and routes qBittorrent, Nzbget, and Prowlarr through it |

**Without VPN:**
```bash
docker compose up -d
```

**With VPN:**
```bash
docker compose -f compose.yml -f compose.gluetun.yml up -d
```

When running without VPN, qBittorrent, Nzbget, and Prowlarr are reachable directly on `servarrnetwork` and expose their own ports. When running with VPN, their traffic is routed through Gluetun and their ports are published via the Gluetun container instead.
