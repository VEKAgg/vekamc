# HOSTING.md

## Host Machine

- **Hostname**: games
- **Internal IP**: 192.168.1.7
- **External IP**: 103.26.233.63 (NAT — port forwarding required)
- **OS**: Ubuntu 24.04, kernel 6.8.0-134-generic
- **Disk**: 15G total, ~5.8G free (`/dev/mapper/ubuntu--vg-ubuntu--lv`)
- **User**: `game` (sudo via password `#Game7`)

## Services

| Service | Status | Purpose |
|---------|--------|---------|
| `php8.3-fpm` | active | Panel PHP backend |
| `nginx` | active | Panel web server (port 80) |
| `mariadb` | active | Panel database (port 3306, localhost only) |
| `redis-server` | active | Queue driver (port 6379, localhost only) |
| `docker` | active | Container runtime (v29.1.3) |
| `pteroq` | active | Laravel queue worker (redis) |
| `wings` | active | Daemon (API :8080, SFTP :2022) |

## Panel

- **Version**: 1.14.1
- **Path**: `/var/www/pterodactyl`
- **Admin user**: `admin@example.com` / `changeme123`
- **API Application Key**: stored in `/tmp/pterodactyl_api_full_key`
- **API Client Key**: stored in `/tmp/pterodactyl_client_api_key`
- **DB**: `panel` / user `pterodactyl` / pass in `/var/www/pterodactyl/.env`
- **Node**: `main` (ID=1, FQDN=192.168.1.7, scheme=http)
- **Location**: `main` (ID=1)

## SMP Server (Pterodactyl)

- **Server UUID**: `9561c281-0671-413a-89e0-43afdb2265d6`
- **Short ID**: `9561c281`
- **Egg**: Paper (ID=5, nest=Minecraft ID=1)
- **Docker image**: `ghcr.io/pterodactyl/yolks:java_21` (Temurin 21.0.11)
- **Allocation**: `0.0.0.0:25565` (also `103.26.233.63:25565`)
- **Memory**: 2048 MB
- **Disk**: 6144 MB
- **Data dir**: `/var/lib/pterodactyl/volumes/9561c281-0671-413a-89e0-43afdb2265d6/`

## Useful Commands

```bash
# Panel config
cd /var/www/pterodactyl && sudo -u www-data php artisan p:node:configuration 1

# Server control via API
KEY=$(cat /tmp/pterodactyl_client_api_key)
FULL_KEY="ptlc_c432b3eP5Lp${KEY}"
curl -H "Authorization: Bearer ${FULL_KEY}" \
  -X POST "http://192.168.1.7/api/client/servers/9561c281/power" \
  -d '{"signal": "restart"}'

# Send command
curl -H "Authorization: Bearer ${FULL_KEY}" \
  -X POST "http://192.168.1.7/api/client/servers/9561c281/command" \
  -d '{"command": "say hello"}'

# View server console
sudo docker logs 9561c281-0671-413a-89e0-43afdb2265d6

# Wings
sudo systemctl status wings
sudo journalctl -u wings -n 50
```

## Known Issues

1. **No DNS** — players connect via raw IP `103.26.233.63:25565`
2. **Disk space tight** — 5.8G free for world + backups + plugins
3. **External ports** — 25565, 8080 (Wings API), 2022 (SFTP) need port forwarding

## Spark Baseline (idle, no players)

- **TPS**: 20.0 / 20.0 / 20.0
- **Tick ms**: 0.2 med / 0.4 95%ile
- **CPU**: 12% process
- **Memory**: ~800MB (within 2048MB limit)
