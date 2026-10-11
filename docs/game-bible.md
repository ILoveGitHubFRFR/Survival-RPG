# Survival RPG — Game Bible Notes

Notes taken from the Game Bible (Batch 5, dated 2026-10-10). Designer/owner: Drew (Roblox account `LuvDrewMoti`).
Separate game from Combat Fighters; shares none of its rules or numbers.
Stack: Roblox Studio, Luau, Rojo + git. **Server decides everything that matters** (currency, inventory, rewards, drops, timers, permissions, damage); clients display and request.

## 1. Vision
- Solo / small co-op arena survival, 7-minute runs, 261 weapons in 10 rarity groups, Cosmic Pass, player Market, weekly leaderboard, rare event drops.
- Loop: fight → XP → upgrades → Chests → Towers → boss → extract.
- Must preserve: Ranked, trading, permanent weapons, limited-time events, multiplayer. Fill and No-Fill queues both exist.
- Look: stylized cartoon-fantasy, purple & gold UI, readable danger in swarms.

## 2. Run structure
- Run = 420 s. Boss Portal usable any time from 0:00; only one boss per run (simultaneous presses = one boss). Early summon doesn't change phase progression.
- Extraction unlocks at 360 s **and** requires boss dead.
- At 420 s, if nobody summoned the boss, portal auto-activates. Overtime: every 30 s enemies gain compounding +20% HP, +15% Move Speed.
- Phases (difficulty tiers): 1x / 3x / 9x. Phase 3 on standard maps has a Huge Boss.
- HUD: "Extraction unlocks in M:SS" (<6:00); "Defeat the boss to extract" (>6:00, boss alive); "Extraction open".
- Fixed UI timers (don't scale with match length): level-up pick 15 s, hold-E 5 s.
- Crystals are session-only. Each run pays once (retries/reconnects safe).

## 3. Maps
All maps: 15 Towers, universal Chest pool (no per-map Zone Chests), Phase pick on Map Select.

| Map | Type | Notes |
|---|---|---|
| Library Falls | Standard | Running Phase 3 here unlocks Builds |
| Map II: Frostbound Wastes | Standard | — |
| Map III: Forest Trunk | Standard | Half extents 600×420, MapKit scale 1.25; Phase 3 = difficulty baseline for Ranked & Hallow Sift |
| Inferno Coliseum (`InfernalColiseum`) | Ranked | Same size as Forest Trunk; ranks Lowest–Gold use Phases 1–3 |
| Hell Shrine | Ranked | Same size; unlocks at Diamond Lobby; Phase 3 = Crystal glass look |
| Hallow Sift | Secret event | Only during Party Market; same size |

Draft (not confirmed): Forest Trunk Huge Boss "The Blightroot Colossus"; Ghost enemy with invisible window.

## 4. Enemies & status
HP/damage = product of phase, queue, Ranked and variant factors, each applied once at spawn.

| Variant | Rule |
|---|---|
| Normal | Baseline |
| Silver | Enhanced rewards (existing 3x XP/Crystal factor) |
| Golden | 3x XP & Crystals; after first hit flees at 1.6x speed, vanishes 5.0 s later (timer never resets); if escaped: no reward, no kill |
| Crown | 3% of eligible normal/elite, max 4 alive; size +25%, dmg +50%, HP +50%, XP +75%, Crystals +75%; gold crown; never boss/Golden/Silver |
| Mini-boss / Huge / Final boss | Scripted, no variant bonuses, immune to Root |

- Bush enemies can be damaged.
- Damage numbers: own hits white full size, teammates' 55% opacity; Settings toggle (default on).
- Status effects: Weaken, Rooted, Frozen, Chill, Bleed, Poison, Blood Loss (Hell Shrine only). Emerald projectiles: +75% damage taken until defeat (bosses still take it).

## 5. Weapons & rarity (261 total)
Damage/hit = 25 × 1.38 (global) × rarity mult × weapon mult (Leaderboard weapons ×10). Roster data: `docs/weapons_export.txt` (not yet in repo).

| Rarity | Count |
|---|---|
| Common | 51 |
| Uncommon | 6 |
| Rare | 51 |
| Epic | 49 |
| Mythic | 21 |
| Legendary | 30 |
| Devine (game's spelling — never change) | 19 |
| Cosmic | 27 (highest Chest rarity; only some Chest-eligible, recheck vs 27) |
| Exclusive | 4 (+ Mining Sake = 5): Blood Infliction, Dice Control, Blood Shed Katana, The Golden Takeover |
| Leaderboard | 3: Aetherbind Scepter, Sunforged Greatsword, Wildfire Reaper |

- Cosmic Pass exclusives (not in Chests): Cosmic Projectile, Cosmic Gravity, Dragonslayer Galaxy. Infernal Chakram = free Tier 1; Stomper = Premium reward.
- Each weapon: Upgrades 1/2/3 unlock at item levels 5/10/15.
- Pools: Base, Zone, Ranked, Synergy, Pass, Crafted (Admin Maker), Achievement, Starter Pack & Limited (Shop), Level Reward, Leaderboard.
- Hidden (no ability code): First Strike, Fracture, Storm Axe, Throwing Daggers, Spirit Aura, Warden's Edge, Voyager, Gold Rush, Silver Tongue, Reaper's Toll, Emerald Sovereign, Midas Touch, Endless March, Curator's Blade, Diamond Fang, Apex Form.
- Not yet designed: 9 Forest Trunk weapons.

## 6. Level-ups, perks, Towers, Chests
**Level-up:** freezes combat, 15 s then auto-pick, exactly 3 cards with 3 different targets. Each card rolls own quality:

| Quality | Chance | Multiplier | Example buff |
|---|---|---|---|
| Uncommon | 50% | ×1.0 | +10% |
| Rare | 30% | ×1.5 | +15% |
| Epic | 15% | ×2.25 | +22.5% |
| Legendary | 5% | ×3.5 | +35% |

- Perk card = +1 perk level × quality. New-item card = item at level 1 × quality.
- Cosmic, Exclusive, Leaderboard, Hallow Sift-only items never appear as cards.
- Refresh re-rolls all three; Banish removes a target for the run; Skip unchanged.
- UI: left = equipped weapons/perks; middle = 3 cards; right = Now → After stats. Tap previews, Choose commits.

**In-game perks:** one shared pool of 35 perks + Death Row (first Cosmic-tier perk; six tiers, Cosmic above Legendary). Chests never give weapons. Drop rates = placeholder.
- Death Row: every 50 kills +20% dmg 120 s; every 200 kills +50% dmg 180 s; every 1,000 kills +50% dmg & +100% weapon speed 18 s. Separate timers. Upgraded thresholds: 10 / 100 / 500.

**Towers:** 15/map, fill 4.2 s (Golden, 3% chance: 9.0 s). Reward = pick 1 of 3 from 50-entry Tower Perk pool. Each completes once per run.
**Chests:** universal perk pool. Shop weapon Chests cost +15% Coins (rounded up).

## 7. Builds
Permanent, account-wide stat trades, unlocked by first Library Falls Phase 3 run.
- Linked pairs: Max HP ↔ Move Speed; Damage ↔ Attack Speed. +10% one side = −10% other. No removing penalties (Build Overloading removed).
- Cap ±30% by global Build Level; only applies to Builds (not perks/weapons; perk OVERLOAD is separate).
- One saved Build per account; applied once at run start as last multiplier; never stacks on respawn.

| Level | Range | Cost |
|---|---|---|
| 1 | ±10% | 2,500 Gems |
| 2 | ±20% | 7,500 Gems |
| 3 | ±30% | 15,000 Gems (25,000 total) |

First Build commit free; later changes 5,000 Gems each; same-values commit free; reset to 0/0 counts as a change.

## 8. Economy
Coins & Gems permanent. Crystals = Hallow Sift Chests, session-only. Sift Tickets = item.

**Run payout** (score S 0–1):

| Component | Weight | Full score |
|---|---|---|
| Kills | 30% | 1,200 / 1,800 / 2,500 (P1/P3/P9) |
| Damage | 25% | 250k / 600k / 1.5M |
| Towers | 20% | 15 of 15 |
| Bosses | 15% | Final = full; each mini/Huge +20% (max 80%) |
| Extraction | 10% | Extracted = full |

Coins = 2,000 + S×28,000 (nearest 10); Gems = 400 + S×4,600. Range 2,000–30,000 Coins, 400–5,000 Gems. Hallow Sift final boss: +25% both. Failed run pays 25% applied last (min 500 Coins, 100 Gems).

**Currency Shop (Robux):** Gold Pouch 5,000/75 · Satchel 15,000+1,000/195 · Coffer 40,000+4,000/449 · Hoard 100,000+15,000/899 · Gem Pouch 500/239 · Satchel 1,500+100/599 · Coffer 4,000+500/1,349 · Hoard 10,000+1,500/2,699.

**Cosmic Pass:** quests only (3 Daily Quests/day, 1 quest = +1 Tier). Auto-Refresh 199 Robux (whole season). Chakram free Tier 1; Stomper Premium Tier 5. Owners keep what they own.
**Chest passes:** Instant Open 149 Robux (3 batches, price = new Chest price ×1.25 for all 3). Multi Open/Spin removed (saved flags untouched).
**Gem sinks:** Builds, Market deals, Hallow Sift resale payouts.

## 9. Markets, Party Market, Owner, chat
- Common Market: no timer, no Hallow Sift, Global Chat.
- Party Market: temporary global event, scheduled only from Admin Panel (Date, Start, Duration 5–60 min, Confirm); notification via Roblox notification or banner. Timer "PARTY MARKET / MM:SS REMAINING".
- 12 live slots; every 60 s 6 stay, 6 replaced. Players list tradable weapons/perks for 100–100,000 Gems, no fee; Owner-only items unlistable.
- Layout: top title/status/timer; Owner Deals; main Live Deals; right Party Chat; bottom-left Hallow Sift preview + Enter Map. 35% brighter lighting, own menu music, Party Hat on Market button.
- Sift Tickets only from: 1% chance Hallow Sift Chest grants one; Owner-created listings for Gems. No auto-listing.
- Owner (E in Party Market): Owner Market Inventory (weapons, perks, Tickets, Coins, Gems, Gamepasses) → price, timer (≤60 min, capped at event end), per-player limit (default 1), optional stock. Owner Deals top, protected from rotation, ≤1 h, gold/purple styling. Non-owner calls rejected & logged.
- Chat: "Party Chat" during event, else "Global Chat"; one channel across servers; Owner picture first, up to 6 recent chatters, "1,000+ Players"; 5 s cooldown, 100 words max, filtered, server-enforced.

## 10. Hallow Sift
Secret celestial event, 7:00 run, normal Boss Portal/extraction.
- Access: only during Party Market; 1 free entry per event, later entries 1 Sift Ticket (win or lose); concurrent-attempt lock; failed start refunds ticket; ≤3 s skippable cinematic on first visit.
- Map: Forest Trunk size; pink clouds, butterflies, auroras. Giant Crystals replace Towers (15, same fill; Golden versions). Crystal Chests also give 100 session Crystals. 6 Crystal Statues (once per player per run: +40% Max HP, +15% Move Speed, 30 s). +25% jump height (restore on leave).
- Modifiers: +25% Tower becomes Legendary; Chests +20% Crystals; enemies 15% stronger than Forest Trunk P3.
- Enemies (only three): Legendary (+15% HP, cap 6), Devine (+30%, cap 4), Cosmic (+45%, cap 2). Final boss carries random Exclusive weapon.
- Drops (one roll per eligible kill; all boss damage dealers; no pity): Low 0.0001% permanent (dupe → resale value) · Medium 0.025% 60 h real time (ticks offline, re-drop resets) · High 0.25% resale-only, 50% of Gem value. Resale: Legendary 12,500→6,250; Devine 25,000→12,500; Cosmic 50,000→25,000. Mining Sake never drops.
- Butterfly events: one at a time, every 40–60 s, never during boss. Aurora: 40-stud area, spawn ×6 for 5 s within cap. Portal: open 12 s, random valid teleport, 1 s invulnerability, 3 s personal cooldown.
- Music: three 80 s loops @72 BPM started together (Celestial Threshold → crossfade 8 s at 3:12 → Aurora Ascent → crossfade at 5:52 → Heaven in Fracture through Overtime). Cloudbound Survival saved for later.

## 11. Ranked & Hell Shrine
- Draft 10 items (weapons/perks, non-Ranked), copies allowed, upgrade levels carry. Enemy baseline +100% HP, +25% dmg, +20% speed; rung multipliers stack on HP/dmg. 3-step tutorial unlock. Standard run rewards.

| Rank | Map/phase | Rung |
|---|---|---|
| Lowest | Inferno Coliseum 1 | 1.00x |
| Silver | Coliseum 2 | 1.15x |
| Gold | Coliseum 3 | 1.15→1.30x |
| Diamond | Hell Shrine 1 | 1.30→1.60x |
| Emerald | Hell Shrine 2 | 1.60→2.20x |
| Crystal | Hell Shrine 3 | 2.20→3.50x |

Ranges rise linearly over 7:00; higher ranks may queue lower unlocked phases.
- Crystal Tier rewards (once): Mining Sake, Title "The Outlaster", free Chest-eligible Cosmic pick.
- Hell Shrine landmarks: Ashen Gate (spawn), Cinder Court, Bone Labyrinth, Blood Pools, Sacrificial Vault, Shrine Altar (extraction). Hell Devil: spear every 3 s up to 35 studs, +15 Blood Loss. Warlord mini-boss: 3-spear volley, +25 each. Final boss: The Infernal Judge. Enemy-only Hell weapons.
- Blood Loss 0–100: spear +15, Warlord +25, Hell weapon hit +10; decays 5/s after 4 s no hit; at 100 defeat regardless of HP; revives reset it.
- Look: P1 dark red; P2 fire; P3 Crystal glass. Cosmetic only; debris client-side ≤200 studs (150/80/30 by graphics).

## 12. Signature weapon kits
**Mining Sake** (Exclusive; base dmg = live average Cosmic; never in Chests/Gem Menu). Random form per shot:
Crystal 1.25 s (1→12→24 split) · Emerald 0.8 s (root 3 s, +75% dmg taken) · Ruby 0.75 s (4 projectiles 90° apart, ~8 stud explosions) · Jet 1.5 s (chain ≤250, +10% per chain cap +500%, 25-stud explosion, black crystals 5 s @20 dmg/0.1 s).
U1: 2 orbiting extras per projectile (main +80%, orbiters 50%). U2: 5 s normal, 4 s at +200% AS all four forms at once, repeat. U3: fifth form Crystal Tier every 9 s on nearest enemy ≤40 studs (instakill regular, heavy boss dmg). Lv10: every 5 s combined explosive shot (18-stud blast, 6x hit + all effects).

**Dice Control** (Exclusive): every 3 s roll Uncommon–Devine weapon, use 5 s with +200% AS, +20% Size. U1 adds Cosmic. U2: 1 s / 10 s. U3: Legendary–Cosmic only, two dice.

**Cosmic Lifeplant:** plant radius traps enemies, drains health to you (full kit in Design canvas).

**Newer Cosmics** (proposed values are Code's to tune):
- Cosmic Configure: orb ~55 studs → 3 s Black Hole pull ~16 studs, detonation every 5 s (~35 dmg proposed). U1 pulse 2.5 s; U2 +25% range; U3 +15% dmg.
- Flame Ignite: melee, fire trail segments 7 s (~6 dps proposed). U1 every 5th step explodes after 2 s; U2 +25% Move Speed; U3 ≤50% HP trail dmg +100%.
- Holy Southern: every 7.5 s lifts enemies ~14 studs, instakills regular, heavy boss dmg. U1 lift delay +2 s; U2 interval 3 s; U3 every 5 s Holy AOE + knockback.

## 13. Admin, Owner, security
- Owner = LuvDrewMoti only via server `IsOwner(player)` (fails closed). Admin = existing list (e.g. BlobHurtsRadahn) via `IsAdmin`. Client attributes/hidden buttons are never security.
- Owner-only: Party Market deals/timers, ticket listings, global events (incl. Hallow Sift), grants to others, save resets, global tuning, admin roles.
- Admin: self-only test tools (spawn enemies, teleport, own stats).
- Every admin remote: re-check role, validate args, rate-limit 10 req/s/player; capped audit log. Tuning is live in-memory only, never written to disk.
- Rewards/prices/drops/timers/damage server-side, granted once per ID. Filter all player text. Never test economy on the Owner profile.

## 14. Presentation
- Purple & gold, one font, shared UIKit; readable 390×844 → 1920×1080; HP text never truncates.
- Enemies: rounded shapes, larger head, saturated two-tone, clear silhouette. Every attack has ≥0.4 s ground telegraph.
- Feedback: damage numbers, hit flashes, XP/Crystal magnet, VFX/SFX/icon remake for Common–Devine. Map polish must not change routes/collision/spawns/extraction markers.
- Missing sound IDs stay silent until Drew supplies them. Settings: Damage Numbers toggle, graphics setting scales debris.

## 15. Open / do not build
- 9 undesigned Forest Trunk weapons; 20-item Achievements list (draft); Blightroot Colossus & Ghost (draft).
- Customize Menu lag (Perks ↔ Weapons) unprofiled.
- Spirits & Spirit Perks arrive in Update 1.
- 50/50 pick between long Hallow Sift tracks postponed.
- Run Chest perk drop rates placeholder; resale Gem values for weapons lacking them; Multi Spin owner compensation; real product/gamepass IDs; missing audio IDs.

## Sources of truth
Design canvas (visuals, "New Systems and Hallow Sift Notes" board); repo docs and Batch 5 prompt file (rules and implementation prompts).

## Notes / inconsistencies spotted
- Phases are described as "Phase 1, 2, 3" on Map Select but the 9x tier is also called "Phase 9" in Run Rewards and Section 2 — confirm the real naming.
- Cosmic count: roster says 27; Chest-eligible count (16 of 21) is stale.
- Ranked "Gold" rung and Hell Shrine "Diamond" start both reference 1.30x — confirm intended boundaries.
