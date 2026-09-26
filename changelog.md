# Redwood Ridges — technical changelog

> The plain-English version of what's new is [here](whats-new.md). This page is for anyone who
> wants the version numbers and the mechanism, not just the effect. Newest first.

## Staged for the 04:00 CDT 2026-09-26 restart — RedwoodAdvancements 0.2.0, RedwoodLens 0.12.3, rr-guide, Essentials + Towny cooldowns (not live yet; calliope `22b8412` merge + `e16777b`)

**RedwoodAdvancements 0.2.0 (the #campfire flood):** the relay posted every chat-announcing
advancement with no limit (one player: 31 lines in 71 minutes). Now, per player: challenges always
post; the first task/goal in a 15-minute window posts; the rest fold into one digest line when the
window closes or the player quits; a global 6-per-minute cap smooths bursts (refused posts wait,
never drop). BlazeandCave's "/trigger bac_statistics" hint is stripped from descriptions.
Config: `throttle.window-minutes`, `instant-per-window`, `global-cap-per-minute`, `max-names`,
`ignore-keys`, `format.digest`. DigestTest: 38 checks; the real 31-line hour replays as 8 posts.

**RedwoodLens 0.12.2 (carried in 0.12.3):** EliteMobs kills reach #campfire only for custom bosses whose EliteMobs
config broadcasts their death (`announcementPriority` >= `world-stream.elitemobs.min-announcement-priority`,
default 1). A spawner "Lvl 3 Elite Slime" had got through the old natural-entity check.
Live config: the two `auction-stream:` blocks are merged (the boot warned "duplicate keys").

**RedwoodLens 0.12.3 (Veinminer/Excavator XP):** SuperEnchants 4.6.2 breaks the extra blocks with
`Block.breakNaturally` a tick later: no BlockBreakEvent, so no XP and no Enlightened bonus past the
first block. `MiningXp` snapshots the candidate blocks before SuperEnchants acts, confirms the break
wasn't cancelled, and after the breaks settle spawns a real XP orb for each changed ore: the vanilla
roll times Enlightened's multiplier (`base + level x per_level`, truncated exactly like
SuperEnchants' `XPBonusAction`), read from SuperEnchants' own `enlightened.yml`/`veinminer.yml`.
No XP with Silk Touch or in creative; a paid-once ledger prevents double pay. `mining-xp.enabled`,
`settle-delay-ticks`. OreXpTest: 32 checks, including the live SuperEnchants files.

**Cooldowns (Carlos 18:59, a player asked):** Essentials `command-cooldowns` `/home` 1800 s -> 900 s,
`/spawn` 600 s -> 900 s; Towny `town_spawn`, `outpost` and the six other town/nation spawn
cooldowns 30 s -> 900 s, which closes the `/t spawn` bypass of the `/home` cooldown.

**rr-guide:** ch9/ch18 death fee, ch17 /receipts + /home + the 15-minute cooldowns (field-guide.md).

## Restart 16:35 CDT 2026-09-25 — RedwoodLedger 0.2.0, RedwoodLens 0.12.1, rr-advfix mine claims (calliope `desk/ride-0925` `91eef20` / `70c0ccd`; advfix main `f77489a`)

Boot 21:35:50Z. Ledger enable checkpoint: 23 accounts; tape verifies clean.

**rr-advfix (mine claims):** 37 "obtain this block" advancements (*Stone Age*, *Seeing Red*,
*G.I. Geode* and others) also require the `minecraft:mined` stat for that block, so taking the item
from a chest no longer grants them. Existing grants are untouched (no revokes). 3,885 advancements
load with no parse errors.

**RedwoodLedger 0.2.0** (new plugin, first run at this restart): an observe-only economy tape.
Every Essentials balance change — player accounts and town banks — is written to a hash-chained
log with the plugin that caused it, plus hourly full-balance checkpoints so a change made outside
the tape shows up as drift. It never changes a balance.

- **`/receipts`** (also `/ledger me`): a player's own last money moves in plain words (Board,
  Towny, Shop, Auction House, `/pay from <name>`, Staff adjustment), seeded from the tape at
  startup. Everyone has it (`rr.ledger.me`, default true); nobody can see another player's.
  Idea credit: GL-EcoAudit (7str1kes), whose `/transactions` shows a player their own history.
