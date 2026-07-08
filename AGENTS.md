# AGENTS.md

## What This Repo Is

Paper Minecraft server **configuration repository** — no application code, no build system. Contains config files, plugin JARs, and plugin configs for a **Survival Multiplayer (SMP) server**.

- **Paper version**: 1.21.1 (`paper-1.21.1.jar`)
- **Branch**: `main` (active server config)
- **Remote**: `https://github.com/VEKAgg/vekamc.git`
- **Domain**: `mc.veka.gg`
- **Owner**: Shafaat Ahmed Sharief

## Hosting Architecture

```
[Gaming PC (Atlas OS / Windows 11)]
    └── [Proxmox VM]
        └── [Pterodactyl Panel]
            └── [PaperMC Server]
                └── [Cloudflare Tunnel] → mc.veka.gg
```

## Key Files

| File | Purpose |
|------|---------|
| `server.properties` | Core Minecraft server settings |
| `spigot.yml` | Spigot-layer settings (mob caps, entity ranges) |
| `bukkit.yml` | Bukkit-layer settings (autosave, chunk GC) |
| `config/paper-global.yml` | Paper global optimizations (v29) |
| `config/paper-world-defaults.yml` | Paper per-world defaults (v31) |
| `run.bat` | Local dev launcher (Windows, 4G heap, Aikar flags) |
| `server-icon.png` | Custom server favicon (64x64) |
| `plugins/*/config.yml` | Per-plugin configuration |
| `plugins/LuckPerms/yaml-storage/groups.yml` | LuckPerms groups and permissions |

## Installed Plugins (15)

### Security & Auth
| Plugin | JAR | Purpose |
|--------|-----|---------|
| AuthMe | `AuthMe-5.7.0.jar` | Login/register for cracked mode |
| Grim Anticheat | `grimac-2.3.71.jar` | Anti-cheat (29 detections) |
| ProtocolLib | `ProtocolLib.jar` | Packet-level checks (required by Grim) |

### Core Infrastructure
| Plugin | JAR | Purpose |
|--------|-----|---------|
| EssentialsX | `EssentialsX-2.21.0.jar` | Commands, economy, homes, kits |
| EssentialsXChat | `EssentialsXChat-2.21.0.jar` | Chat formatting with rank prefixes |
| LuckPerms | `LuckPerms-Bukkit-5.5.59.jar` | Permission management |
| Vault | `Vault-1.7.3-b131.jar` | Economy/permissions API bridge |
| PlaceholderAPI | `PlaceholderAPI-2.11.6.jar` | Placeholder system |
| ViaVersion | `ViaVersion-5.1.1.jar` | Multi-version support (1.8-1.21) |

### Protection
| Plugin | JAR | Purpose |
|--------|-----|---------|
| WorldGuard | `worldguard-bukkit-7.0.12-dist.jar` | Region protection |
| WorldEdit | `worldedit-bukkit-7.3.9.jar` | Building tool (staff) |
| CoreProtect | `CoreProtect-CE-23.2.jar` | Block logging and rollback |
| GriefPrevention | `GriefPrevention.jar` | Golden shovel land claims |

### Performance & QoL
| Plugin | JAR | Purpose |
|--------|-----|---------|
| Harbor | `Harbor.jar` | Sleep/night-skip mechanics |
| spark | `spark.jar` | Performance profiler |

## Running the Server

```bash
# Local (Windows) — requires Java in PATH or edit run.bat
run.bat
```

The `server.jar` is gitignored. The actual JAR is `paper-1.21.1.jar`.

## Server State

- **Branch**: `main` (active)
- **Difficulty**: Normal
- **Gamemode**: Survival (default)
- **Max players**: 69
- **Online mode**: false (cracked — AuthMe for security)
- **View distance**: 16 chunks
- **Simulation distance**: 5 chunks
- **Spawn protection**: 0 (WorldGuard handles it)
- **Allow flight**: true
- **PVP**: true
- **Economy**: $ currency, starting balance $50
- **Homes**: 1 default, 3 moderator, unlimited admin
- **Teleport delay**: 3 seconds
- **Sleep threshold**: 60%
- **World border**: 10k blocks (set in-game)

## Plugin Config Highlights

### EssentialsX
- Economy: `$` currency, starting balance $50, max $10T
- Homes: 1 default, 3 moderator, unlimited admin
- Chat format: `{PREFIX}&r {USERNAME} >> {MESSAGE}`
- Starter kit: wooden tools (enchanted) + bread + compass + torches + guide book
- Teleport delay: 3 seconds
- Respawn at home/bed: enabled

### LuckPerms
- Storage: `yaml-combined`
- Groups:
  * `default` (weight 10, prefix `[Player]`, essentials basics, signs, GriefPrevention)
  * `moderator` (weight 50, prefix `[Mod]`, inherits default, WorldGuard, WorldEdit, CoreProtect, moderation)
  * `admin` (weight 100, prefix `[Admin]`, inherits moderator, wildcard permissions)

### Harbor
- Night skip: 60% players needed
- AFK detection: 15 min timeout
- Excluded worlds: nether, end
- Action bar and boss bar enabled

### WorldGuard (after server start)
- Spawn region: 20 blocks, no PvP/explosions/fire
- Protection zone: next 20 blocks, no build, PvP allowed

### GriefPrevention
- Golden shovel land claims
- Trust system

## Conventions

- This is a **config-only repo** — no Java source, no build tools, no tests
- Plugin JARs are committed directly (not managed by Maven/Gradle)
- Plugin config files are YAML (read/write as YAML)
- `.gitignore` excludes all runtime data but tracks plugin configs

## Common Gotchas

- `paper-1.21.1.jar` is the Paper JAR, not `server.jar` (gitignored)
- `run.bat` uses a hardcoded JetBrains JRE path — won't work on other machines without editing
- `online-mode=false` means AuthMe must be installed before going live
- GrimAnticheat requires ProtocolLib — both must be installed together
- Cloudflare Tunnel must be running for external access
- Java 24 is required
