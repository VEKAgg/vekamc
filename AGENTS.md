# AGENTS.md

## What This Repo Is

Paper Minecraft server **configuration repository** — no application code, no build system. Contains config files, plugin JARs, and plugin configs for a **multi-gamemode Minecraft network** (Survival, Skyblock, Factions, Prison, Bedwars, Skywars, Plots, Towny).

- **Paper version**: 1.21.1 (`paper-1.21.11.jar`)
- **Branch**: `main` (active server config). `Old` has legacy config.
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

## Installed Plugins (31)

### Security & Core
| Plugin | JAR | Purpose |
|--------|-----|---------|
| AuthMe | `AuthMe-5.6.0-FORK-Universal.jar` | Login/register for cracked mode |
| Grim Anticheat | `grimac-bukkit-2.3.74-7807650.jar` | Anti-cheat (29 detections) |
| ProtocolLib | `ProtocolLib.jar` | Packet-level checks (required by Grim) |
| EssentialsX | `EssentialsX-2.21.0.jar` | Commands, economy, homes, kits |
| EssentialsXChat | `EssentialsXChat-2.21.0.jar` | Chat formatting with rank prefixes |
| EssentialsXSpawn | `EssentialsXSpawn-2.21.0.jar` | Global spawn management |
| EssentialsXProtect | `EssentialsXProtect-2.21.0.jar` | Block protection and physics control |
| EssentialsXAntiBuild | `EssentialsXAntiBuild-2.21.0.jar` | Build restrictions for new players |
| LuckPerms | `LuckPerms-Bukkit-5.5.59.jar` | Permission management |
| Vault | `Vault-1.7.3-b131.jar` | Economy/permissions API bridge |
| PlaceholderAPI | `PlaceholderAPI-2.11.6.jar` | Placeholder system |
| ViaVersion | `ViaVersion-5.1.1.jar` | Multi-version support (1.8-1.21) |
| Multiverse-Core | `multiverse-core-5.7.2.jar` | Multi-world management |

### Protection
| Plugin | JAR | Purpose |
|--------|-----|---------|
| WorldGuard | `worldguard-bukkit-7.0.12-dist.jar` | Region protection |
| WorldEdit | `worldedit-bukkit-7.3.10-beta-01.jar` | Building tool |
| CoreProtect | `CoreProtect-CE-23.2.jar` | Block logging and rollback |
| GriefPrevention | `GriefPrevention.jar` | Golden shovel land claims |

### Gamemode Plugins
| Plugin | JAR | Purpose |
|--------|-----|---------|
| BentoBox | `BentoBox.jar` | Skyblock platform |
| BSkyBlock | `BSkyBlock.jar` | Skyblock gamemode addon |
| PvPIndex Factions | `PvPIndex-Factions.jar` | Factions (claims, power, raids) |
| ScreamingBedWars | `ScreamingBedWars.jar` | Bedwars minigame |
| Towny | `Towny.jar` | Towns, nations, diplomacy |

### Economy & Shop
| Plugin | JAR | Purpose |
|--------|-----|---------|
| ChestShop | `ChestShop.jar` | Player-run chest shops |
| Harbor | `Harbor.jar` | Sleep/night-skip mechanics |

### Admin & Utility
| Plugin | JAR | Purpose |
|--------|-----|---------|
| AdvancedBan | `AdvancedBan.jar` | Ban/mute/tempban with history |
| TAB | `TAB.jar` | Custom tab list |
| SkinRestorer | `SkinRestorer.jar` | Custom skins for cracked players |
| ClearLag | `ClearLag.jar` | Auto-remove excess items |
| spark | `spark.jar` | Performance profiler |

### Disabled / Not Installed
| Plugin | Status | Reason |
|--------|--------|--------|
| EssentialsXGeoIP | Disabled | Requires MaxMind license |
| EssentialsXDiscord | Disabled | No Discord bot token |
| EssentialsXDiscordLink | Disabled | Depends on EssentialsXDiscord |

## Running the Server

```bash
# Local (Windows) — requires Java in PATH or edit run.bat
run.bat

# The script uses JetBrains bundled JRE by default.
# For Pterodactyl, use the start command with Aikar flags (see run.bat).
```

