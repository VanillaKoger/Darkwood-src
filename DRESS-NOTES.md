# Darkwood graybox — dress notes (feel pass)

**Date:** 2026-10-27 (America/Chicago)  
**Entry:** `graybox/index.html` (Three.js + GLTF)  
**Empty 2b backup:** `graybox/index-2b-empty.html` (Canvas2D circulation)

## How to open

```bash
cd /workspace/games/high-fantasy-biome-incremental/graybox
npx --yes serve -p 4173
```

Open `http://localhost:4173` (ES modules + CDN Three.js — **prefer http serve**, not `file://`).

## What was placed where

| Region | Dress | Packs |
|--------|-------|-------|
| Whole map foliage | KayKit trees/bushes/rocks; denser keep-rate **W/S**; bare/scorched trees **E/NE** | KayKit Forest Nature FREE |
| W accents | Extra Kenney tall pines | Kenney Nature Kit |
| Ring + cardinal roads / lake / platform | Procedural meshes (same 2b layout constants) | — |
| East + west bridges | Kenney wood bridge + Hex bridge accent + plank deck | Kenney + Medieval Hexagon FREE |
| South spawn | ByndWalker cabins + Kenney stump | ByndWalker + Kenney |
| Road encampments | Tents + campfires + branch fences along N/E/W/S and ring shoulders | ByndWalker + Hex tent + Kenney campfire |
| Center platform | Stone house, tent, log stack, fence, Skeleton Warrior (boss marker) | ByndWalker + Kenney + Skeletons FREE |
| NE spirit | Hex destroyed building, Kenney columns/rock, Quaternius Ghost (bobbing), bare tree cluster, purple point light | Hex + Kenney + Quaternius Ghost |
| SW legendary | Quaternius Dragon + Wolf×2 + Fox + Deer + Stag | Quaternius animals + Dragon |
| North edge | Emissive fire-curse wall + point light (**fixed to north / −Y**; empty 2b had wall on south by bug) | Procedural |
| Trash feel | Skeleton Minion/Warrior markers by quadrant | Skeletons FREE |
| Player | KayKit Ranger | Adventurers FREE |
| Lighting / env | Hemisphere + sun shadows; optional Poly Haven autumn forest HDRI | Poly Haven |

Lean copies live under `graybox/models/` (~9 MB). Provenance packs remain under `assets/` with LICENSE/CREDITS untouched.

## Layout / scale

- World **220×220**, walk **3.85** u/s → ~**57s** N–S tip-to-tip (unchanged).
- Landmarks match 2b (shack S, legendary SW, spirit NE, boss center) except **fire wall moved to true north** per DESIGN lock.
- Camera: phone-friendly overhead isometric follow (Blood-hunt-ish).
- No combat / systems — walk + region labels only.

## Known gaps

- Bridge Kenney pieces are scaled accents; walkable surface is the procedural plank deck (reliable collision).
- Hex GLTFs needed a copied `hexagons_medieval.png` beside them (upstream free pack keeps PNG under `fbx/`).
- Full PWA game shell not in this graybox.
- Tree count is a feel density, not final art budgeting.

## Not done (intentional)

- No Extra/paid pulls, no purchases.
- Full Three.js game scaffold deferred until Devon says mint greenfield.

## Second dress pass — 2026-11-02 (America/Detroit)

**Published:** https://github.com/VanillaKoger/Darkwood-src → Pages https://vanillakoger.github.io/Darkwood-src/

- **Animations wired.** `models/anim/` holds KayKit Character Animations FREE `Rig_Medium_General` + `Rig_Medium_MovementBasic` (shared 23-bone rig with Adventurers/Skeletons). Models are cloned with `SkeletonUtils.clone` so each skinned copy animates independently; one `AnimationMixer` per animated actor.
  - Player Ranger: `Idle_A` ↔ `Walking_A` crossfade (0.2s) driven by movement input (keys + touch pad).
  - Center boss skeleton: `Idle_B`. Trash skeletons: every third patrols a 3.5u loop with `Walking_B`, rest `Idle_A/B`.
  - Quaternius Ghost/Dragon: built-in `Flying_Idle` (plus existing bob). SW animals: `Idle`/`Eating`.
- **West beast quadrant filled.** Pulled the remaining 8 of the 12-animal Quaternius Animated Animal Pack from Poly Pizza (CC0): Cow, Bull, Alpaca, Horse, White Horse, Donkey, Husky, Shiba Inu. Five dens off the W road (wolf pack, horse paddock, cattle, deer/stag/fox, dogs); grazers `Eating`/`Idle`, wolves/dogs/fox patrol loops with `Walk`. Label "West beast dens".
- **Obstacle fade.** Trees, bushes, pines, bare trees, tents, cabins, stone house, ruins, column register as occluders; each frame any whose footprint intersects the camera→player segment eases to 28% opacity (materials cloned lazily per object so shared templates are untouched).
- Fixed: random tree yaw was being overwritten to 0 by `place()`.
- `<link rel="icon" href="data:,">` to kill favicon 404.
- LICENSE copied beside every `models/*` pack; top-level `CREDITS.md`.
- Paths are all relative (`models/...`) → works under the `/Darkwood-src/` Pages subpath.
- **ByndWalker:** license reviewed — `Free` folder is CC0 (redistribution OK); only Free-folder GLBs used, Full/Extra (no-redistribution) never pulled. Kept.

