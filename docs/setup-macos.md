# macOS Setup

Complete these steps before using Cursor Agent with Blender.

**Stack:** Blender **5.1.2** + [official Blender Lab MCP](https://www.blender.org/lab/mcp-server/) (not the third-party ahujasid addon).

## 1. Install prerequisites

```bash
brew install uv
```

- **Blender 5.1.2** — installed; must match `.blender-version`. Official MCP requires **Blender 5.1 or newer**.
- **Meshmixer** — free from Autodesk (archive download). Required repair gate.

Record your printer and resin in `docs/equipment.md`.

## 2. Blender extensions and add-ons (one-time)

Blender 5.1 uses **Edit → Preferences → Get Extensions** for most add-ons. Enable online access when prompted so packages can install from [extensions.blender.org](https://extensions.blender.org/).

### Required for casting prep

| Add-on | Extension ID / module | Purpose |
|--------|----------------------|---------|
| **3D Print Toolbox** | `object_print3d_utils` (legacy) or `print3d-toolbox` | Wall thickness, overhang, manifold checks |
| **MeasureIt** | `measureit` | Dimension overlays in the viewport |
| **LoopTools** | `looptools` | Circle, bridge, flatten — band cleanup |
| **Bool Tool** | `bool-tool` | Boolean cuts for hollows and stone pockets |
| **Extra Mesh Objects** | `add_mesh_extra_objects` (legacy) or `extra-mesh-objects` | Extra primitives and generators |
| **JewelCraft** | `jewelcraft` | Gems, bezels, prongs, metal weight |
| **CAD Sketcher** | `CAD_Sketcher` | Constraint-based parametric sketches |

### Already required for this repo

| Add-on | Source | Purpose |
|--------|--------|---------|
| **Blender Lab MCP** | [lab.blender.org](https://www.blender.org/lab/mcp-server/) | Cursor Agent ↔ Blender bridge |

### Install from extensions.blender.org

Edit → Preferences → **Get Extensions** → search and install:

1. `print3d-toolbox` (or enable legacy `object_print3d_utils` if online install fails)
2. `measureit`
3. `looptools`
4. `bool-tool`
5. `extra-mesh-objects` (or enable legacy `add_mesh_extra_objects` if online install fails)
6. `jewelcraft` — drag [JewelCraft zip](https://github.com/mrachinskiy/jewelcraft/releases) into Blender if not in the repo
7. `CAD Sketcher` — install from disk via [CAD Sketcher releases](https://github.com/hlorus/CAD_Sketcher) if not in the repo

After each install, confirm the add-on is **enabled** under Preferences → Add-ons.

### Verify installation

In Blender Preferences → Add-ons, search and confirm these are enabled:

- 3D-Print Toolbox
- MeasureIt
- LoopTools
- Bool Tool
- Extra Objects
- JewelCraft
- CAD Sketcher
- MCP

### Notes

- **CAD Sketcher** is still experimental — keep `.blend` backups before parametric edits.
- **JewelCraft** targets Blender 4.2+; verify behavior on 5.1.2 before relying on it in production.
- If `print3d-toolbox` or `extra-mesh-objects` fail to install from the online repo, use the legacy modules from [blender/blender-addons](https://github.com/blender/blender-addons): `object_print3d_utils` and `add_mesh_extra_objects`.

## 3. Install official Blender Lab MCP add-on

Source: [blender.org/lab/mcp-server](https://www.blender.org/lab/mcp-server/) · repo: [bpype/blender_mcp](https://github.com/bpype/blender_mcp)

**Recommended (drag-and-drop into Blender):**

1. Drag the Blender Lab extension bundle into Blender → adds the **Blender Lab** repository
2. Drag again → installs the **MCP Server** add-on
3. Enable the add-on in Preferences → Add-ons

**Alternative:** download the add-on zip from the [release page](https://github.com/bpype/blender_mcp/releases) → Edit → Preferences → Add-ons → Install from Disk.

**In Blender preferences for the add-on:**

- Configure host/port if needed (defaults usually fine)
- Optional: enable auto-start
- Click **Start** in the add-on panel when ready to connect

## 4. Cursor MCP

This repo's `.cursor/mcp.json` launches the **official** MCP server from the pinned git commit (not PyPI `blender-mcp`, which is a different third-party package):

```json
{
  "mcpServers": {
    "blender": {
      "command": "uvx",
      "args": [
        "--from",
        "git+https://github.com/bpype/blender_mcp.git@98b0e49#subdirectory=mcp",
        "blender-mcp"
      ]
    }
  }
}
```

1. Open **this folder** in Cursor (File → Open Folder)
2. Settings → MCP → confirm `blender` server appears and is enabled
3. **Only run MCP in one client at a time** (Cursor OR another MCP host)

### Security note

The official MCP executes LLM-generated Python inside Blender **without sandboxing**. See the [security warning](https://www.blender.org/lab/mcp-server/). For this jewelry pipeline that is acceptable — no sensitive data in Blender scenes — but do not point it at production blend files with secrets.

## 5. Smoke test (required)

With Blender 5.1.2 open, MCP add-on running, and Cursor MCP connected, switch to **Agent mode** and run:

> Create a UV sphere at origin, apply Subdivision Surface level 2, export as `fixtures/smoke-test.stl`.

If this fails, fix MCP before any jewelry work.

## 6. Validate fixture

Run Agent workflow on `fixtures/test-ring-size8.stl` before first production mesh. See README checklist.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP not connecting | Blender add-on started? Only one MCP client open? |
| Wrong MCP package | Do **not** use PyPI `blender-mcp` (ahujasid) — use this repo's `.cursor/mcp.json` |
| uvx not found | `brew install uv`, restart terminal |
| Blender version error | Official MCP requires 5.1+; check `.blender-version` |
| Wrong scale on import | Check mm units, Apply Transforms, bounding box |
| First command timeout | Retry; known quirk on first MCP call |
| Extension install fails | Use **Install from Disk** (arrow menu in Get Extensions); keep zips uncompressed |
| 3D Print Toolbox missing | Install legacy `object_print3d_utils` from blender-addons repo |
