# AGENTS.md — VEKAMC

> PaperMC 1.21.11 multi-gamemode SMP network config repo. No application code — only plugin JARs, configs, and server properties.

---

## Session Protocol (read first — every session)

1. **Read** this AGENTS.md fully before touching any code.
2. **Do** the current task.
3. **Update** this AGENTS.md (Issue Tracker, decisions, gotchas) whenever context changes.
4. **Verify** quality gates → **commit + push** — one clean commit per task.
5. **Log** one line to the Bedrock vault journal: `C:\Users\Shafaat\Documents\Bedrock\60 Journal\<YYYY-MM-DD>.md` (create the file if missing).

---

## Agent Workflow (MANDATORY)

Every coding session must follow this loop for each task:

1. **Write the code** for the current task / step.
2. **Audit** — validate configs, check for syntax errors, ensure consistency.
3. **Commit** — stage changes and commit with a descriptive message.
4. **Push** — push the commit.
5. **Move on** — only start the next task after the current one is validated, committed, and pushed.

**Never batch multiple tasks into one commit. Each completed task = one clean commit.**

---

## Quality Gates (run before every push)

```bash
# Validate YAML configs
python -c "import yaml; yaml.safe_load(open('spigot.yml'))"
python -c "import yaml; yaml.safe_load(open('paper-global.yml'))"

# Check for gitignored files that shouldn't be
git status --ignored

# Verify plugin JARs are present
ls plugins/*.jar
```

| Gate | Command | Must pass |
|------|---------|-----------|
| YAML syntax | `python -c "import yaml; ..."` | Yes |
| Git status | `git status --ignored` | Yes |
| Plugin JARs | `ls plugins/*.jar` | Yes |

---

## Git History Audit (First Session Only)

On your first session with this project, run through the git log and populate the Issue Tracker and Locked Design Decisions sections above with historical context:

```bash
git log --oneline -50          # recent commits
git log --all --oneline -100   # all branches
git log --grep="fix" --oneline # bug fix history
git log --grep="breaking" --oneline # breaking changes
```

- Add past bugs/fixes to the Issue Tracker (status: Done)
- Add confirmed decisions to Locked Design Decisions (with dates)
- Note any patterns of recurring issues
- This gives future agents full context without re-auditing

---

## Issue Tracker

Track all known issues, bugs, and technical debt here. Use consistent IDs.

### Format

| ID | Severity | Status | File | Description |
|----|----------|--------|------|-------------|
| FIX-001 | CRITICAL | ✅ Done | `spigot.yml:42` | One-line problem description |
| FIX-002 | HIGH | ⬜ Open | `plugins/EssentialsX/config.yml:15` | One-line problem description |
| GAP-001 | MEDIUM | ⬜ Deferred | — | Missing feature or capability |

### Severity Levels

- **CRITICAL**: Server won't start, data loss, security vulnerability
- **HIGH**: Broken gameplay, incorrect config values
- **MEDIUM**: Missing config optimization, deprecated settings
- **LOW**: Documentation, naming, organization

### Status Values

- `✅ Done` — Fixed in a previous session
### Open Issues & Architectural Gaps

| ID | Severity | Status | File | Description |
|----|----------|--------|------|-------------|
| GAP-001 | CRITICAL | ⬜ Open | `plugins/AuthMe/config.yml` | Configure AuthMe (BCrypt hash, rate limits) for cracked security before launch |
| GAP-002 | HIGH | ⬜ Open | `server.properties` | Set world border (20,000) and spawn WorldGuard flags (deny pvp/mob-spawn) |
| GAP-003 | MEDIUM | ⬜ Open | `plugins/DiscordSRV/` | Set up DiscordSRV / webhook bridge with `DiscordBot` for real-time chat sync |
| GAP-004 | LOW | ⬜ Open | — | Expose server ping and online player count for `veka.gg` sister sites panel |

### Completed Fixes

| ID | Status | File |
|----|--------|------|
| — | — | Initial configuration baseline established |

---

## Locked Design Decisions

Decisions confirmed by the owner. **Do not deviate without explicit instruction.**

| Decision | Chosen | Date |
|----------|--------|------|
| Server JAR | `paper-1.21.11.jar` (Build 132) — NOT `server.jar` | 2026-01-15 |
| Branch | `main` — active server config | 2026-01-15 |
| Domain | `mc.veka.gg` | 2026-01-15 |
| Network vision | Velocity proxy → Hub, Vanilla+ SMP, Vanilla+ Skyblock, Cobblemon | 2026-01-15 |
| Plugin management | Paper plugins only, no Fabric/Forge | 2026-01-15 |

