# Homelab
Personal homelab running 8 self-hosted services in Docker, accessible remotely via [Tailscale](https://tailscale.com/).

## Specs
| Component | Details |
|-----------|---------|
| CPU | AMD Ryzen 5 5600X (6c/12t) |
| GPU | AMD Radeon RX 6500 XT |
| RAM | 16 GB |
| Storage | 500 GB SSD |
| OS | Ubuntu 24.04 LTS |

## Services
| Service | Description |
|---------|-------------|
| [Homepage](https://gethomepage.dev/) | Dashboard for the homelab |
| [Jellyfin](https://jellyfin.org/) | Media server |
| [Minecraft](https://www.minecraft.net/) | Game servers |
| [Nextcloud](https://nextcloud.com/) | Self-hosted cloud storage and collaboration |
| [Pi-hole](https://pi-hole.net/) | Network-wide DNS filtering and ad blocking |
| [playit.gg](https://playit.gg/) | IP tunneling for game servers |
| [Syncthing](https://syncthing.net/) | Continuous file sync between machines |
| [Uptime Kuma](https://uptime.kuma.pet/) | Self-hosted uptime monitoring |

## Scheduled jobs
Cron jobs on the host, mostly [rclone](https://rclone.org/) syncs to [Filen](https://filen.io/) cloud storage. The crontabs live in [`cron/`](cron/).

| Schedule | User | Job |
|----------|------|-----|
| Every 5 min | harley | Update the NextDNS linked IP |
| Every 5 min | root | Nextcloud `occ files:scan --all` |
| Every 6 h (:00) | harley | Filen `/Archive` → `/srv/filen-archive` (mounted read-only in Nextcloud) |
| Every 6 h (:30) | root | Nextcloud files → Filen `/Active` |
| Every 6 h (:45) | harley | Syncthing `documents` → Filen `/Syncthing` |

rclone logs are written to `~/logs/`.

To restore on a new machine:
```bash
sudo crontab cron/root.crontab
crontab cron/harley.crontab
```
