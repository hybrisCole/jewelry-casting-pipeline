---
name: jewelry-casting-prep
description: Prepare jewelry meshes for castable resin printing via Blender MCP. Use when importing STL, ring sizing, hollowing, shrinkage scaling, cast-ready export, or Blender CAD prep with JewelCraft/CAD Sketcher.
---

# Jewelry Casting Prep (v1 stub)

Full skill content deferred until two complete manual+Agent pipeline runs.

## When to use

- Importing repaired meshes from `meshes/repaired/`
- Measuring inner diameter and band width (MeasureIt)
- Sizing to US ring sizes
- Hollow, shrinkage scale, cast-ready export
- 3D Print Toolbox wall-thickness and manifold checks

## Required reading (in order)

1. [docs/setup-macos.md](../../docs/setup-macos.md) — Blender 5.1.2, casting add-ons, official MCP setup
2. [docs/mesh-health-gate.md](../../docs/mesh-health-gate.md)
3. [docs/ring-size-chart.md](../../docs/ring-size-chart.md)
4. [docs/shrinkage-table.md](../../docs/shrinkage-table.md)
5. [docs/sprue-placement-sop.md](../../docs/sprue-placement-sop.md)
6. [docs/agent-prompt-templates.md](../../docs/agent-prompt-templates.md)

## Blender add-ons for this workflow

| Add-on | Use in pipeline |
|--------|-----------------|
| 3D Print Toolbox | Wall thickness, manifold, overhang checks before export |
| MeasureIt | Verify ring ID, band width, drain-hole size |
| LoopTools | Clean band geometry and seams |
| Bool Tool | Hollow interiors, stone pockets, cutaways |
| Extra Mesh Objects | Reference primitives and generators |
| JewelCraft | Gem sizes, bezels, prongs (solitaire settings) |
| CAD Sketcher | Parametric band profiles and constrained dimensions |

## Agent prompts

Use templates in `docs/agent-prompt-templates.md`. Always checkpoint STL versions.
