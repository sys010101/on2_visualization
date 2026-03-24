# O(N²) Loop Visualizer

## Project Overview
A Three.js-powered 3D visualization tool for understanding nested loop complexity (O(N²)). Built as a learning aid for QA Automation Engineers studying basic programming concepts — specifically how nested `for` loops produce quadratic time complexity.

## What It Does
- Renders an N×N grid of 3D cubes representing iterations of a nested loop
- Animates step-by-step execution: outer loop (i) and inner loop (j)
- Live Java code editor with loop parsing — edit the loop bounds and watch the grid adapt
- Shows real-time stats: current i, j, operations count, progress percentage
- Displays computed time complexity (O(N²), O(N·M), etc.)

## Tech Stack
- **Single HTML file** — zero build tools, just open in a browser
- **Three.js r128** (CDN) — 3D rendering, particles, lighting
- **Web Audio API** — procedural sound effects (no audio files needed)
- **Vanilla JS** — no frameworks

## Architecture Notes
- All state is global (single-file app, not a module system)
- `buildGrid()` rebuilds the entire cube matrix when N changes — tracked disposables prevent GPU memory leaks
- `safeEvalBound()` uses a hand-written recursive-descent parser instead of `new Function()` / `eval()` to prevent XSS
- Particle system is capped at `MAX_PARTICLES = 50` to prevent performance degradation at high speeds
- Camera uses spherical coordinates (`sph.theta`, `sph.phi`, `sph.radius`) with lerp smoothing
- Mobile layout is fully responsive with separate control sets for touch

## Key Features
- **Sound effects**: Web Audio API generates tones procedurally — tick per cell, arpeggio per row, fanfare on completion
- **Auto-orbit**: Camera slowly rotates after 5 seconds of no interaction (Homeworld-style idle cam)
- **Keyboard shortcuts**: Space (play/pause), S (step), R (reset), T (top view), M (mute), arrows (N/speed), ? (help)
- **Step mode**: Single-step through iterations one at a time for learning
- **Homeworld-style background**: Rotating starfield, procedural nebulae with drift animation, distant lit planets with glow halos
- **Completion celebration**: Diagonal wave animation + screen flash + particle burst + fanfare
- **Live code editing**: Modify the Java loop bounds and see the grid restructure in real-time

## Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward (single iteration) |
| `R` | Reset visualization |
| `T` | Toggle top-down view |
| `M` | Mute / Unmute sound |
| `Up/Down` | Change N (grid size) |
| `Left/Right` | Change animation speed |
| `?` | Show/hide keyboard shortcuts overlay |
| `Esc` | Close overlay |

## Security Fixes Applied
- **Removed `new Function()` eval** — the original `safeEval()` used `new Function('n', ...)` which is essentially `eval()` and vulnerable to code injection if loop bound expressions were crafted maliciously. Replaced with `safeEvalBound()`, a strict recursive-descent parser that only accepts digits, `n`, and basic arithmetic operators.

## Performance Fixes Applied
- **Memory leak in `buildGrid()`** — Three.js geometries, materials, and textures were not being disposed when rebuilding the grid (e.g., when changing N). Every rebuild leaked GPU memory. Now tracked via `disposables[]` array and properly disposed with `.dispose()` calls.
- **Unbounded particle spawning** — particle systems were created without limit. At high speeds with large N, hundreds of particle systems could exist simultaneously. Capped at `MAX_PARTICLES = 50`.
- **Particle geometry/material disposal** — dead particles now properly dispose their geometry and material.

## Changelog

### v2.1 — 2026-03-24
- **Feature**: Live debugger code view — code panel shows real-time execution state during animation
  - Execution pointer (▶) moves between lines showing current execution point
  - Live variable annotations: `i=3`, `j=5 (5 < 8 ✓)`, `process(3, 5) ◀ NOW`
  - Sub-phase rendering: outer-check → inner-check → body-exec → j++ → row-done → i++
  - Condition evaluation shown inline: `✓ enter` when true, `✗ exit inner/loop` when false
  - Panel title changes: "JAVA SOURCE" → "LIVE DEBUGGER" → "EXECUTION COMPLETE"
  - Color-coded line highlights: gold=outer, cyan=inner, green=body, red=exit, amber=increment
- **Fix**: Reduced scene fog density (0.022 → 0.006) so background space objects are visible
- **Fix**: Stars, nebulae, and planets now exempt from fog (`fog = false`)
- **Fix**: Planets switched from MeshPhongMaterial to MeshBasicMaterial (self-lit, visible at distance)
- **Enhancement**: Stars increased to 1500 with multi-color variation (blue-white, warm, deep blue)
- **Enhancement**: Nebulae moved closer and made brighter, plus large background wash nebula
- **Enhancement**: Code panel widened (310px → 440px) to accommodate debug annotations

### v2.0 — 2026-03-24
- **Security**: Replaced `new Function()` eval with safe recursive-descent parser
- **Performance**: Fixed memory leak — proper disposal of Three.js geometries/materials/textures on grid rebuild
- **Performance**: Capped particle system count, added disposal for dead particles
- **Feature**: Web Audio API sound system (cell tick, row arpeggio, completion fanfare, reset sound)
- **Feature**: Auto-orbit camera after 5s idle with indicator
- **Feature**: Keyboard shortcuts (Space, S, R, T, M, arrows, ?, Esc)
- **Feature**: Step-forward button for single-step execution
- **Feature**: Enhanced completion celebration (diagonal wave + screen flash + particles)
- **Feature**: Rotating starfield atmosphere
- **Feature**: Homeworld-style procedural nebulae with drift animation
- **Feature**: Distant planets with glow halos and self-rotation
- **Feature**: Keyboard shortcuts help overlay (? key)
- **UI**: Added SFX toggle button, STEP button, KEYS button to control bars (desktop + mobile)

### v1.0 — Original
- Base Three.js visualization with N×N cube grid
- Play/pause/reset controls with speed and light sliders
- Live Java code editor with syntax highlighting and loop parsing
- Mobile-responsive layout with touch orbit/pan/zoom
- Particle effects on cube activation
- Top-down view toggle
