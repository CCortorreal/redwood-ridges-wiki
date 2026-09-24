# Redwood Ridges — technical changelog

> The plain-English version of what's new is [here](whats-new.md). This page is for anyone who
> wants the version numbers and the mechanism, not just the effect. Newest first.

## RedwoodBoard 1.9.2 — 2026-09-23, restart ~21:18 CDT (calliope main `14c8697`)

RedwoodBoard 1.9.2, 240,927 B:

- **Furnace stall fixed.** 1.9.1 tagged `FurnaceSmeltEvent` results, and vanilla's burn check
  compares the new result's components against the output slot, so the second smelt no longer
  matched and the furnace stopped. Furnace, blast-furnace and smoker output is no longer tagged.
- **Autoreel owned by RedwoodBoard.** SuperEnchants' `auto_reel` action is removed from
  `SuperEnchants/enchants/autoreel.yml` (the enchant, its lore and how you obtain it are unchanged).
  RedwoodBoard reels in 1 tick after the bite with `FishHook#retrieve`, the vanilla catch path, so
  `CAUGHT_FISH`, XP orbs, Mending, AuraSkills and tag-at-birth all fire. The 1.9.1 XP top-up is retired.
- **Restack sweep** (`stacking.sweep-seconds`, 5). A timer merges split trim-material stacks for
  players with only their own inventory open and an empty cursor. It skips players whose
  trim-material contents haven't changed since the last pass.
- **High-contrast waypoint.** Two `TextDisplay`s, one normal and one see-through, with pure white
  text and an ARGB(220,0,0,0) background. Bedrock gets the see-through one only.
- **Archive chest** (`archive-chest.*`). Right-clicking a chest or barrel named `the archive` inside
  the board cuboid opens `ArchiveMenu`. Sneaking with a block in hand still places the block.
- **Sleep quorum** (`sleep.quorum`, 2). Recomputes `players_sleeping_percentage` on join, quit and
  world change as `floor(quorum × 100 ÷ non-spectators)`, clamped to 1–100. Overworld only.

**Staged for the next restart:** QuickShop-Hikari `allow-stacks: true` (bundle shops through
`/qs size`).

## RedwoodBoard 1.9.1 — 2026-09-23, restart window ~19:15 CDT (calliope main `8174e5d`)

RedwoodBoard 1.9.1, sha256 `97c6ba29…`:

- **`/tour dryrun`.** Staff can rehearse the full Camp Tour solo, replayable, without anyone
  getting paid or a record created.
- **Tour route order + waypoint.** The tour now targets stops in the order you can actually
  walk them, reports an honest "Stops n/8" count, and points a per-player in-world gold
  waypoint marker (label + distance, ring + pillar) at your next stop.
- **One-surface archive.** `/contracts archive` and `/contracts ledger` now open the exact same
  chest, for everyone (previously staff-only for `archive`). Click any book for the full job
  card: the original posting text plus the payout record (purse, date, receipts). Staff-only
  actions (Repost fresh, Retire) live on the same card instead of a separate detail screen or
  dialog. Replaces the old "Redwood Ledger" dialog and the paged detail-inventory browser
  outright.
- **Trim-material stacking fix.** All 97 BF7 More Trim Materials items are now tagged with
  their identity the moment they exist, drop, loot roll, fish catch, pickup, craft, or smelt,
  instead of only when the game notices on its own. A fallback restacker also merges
  same-material stacks a couple of ticks after you pick one up, so a split stack doesn't sit
  split. If the tagging self-test ever fails at boot, the restacker keeps running on its own;
  either way stacks converge.
- **Autoreel fishing XP.** Rods enchanted with SuperEnchants' Autoreel now top up the normal
  1-6 fishing XP per catch when the catch gave none. Autoreel's own catch path was skipping the
  vanilla catch event entirely, which is also why AuraSkills' fishing-skill XP and
  SuperEnchants' own Angler bonus still don't apply to an Autoreel catch. That's a separate,
  pre-existing gap this fix doesn't touch.
- **Advancement counting, one rule.** Rank-relevant advancements are now counted by a single
  rule instead of the old "exclude recipes/ only" filter: not a recipe grant, has a display,
  and not on the 298-entry not-counted list. Past grants are unchanged.
- Claim-cap bars are unchanged tonight (40/80 in the live config); a retune is pending
  Carlos's word, not part of this restart.

**rr-advfix datapack** (134 overrides, loads last): recenters every coordinate-based
advancement on the world's actual border center instead of an older center, so "Kilometre
Walk" no longer pops the moment you log in. About 130 "obtain an item" advancements now also
require crafting one, so a reward payout, a chest pull, or a trade no longer falsely pops a
"crafted X" advancement. BACAP's own reward-item freebies are unaffected. Nothing already
earned is being taken away; no revoke pass has run.

**RedwoodLens 0.10.0** (sha256 `1509979e…`): a PII-free-by-construction Minecraft game-data
snapshot on a 10-minute cycle, feeding the internal desk cockpit. Staff/ops-side only, not
player-facing.

## RedwoodBoard 1.9.0 — 2026-09-23 15:41 CDT (restart #7)

- **Job milestones.** Claiming a job shows a title and a chime. Marking a job ready pings staff.
  Getting paid for your first job promotes you to Scout with a title, a firework, and a server
  announcement. The board announces movement at most every 15 seconds.
- **Staff archive chest.** A 54-slot staff view of finished jobs (superseded the same night by
  the one-surface archive above).
- **Camp Tour, shipped inert.** The board-side machinery for the first-session tour went live
  but did nothing until every stop's location was set in the world.

---
*Full detail, receipts, and rollback plans live in the ops log; ping staff in Discord if a
number here doesn't match what you see in game.*
