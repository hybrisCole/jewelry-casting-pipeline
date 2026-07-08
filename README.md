# Jewelry Casting Pipeline

Standalone Cursor project for AI-assisted jewelry design through investment casting.

**Path:** `/Users/albertocole/WebstormProjects/jewelry-casting-pipeline`

## Pipeline

1. **Nano Banana** (Gemini) — concept PNGs → `concepts/`
2. **Tashvi** — jewelry mesh export (STL/OBJ/GLB, Pro required) → `meshes/raw/`
3. **Meshmixer** — mandatory repair gate → `meshes/repaired/`
4. **Blender + MCP** (Cursor Agent mode) — measure, size, hollow, shrinkage → `meshes/prepared/`
5. **Manual sprue** in Blender (see `docs/sprue-placement-sop.md`)
6. **Castable resin print** → investment casting → caliper QA

## Quick start

Full project plan: [.cursor/plans/jewelry-casting-pipeline.plan.md](.cursor/plans/jewelry-casting-pipeline.plan.md)

1. Open this folder in Cursor: **File → Open Folder**
2. Complete [docs/setup-macos.md](docs/setup-macos.md)
3. Run MCP smoke test (see setup doc)
4. Validate with `fixtures/test-ring-size8.stl` before first Tashvi import
5. Use prompts from [docs/agent-prompt-templates.md](docs/agent-prompt-templates.md)

## Folder layout

```
concepts/           Nano Banana reference PNGs
meshes/raw/         Tashvi exports
meshes/repaired/    Post-Meshmixer (mandatory gate)
meshes/prepared/    Versioned Blender checkpoints
fixtures/           Known-good test meshes
docs/               SOPs and reference tables
.cursor/            MCP config and Agent rules
```

## Checkpoint naming (never overwrite)

`ring_v01_tashvi-export.stl` → `v02_meshmixer-repaired` → `v03_blender-imported` → `v04_sized` → `v05_hollowed` → `v06_cast-ready`

## Tool versions (pinned)

- Blender: **5.1.2** (see `.blender-version`)
- MCP: [official Blender Lab MCP](https://www.blender.org/lab/mcp-server/) from [bpype/blender_mcp](https://github.com/bpype/blender_mcp) @ `98b0e49` (see `.cursor/mcp.json`)

## External tools (not in repo)

- [Tashvi](https://tashvi.ai/) — mesh generation
- [Blender Lab MCP](https://www.blender.org/lab/mcp-server/) — official Blender ↔ LLM bridge (requires Blender 5.1+)
- Meshmixer — mesh repair gate
