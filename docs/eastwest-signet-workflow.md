# East-West Signet Workflow

Reference build: **east-west emerald signet ring** (semi-bezel / tension-style setting).

Canonical assets (commit `5d19f77`):

| Asset | Path |
|-------|------|
| Blender scene | `fixtures/eastwest-emerald-ring.blend` |
| Best mesh checkpoint | `meshes/prepared/eastwest_v17_refine5-polished-lock.stl` |
| Product refs | `refs/eastwest-emerald-ring/reference-side-pink.png`, `reference-34-orange.png` |
| Review screenshots | `refs/eastwest-emerald-ring/refine*.png`, `lock*.png` |

## Locked design parameters

| Parameter | Value |
|-----------|-------|
| US size | 8 → **18.14 mm** inner diameter |
| Band width (head / bottom) | 8.0 / 6.0 mm |
| Band thickness | 2.0 mm |
| Stone | Emerald cut, east-west, ~7 × 5 mm |
| Hollow | Solid first (hollow deferred to cast prep) |

Gem placement (visual only until seat is cut):

- Location ≈ `(0, 12.603, 0)`, rotation `(-90°, 90°, 0)`
- World bbox: x ±3.5, y 10.07–13.34, z ±2.5

## Pipeline steps

| Step | Goal | Checkpoint prefix |
|------|------|-------------------|
| 0 | Scene units, refs, collection | — |
| 1 | Band blank | `eastwest_v01_band-blank` |
| 2 | Taper head/bottom widths | `eastwest_v02_tapered` |
| 3 | JewelCraft gem (visual) | — |
| 4 | Bezel block | `eastwest_v03_bezel-block` |
| 5 | Seat cut | `eastwest_v04_seat-cut` |
| 6 | Bevel cleanup | `eastwest_v05_cleaned` |
| 7 | Head reshape | `eastwest_v06_head-reshape` |
| 8 | Enclosure iterations | `eastwest_v07` … `v09` |
| 9 | Locked seat (physics fix) | `eastwest_v10` … `v12` |
| 10 | Design refine passes | `eastwest_v13` … `v17` |
| 11 | Meshmixer gate | `meshes/raw/` → `meshes/repaired/` |
| 12 | Cast prep | ask metal/resin; `v06_cast-ready` |
| 13 | Sprue | manual per `docs/sprue-placement-sop.md` |

Never overwrite checkpoints. Export a new versioned STL after each major step.

## Gem enclosure — mechanical lock (critical)

A stone placed *beside* pillars will fall out. The metal must **overlap the stone footprint** on the short ends and carve a seat from an offset gem copy.

### Build pattern

1. **Shoulder mass** — lofted blocks on ±X short ends, starting above the finger bore (inner x > ~2.0 mm at shank level). Full band width at base, tapering toward the stone.
2. **Crown rails** — slim bars over the short-end girdle (not bulky posts).
3. **Seat cut** — boolean **DIFFERENCE** with two gem cutters:
   - Girdle fit: scale **1.03**, dy **0** (snug pocket at girdle height)
   - Pavilion relief: scale **1.004–1.010**, dy **-0.06 to -0.12** (clearance under stone)
4. **Bevel** shoulders before seat cut; bevel rails lightly after union.

### What the seat must provide

| Feature | Role |
|---------|------|
| **Bearing** | Metal below girdle on short ends — stone cannot drop |
| **Lip** | Metal above girdle on short ends — stone cannot lift out |
| **Open long sides** | ±Z faces unobstructed — light path, reference match |
| **Open table** | No metal blocking the crown from above |
| **Pavilion wall** | ≥0.8 mm metal between pocket bottom and inner bore (prefer 1.0 mm) |

## Adversarial review gates

Run before exporting any head iteration. **Stop and revert** if any gate fails.

### Physics (ray-cast or equivalent)

Cast from outside the stone toward the girdle region:

| Test | Pass criteria |
|------|---------------|
| Lip +X / -X | Metal hit above girdle (~y 12.55–12.80) |
| Bearing +X / -X | Metal hit below girdle (~y 12.55) |
| Table open | No metal within 6.4 mm of table center from above |
| Long side open | No metal blocking ±Z light path at stone mid-height |

All four lip/bearing tests must pass **and** table + long sides must be open.

### Mesh health

| Check | Pass criteria |
|-------|---------------|
| Inner diameter | 18.14 mm ± 0.15 mm |
| Non-manifold edges | 0 |
| Boundary edges | 0 |
| Wall under pavilion | ≥ 0.8 mm |
| Band thickness | ≥ 1.5 mm at shank sample |

### Visual (compare refs)

- Short-end lips grip the stone corners, not floating pillars
- Long sides fully open
- Shoulders blend into band (not isolated boxes on top)
- Silhouette closer to `reference-side-pink.png` than a rectangular bezel block

Capture screenshots to `refs/eastwest-emerald-ring/` after each review round.

## Known failure modes (do not repeat)

| Failure | Symptom | Fix |
|---------|---------|-----|
| Pillars beside stone | Stone visually floats; no lock | Overlap shoulders onto stone footprint; boolean-subtract gem |
| Shoulder into bore | ID collapses to ~14 mm | Start shoulder lofts at y ≥ 8.5, inner x ≥ 2.0 mm |
| Voxel remesh after seat | Seat pocket closes | Skip voxel remesh on seated heads; use bevel + bmesh cleanup |
| Aggressive thin-lip boolean | Mesh collapses to fragment | Build lips as separate lofts; gentler bevel; verify vert count |
| Deep pavilion cut | Bearing lost | Shallow pavilion relief (dy ≥ -0.12); re-test bearing rays |
| Long-side block | `long_side_open` fails | Do not wrap metal onto ±Z; only ±X short ends |

## Resume prompt

```
Open fixtures/eastwest-emerald-ring.blend.
Read docs/eastwest-signet-workflow.md.
Continue from eastwest_v17_refine5-polished-lock.stl.
Compare against refs/eastwest-emerald-ring/reference-side-pink.png.
Run adversarial lock + manifold gates before any new export.
Do not apply shrinkage or sprue until I specify metal/resin.
```

## Version history (this build)

| Version | Notes |
|---------|-------|
| v04–v06 | Seat cut, cleanup, initial head reshape |
| v07–v09 | Enclosure iterations (shoulders, blend, corners) |
| v10–v12 | Locked seat — physics fix via gem boolean |
| v13–v17 | Design refine — slimmer lips, swept shoulders, open gallery |
| **v17** | Best current candidate — all gates pass |