- Staff: `/ledger status`, `/ledger checkpoint`, `/ledger player <name>` (`rr.ledger.admin`, op).

**RedwoodLens 0.12.1** (builds on 0.12.0):

- **Treeline reminder.** Every `shrine.announce-minutes` (default 45, 0 turns it off) chat hears
  the Treeline's progress and what's left, only while someone is online, an altar place is armed,
  and the goal isn't met. No altar is armed yet, so it is silent until the Temple is built. Idea credit: CommunityGoals (ByteBurrow, Papaphrog), whose
  `announcement-interval` rebroadcasts an open goal.

## Restart ~13:51 CDT 2026-09-25 — RedwoodBoard 1.9.8, RedwoodLens 0.12.0, Essentials cooldowns (calliope main `d9fe6dc`)

Boot 18:51Z: both enabled, tour ARMED (8 stops), stacking self-test passed. The Founding Day pack
itself is data (`plugins/RedwoodBoard/packs/founding-day.yml`), uploaded separately and loaded live
at the Saturday 14:10 ceremony. Places ship dormant: none are armed until each build is finished.

**Essentials** (`ess reload` 18:42Z, no restart needed): `command-cooldowns` `/home` 1800 s,
`/spawn` 600 s, `command-cooldown-persistence: true`.

**RedwoodBoard 1.9.8 "Megapack"** (builds on 1.9.7):

- **The pack loader.** Staff can load a whole batch of jobs onto the board in one command, from a
  file instead of typing each one by hand. Founding Day's 23-job pack is the first to use it.
- **Default expiry.** `/makecontract`'s Expires box now pre-fills a sensible date based on the
  job's size, instead of starting blank. Staff can still change it.
- **A quiet load, then one announcement.** Loading a whole pack doesn't spam chat or Discord once
  per job — it posts a single summary line once the load finishes.
- **Mega jobs stand out.** The biggest jobs on the wall get a distinct look on hover.
- **Idea credit.** A pack job's book can name whose idea it was, shown as the last line of the
  book.

**RedwoodLens 0.12.0 "Places"** (new):

- **Places, dormant until built.** A new system lets staff mark a finished build as a working
  place — a plaque, a mailbox, or the Temple of Mallow's altar — once it's actually built. Nothing
  works until staff switch it on; there's no such thing as an early or temporary version.
- **Plaque.** Right-click one on any finished big build to see its name, its builders, and whose
  idea it was.
- **Mailbox.** Right-click to read any mail waiting for you. Parcels: from inside the post office, a player can
  send the stack in their hand to another player's registered mail chest (all-or-nothing, logged;
  only the owner and staff can open a mail chest). Staff: `/rrlens place new`, `place set`,
  `place mailchest <player>`.
- **The Temple of Mallow's altar.** Right-click to offer Marshmallows toward the shrine's goal.
  Offerings don't come back and don't count toward rank — they're a collective gift, and when the
  goal is met, the treeline moves.

## Restart 08:29 CDT 2026-09-25 — RedwoodLens 0.11.9 (carries 0.11.6-0.11.8)

- **0.11.9:** in-game `[Auction] <seller> sold <item> to <buyer> for <price>` in AuctionHouse's
  colours. Live config `auction-stream.highlight-min-price: -1` turns off Lens's own 100+ sale line
  in `#campfire`, since Union Rep now posts sales there.