The `server.jar` is gitignored. The actual JAR is `paper-1.21.11.jar`. Do not create symlinks — just place the Paper JAR in root.

## Gitignored (Not Tracked)

- `world/`, `world_nether/`, `world_the_end/` — all world data
- `server.jar` — the actual server JAR
- `logs/`, `cache/`, `libraries/`, `versions/`
- Plugin runtime data: `userdata/`, `*.db`, `usermap.bin`, `uuids.bin`

## Server State

- **Branch**: `main` (active). `Old` has legacy config.
- **Difficulty**: Normal
- **Gamemode**: Survival (default)
- **Max players**: 69
- **Online mode**: false (cracked — requires AuthMe for security)
- **View distance**: 16 chunks
- **Simulation distance**: 5 chunks
- **Spawn protection**: 0 (Essentials handles it)
- **Allow flight**: true
- **PVP**: true
- **No ops** configured (`ops.json` empty)
- **No whitelist** enforced
- **Pause when empty**: 60 seconds
- **EULA**: accepted

## Plugin Config Highlights

### EssentialsX (`plugins/Essentials/config.yml`)
- Economy: `$` currency, starting balance 0, max $10T
- Homes: 3 default, 5 VIP, 10 staff
- Kits in `plugins/Essentials/kits.yml`: `tools`, `dtools`, `notch`, `color`, `firework`
- Chat format: `{PREFIX}&r{DISPLAYNAME}&7:&f {MESSAGE}`
- MOTD in `plugins/Essentials/motd.txt`
- Item sell prices in `plugins/Essentials/worth.yml`
- Random teleport configured in `plugins/Essentials/tpr.yml`
- Teleport delay: 3 seconds
- Spawn protection handled by WorldGuard

### LuckPerms
- Server name: "smp"
- Storage: `yaml-combined` (permits offline file-based edits inside `yaml-storage/groups.yml`)
- Groups configured:
  * `default` (weight 10, prefix `&7[Player] `, basics like `/spawn`, `/tpa`, `/home`, trade signs)
  * `moderator` (weight 50, prefix `&b[Mod] `, inherits `default`, kicks, bans, mutes, fly, tp)
  * `admin` (weight 100, prefix `&c[Admin] `, inherits `moderator`, wildcard `*` operator permissions)

### Harbor
- Night skip: 50% players needed
- AFK detection: 15 min timeout
- Excluded worlds: nether, end

### Multiverse-Core
- Manages separate worlds per gamemode
- Worlds: survival, skyblock, factions, prison, bedwars, skywars, plots, towny
- `/mv tp <world>` to switch gamemodes

### EssentialsXAntiBuild
- Will be used to prevent recruits/guests from editing protected locations
- Works with LuckPerms permission nodes

### EssentialsXGeoIP
- Disabled (requires MaxMind license)
- May remain disabled unless country lookup is needed

### EssentialsXDiscord / DiscordLink
- Disabled (no bot token)
- Will be configured when Discord integration becomes a priority

## Conventions

- This is a **config-only repo** — no Java source, no build tools, no tests
- Plugin JARs are committed directly (not managed by Maven/Gradle)
- Plugin config files are YAML (read/write as YAML)
- Paper config versions: `paper-global.yml` v29, `paper-world-defaults.yml` v31
- `.gitignore` excludes all runtime data but tracks plugin configs

## Common Gotchas

- `paper-1.21.11.jar` is the Paper JAR, not `server.jar` (gitignored)
- `run.bat` uses a hardcoded JetBrains JRE path — won't work on other machines without editing
- `EssentialsX` uses both `config.yml` and separate files (`kits.yml`, `motd.txt`, `worth.yml`, `tpr.yml`, `custom_items.yml`)
- `spigot.yml` and `bukkit.yml` are auto-generated by Paper on first run — edits should be minimal
- Paper config files (`config/paper-*.yml`) use versioned schemas — don't manually add unknown keys
- `online-mode=false` means AuthMe must be installed before going live or anyone can steal usernames
- GrimAnticheat requires ProtocolLib — both must be installed together
- X-Prison and PlotSquared need manual download from SpigotMC (not on Modrinth)
- Cloudflare Tunnel is used for external access — ensure tunnel is running before players can join
- Java 24 is required — server won't start on older Java versions
