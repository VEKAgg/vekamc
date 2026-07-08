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
- Homes: Recruit 1, Player 2, Active 4, Supporter 6, Moderator+ unlimited
- Kits in `plugins/Essentials/kits.yml`: `starter`, `tools`, `dtools`, `notch`, `color`, `firework`
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

### Harbor (Sleep Mechanics)
- Night skip: 60% players needed (updated from 50%)
- AFK detection: 15 min timeout
- Excluded worlds: nether, end
- Action bar countdown enabled
- Boss bar display enabled
- Clear rain/thunder on skip enabled
- Phantom statistic reset enabled
- Messages show sleeping count and more-needed count

### WorldGuard
- **Status**: Installed, no regions created yet
- **Planned regions**:
  * **Spawn Region** (first 20 blocks): Build allowed, PvP/PvE/TNT/Fire/Creeper/Ghast/Enderman/Wither disabled
  * **Outer Protection Zone** (next 20 blocks): No build/break, PvP/PvE allowed, TNT/Fire/Lava/Water/Creeper/Ghast/Enderman disabled
  * **Future**: Towns, shopping district, community builds, event arenas, staff-only zones, nether/end hubs, resource world, minigame areas
- **Messages**: "Welcome to Spawn" / "You are now leaving Spawn" / "You have entered the Protected Zone" / "You have left the Protected Zone"

### CoreProtect
- **Status**: Installed, working
- **Purpose**: Block logging, rollback, grief investigation, chest history
- **Future**: Database backups, automatic purge, staff-only permissions

### Spark (Profiler)
- **Status**: Installed, running correctly
- **Note**: Using Java sampler (not async-profiler) because Windows doesn't support async-profiler
- **Commands**: `/spark tps`, `/spark profiler`, `/spark healthreport`, `/spark heap`
- **Usage**: TPS analysis, memory analysis, CPU bottlenecks, chunk generation profiling, plugin profiling, entity profiling, tick analysis

### Plan (Player Analytics)
- **Status**: Installed, running
- **Not configured**: HTTPS certificate not set, GeoLite database not configured
- **Future**: TPS monitoring, player retention, session lengths, most active players, chunk statistics, performance analysis, plugin performance, network statistics

### SkinsRestorer
- **Status**: Installed, working
- **Note**: Permission consent warning still pending configuration
- **Future**: Allow players to change skins, support skin URLs, premium account skin lookup, skin reset command

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

### Server Design
- **Philosophy**: Vanilla+ survival, no unnecessary complexity, no pay-to-win
- **Spawn Design**: Welcoming, informative, useful without being a large hub
- **World Structure**:
  * Overworld: Spawn → Wilderness → Future shopping/community/events
  * Nether: Spawn Portal → Nether Hub → Ice Boat Roads
  * The End: Spawn Island → Future Enderman Farm/Dragon Arena
- **Homes per Rank**:
  * Recruit: 1 home
  * Player: 2 homes
  * Active: 4 homes
  * Supporter: 6 homes
  * Moderator+: Unlimited
- **Warps**: /spawn (public), future: /market, /events, /nether, /end, /community, /staff
- **Backups**: Automatic save every 30 min, daily compressed, weekly/monthly archives
- **Restarts**: Future automatic daily restart during lowest activity with countdown
- **Chat Format**: `[Rank] PlayerName >> Message`
- **TAB List**: Future header (VEKA.GG, Vanilla+ Survival, Discord, Website) / footer (Online Players, TPS, Ping, World, Website)
- **Scoreboard**: Future sidebar (Rank, Balance, Playtime, Online Players, Coordinates, Season, TPS, Homes Used)

### Server Values
- Community, Respect, Creativity, Fairness, Learning
- Helping New Players, Long-Term Worlds, Performance
- No Pay-To-Win, No Gambling, No Lootboxes, No Fake Voting Rewards

### Server Identity
- **Lore**: VEKA.GG is a long-term world, not just another survival server
- **Culture**: Friendly players, helpful veterans, beautiful builds, respectful chat, honest moderation
- **Philosophy**: Vanilla Minecraft experience, polished but not modified
- **Economy**: Player-driven, no virtual money, no purchasable gameplay advantages
- **Progression**: Wood → Stone → Iron → Diamond → Nether → Netherite → Large Projects → Community Builds
- **Building**: Villages, castles, factories, farms, ports, roads, bridges, airships, railways, cities
- **Transportation**: Early game walking/boats → Mid game nether highways/ice roads → Late game player-built systems
- **Events**: Build competitions, treasure hunts, PvP tournaments, boat racing, elytra racing, seasonal festivals
- **Hall of Fame**: First player, largest base, best builder, community choice, event winners
- **Promise**: No pay-to-win, no abusive staff, no hidden changes, no unnecessary resets, transparent moderation

