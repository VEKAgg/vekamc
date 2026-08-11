# AGENTS.md

## What This Is

PaperMC 1.21.1 **SMP server config repo**. No application code — only plugin JARs, configs, and server properties.

- **Branch**: `main` — active server config
- **Remote**: `https://github.com/VEKAgg/vekamc.git`
- **Domain**: `mc.veka.gg`

## Server Settings (server.properties)

| Setting | Value | Why |
|---------|-------|-----|
| `online-mode` | `false` | Cracked mode — AuthMe required |
| `enforce-secure-profile` | `false` | Required for cracked mode |
| `difficulty` | `normal` | |
| `max-players` | `69` | |
| `view-distance` | `16` | |
| `simulation-distance` | `5` | |
| `spawn-protection` | `0` | WorldGuard handles spawn |
| `allow-flight` | `true` | Prevents false anticheat kicks |
| `sync-chunk-writes` | `false` | Better performance |
| `pvp` | `true` | |

## Plugins (15)

**Security**: AuthMe (login), Grim (anticheat), ProtocolLib (Grim dep)
**Core**: EssentialsX, EssentialsXChat, LuckPerms, Vault, PlaceholderAPI, ViaVersion
**Protection**: WorldGuard, WorldEdit, CoreProtect, GriefPrevention
**QoL**: Harbor (sleep), spark (profiler)

## Key Configs

### EssentialsX (`plugins/Essentials/config.yml`)
- Chat format: `{PREFIX}&r &f{USERNAME} &7>> &f{MESSAGE}`
- Homes: 1 default, 3 mod, unlimited admin
- Economy: $50 starting balance, $ currency
- Starter kit: `starter` (wooden tools + bread + compass + torches + guide book)
- Teleport delay: 3 seconds
- Respawn at home/bed: enabled

### LuckPerms (`plugins/LuckPerms/yaml-storage/groups.yml`)
- `default` (weight 10, `[Player]`): essentials basics, signs, GriefPrevention
- `moderator` (weight 50, `[Mod]`): WorldGuard, WorldEdit, CoreProtect, moderation
- `admin` (weight 100, `[Admin]`): wildcard `*`

### Harbor (`plugins/Harbor/config.yml`)
- 60% sleep threshold, 15 min AFK timeout, nether/end excluded

## Gotchas

- `paper-1.21.1.jar` is the server JAR (not `server.jar` which is gitignored)
- `run.bat` hardcodes a JetBrains JRE path — won't work elsewhere
- `online-mode=false` without AuthMe = anyone can impersonate any username
- Grim requires ProtocolLib — always install both
- Plugin dirs (WorldGuard, CoreProtect, GriefPrevention) don't exist until first server start
- Cloudflare Tunnel must be running for external access
- Java 24 required

## In-Game Setup (After Server Start)

1. Set world border: `/worldborder center ~ ~` + `/worldborder set 20000`
2. Create WorldGuard spawn region (`/rg define spawn` with 20-block radius)
3. Build physical chest shops in spawn

## Conventions

- Config-only repo — no Java, no build tools
- Plugin JARs committed directly (no Maven/Gradle)
- All runtime data gitignored (worlds, logs, cache)