---

## File Modification Restriction

- **Always edit**: `AGENTS.md`, `README.md` — these are agent-maintained files.
- **Edit with caution**: YAML config files — only when executing a specific task or fix.
- **Never edit**: `paper-1.21.11.jar`, plugin JARs, `server.properties` (server identity) unless explicitly instructed.
- **Document all changes**: Note exact files modified and what was changed in commit messages.
- **Backup first**: Always back up a config before making breaking changes.

---

## Pre-Submit Checklist

Before finishing any task, verify:

- [ ] YAML syntax valid (no tabs where spaces expected)
- [ ] Config changes tested on dev server if possible
- [ ] Plugin compatibility verified (no version conflicts)
- [ ] `.gitignore` not excluding needed configs
- [ ] AGENTS.md is still accurate
- [ ] Commit message describes what changed and why

---

## What This Is

PaperMC 1.21.11 **multi-gamemode SMP network config repo**. No application code — only plugin JARs, configs, and server properties. The long-term vision is a **Velocity proxy network** with Hub, Vanilla+ SMP, Vanilla+ Skyblock, and eventually Cobblemon.

- **Branch**: `main` — active server config
- **Remote**: `https://github.com/VEKAgg/vekamc.git`
- **Domain**: `mc.veka.gg`
- **Server JAR**: `paper-1.21.11.jar` (Build 132) — NOT `server.jar` (gitignored)
- **Java**: OpenJDK 21.0.7 via PyCharm JBR at `C:\Program Files\JetBrains\PyCharm Community Edition 2025.2.0.1\jbr\bin\java.exe`
- **Launch**: `run.bat` (Windows, 4G heap, Aikar G1GC flags)

---

## Server Settings (server.properties)

| Setting | Value | Why |
|---------|-------|-----|
| `online-mode` | `false` | Cracked mode — AuthMe handles auth |
| `enforce-secure-profile` | `false` | Required for cracked mode |
| `difficulty` | `normal` | |
| `max-players` | `69` | |
| `view-distance` | `8` | Reduced from 16 — 16 caused TPS collapse |
| `simulation-distance` | `4` | Reduced from 5 for performance |
| `spawn-protection` | `0` | WorldGuard handles spawn region |
| `allow-flight` | `true` | Prevents false anticheat kicks |
| `sync-chunk-writes` | `false` | Better I/O performance |
| `pvp` | `true` | |
| `motd` | 2-line colored text | Server list max = 2 lines; ASCII art is in `motd.txt` |

---

## Installed Plugins (17)

**Security**: AuthMe 5.7.0 (login/auth), grimac 2.3.71 (anticheat), ProtocolLib (Grim dep)
**Core**: EssentialsX 2.21.0, EssentialsXChat 2.21.0, LuckPerms 5.5.59, Vault 1.7.3, PlaceholderAPI 2.11.6, ViaVersion 5.1.1
**Protection**: WorldGuard 7.0.12, WorldEdit 7.3.9, CoreProtect CE 23.2, GriefPrevention
**QoL**: Harbor (sleep skip), spark (profiler), Chunky 1.4.40 (world pre-gen), TAB 6.1.2 (tab list)

---

## Key Configs

### EssentialsX (`plugins/Essentials/config.yml`)
- Chat format: `{PREFIX}&r{DISPLAYNAME}&7:&f {MESSAGE}`
- Homes: 1 default, 3 mod, unlimited admin
- Economy: $50 starting balance, $ currency
- Starter kit: `starter` (wooden tools + bread + compass + torches + guide book)
- Teleport delay: 3 seconds
- Tab list shows LuckPerms prefixes (enabled)
- Respawn at home/bed: enabled

### EssentialsX MOTD (`plugins/Essentials/motd.txt`)
- Full ASCII art "VEKA" logo rendered in-game on join
- Shows live player count, server time, feature list
- This is the *in-game* welcome — NOT the server list MOTD

### LuckPerms (`plugins/LuckPerms/yaml-storage/groups.yml`)
- Storage: `yaml-combined` (flat file, offline-editable)
- `default` (weight 10, `&7[Player]`): essentials basics, /tpa, /home, economy, signs, GriefPrevention claims
- `moderator` (weight 50, `&b[Mod]`): inherits default + kick/ban/mute, WorldGuard, WorldEdit, CoreProtect
- `admin` (weight 100, `&c[Admin]`): inherits moderator + wildcard `*`

### Harbor (`plugins/Harbor/config.yml`)
- 50% sleep threshold, 15 min AFK timeout, nether/end excluded