### Development Roadmap
- **Phase 1 (Current)**: Foundation — Install/configure all core plugins, configure LuckPerms, EssentialsX, WorldGuard, MOTD, starter kit, spawn, sleep system, chat formatting, guide book, backups
- **Phase 2**: Community — Open to friends, stress test, collect feedback, improve onboarding, balance spawn, tune gameplay
- **Phase 3**: Public Release — Website, Discord, server trailer, rules finalized, ranks finalized, public announcement
- **Phase 4**: Expansion — Shopping district, community town, player markets, public farms, nether hub, road network, railway, server events
- **Phase 5**: Advanced Features — Separate modded server, shared auth, cross-server chat, proxy (Velocity), crossplay, resource pack

## Server Administration

### Startup Procedure
1. Verify Windows started correctly
2. Verify internet connection
3. Verify Cloudflare Tunnel running
4. Start Minecraft server (`run.bat`)
5. Monitor startup console
6. Verify plugins loaded (check for errors)
7. Verify TPS (should be 20.0)
8. Verify memory usage
9. Verify website still online
10. Open server to players

### Shutdown Procedure
**Never close the console window.** Always use `stop` command.
Wait for: Saving Worlds → Saving Chunks → Closing Plugins → Server Stopped.
Then close the terminal.

### Backup Policy
- **Naming**: `YYYY-MM-DD_HH-MM.zip`
- **Contents**: world/, world_nether/, world_the_end/, plugins/, server.properties, LuckPerms data
- **Frequency**: Every 30 minutes (automatic), daily compressed, weekly/monthly archives
- **Storage**: Separate storage device, future cloud backup

### Disaster Recovery (World Corruption)
1. Stop server immediately
2. Copy latest backup
3. Restore worlds
4. Restore plugin data
5. Start server
6. Run `/spark healthreport`
7. Verify player data intact
8. Open server to players

### Weekly Health Checklist
- Check disk space
- Check Java updates
- Check plugin updates
- Check Paper updates
- Review console errors
- Verify backups working
- Review CoreProtect database
- Review Plan analytics
- Review Spark reports

### Monthly Maintenance
- Review inactive players
- Review permissions
- Review region flags
- Archive old logs
- Update documentation
- Optimize plugin configurations
- Clean temporary files
- Review performance metrics

### Plugin Update Policy
1. Create backup
2. Read changelog
3. Verify compatibility
4. Update test server first
5. Stress test
6. Deploy to production
7. Monitor console
8. Keep previous version as fallback

### Plugin Removal Policy
Remove a plugin if it: becomes abandoned, causes TPS loss, duplicates another plugin, introduces security issues, requires excessive maintenance, or negatively affects gameplay.

### Plugin Load Order
Paper → Vault → LuckPerms → EssentialsX → Essentials Modules → WorldEdit → WorldGuard → CoreProtect → Spark → Plan → Sleeper → SkinsRestorer

### Plugin Responsibilities
| Plugin | Responsibility |
|--------|---------------|
| EssentialsX | Player management, spawn, homes, warps, teleportation, kits, commands |
| WorldGuard | World protection, regions, flags, greeting messages, area permissions |
| WorldEdit | Building, selections, clipboard, schematics |
| CoreProtect | Logging, rollback, lookup, inspection |
| Spark | Performance profiling, memory analysis, TPS analysis |
| Plan | Analytics, playtime, statistics |
| LuckPerms | Ranks, permissions, groups, inheritance |
| Vault | Permission/economy/chat bridge |
| Sleeper | Night skipping |
| SkinsRestorer | Player skins |

### Error Handling Policy
- **Critical**: Server crash, world corruption, database failure → Interrupt gameplay
- **High**: Plugin failure, memory leak, TPS below 15 → Interrupt gameplay
- **Medium**: Configuration error, permission error → Log and fix
- **Minor**: Warnings, deprecated APIs, console noise → Log only

### Configuration Management
Every config change should include: version, date modified, reason, administrator, backup created.

### Current Development Status
- Infrastructure: 100%
- Core Plugins: 100%
- Protection: 100%
- Performance: 100%
- Permission Planning: 80%
- Spawn Planning: 80%
- Chat Planning: 80%
- Guide Book Planning: 80%
- Player Experience: 60%
- Public Alpha Readiness: 60%

### Next Milestones
1. Configure LuckPerms ✓
2. Configure EssentialsX ✓
3. Create Spawn
4. Configure WorldGuard Regions
5. Configure Starter Kit ✓
6. Configure Guide Book
7. Configure MOTD ✓
8. Configure Chat ✓
9. Private Testing
10. Public Alpha Launch

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
