# Mesh Health Gate

**Do not open Blender until this gate passes.**

Source meshes (AI-generated, sculpted, or CAD-exported) are aesthetic geometry, not engineering models. Expect non-manifold edges, holes, and self-intersections.

## Checklist

1. **Export from Blender CAD** → save to `meshes/raw/ring_v01_raw-export.stl`
2. **Scale sanity** — bounding box approximately 20–24 mm × 20–24 mm × 6–10 mm for a ring
3. **Poly count** — if >500K triangles, decimate target ~50–100K before Blender booleans
4. **Meshmixer repair**
   - Import STL
   - Analysis → Auto Repair All
   - Inspector → fix until **zero errors**
5. **Pass criteria**
   - Watertight / manifold
   - Zero non-manifold edges
   - Zero self-intersections (uncheck overlapping checks if false positives on dense meshes)

6. **Export** → `meshes/repaired/ring_v02_meshmixer-repaired.stl`

## If repair fails

- Simplify the design in Blender (reduce detail, fix booleans)
- Reduce decorative detail
- Do **not** force unrepairable mesh through Blender MCP

## Optional cross-check

Open repaired STL in PrusaSlicer or MeshLab and confirm dimensions before Blender.