- **0.11.8:** extended descriptions carried into shops and menus: live auction listings are enriched
  at the source (so AuctionHouse's 1 s redraw keeps them), plugin menus on open/click; the 2 s
  inventory sweep skips menus (it had made auction lore flicker).
- **0.11.7:** `#campfire` kill lines ignore the Training Dummy, the Adventurer Instructor and the
  guild world; format `⚔️ **X** defeated **Name** · Lv N`.
- **0.11.6:** soulbound refusal message points to the Adventurer's Guild (sell or scrap); the
  "unbind scroll" claim was removed (no unbind scroll on this server).

## Restart 17:10 CDT 2026-09-24 — RedwoodLens 0.11.5

- **Soulbound listing guard:** `/ah sell` and `/ah bid` refuse items carrying `elitemobs:soulbind`
  (the owner-only EliteMobs binding). The three soulbound listings already up were expired by staff
  and their sellers mailed.

## Restart 16:41 CDT 2026-09-24 — RedwoodLens 0.11.4 (carries 0.11.3), Armor Stand Poses

- **0.11.3:** plain seller/buyer names (no colour codes) at the source; per-listing lore in
  `auctions.json`.
- **0.11.4:** `listed_at` / `expires_at`, auction type, bids, top bidder and durability in
  `auctions.json` (read from AuctionHouse by reflection). Union Rep's `#auction-house` posts show them.
- Armor Stand Poses datapack enabled (fixed `pack.mcmeta`).

## Restart 16:04 CDT 2026-09-24 — RedwoodBoard 1.9.7, RedwoodLens 0.11.2

Uploaded at 14:06 as 1.9.7 / 0.11.1. The 16:00 boot failed to load Lens (an unquoted `plugin.yml`
description); 0.11.2 fixed it and adds the unified tooltips (SuperEnchants 98, vanilla 43,
datapack runes 18) and booted at 16:04. The Discord routing lines below were already in the live config.

**RedwoodBoard 1.9.7** (285,407 B, calliope main `dec202b` + `007c0c1`, includes 1.9.6):

- `/makecontract` always hangs the new book in an empty board frame (1.9.6). The chest path is gone.
- **Board hover status.** The book's display name carries its live state, which is what the item
  frame shows: `open 0/2`, `Tylerbro1 · 1/2`, `… · full`, `… · needs N more`,
  `… · ready for review`, `… · closed`, `… · paid`. Claimant names cut at 10 characters, two
  shown, then `+N`. Only the name changes. The book's pages, title and author (what identifies a
  contract) are untouched.
- **Crew minimum.** A range like "2-4 builders" now sets a minimum of 2, and "N builders" / "duo"
  set the minimum to the full crew. It's checked at **Mark ready**, at pay and at verify, not at
  claim, so a crew can gather over time. It only applies to books made with `/makecontract` from
  1.9.7 on (a flag on the book), so jobs already on the wall aren't blocked mid-build.
- **`/tip`** (staff, `rr.board.tip`). A dialog: pick a player, an amount or a preset, a reason,
  confirm. Announced in chat and in `#campfire` (EssentialsDiscord type `rr-tip`). Draws on a
  weekly pot (1,000, resets Monday 00:00 America/Chicago). An overdraft warns staff but still
  pays. Every tip writes a receipt (`kind=tip`, faucet `public_works`). `/tip log` shows the week.
- **Paid-out push.** Multi-slot jobs post "💰 {title} paid out — …" to `#campfire` when the last
  payout lands (`discord-push.events.campfire` gains `paid`). Single-slot jobs keep their one
  "completed" line.

**RedwoodLens 0.11.1** (156,900 B, calliope main `23558e8` + `ea19103`): world events → Discord through
EssentialsX Discord message types. Each source is skipped quietly if its plugin is absent.

- `rr-world` → `#campfire`: towns founded, residents joining, towns opening or closing (Towny);
  non-natural EliteMobs boss kills and dungeon clears; AuraSkills levels every 5th level. At most 10 posts a minute across all kinds, plus a per-kind cooldown.
- The auction watcher (AuctionHouse 1.5.5 fires no events, so Lens polls its storage every 5 s and
  diffs). 0.11.0 re-posted every stored listing on each boot; 0.11.1 primes silently on the first
  poll. It writes `plugins/RedwoodLens/auctions.json` (`rr-lens/auctions/v1`: id, seller, item,
  amount, price, status for_sale/sold/expired, buyer, enchants) and posts 100+ marshmallow sales to
  `#campfire`. The old `rr-auction` text stream is off (`auction-stream.text-enabled: false`).
- `plugins/RedwoodLens/world.json` (`rr-lens/world/v1`): towns (mayor, residents, open, public,
  board text, spawn, nation, ruined, recruiting) and nations. Rewritten 15 s after a Towny change.
  Union Rep builds `#the-world` from it.

## Discord, live 14:02 CDT 2026-09-24 — Union Rep mirror fixes (tabletop main `c98bab9`)

- **`#contracts` drift fixed.** Three bugs: one failing contract used to stop the whole update
  pass (and every contract after it in the list); a thread could only archive after a payout was
  recorded in `archive.json`; a reopened job left its old thread live. Now each contract is
  isolated, a contract missing from two reads in a row archives (never deletes), a superseded
  run archives at once, and two live jobs sharing a title are reported to staff, not merged.
