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