### AuthMe (`plugins/AuthMe/config.yml`) ⚠️ NOT YET CONFIGURED
- Plugin is installed but config dir does not exist
- Default settings are insecure on a cracked server
- See **Pending Tasks** below for required configuration

---

## Gotchas

- `paper-1.21.11.jar` is the server JAR (Build 132), NOT `server.jar` (gitignored)
- `run.bat` hardcodes PyCharm 2025.2.0.1 JBR path — won't work on other machines without editing
- `online-mode=false` without a configured AuthMe = anyone can impersonate any username including staff
- Grim requires ProtocolLib — always install both or neither
- Plugin config dirs (WorldGuard, CoreProtect, GriefPrevention, AuthMe) don't exist until first server start
- Cloudflare Tunnel must be running for external access via `mc.veka.gg`
- LuckPerms `yaml-storage/groups.yml` uses `yaml-combined` format — do NOT manually add/remove keys carelessly
- Server list MOTD supports **max 2 lines** — do not put multi-line text in `server.properties motd=`
- ASCII art / welcome message belongs in `plugins/Essentials/motd.txt` (no line limit)
- `view-distance=8` is intentional — was reduced from 16 to prevent TPS collapse

---

## In-Game Setup (After First Server Start)

1. Set world border: `/worldborder center 0 0` then `/worldborder set 20000`
2. Create WorldGuard spawn region: select 20-block radius with `//wand`, then `/rg define spawn`
3. Set spawn region flags: `/rg flag spawn pvp deny` + `/rg flag spawn mob-spawning deny`
4. Configure AuthMe (see Pending Tasks — CRITICAL for cracked server security)
5. Grant yourself admin: `lp user <yourname> parent set admin`
6. Build physical spawn with NPC portals and chest shops

---

## Pending Tasks (Ordered by Priority)

### 🔴 CRITICAL — Do Before Opening to Public

#### 1. Configure AuthMe
AuthMe is installed but has no config. Default settings are a security hole.
After first server start, edit `plugins/AuthMe/config.yml`:
```yaml
security:
  passwordHash: BCRYPT
  minPasswordLength: 6
  maxLoginTries: 5
  tempBanLength: 60
settings:
  restrictions:
    allowedNicknameCharacters: '[a-zA-Z0-9_]*'
    maxNickLength: 16
  sessions:
    sessionExpireAfterSeconds: 900
```
Also run in-game: `/authme debug` to verify hash algorithm is BCRYPT, not MD5.

#### 2. Fix World Border
Run in-game after first start:
```
/worldborder center 0 0
/worldborder set 20000
```

#### 3. Create Spawn WorldGuard Region
```
# Use WorldEdit wand to select spawn area
/rg define spawn
/rg flag spawn pvp deny
/rg flag spawn mob-spawning deny
/rg flag spawn entry allow
```

#### 4. Pre-generate World with Chunky
Install `Chunky` plugin, then run in-game to eliminate chunk gen lag permanently:
```
/chunky radius 5000
/chunky start
```
This will take ~30 minutes to run in the background. Do before opening the server.

---

### 🟡 IMPORTANT — Do in Phase 1 & 2

#### 5. Install TAB Plugin
Download: https://www.spigotmc.org/resources/tab.57806/
Configure a custom tab list showing: player rank prefix, ping, server TPS.
Requires PlaceholderAPI (already installed).

#### 6. Install DecentHolograms
Download: https://www.spigotmc.org/resources/decentholograms.96927/
Create floating holograms at spawn showing: server rules, feature highlights, top players.

#### 7. Install Citizens (NPC Server Portals)
Download: https://www.spigotmc.org/resources/citizens.13811/
Create clickable NPC characters at spawn that warp players to different gamemodes.

#### 8. Install AuraSkills (Skills System)
Download: https://modrinth.com/plugin/auraskills
Full skill tree (Mining, Combat, Farming, Fishing, etc.) with leveling UI.
Drop-in, works alongside mcMMO if added later.

#### 9. Install QuickShop-Hikari (Chest Shops)
Download: https://modrinth.com/plugin/quickshop-hikari
Floating 3D item display above chest shops. Players create shops by placing a chest and sign.

#### 10. Install AuctionHouse (Player Market)
Download: https://www.spigotmc.org/resources/auctionhouse.61866/
GUI marketplace. Players list items for auction or buyout. Run `/ah` to open.
Requires Vault (already installed) and economy plugin (EssentialsX Economy is already active).

#### 11. Install VeinMiner
Download: https://modrinth.com/plugin/veinminer
Sneak + mine ore → entire vein mined at once. Toggle per-player with `/veinminer toggle`.
Major QoL feature players expect from popular servers.

