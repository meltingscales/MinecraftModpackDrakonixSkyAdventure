- Standalone mod `drakonix-sky-adventure-mod`: `CertusTraderBlock` (emeralds for
  Certus Quartz Crystal) is built. `DungeonPortalBlock` is built but not yet
  wired into an actual spawn structure template, and its structure-tag config
  still points at the vanilla placeholder (`minecraft:mineshaft`), not When
  Dungeons Arise's real tag. See that repo's README.

- No general sell-loot-for-currency shop yet, and no purchase path for
  automation gear (Mekanism/Create) or battle artifacts (Relics) — only Certus
  Quartz is buyable so far. Core gameplay loop (README) wants all of loot →
  sell → reinvest; right now only the "reinvest in Certus Quartz" leg exists.

- Mining-focused ore-gen/exploration mods not picked yet — deferred past the
  initial skeleton. Tech pack's veinminer/mouse-tweaks QoL mods are candidates
  to port over too if mining feel needs it.

- Aether-themed skyblock idea (void overworld, Aether as main explorable world)
  raised and explicitly deferred — core mechanics (AE2, teleport/home, dungeons,
  loot) come first. Revisit once the skeleton pack is actually played.

- No real server deployed yet — README "Server URL" is a placeholder. Port
  25568/25568 (game/query) reserved, not yet live.

- `pack/options.txt` / other `config/` defaults (beyond `config/iris.properties`
  pinning Complementary Reimagined) not set up yet — copy the tech pack's
  pattern (keybinds, xaero defaults) once there's an opinion on what to pin.