- **`#campfire` topic** shows who's on now, from the same heartbeat as `#server-status`,
  rewritten at most every 5 minutes. `#server-status` is unchanged.
- **`#the-world`** (new forum): one post per town and nation, tagged open / closed / recruiting /
  ruined. It fills once RedwoodLens 0.11.1 is live.
- **`#auction-house`** (new forum, recreated 14:16 CDT; the text channel was empty): one post per
  auction item, tagged For sale / Sold / Expired, written by Union Rep from a RedwoodLens
  `auctions.json` (tabletop `b8d0c16`). Up to 8 new posts per pass; items already sold or expired
  before the first pass are skipped. It fills once RedwoodLens 0.11.1 is live. EssentialsDiscord `rr-auction` is `none`.

## Restart 10:25 CDT 2026-09-24 — RedwoodBoard 1.9.5 (`/rreport`), CoreProtect CE 24.1

- **`/rreport`:** a private report form (Paper dialog). Name optional (blank sends as Anonymous),
  where-and-what, routed through EssentialsDiscord `rr-report` to the staff channel. No reply path;
  Wardens reach out directly.
- **CoreProtect CE 24.1** runs alongside Prism (SQLite; `explosions: true`, default radius 10, max
  100) because Prism logs no explosion action.
- Same morning, no restart: Seedlings gained `essentials.home` + `essentials.sethome` (10:14);
  `/grave` / `/graves` teleport and GUI made staff-only (11:49); Towny wilderness explosion revert
  turned off in `world` (12:53); rank colours (18:59).

## Restart 04:01 CDT 2026-09-24 — RedwoodBoard 1.9.4, rr-advfix update, BlueMap, Warden shield pack

**RedwoodBoard 1.9.4** (242,456 B, calliope `b5b66ee`):

- **Through-walls tour beacon.** Under the waypoint label sits a per-player `BlockDisplay` (a
  gold block stood on its corner, spinning once every 4 s) with `setGlowing(true)` and a gold
  glow-colour override. The vanilla glowing outline renders through terrain, which text can't.
  Config `tour.beacon` (default true). Bedrock has no glowing outline and sees the plain block.

**rr-advfix** (135 overrides, 103,369 B, sha256 `4f7c8af1…`, calliope `2525a31`):

- *Death Pointer* ("Using Echo Shards…, craft a Recovery Compass") gains the crafted-stat
  condition. The audit's make-claim detector missed the "Using X, craft Y" phrasing. GravesX's
  respawn recovery compass is now listed as a plugin grant in the audit report.
- The six recentred distance advancements (`kilometre_walk`, `ten_thousand_blocks`, `voyage`,
  `spawn_camping`, `a_million_blocks_away`, `farlander`) now name `minecraft:overworld`.
  Without it the shifted bounds matched in every world, and the EliteMobs hub sits inside them.
- No revokes. Existing grants stay.

**BlueMap** `core.conf`: `update-cooldown` 60 → 30 s, `full-update-interval` 1440 → 60 min.

**Warden shield pack** (`RRWardenShield_resource_pack.zip`, merged by ResourcePackManager, first in
`priorityOrder`): an 8×8 bitmap glyph for U+26E8 ⛨. Inert until the staff groups carry the mark.
In the merged font it sits after vanilla's Unifont reference, which already has a ⛨, so clients
currently draw Unifont's shield instead of ours. Either way it reads as a shield.

## RedwoodBoard 1.9.3 — 2026-09-23, restart ~22:04 CDT (calliope main `7ff30ba`)

- **Restack only what the game split.** The 5-second sweep is retired.
  `PlayerInventorySlotChangeEvent` marks a slot when an untagged trim material lands in it, or
  when the same material turns from untagged to tagged. Only those slots merge, so a split you
  made yourself stays split. The merge waits while any container is open and runs when it closes.
- **Waypoint legibility v2.** One non-see-through `TextDisplay`. The 1.9.2 see-through copy drew
  its background over the normal copy's text and showed as a blank black slab.

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

**Live since the 22:04 CDT restart (with 1.9.3):** QuickShop-Hikari `allow-stacks: true` (bundle
shops through `/qs size`).

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