#### 12. Install GSit (Sitting)
Download: https://modrinth.com/plugin/gsit
Players can sit on stairs, slabs, and ground with right-click. Small feature, huge community vibe.
No configuration needed — works out of the box.

#### 13. Install Dynmap (Live Web Map)
Download: https://www.spigotmc.org/resources/dynmap.274/
Renders a live Google Maps-style view of your world at `map.veka.gg` (or similar subdomain).
Heavy on first render — run `/dynmap fullrender world` after Chunky completes.

---

## Plugins to Add in Future (Not Yet Installed)

### Phase 1 Plugins (Foundation)
| Plugin | Purpose | Download |
|--------|---------|---------|
| **Chunky** | World pre-generation — eliminates chunk gen lag | hangar.papermc.io |
| **TAB** | Custom animated tab list + scoreboard | spigotmc.org #57806 |
| **DecentHolograms** | Floating text, leaderboard displays | spigotmc.org #96927 |
| **Citizens** | Clickable NPCs for server portals | spigotmc.org #13811 |

### Phase 2 Plugins (SMP Enhancement)
| Plugin | Purpose | Download |
|--------|---------|---------|
| **AuraSkills** | RPG skills system (Mining, Combat, Farming etc.) | modrinth.com |
| **QuickShop-Hikari** | Player chest shops with 3D item preview | modrinth.com |
| **AuctionHouse** | Global GUI player marketplace | spigotmc.org #61866 |
| **VeinMiner** | Mine full ore veins while sneaking | modrinth.com |
| **TreeAssist** | Chop entire trees by breaking bottom log | spigotmc.org |
| **GSit** | Sit on stairs/slabs/ground | modrinth.com |
| **Brewery** | Craft custom drinks, immersive bar culture | spigotmc.org |
| **Dynmap** | Live web map of the world | spigotmc.org #274 |
| **Jobs Reborn** | Earn money passively for mining/farming/etc. | spigotmc.org |

### Phase 2 — World Generation (Datapacks, not plugins)
| Datapack | Purpose | Note |
|----------|---------|------|
| **Terralith** | Beautiful biome overhaul (mountains, canyons) | Requires new world — cannot retrofit |
| **Terratonic** | Ocean depth + Tectonic-compatible terrain | Use instead of Tectonic when pairing with Terralith |
> ⚠️ World gen datapacks require a **fresh world**. Place `.zip` files in `world/datapacks/` before first start.

### Phase 3 Plugins (Skyblock Server — Separate Paper Instance)
| Plugin | Purpose | Download |
|--------|---------|---------|
| **IridiumSkyblock** | Core island engine (free, modern, 1.21 compatible) | modrinth.com |
| **SlimeFun4** | 450+ custom machines/items using resource pack — zero client mods | github.com/Slimefun |
| **ShopGUI+** | Admin shops for island resources | spigotmc.org |
| **WildChests** | Autosell chests and crop hoppers | spigotmc.org |

### Phase 4 Plugins (Economy & Skills Overhaul)
| Plugin | Purpose | Download |
|--------|---------|---------|
| **mcMMO** | Classic skills RPG system, 1.21 compatible | mcmmo.org |
| **Jobs Reborn** | Task-based income system | spigotmc.org |

### Phase 5 Plugins (Cobblemon — Fabric Server, NOT Paper)
| Component | Purpose | Note |
|-----------|---------|------|
| **Velocity Proxy** | Routes players between Paper + Fabric servers | velocitypowered.com |
| **Cobblemon mod** | Pokémon in Minecraft | Fabric only — clients need modpack |
| **FabricProxy-Lite** | Connects Fabric backend to Velocity proxy | modrinth.com |
| **Modrinth Modpack** | 1-click install for players joining Cobblemon server | Create modpack on modrinth.com |

### Future Considerations (No Timeline Yet)
| Plugin | Purpose |
|--------|---------|
| **Oraxen** | Custom items/blocks via auto-downloaded resource pack (open source) |
| **ModelEngine** | 3D animated custom mobs via resource pack (needed for Cobblemon-lite alternative) |
| **MythicMobs** | Custom mob AI, skills, events — pairs with ModelEngine |
| **GeyserMC** | Bedrock Edition players join Java server (no client mods needed) |
| **DeluxeMenus** | Custom interactive GUI menus for server selector, settings |
| **Skript** | Server-side scripting for custom mini-events and game logic |

---

## Network Architecture Plan (Velocity Proxy)

When ready to expand beyond single SMP, migrate to this architecture:

