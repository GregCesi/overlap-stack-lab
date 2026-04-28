# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Overlap Stack Lab** is a standalone, zero-dependency photo composition tool built as a single HTML file (`index.html`). It is used to pre-integrate photo layouts for the Jardin de Marguerite website. No build system, no package manager, no dependencies.

## Running the Project

Open directly in a browser, or (recommended) serve locally for reliable localStorage:

```bash
python3 -m http.server 8000
# Then open http://localhost:8000
```

`file://` protocol works in Chrome but is blocked by Safari for localStorage. There are no tests, no linting, no compilation steps.

## Architecture

Everything lives in `index.html` (~1700 lines): styles (lines 8–429), HTML structure (lines 430–579), and JavaScript (lines 580–1693).

### State (`S` object)

All application state is in a single global `S`:

```javascript
S = {
  sections: [{ id, name, variant, slots, photos, pendingSlots, imgTransforms }],
  activeSection: string,
  selectedSlot: string | null,
  bankFilter: 'all' | 'unassigned',
  _pid: number  // auto-increment photo ID
}
```

`saveState()` persists section metadata and slot assignments to `localStorage` under the key `overlap-stack-lab-v1`. Photo blobs are not persisted (too large); only filenames are stored and re-linked on the next session if photos are re-uploaded with matching names. This is the **pending slots** mechanism.

### Render cycle

State mutations always end with a call to `renderAll()` or one of its parts:

| Function | Purpose |
|---|---|
| `renderCanvas()` | Center overlay grid with photo slots |
| `renderBank()` | Left panel photo thumbnails |
| `renderSlots()` | Right panel slot assignment list |
| `renderSections()` | Right panel section selector |
| `renderAll()` | Calls all four |

### Layout Variants

Five variants (`v01`–`v05`) are defined in the `VARIANTS` constant. Each variant has a fixed set of named slots (`main`, `second`, `detail`, `open`, `extra`) and a canvas height. Slot geometry is defined purely in CSS via variant-specific absolute positioning classes.

### Image Editing

Per-slot image transforms (`scale`, `x`, `y`, `rotation`, `flipH`) are stored in `S.sections[i].imgTransforms[slot]`. The editor uses two rendering modes:

- **Display mode**: `object-fit: cover` at 100% × 100% — default view
- **Edit mode**: Natural-size image positioned with `inset: auto`, full CSS transform stack applied — activated by clicking the edit icon or double-clicking a slot

Exiting edit mode restores display mode. Zoom uses cursor-centered pivot math; scroll wheel and `+`/`-` keys both work.

### Keyboard Shortcuts

| Key | Action |
|---|---|
| `1`–`5` | Switch layout variant |
| `E` | Enter edit mode for selected slot |
| `+` / `-` | Zoom in/out (edit mode) |
| `R` | Reset image to default framing |
| `0` | Zoom to 100% |
| `Escape` | Exit edit mode / deselect slot |

### Export

The right panel exports a JSON config (`overlap-stack-config.json`) containing section metadata, slot-to-filename mappings, and per-slot transforms. This JSON is consumed by the Jardin de Marguerite website.
