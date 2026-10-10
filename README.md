# Drakonix Sky Adventure

Modded Minecraft server: **NeoForge 1.21.1**. Same infra pattern as the sibling
`MinecraftModpackDrakonixTechPack` repo (justfile/systemd/scripts, packwiz-tracked
pack instead of a hand-dropped zip) — see that repo's README for the full
infra walkthrough (deploy, backups, RCON, releasing). This README covers what's
specific to this pack.

## What is this

A roguelike-adventure pack: battle and mining are the core loop, random teleport +
`/sethome`/`/home` handle navigation, and **Applied Energistics 2** (Certus Quartz)
is the central progression system. Also ships (planned) a standalone NeoForge mod,
`drakonix-sky-adventure-mod` (separate repo), for the gameplay pieces no
off-the-shelf mod covers: a Certus Quartz growth-boost + purchase mechanism, and a
custom spawn with unbreakable "teleport to random dungeon" portal blocks. Not yet
built — see `TODO.md`.

## Core gameplay loop

1. **Explore & fight** — venture into a **When Dungeons Arise** structure, fight
   through it.
2. **Loot** — per-player **Lootr** containers (no race-to-loot), plus **Relics**
   drops and raw materials.
3. **Sell** — bring loot back and sell it for currency.
4. **Reinvest** — spend on AE2 progression (Certus Quartz), automation gear
   (Mekanism/Create), and battle artifacts (Relics), to go deeper and fight
   tougher dungeons. Repeat from 1.

**Current state**: only the Certus Quartz leg of step 4 is actually implemented —
`CertusTraderBlock` in the standalone mod sells Certus Quartz Crystal for emeralds.
There's no general sell-loot-for-currency shop yet, and no purchase path for
automation gear or artifacts (those are still crafted/found only, not bought).
Tracked in `TODO.md`.

## Server URL

- TBD (not yet deployed)

## Quickstart

```
just packwiz-export                 # installs the real NeoForge server + mods into build/server, zips it, exports build/*.mrpack
just setup-user                     # creates the minecraft system user + /srv/minecraft
just deploy                         # rsyncs build/server -> /srv/minecraft/drakonixskyadventure, writes eula.txt=true, installs the unit
just enable                         # systemctl enable minecraftserver-drakonixskyadventure
just start                          # systemctl start minecraftserver-drakonixskyadventure
just status                         # confirm it's running
just setup-backups                  # install + enable the daily backup timer (keeps last 10)
```

`pack/server.properties` pins the port:

```
server-port=25568
query.port=25568
```

(offset from the other Drakonix/sibling servers already running on this host —
`buiz` uses 25565, `liminalindustries` uses 25566, `drakonixtechpack` uses 25567 —
so this one takes 25568.)

## Testing in singleplayer

```
just test-client
```

Serves `pack/pack.toml` on `http://localhost:8080` (via `packwiz serve`). Import
`http://localhost:8080/pack.toml` into Prism Launcher (Add Instance → Import).

## Mods

Full, current mod list lives in `pack/` (packwiz pack). To see it:

```
cd pack && packwiz list
```

Mechanics-to-mod mapping (why each roguelike-theme mod is here, and what it's
standing in for until the standalone mod exists):

| Mechanic | Mod(s) | Notes |
|---|---|---|
| Random teleport | **ExtraRTP** | cross-dimension `/rtp`, cooldown |
| `/sethome`, `/home` | **Easy Homes** | `/sethome [name]`, `/home [name]`, `/back`, `/delhome`, `/renhome`, `/homes`; `maxHomes` configurable (default 5) |
| AE2 progression | **Applied Energistics 2** + **GuideME** | central crafting/automation system; Certus Quartz growth-boost + purchase is the planned standalone mod's job |
| Battle/dungeons | **When Dungeons Arise** | adds battle-focused dungeon structures across the overworld |
| Loot/relics | **Lootr**, **Curios API**, **Relics** | Lootr = per-player loot containers (no loot-stealing races), Curios = accessory slots, Relics = equippable magic relics that use Curios slots |
| Tech/automation | **Mekanism** + **Mekanism Generators**, **Create** | general tech progression alongside AE2 |
| Storage/QoL | **Sophisticated Backpacks** (+ Core), **Waystones**, **Jade**, **JEI**, **AppleSkin**, **Xaero's Minimap/World Map** | standard QoL layer |
| Gravestones | **Corail Tombstone** | death-item recovery, fits the "battle is common" theme |
| Quests | **FTB Quests** (+ Library/Teams, CurseForge-sourced — not on Modrinth) | quest chapters authored as plain `.snbt` text files under `config/ftbquests/quests/` — no code, any agent/session can write them directly |
| Shaders | **Iris** + **Complementary Shaders - Reimagined** | pinned as the default active shaderpack via `pack/config/iris.properties`, same pattern as the tech pack |

To add/remove a mod: `packwiz modrinth add <slug>` / `packwiz curseforge add <slug>`
/ `packwiz remove <slug>` from inside `pack/`, then `just packwiz-export`.

## Writing quests

FTB Quests chapters are plain SNBT files, not compiled content — write/edit them
directly under `pack/config/ftbquests/quests/` (or point FTB Quests' in-game editor
at the same files). No mod fork or custom tooling needed for AI-assisted quest
authoring; the format is already plain text.

## Standalone mod (planned, not yet started)

`drakonix-sky-adventure-mod` — separate repo, own NeoForge 1.21.1 Gradle build,
pulled into `pack/` via `packwiz url add` once built (same pattern as the tech
pack's `waylandcraft` pull). Scope:

1. **Certus Quartz economy** — `CertusTraderBlock`: buy Certus Quartz Crystal for
   emeralds. (AE2's own Growth Accelerator block already covers "speed up
   growth," so that wasn't reimplemented here.) **Built.**
2. **Custom spawn + random-dungeon portal** — `DungeonPortalBlock`: unbreakable,
   teleports to a randomly-offset nearby dungeon structure. **Built**, but not
   yet wired into an actual spawn structure template or pointed at When Dungeons
   Arise's real structure tag — see that repo's README.

See the tech pack's README for everything else (deploy, backups, RCON, releasing,
carrying over client settings) — it's the same `justfile`/`scripts/`/`systemd/`
pattern verbatim, just pointed at `drakonixskyadventure` instead of
`drakonixtechpack`.

## layout

Same as the tech pack (`justfile`, `.github/workflows/release.yml`, `pack/`,
`scripts/`, `systemd/`) — see that repo's README for what each piece does.
`build/` is not committed; run `just packwiz-export` to regenerate.
