---
name: jewelry-casting-prep
description: Prepare jewelry meshes for castable resin printing via Blender MCP. Use when importing STL, ring sizing, hollowing, shrinkage scaling, cast-ready export, east-west signet head design, gem enclosure locking, adversarial mesh review, or Blender CAD prep with JewelCraft/CAD Sketcher.
---

# Jewelry Casting Prep

## When to use

- Importing repaired meshes from `meshes/repaired/`
- Building or refining ring heads / gem enclosures in Blender
- Measuring inner diameter and band width (MeasureIt)
- Sizing to US ring sizes
- Hollow, shrinkage scale, cast-ready export
- 3D Print Toolbox wall-thickness and manifold checks
- Resuming the east-west emerald signet build

## Required reading (in order)

1. [docs/setup-macos.md](../../docs/setup-macos.md) — Blender 5.1.2, casting add-ons, official MCP
2. [docs/mesh-health-gate.md](../../docs/mesh-health-gate.md)
3. [docs/ring-size-chart.md](../../docs/ring-size-chart.md)
4. [docs/eastwest-signet-workflow.md](../../docs/eastwest-signet-workflow.md) — **gem lock, adversarial gates, checkpoint naming**
5. [docs/shrinkage-table.md](../../docs/shrinkage-table.md)
6. [docs/sprue-placement-sop.md](../../docs/sprue-placement-sop.md)
7. [docs/agent-prompt-templates.md](../../docs/agent-prompt-templates.md)

## Quick workflow

```
Task Progress:
- [ ] Scene: Metric, mm, scale 1.0; apply transforms
- [ ] Band blank + taper (checkpoint STL)
- [ ] JewelCraft gem (visual only)
- [ ] Head / enclosure with mechanical stone lock
- [ ] Adversarial gates (physics + mesh + visual)
- [ ] Meshmixer repair gate
- [ ] Cast prep (ask metal/resin first)
- [ ] Manual sprue (user only)
```

## Gem enclosure — non-negotiable

Stone must be **mechanically locked**, not visually placed:

1. Shoulders **overlap** stone footprint on short ends (±X).
2. Boolean-subtract offset gem copy: girdle fit (scale ~1.03) + pavilion relief (scale ~1.004–1.010, dy -0.06 to -0.12).
3. Verify **bearing** (metal below girdle) and **lip** (metal above girdle) on both short ends.
4. Keep **long sides open** (±Z) and **table open** from above.

Full build pattern and failure modes: [docs/eastwest-signet-workflow.md](../../docs/eastwest-signet-workflow.md).

## Adversarial gates (run before export)

| Gate | Pass |
|------|------|
| Lip +X / -X | Metal above girdle |
| Bearing +X / -X | Metal below girdle |
| Table / long sides | Open (no blocking metal) |
| Inner diameter | Target ± 0.15 mm |
| Manifold / boundary | 0 / 0 |
| Wall under pavilion | ≥ 0.8 mm |

Stop and revert to previous checkpoint on failure.

## Checkpoint naming

Never overwrite. Increment version per major step:

`{project}_v01_…` → `v02_…` → … → `v06_cast-ready`

East-west example: `eastwest_v17_refine5-polished-lock.stl` (current best).

## Canonical east-west resume

| Asset | Path |
|-------|------|
| Scene | `fixtures/eastwest-emerald-ring.blend` |
| Best mesh | `meshes/prepared/eastwest_v17_refine5-polished-lock.stl` |
| Refs | `refs/eastwest-emerald-ring/` |

## Blender add-ons

| Add-on | Use |
|--------|-----|
| 3D Print Toolbox | Wall thickness, manifold, overhang |
| MeasureIt | ID, band width, drain hole |
| LoopTools | Band cleanup, seams |
| Bool Tool | Hollows, stone pockets |
| JewelCraft | Gem sizes, visual placement |
| CAD Sketcher | Parametric profiles (keep backups) |

## Agent constraints

- **Ask** metal alloy and resin before shrinkage
- **Never** uniform-scale head + band for sizing
- **Never** auto-generate sprues
- Band/head separation for sizing is **supervised**
- Decimate to ~50–100K tris before complex booleans if >500K
- Save `.blend` to `fixtures/` before closing Blender

## Prompt templates

See [docs/agent-prompt-templates.md](../../docs/agent-prompt-templates.md) — Template F resumes the east-west build.
