# Darkwood Forest — Dressed Graybox (feel pass)

Local Three.js + GLTF preview of **The Darkwood Forest** so Devon can walk the locked layout with the free asset family dressed in.

**Play:** https://vanillakoger.github.io/Darkwood-src/ (GitHub Pages, phone-first). No combat, no paid/Extra packs. Empty circulation backup: `index-2b-empty.html`.

## Open

```bash
npx --yes serve -p 4173   # from the repo root
```

Then open `http://localhost:4173`.

**Use a local http server** — the page loads Three.js via CDN import map and GLTFs as modules/assets. `file://` often fails CORS for ES modules.

Phone: same URL on the LAN; touch joystick appears on coarse pointers.

## Controls

| Input | Action |
|-------|--------|
| **WASD** or **Arrow keys** | Walk |
| **Right touch pad** (phones) | Virtual stick |
| **Reset timer** button | Zero the on-screen elapsed clock |

HUD: elapsed time, nearest landmark, N–S tip time, walk-script checklist, minimap. Region labels float in-world.

## Scale notes

- World footprint: **220 × 220** units.
- Walk speed: **3.85** units/sec.
- Straight **N–S tip-to-tip** ≈ **57 seconds** (45–60s band).
- Tunables at top of the module script in `index.html` (`WORLD`, `WALK_SPEED`).

## What you should notice on the walk

1. **South shack** — ByndWalker cabins at spawn  
2. **Ring + cardinal dirt roads** around the lake; denser KayKit woods **west/south**  
3. **SW legendary** — dragon + wolves/fox/deer/stag; **west beast dens** — all 12 Quaternius animals (grazing / patrolling)  
4. **NE spirit** — ruined hex building, columns, floating ghost, scorched bare trees / ash ground  
5. **Two bridges** onto the **center bandit platform** (stone house + skeleton boss marker)  
6. **Tents/encampments** along roads  
7. **North fire-curse wall** — glowing blockage / exit stub  
8. Skeleton “trash” markers by quadrant (idle or patrolling)
9. Player Ranger idles / walks (KayKit animations); trees & props between camera and you fade out

See `walk-script.md` and `DRESS-NOTES.md` for placement + pack credits.

## Models

Lean CC0 GLBs/GLTFs under `models/`, each folder with its `LICENSE`. Full credits: [`CREDITS.md`](CREDITS.md).

## Status

Second dress pass (2026-11-02): animations, full animal set, obstacle fade. See `DRESS-NOTES.md`.
