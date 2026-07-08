# Agent Prompt Templates

Use in **Cursor Agent mode** with Blender open and MCP connected.

## Template A — Import and measure

```
Import meshes/repaired/ring_v02_meshmixer-repaired.stl.
Set scene units to mm. Apply all transforms.
Report: inner diameter, band width, triangle count, bounding box, manifold status.
Do not modify the mesh yet.
Export checkpoint as meshes/prepared/ring_v03_blender-imported.stl.
```

## Template B — Size to target (supervised)

```
Target US ring size 8 (18.14 mm inner diameter). See docs/ring-size-chart.md.
Separate band from head per jewelry-casting rules. Scale band only. Rejoin and clean seam.
Log before/after inner diameter.
Export meshes/prepared/ring_v04_sized.stl.
Stop and ask if band/head boundary is unclear.
```

## Template C — Cast prep (no sprue)

```
Ask me for metal alloy and castable resin brand before applying shrinkage.
Hollow to 1.2 mm wall thickness with ≥1 mm drain hole for flask drainage.
Apply shrinkage from docs/shrinkage-table.md after I confirm metal/resin.
Run 3D Print Toolbox checks; flag any wall < 0.8 mm.
Export meshes/prepared/ring_v06_cast-ready.stl.
Do NOT add sprue — I will attach manually per docs/sprue-placement-sop.md.
```

## Template D — Smoke test

```
Create a UV sphere at origin, apply Subdivision Surface level 2,
export as fixtures/smoke-test.stl. Confirm file was written.
```

## Template E — Fixture validation

```
Import fixtures/test-ring-size8.stl. Measure inner diameter — expect ~18.14 mm.
Run cast prep template logic (ask metal/resin first).
Export to meshes/prepared/fixture-test-output.stl with measurement report.
```
