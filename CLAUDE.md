# Java Code Visualizer

## Project Overview

A single-file 3D visualization tool that teaches algorithm time complexity (starting with O(N²)) to people with little or no math background. Instead of equations, it **shows** nested loops as a 3D grid of cubes that light up as the algorithm runs — making the "N times N" concept tangible.

Built as a standalone HTML file using Three.js (r128) — no build step, no dependencies to install. Open in a browser and go.

## Target Audience

People who are visual learners and want to understand Big-O complexity without heavy math. The project prioritizes intuition over formalism. Originally built as a learning aid for QA Automation Engineers studying basic programming concepts.

## Tech Stack

- **Single HTML file** — all CSS, HTML, and JS inline
- **Three.js r128** (CDN) — 3D rendering, particles, lighting
- **Web Audio API** — procedural sound effects (no external audio files)
- **Vanilla JS** — no frameworks, no build tools

## Architecture

The file is organized into clearly commented sections:

1. **CSS** — Responsive layout with desktop and mobile breakpoints (767px)
2. **HTML** — UI panels: stats, code viewer/editor, controls, comparison panel
3. **JS Sections:**
   - DOM helpers & slider sync
   - Web Audio sound system (oscillators, tones, fanfare)
   - Safe expression evaluator (recursive-descent parser, no eval/Function)
   - Three.js renderer, scene, camera, lights
   - Orbit controls (mouse + touch + auto-rotate with idle detection)
   - Environment (grid, multi-color starfield, procedural nebulae, planets with glow)
   - State management
   - Java code parsing & syntax highlighting
   - Live debugger view (value substitution, execution phases)
   - Code editor (live edit with re-parse)
   - Heat map coloring system
   - Traversal pattern (row-major / column-major)
   - Comparison & projection panel
   - Ops counter pulse animation
   - Grid builder (with GPU resource disposal)
   - Particle system (with pool limit + disposal)
   - Step logic with sub-phase timing
   - Keyboard shortcuts
   - Main animation loop

## Features

### Core
- 3D NxN grid of cubes representing nested loop iterations
- Adjustable N (3–14), speed, and lighting via sliders
- Play/Pause/Reset animation controls
- Live Java code editor — edit the loop bounds and see the grid adapt
- Syntax-highlighted code panel with active line highlighting
- Orbit camera (drag), zoom (scroll/pinch), pan (right-click/2-finger)
- Fully responsive (desktop + mobile layouts)

### Interactive
- **Live debugger view** — code panel shows real-time execution state with value substitution, execution pointer, condition evaluation (✓/✗), phase highlighting (gold=outer, cyan=inner, green=body, red=exit)
- **Step-through mode** — advance one operation at a time
- **Sound effects** — pitch-rising ticks per cell, ascending arpeggio per row, C-major fanfare on completion
- **Heat map** — toggle to color cubes blue→red by visit order
- **Traversal toggle** — switch between row-major and column-major sweep
- **Comparison panel** — O(N) vs O(N²) vs O(N³) visual bars
- **"What if N was bigger?"** — projections for N=50, 100, 1,000, 10,000
- **Ops counter pulse** — animated highlight on each increment
- **Camera auto-rotate** — gentle orbit after 5s idle with indicator, stops on interaction
- **Top/Orbit view** — toggle bird's-eye view
- **Completion celebration** — diagonal wave animation + screen flash + particle burst + fanfare
- **Keyboard shortcuts** — Space (play/pause), S (step), R (reset), T (top view), M (mute), arrows (N/speed), ? (help overlay)

### Environment
- **Rotating starfield** — 1500 multi-color stars (blue-white, warm, deep blue) with depth
- **Procedural nebulae** — Homeworld-style gas clouds with drift animation
- **Distant planets** — self-lit spheres with glow halos and slow rotation

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

## Security Notes

Expression evaluation in the code editor uses a **safe recursive-descent parser** (`safeEvalBound()`) that only accepts digits, `n`, `+`, `-`, `*`, `/`, `(`, `)`, and spaces. No `eval()`, no `new Function()`, no property access. Maximum expression length capped at 30 characters.

## Performance Notes

- **Memory leak prevention**: All Three.js geometries, materials, and textures tracked via `disposables[]` array and `.dispose()`d on grid rebuild
- **Particle pool cap**: `MAX_PARTICLES = 50` prevents unbounded particle system growth at high speeds
- **Scene fog**: Light density (0.006) so deep-space background objects remain visible; stars/nebulae/planets exempt from fog

## Known Limitations

- Three.js r128 is older; a version bump would bring performance improvements
- Single-file architecture means no code splitting — fine for this scale but doesn't scale to many visualizations
- The Java code parser uses regex and handles standard `for(int i=0; i<expr; i++)` patterns — non-standard loop forms won't parse

## Development

No build step. Edit `on2_visualization.html` and refresh the browser.

To test: open the file in any modern browser (Chrome, Firefox, Edge, Safari).

## Changelog

### v3.0 (2026-03-24)
**Merge of v2.1-a (this Claude) and v2.1-b (other Claude):**
- Combined all features from both independent development branches
- v2.1-a contributed: comparison panel, heat map, traversal toggle, ops counter pulse
- v2.1-b contributed: live debugger view, Homeworld-style space background, keyboard shortcuts, completion celebration, auto-orbit indicator
- Unified sound system (tick/arpeggio/fanfare from v2.1-b, integrated with traversal from v2.1-a)
- Traversal mode now works with live debugger view and sub-phase rendering

### v2.1-b (2026-03-24) — Other Claude
- Live debugger code view with execution pointer, variable annotations, sub-phases
- Reduced fog density, stars/nebulae/planets exempt from fog
- Planets switched to MeshBasicMaterial (self-lit, visible at distance)
- Stars increased to 1500 with multi-color variation
- Nebulae moved closer and made brighter, plus large background wash
- Code panel widened (310px → 440px) for debug annotations

### v2.1-a (2026-03-24) — This Claude
- Mobile layout fix: toggle buttons in second row, badge repositioning
- Added `#tb-row2` to touch UI exclusion list

### v2.0 (2026-03-24)
**Bug Fixes:**
- **Security**: Replaced `new Function()` eval with safe recursive-descent math parser
- **Memory leaks**: All Three.js geometries, materials, and textures now tracked and `.dispose()`d
- **Slider sync**: Desktop and mobile sliders stay in sync bidirectionally

**New Features:**
- Step-through mode, sound effects, comparison panel, heat map, ops counter pulse
- Traversal pattern toggle, camera auto-rotate, keyboard shortcuts
- Enhanced completion celebration, rotating starfield, procedural nebulae, distant planets

### v1.0 (initial)
- Core O(N²) 3D visualization
- Play/Pause/Reset controls
- Live code editor with syntax highlighting
- Responsive desktop + mobile layout
- Orbit/zoom/pan camera controls
- Particle effects on cube activation
- O(N²) complexity badge
