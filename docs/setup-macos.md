# macOS Setup

Complete these steps before using Cursor Agent with Blender.

## 1. Install prerequisites

```bash
brew install uv
```

- **Blender 4.1.1** — download from [blender.org](https://www.blender.org/download/). Match `.blender-version`.
- **Meshmixer** — free from Autodesk (archive download). Required repair gate.
- **Tashvi Pro** — required for STL/OBJ/GLB mesh export.

Record your printer and resin in `docs/equipment.md`.

## 2. Blender addons

Edit → Preferences → Add-ons → enable:

- **Mesh: 3D Print Toolbox**
- **MeasureIt** (optional)

## 3. Install blender-mcp addon

1. Download `addon.py` from [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp)
2. Blender → Edit → Preferences → Add-ons → Install → select `addon.py`
3. Enable **Interface: Blender MCP**
4. Press **N** in 3D viewport → BlenderMCP panel → Start server → **Connect**

## 4. Cursor MCP

This repo includes `.cursor/mcp.json` with `blender-mcp==1.6.4`.

1. Open **this folder** in Cursor (File → Open Folder)
2. Settings → MCP → confirm `blender` server appears and is enabled
3. **Only run MCP in Cursor OR Claude Desktop**, not both (port 9876 conflict)

## 5. Smoke test (required)

With Blender open and MCP connected, switch to **Agent mode** and run:

> Create a UV sphere at origin, apply Subdivision Surface level 2, export as `fixtures/smoke-test.stl`.

If this fails, fix MCP before any jewelry work.

## 6. Validate fixture

Run Agent workflow on `fixtures/test-ring-size8.stl` before first Tashvi import. See README checklist.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP not connecting | Blender addon running? Only one MCP client open? |
| uvx not found | `brew install uv`, restart terminal |
| Wrong scale on import | Check mm units, Apply Transforms, bounding box |
| First command timeout | Retry; known blender-mcp quirk on first call |
