# macOS Setup

Complete these steps before using Cursor Agent with Blender.

**Stack:** Blender **5.1.2** + [official Blender Lab MCP](https://www.blender.org/lab/mcp-server/) (not the third-party ahujasid addon).

## 1. Install prerequisites

```bash
brew install uv
```

- **Blender 5.1.2** — installed; must match `.blender-version`. Official MCP requires **Blender 5.1 or newer**.
- **Meshmixer** — free from Autodesk (archive download). Required repair gate.
- **Tashvi Pro** — required for STL/OBJ/GLB mesh export.

Record your printer and resin in `docs/equipment.md`.

## 2. Blender addons (one-time)

Edit → Preferences → Add-ons → enable:

- **Mesh: 3D Print Toolbox** — wall thickness, overhang, manifold checks
- **MeasureIt** (optional) — dimension overlays

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

Run Agent workflow on `fixtures/test-ring-size8.stl` before first Tashvi import. See README checklist.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| MCP not connecting | Blender add-on started? Only one MCP client open? |
| Wrong MCP package | Do **not** use PyPI `blender-mcp` (ahujasid) — use this repo's `.cursor/mcp.json` |
| uvx not found | `brew install uv`, restart terminal |
| Blender version error | Official MCP requires 5.1+; check `.blender-version` |
| Wrong scale on import | Check mm units, Apply Transforms, bounding box |
| First command timeout | Retry; known quirk on first MCP call |
