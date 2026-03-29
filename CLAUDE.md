# Java Code Visualizer

## Project Overview

A collection of single-file 3D visualizations that teach Java programming concepts to people with little or no coding background. Instead of dry explanations, each visualization **shows** how the code executes — making abstract concepts tangible.

Built as standalone HTML files using Three.js (r128) — no build step, no dependencies to install. Open in a browser and go.

### Visualizations

| File | Concept | Description |
|------|---------|-------------|
| `on2_visualization.html` | O(N²) nested loops | 3D NxN grid of cubes that light up as nested `for` loops run |
| `username_validator_vis.html` | do-while / continue / break | 3D character cubes scanned by sequential validation rules |

## Target Audience

People who are visual learners and want to understand Big-O complexity without heavy math. The project prioritizes intuition over formalism. Originally built as a learning aid for QA Automation Engineers studying basic programming concepts.

## Tech Stack

- **Single HTML file** — all CSS, HTML, and JS inline
- **Three.js r128** (CDN) — 3D rendering, particles, lighting
- **Web Audio API** — procedural sound effects (no external audio files)
- **Vanilla JS** — no frameworks, no build tools

## Architecture

Each visualization is a single HTML file with inline CSS, HTML, and JS, organized into clearly commented sections. Shared patterns across all visualizations:

- **Three.js r128** scene with renderer, camera, lights, orbit controls
- **Space background** — multi-color starfield, procedural nebulae, distant planets with glow
- **Web Audio API** — procedural sound effects (no external audio files)
- **Syntax-highlighted code panel** — shows the Java source with live execution annotations
- **Particle system** — pool-limited for GPU safety
- **Keyboard shortcuts** — consistent across visualizations (Space, S, R, M, ?, Esc)

## O(N²) Loop Visualizer (`on2_visualization.html`)

### Features
- 3D NxN grid of cubes representing nested loop iterations
- Adjustable N (3–14), speed, and lighting via sliders
- Live Java code editor — edit the loop bounds and see the grid adapt
- **Live debugger view** — value substitution, execution pointer, condition evaluation (✓/✗)
- **Step-through mode** — advance one operation at a time
- **Heat map** — color cubes blue→red by visit order
- **Traversal toggle** — row-major / column-major sweep
- **Comparison panel** — O(N) vs O(N²) vs O(N³) visual bars + projections for large N
- **Completion celebration** — diagonal wave + screen flash + particle burst + fanfare

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward |
| `R` | Reset |
| `T` | Toggle top-down view |
| `M` | Mute / Unmute |
| `Up/Down` | Change N (grid size) |
| `Left/Right` | Change speed |
| `?` | Help overlay |

### Security Notes
Expression evaluation uses a **safe recursive-descent parser** (`safeEvalBound()`) — no `eval()`, no `new Function()`. Max expression length capped at 30 characters.

## Username Validator Visualizer (`username_validator_vis.html`)

### Concepts Taught
- **do-while loop** — runs at least once, then checks condition
- **continue** — skips remaining rules and jumps to the while condition
- **break** — exits the for loop early when a match is found
- **Sequential validation** — three rules checked in order

### Features
- 3D character cubes with letters rendered on faces, rebuilt live as user types
- **Scanning beam** moves across cubes during character-by-character checks (Rules 2 & 3)
- **Color-coded states** — cyan=scanning, green=passed, red=failed, gold=length check
- **Live code panel** — shows the full Java source with current-line highlighting, variable values, and ✓/✗ condition results
- **Resizable code panel** — drag left edge, bottom edge, or bottom-left corner
- **Custom username input** — type anything, cubes update in real-time
- **Preset examples** — ab1 (too short), test user1 (space), testuser (no digit), player1 (valid)
- **do-while loop-back** — on failure, pauses for new input (simulates `scanner.nextLine()`)
- **Concept badge** — updates to explain the current concept (do-while, continue, break)
- **Auto-orbit toggle** — on/off button, disabled by default for clarity

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward one phase |
| `R` | Reset |
| `M` | Mute / Unmute |
| `O` | Toggle auto-orbit camera |
| `1-4` | Load preset username |
| `Left/Right` | Change speed |
| `Enter` | Start validation (when input focused) |
| `?` | Help overlay |

## Performance Notes

- **Memory leak prevention**: All Three.js geometries, materials, and textures tracked and `.dispose()`d on rebuild
- **Particle pool cap**: `MAX_PARTICLES` prevents unbounded growth at high speeds
- **Scene fog**: Low density so deep-space background objects remain visible; stars/nebulae/planets exempt

## Known Limitations

- Three.js r128 is older; a version bump would bring performance improvements
- Single-file architecture means no code splitting — fine for this scale
- O(N²) code parser uses regex for standard `for(int i=0; i<expr; i++)` patterns only

## Development

No build step. Edit the HTML file and refresh the browser.

To test: open any visualization file in a modern browser (Chrome, Firefox, Edge, Safari).

## Changelog

### v4.0 (2026-03-29)
- **New visualization**: Username Validator (`username_validator_vis.html`)
  - Teaches do-while, continue, break, and sequential validation
  - 3D character cubes with live input, scanning beam, color-coded states
  - Tokenized syntax highlighter (single-pass, no cascading corruption)
  - Resizable code panel (left/bottom/corner drag handles)
  - Auto-orbit camera toggle (default off)
  - do-while loop-back pauses for user input like real `scanner.nextLine()`
- Updated CLAUDE.md to document multi-visualization project structure

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