```
[Players at mc.veka.gg:25565] → Velocity Proxy
                                      |
            ┌─────────────────────────┼──────────────────────┐
            ↓                         ↓                      ↓
     Paper Hub :30000         Paper SMP :30001     Paper Skyblock :30002
                                                   [Future] Fabric Cobblemon :30003
```

**Setup steps when ready:**
1. Download Velocity from velocitypowered.com
2. Configure `velocity.toml` — set `player-info-forwarding-mode = modern`
3. In each Paper backend's `config/paper-global.yml`:
   ```yaml
   proxies:
     velocity:
       enabled: true
       online-mode: true
       secret: 'copy-from-velocity-forwarding.secret'
   ```
4. Set `online-mode=false` in each backend's `server.properties` (Velocity handles auth)
5. Firewall backend ports (30000-30003) — only expose 25565 publicly

---

## Phased Rollout Plan

### Phase 1 — Stabilize & Secure (Do This Week)
**Goal:** Server is safe, performant, and ready for first real players.

- [x] Fix `server.properties` MOTD to 2-line max
- [x] Reduce `view-distance` from 16 → 8
- [x] Reduce `simulation-distance` from 5 → 4
- [x] Reduce entity tracking ranges in `spigot.yml`
- [x] Fix bukkit.yml spawn limits (animals 10→20, monsters 70→50)
- [x] **Install Chunky** (1.4.40) — pre-gen 5,000-block radius after first start
- [x] **Install TAB** (6.1.2) — custom tab list + scoreboard
- [ ] **Configure AuthMe** (CRITICAL — after first server start)
- [ ] Set world border (20,000 blocks) — in-game command
- [ ] Create spawn WorldGuard region with proper flags — in-game command
- [ ] Grant yourself admin via LuckPerms console

### Phase 2 — SMP Enhancement (Month 1)
**Goal:** Make survival feel like a premium Vanilla+ experience.

- [ ] Install AuraSkills (skills system)
- [ ] Install QuickShop-Hikari (chest shops)
- [ ] Install AuctionHouse (player market)
- [ ] Install VeinMiner, TreeAssist, GSit, Brewery (QoL)
- [ ] Install Dynmap → set up `map.veka.gg` subdomain
- [ ] Install DecentHolograms + Citizens → build spawn NPCs
- [ ] Plan/build physical spawn area
- [ ] Add Terralith + Terratonic datapacks to a **fresh** SMP world

### Phase 3 — Skyblock Launch (Month 2–3)
**Goal:** Open a second gamemode to grow the community.

- [ ] Set up Velocity proxy (separate server instance)
- [ ] Launch Hub server (Paper, port 30000)
- [ ] Launch Skyblock server (Paper, port 30002) with IridiumSkyblock + SlimeFun4
- [ ] Migrate SMP to port 30001 behind proxy
- [ ] Configure Citizens NPCs in Hub → each gamemode portal
- [ ] Set up network-wide LuckPerms (sync ranks across servers)

### Phase 4 — Economy & Skills Overhaul (Month 3–4)
**Goal:** Deep progression loop to keep players engaged long-term.

- [ ] Install mcMMO on SMP server
- [ ] Install Jobs Reborn (money sink + passive income)
- [ ] Balance economy: set item prices in `worth.yml`, job pay rates
- [ ] Create rank progression: Player → Member → Veteran → Elite (based on playtime/economy)
- [ ] Add VIP rank shop (cosmetics only, no pay-to-win)

### Phase 5 — Cobblemon Server (Month 6+)
**Goal:** Optional Pokémon gamemode for players willing to install modpack.

- [ ] Spin up Fabric 1.21 server (port 30003)
- [ ] Install Cobblemon mod + dependencies
- [ ] Install FabricProxy-Lite for Velocity compatibility
- [ ] Create Modrinth modpack for 1-click player install
- [ ] Add Cobblemon portal NPC in Hub (with clear "requires modpack" warning)
- [ ] Add GeyserMC to Velocity proxy (Bedrock players, Cobblemon excluded)

---

## Conventions

- Config-only repo — no Java source, no build tools, no tests
- Plugin JARs committed directly (not managed by Maven/Gradle)
- Plugin config files are YAML (read/write as YAML, mind indentation)
- Paper config versions: `paper-global.yml` v29, `paper-world-defaults.yml` v31
- `.gitignore` excludes all runtime data but tracks plugin configs and JARs
- LuckPerms storage: `yaml-combined` — edit `plugins/LuckPerms/yaml-storage/groups.yml`
- Server list MOTD = max 2 lines in `server.properties`
- In-game welcome = unlimited lines in `plugins/Essentials/motd.txt`
