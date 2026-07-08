# Pterodactyl SMP Server — Operational Context

> **Initial state**: Panel was a flat tar.gz dump — no `composer install`, empty `APP_KEY`, uid 1001/root ownership, no Nginx site config, missing `php8.3-redis`, no queue worker, no scheduler cron, broken MariaDB auth, no Docker, no Wings, no eggs/nests, no allocations, no servers.
>
> **Remediation** (12 July 2026): Fixed all 10 panel issues (redis ext, DB auth, ownership, composer install, APP_KEY, migrations, Nginx config, pteroq unit, cron, Docker + Wings install). Created admin user, location, and node. Provisioned SMP Paper 1.21 server at `0.0.0.0:25565` with 2GB RAM / 6GB disk. Installed 8 plugins (LuckPerms 5.5.59, Vault 1.7.3, PlaceholderAPI 2.12.2, EssentialsX 2.21.0, EssentialsXChat 2.21.0, ViaVersion 5.1.1, Spark, Harbor 1.6.3). Configured LuckPerms `admin` (`*`) and `default` (essentials/harbor/minecraft commands) groups. Spark baseline: 20 TPS, 0.2ms tick, 12% CPU. Later merged from `VEKAgg/vekamc` git repo, adding 7 more plugins (AuthMe, CoreProtect, GriefPrevention, ProtocolLib, GrimAC, WorldEdit, WorldGuard) and a 3-tier LuckPerms setup (default/moderator/admin).

## Host
- **Hostname**: games
- **Internal IP**: 192.168.1.7
- **External IP**: 103.26.233.63 (NAT — port forwarding required)
- **OS**: Ubuntu 24.04, kernel 6.8.0-134-generic
- **Disk**: 15G total, ~5.8G free (`/dev/mapper/ubuntu--vg-ubuntu--lv`)
- **User**: `game` (sudo via password `#Game7`)
- **Panel URL**: http://192.168.1.7

## Services (all active)
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

## SMP Server — Paper 1.21
- **Server UUID**: `9561c281-0671-413a-89e0-43afdb2265d6`
- **Short ID**: `9561c281`
- **Name**: SMP
- **Egg**: Paper (ID=5, nest=Minecraft ID=1)
- **Docker image**: `ghcr.io/pterodactyl/yolks:java_21` (Temurin 21.0.11)
- **Allocation**: `0.0.0.0:25565` (also `103.26.233.63:25565`)
- **Memory**: 2048 MB
- **Disk**: 6144 MB
- **Data dir**: `/var/lib/pterodactyl/volumes/9561c281-0671-413a-89e0-43afdb2265d6/`

### server.properties
- Port: 25565, Online-mode: true, Difficulty: hard, Max players: 20
- EULA: accepted

### Installed Plugins
| Plugin | File | Status |
|--------|------|--------|
| LuckPerms | `LuckPerms-Bukkit-5.5.59.jar` | ✅ v5.5.59 (loaded) |
| Vault | `Vault-1.7.3-b131.jar` | ✅ v1.7.3 (loaded) |
| PlaceholderAPI | `PlaceholderAPI-2.11.6.jar` | ✅ v2.12.2 (loaded) |
| EssentialsX | `EssentialsX-2.21.0.jar` | ✅ v2.21.0 (loaded, "unsupported version" warning but functional) |
| EssentialsXChat | `EssentialsXChat-2.21.0.jar` | ✅ v2.21.0 (loaded) |
| ViaVersion | `ViaVersion-5.1.1.jar` | ✅ v5.1.1 (loaded) |
| Spark | `spark.jar` | ✅ loaded |
| Harbor | `Harbor.jar` | ✅ v1.6.3 (loaded) |
| **AuthMe** | `AuthMe-5.7.0.jar` | ✅ v5.7.0 (needs first-run config) |
| **CoreProtect** | `CoreProtect-CE-23.2.jar` | ✅ CE 23.2 (needs first-run config) |
| **GriefPrevention** | `GriefPrevention.jar` | ✅ (needs first-run config) |
| **ProtocolLib** | `ProtocolLib.jar` | ✅ (loaded) |
| **GrimAC** | `grimac-2.3.71.jar` | ✅ v2.3.71 (needs first-run config) |
| **WorldEdit** | `worldedit-bukkit-7.3.9.jar` | ✅ v7.3.9 (loaded) |
| **WorldGuard** | `worldguard-bukkit-7.0.12-dist.jar` | ✅ v7.0.12 (loaded) |

### LuckPerms Groups
- **admin**: `*` (wildcard, full access) — inherits moderator
- **moderator**: essentials moderation (kick/ban/mute/fly/tp/invsee/vanish/heal/gamemode), worldedit, worldguard, coreprotect, griefprevention.admin — inherits default
- **default**: full essentials suite (spawn/tpa/home/warp/kit/pay/balance/sell/back/afk/near/ping/mail/help), sign use/create, griefprevention claim tools, harbor.ignored

### Spark Baseline (idle, no players)
- **TPS**: 20.0 / 20.0 / 20.0 (5s/10s/1m)
- **Tick ms**: 0.2 med / 0.4 95%ile
- **CPU**: 12% process, 12% system (10s)
- **Memory**: ~800MB (within 2048MB limit)

### Known Issues
1. **Chunky** not operational — download from https://github.com/pop4959/Chunky/releases (need Paper 1.21+ compatible version, likely 1.5.4+). Then upload via Panel file manager to `plugins/` and restart.
2. **EssentialsX 2.21.0** shows "unsupported server version" on Paper 1.21 — functional but some features may be incomplete. Upgrade to 2.22.0+ if available.
3. **No DNS** — players connect via raw IP `103.26.233.63:25565`. Set up an A record if desired.
4. **Disk space tight** — 5.8G free for world + backups + plugins. Monitor closely after pregeneration.
5. **External ports** — 25565, 8080 (Wings API), 2022 (SFTP) need port forwarding on the gateway/router for external access. UFW is configured to allow these.

## Future Phases (not configured)
- Skyblock: separate server/node, not set up
- Multi-gamemode: downstream plan
- DiscordSRV, anti-cheat, NPCs, mcMMO: deferred

## Useful Commands
```bash
# Panel
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