### Known issues (pass 2)
- Patrol loops are simple circles; animals/skeletons can clip props or cross road shoulders.
- Fade uses bounding-footprint test, not per-ray mesh hits — large trees may fade a touch early.
- Three.js still from jsDelivr CDN (needs network).
- `models/` ~19 MB; first load on mobile data takes a few seconds.

## Kill → absorb prototype (2026-11-02)
- **Attack:** auto-attack ON by default — every 0.6s (÷ attack speed) all enemies within 2.5m (+ enemy radius) take 10 (+DMG) damage. Manual: big **Attack** button (bottom-left) / Space. **Auto: ON/OFF** toggle / T.
- **Feedback:** head HP bars (show after first hit, 6s), floating yellow damage numbers (pooled CSS2D).
- **Death:** model shrinks/dissolves (0.35s; boss 0.7s) → 24–40 additive glowing particles in the biome element colour (Darkwood = Nature `#5BE36A`; `ELEMENT_COLORS` also defines Cold `#8FD8FF`, Fire `#FF7A2A`, Lightning `#D8C8FF`). Burst outward, hang ~0.3s, then quadratic-bezier home onto the player with 3-point trails. No pickup radius — always auto-absorbed.
- **Arrival:** green ring pulse + point-light glow on player, floating gain text. Gains per enemy *type* (element is per biome, all Darkwood = Nature): beasts +1 HP, spirits +1% atk spd, bandits +1 DMG, skeletons +1 Armor. HUD stat line above the Attack button.
- **Bosses:** center bandit king (skeleton warrior stand-in, 260 HP) and SW legendary dragon (300 HP): ~120 particles, 0.75s hang, slower stream, big repeating pulse, +10 gain.
- **Debug:** **Boss test** button / **B** key spawns the boss stream 6m in front of the player.
- **Respawn:** 20s, full HP.
- **Perf:** single pooled `THREE.Points` (1400 particles × 4 verts incl. trails), struct-of-arrays state, no per-frame allocations in the FX loop.
- Known: no regular bandit enemies yet (only the king); enemies don't fight back; dissolve is scale-only (no material fade); on phones the walk script collapses to the current step to make room for combat buttons.

## 2026-11-02 — Move speed 2x + Training Yard
- `WALK_SPEED` 3.85 → 7.7 (exactly 2x). Player walk clip `timeScale` 1 → 2 to limit foot skate.
- Training Yard: 4 stationary KayKit skeleton dummies (Minion/Warrior, Idle_A, no patrol, don't fight back) in a 2x2 grid at x 10–12.6, z HALF-13..HALF-10, east of the shack spawn and off the S road; "Training Yard" label. Registered as `skeleton` trash (30 HP, HP bars, damage numbers, Nature-green essence stream, +1 Armor).
- `regEnemy` opts: `respawn` (seconds, default 20) and `dummy`. Dummies respawn at the same spot with full HP 1.0s after the killing blow (0.35s shrink + 0.65s hidden). Debug: `window.__dummyRespawnMs` logs measured game/wall delay.

## Ground texture pass (2026-11-02)
- **Walk anim** timeScale 2.0 → **1.5** (move speed unchanged 7.7).
- **Grass** (Poly Haven sparse_grass): world-space UV, 6 m tile, on the ground plane; old quadrant vertex colors kept as a tint (W/S greener, E/NE ash, north scorched).
- **Dirt roads** (Poly Haven dirt): ring road = RingGeometry + wider feathered shoulder ring (alpha gradient via uv1 alphaMap); 4 cardinal roads = flat planes + feathered shoulder planes. 5 m tile. Layout constants unchanged.
- sRGB on color maps, anisotropy ≤8, mipmaps. Textures total ≈1.7 MB.
- Preview: `textures-preview.png` (390×844 @2x).
- Known issues: dirt/grass blend is an alpha-fade shoulder, not a true splat; road crossings at ring overlap with transparent shoulders (minor sorting).

## Stone removal + floating joystick (2026-11-02)
- **Cobblestone removed entirely**: stone patches, bridge-approach pads, and platform/rim stone texturing deleted; `models/textures/stone/` removed (and from CREDITS). Platform + rim keep geometry with plain untextured earthy MeshStandardMaterial (0x8a7a62 / 0x9a8a70). Grass + dirt unchanged.
- **Floating joystick**: pointerdown anywhere (except buttons, `#combat`, `#script`, `.panel`) spawns the base at the touch point; drag moves knob (48 px full deflection); release hides it. Tracks a single pointerId, so a second finger can tap Attack. Works with mouse drag on desktop; keys unchanged. Faint "drag anywhere to move" hint bottom-center on coarse pointers.
